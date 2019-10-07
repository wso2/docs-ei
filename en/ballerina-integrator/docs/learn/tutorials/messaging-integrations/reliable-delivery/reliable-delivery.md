---
title: Reliable message delivery
commitHash: 1d8ae9a112965cc1b7c0409fec0adb877181bd0c
note: This is an auto-generated file do not edit this, You can edit content in "ballerina-integrator" repo
---

## About

This guide describes how to use **MessageStore** module to achieve reliable message delivery. In asynchronous messaging
scenarios, we accept the HTTP message and store it in a remote **message broker** for later processing. When the message forwarder picks the message from the **message broker** and try to forward it to the intended HTTP service, that service may not be available at that moment, or it may respond with *500 (internal server error)*. In that case, the service should retry to deliver the message.

Only upon successful message delivery, the message will be removed from the queue. Otherwise, after several retries, message
forwarder can stop or backup the message to a different store (Dead Letter Store) and continue with the next message.

Above scenario is called **reliable message delivery**. There are also other use-cases you can achieve using the *MessageStore* module.

1. **in-order message delivery** : If only one message processor is picking messages from the particular store, in-order message delivery pattern is also achieved. Messages are delivered to the endpoint in a reliable manner keeping the order of the messages they were put to the store.

2. **throttling** : Consider a legacy backend which can process messages only up to 100 TPS. This service is exposed to a system from which it gets message bursts, sometimes exceeding its limit of 100 TPS. To regulate the load we can use message store and forward pattern. System can store the message in the store in the speed it likes and message processor will forward the messages to the backend in a regulated way with its defined polling interval.

## What you'll build

Let's consider a scenario where an incoming HTTP request is stored using *messageStore*. The message store is pointed
to a queue of *ActiveMQ* broker. The caller of the HTTP request will receive an HTTP accept *202* response, if the message
is stored successfully on the broker. Message forwarding task is initiated by another Ballerina service which polls messages
from the above store and invoke configured backend HTTP service.

This backend service is a test service which will print headers and the incoming message, and respond with a static payload.

Upon receiving the response from the backend, user is allowed to perform any action upon the response as per the design
of *messageStore* module. This action is passed as a Lambda (function pointer) to the message processor when it is created.
For simplicity, in this guide it will just log the response from the backend to the Ballerina log file.

The following diagram illustrates the scenario in high level:

![Tutorial Image](https://github.com/wso2/ballerina-integrator/blob/d31a6b9d5579ce3bde4a90a7064eba4c8dddb6a6/examples/guides/messaging/reliable-delivery/resources/message-store-processor-guide.svg)

## Prerequisites
 
* Ballerina Integrator
* Oracle JDK 1.8.*
* A Text Editor or an IDE 
> **Tip**: For a better development experience, install the Ballerina Integrator extension in [VS Code](https://code.visualstudio.com).
* [Apache ActiveMQ](http://activemq.apache.org/getting-started.html)
  * After you install ActiveMQ, copy the .jar files from the *<AMQ_HOME>/lib* directory to the *<BALLERINA_HOME>/bre/lib* directory.
  * If you use ActiveMQ version 5.12.0, you only have to copy *activemq-client-5.12.0.jar*, *geronimo-j2ee-management_1.1_spec-1.0.1.jar*, and *hawtbuf-1.11.jar* from the *<AMQ_HOME>/lib* directory to the *<BALLERINA_HOME>/bre/lib* directory.
  
## Get the code

Pull the module from [Ballerina Central](https://central.ballerina.io/) using the following command.

```bash
ballerina pull wso2/<<<MODULE_NAME>>>
```

Alternately, you can download the ZIP file and extract the contents to get the code.

<a href="../../../../../assets/zip/reliable-delivery.zip">
    <img src="../../../../../assets/img/download-zip.png" width="200" alt="Download ZIP">
</a>

## Implementation

> If you want to skip the basics and move directly to the [Testing](#testing) section, you can download the project from
> git and skip the [Implementation](#implementation) instructions.

### Creating the project structure

Ballerina is a complete programming language that supports custom project structures.

To implement the scenario in this guide, you can use the following package structure:

```bash
    reliable-delivery
        └── guide
          ├── backend_service.bal
          ├── http_message_listener.bal
          └── message_forwarder.bal
```

- Create the above directories in your local machine and also create the empty .bal files.
- Then open a terminal, navigate to project directory and run the below command to initialize a Ballerina project.

```bash
   $ ballerina init
```

Now that you have created the project structure, the next step is to develop the service.

### Setting up backend service

The backend service exposes */testservice/test* endpoint as a *POST* resource, which accepts a Json message. It logs the incoming
message payload with headers and returns a static response.

**backend_service.bal**

```ballerina
import ballerina/log;
import ballerina/http;


// Export http listner port on 9095
listener http:Listener httpListener = new(9095);

// Healthcare Service, which allows users to channel doctors online
@http:ServiceConfig {
    basePath: "/testservice"
}
service healthcareService on httpListener {
    // Resource that allows users to make appointments
    @http:ResourceConfig {
        methods: ["POST"],
        consumes: ["application/json"],
        produces: ["application/json"],
        path: "/test"
    }
    resource function make_appointment(http:Caller caller, http:Request request) returns error? {
        http:Response response = new;
        var payload = request.getJsonPayload();

        if (payload is json) {
            string[] headerNames = request.getHeaderNames();
            string headers = "";
            foreach var header in headerNames {
                headers = headers + "[" + header + ": ";
                string headerVal = request.getHeader(untaint header);
                headers = headers + headerVal + "] , ";
            }
            log:printInfo("HIT!! - payload = " + payload.toString() + "Headers = " + headers);
            json responseMessage = {
                "Message": "This is Test Service"
            };
            response.setPayload(responseMessage);
            response.statusCode = http:ACCEPTED_202;
            check caller->respond(response);
        } else {
            response.statusCode = http:INTERNAL_SERVER_ERROR_500;
            response.setJsonPayload({
                "Message": "Invalid payload - Not a valid JSON payload"
            });
            check caller->respond(response);
            return;
        }
    }
}
```

### Developing message storing service

This service will accept any incoming HTTP message and store it in the previously defined *messageStore*. Note the following when configuring
message store client.

1. `MessageStoreConfiguration` is specified with required Message Broker detail when creating message store client.
2. A fail-over store client is also defined and set to the primary message store client (optional). If message could not be forwarded
   to the primary store, then this store is used to store the message.
3. `MessageStoreRetryConfig` specifies resiliency parameters in case if message is not stored successfully.

**http_message_listener.bal**

```ballerina
import ballerina/log;
import ballerina/http;
import wso2/messageStore;


messageStore:MessageStoreConfiguration secondaryMessageStoreConfig = {
    messageBroker: "ACTIVE_MQ",
    providerUrl: "tcp://localhost:61616",
    queueName: "myFailoverStore"
};
messageStore:Client secondaryStoreClient = checkpanic new messageStore:Client(secondaryMessageStoreConfig);

messageStore:MessageStoreRetryConfig storeRetryConfig = {
    count: 4,
    interval: 2000,
    backOffFactor: 1.5,
    maxWaitInterval: 15000
};

messageStore:MessageStoreConfiguration messageStoreConfig = {
    messageBroker: "ACTIVE_MQ",
    providerUrl: "tcp://localhost:61616",
    queueName: "myStore",
    retryConfig: storeRetryConfig,
    secondaryStore: secondaryStoreClient
};

messageStore:Client storeClient = checkpanic new messageStore:Client(messageStoreConfig);

// Export http listner port on 9091
listener http:Listener httpListener = new(9091);


// Service to store message and respond to client with 202
@http:ServiceConfig {
    basePath: "/healthcare"
}
service healthcareService on httpListener {
    // HTTP resource listening for messages
    @http:ResourceConfig {
        methods: ["POST"],
        consumes: ["application/json"],
        produces: ["application/json"],
        path: "/appointment"
    }
    resource function make_appointment(http:Caller caller, http:Request request) returns error? {
        http:Response response = new;
        var payload = request.getJsonPayload();
        var result = storeClient->store(request);
        if (result is error) {
            // Hanlde error occured during storing message
            response.statusCode = http:INTERNAL_SERVER_ERROR_500;
            response.setJsonPayload({
                "Message": "Error while storing the message on JMS broker"
            });
            check caller->respond(response);
            return;
        } else {
            // Respond with 202
            response.statusCode = http:ACCEPTED_202;
            check caller->respond(response);
            return;
        }
    }
}
```

If the message is successfully stored, HTTP caller will get *202-accepted* message, or else *500-internal server error*. In this case,
the caller is notified of successful message acceptance.

### Developing message forwarding service

When configuring **MessageForwardingProcessor**, there is a few things you need to consider.

1. Same `MessageStoreConfiguration` you used to specify `messageStore` to store message in the above service should be
   used in `ForwardingProcessorConfiguration`. Notice in this example we are using the same pointing to the queue `myStore`.
2. You can specify the speed for message polling or specify a cron. Here we configure it to poll a message
   every 2 seconds by the cron expression `0/2 * * * * ?`. You can leverage this to run the processor at a specified time
   of the day.
3. You can configure the message processor to retry forwarding the messages it polls from the broker. In this example, message will
   retry `5 times` with an interval of `3 seconds` between each retry. If backend responds with `500` or `400` status codes,
   processor will consider them as failed to forward messages.
4. Once all retries to forward the message to the backend are over, you can either stop the message processor or drop the message and
   continue. Optionally, if a DLC store (another messageStore) is configured, message will be forwarded to it, and processor
   will move to the next message. In this example, we have specified a DLC store.
5. In case of connection failure to the message broker, message processor will retry to regain the connection and initialize
   message consumer back. You also have the freedom to configure that retry. In here, once every `15 seconds`, it will try
   to connect to the broker.
6. Once created, you need to `start` the processor to start running the forwarding processor.

Also note that in the example, we have exited the service in any case where it failed to initialize message processor (i.e
connection establishment failed for the broker).

**message_forwarder.bal**

```ballerina
import ballerina/log;
import ballerina/http;
import wso2/messageStore;
import ballerina/runtime;


public function main(string... args) {

    messageStore:MessageStoreConfiguration myMessageStoreConfig = {
        messageBroker: "ACTIVE_MQ",
        providerUrl: "tcp://localhost:61616",
        queueName: "myStore",
        retryConfig: {
            count: -1,
            interval: 10,
            backOffFactor: 1.5,
            maxWaitInterval: 60
        }
    };

    // Create a DLC store
    messageStore:MessageStoreConfiguration dlcMessageStoreConfig = {
        messageBroker: "ACTIVE_MQ",
        providerUrl: "tcp://localhost:61616",
        queueName: "myDLCStore"
    };

    messageStore:Client dlcStoreClient = checkpanic new messageStore:Client(dlcMessageStoreConfig);

    messageStore:ForwardingProcessorConfiguration myProcessorConfig = {
        storeConfig: myMessageStoreConfig,
        HttpEndpointUrl: "http://127.0.0.1:9095/testservice/test",
        HttpOperation: http:HTTP_POST,
        pollTimeConfig: "0/2 * * * * ?",

        // Forwarding retry
        retryInterval: 3000,
        retryHttpStatusCodes: [http:INTERNAL_SERVER_ERROR_500, http:BAD_REQUEST_400],
        maxRedeliveryAttempts: 5,
        forwardingFailAction: messageStore:DLC_STORE,
        DLCStore: dlcStoreClient
    };

    // Create message processor
    var myMessageProcessor = new messageStore:MessageForwardingProcessor(myProcessorConfig, handleResponseFromBE);
    if (myMessageProcessor is error) {
        log:printError("Error while initializing Message Processor", err = myMessageProcessor);
        panic myMessageProcessor;
    } else {
        // Start the processor
        var processorStartResult = myMessageProcessor. start();
        if (processorStartResult is error) {
            panic processorStartResult;
        } else {
            // TODO:temp fix
            myMessageProcessor.keepRunning();
        }
    }
}

// Function to handle response
function handleResponseFromBE(http:Response resp) {
    var payload = resp.getJsonPayload();
    if (payload is json) {
        log:printInfo("Response received " + "Response status code= " + resp.statusCode + ": " + payload.toString());
    } else {
        log:printError("Error while getting response payload", err=payload);
    }
}
```

## Deployment

Once you are done with the development, you can deploy the services using any of the methods listed below.

### Deploying locally

To deploy locally, navigate to *reliable-delivery/guide*, and execute the following command.

```bash
$ ballerina build
```

This builds a Ballerina executable archive (.balx) of the services that you developed in the target folder.
You can run them using the following command.

```bash
$ ballerina run <Exec_Archive_File_Name>
```

## Testing

Follow the below steps to test out the reliable message delivery functionality using the services we developed above.

On a new terminal, navigate to `<AMQ_HOME>/bin`, and execute the following command to start the ActiveMQ server.

```bash
$ ./activemq start
```

### Happy path scenario

- Navigate to `reliable-delivery/guide`, and execute the following commands via separate terminals to start each service:

```bash
    $ ballerina run backend_service.bal
    $ ballerina run message-forwarder.bal
    $ ballerina run message-listener.bal
```

- Go to [ActiveMQ console](http://localhost:8161/admin/queues.jsp), and notice that there is one subscriber for queue `myStore`.

- Send a message using curl to the HTTP service exposed by `http_message_listener` service like below. File `input.json` can
  have any valid json message.

```bash
curl -v -X POST --data @input.json http://localhost:9091/healthcare/appointment --header "Content-Type:application/json" --header "Company:WSO2"
```

- Note that the curl request receives an HTTP response with 202 status code.

```bash
< HTTP/1.1 202 Accepted
< content-length: 0
< server: ballerina/0.991.0
< date: Tue, 2 Jul 2019 18:09:56 +0530
```

- Note that in the backend server log, incoming request is logged as below with custom headers included.

```bash
2019-07-02 18:09:56,374 INFO  [] - HIT!! - payload = {"Status":"SUCCESS"}Headers = [Accept: */*] , [Company: WSO2] , [connection: keep-alive] , [content-length: 27] , [Content-Type: application/json] , [host: 127.0.0.1:9095]
```

- Note that the message_forwarder logic to handle the response is executed.

```bash
2019-07-02 18:20:50,186 INFO  [wso2/messageStore] - Response received Response status code= 200: {"Message":"This is Test Service"}
```

- Note that the message is removed from the queue and that the message count is zero.

### Backend returns an error scenario

- Shutdown `message_forwarder.bal` and `backend_service.bal` services.

- Modify `backend_service.bal` service to always return `500` HTTP status code.

```bash
response.statusCode = 500;
```

- Start the service and `message_forwarder.bal`

- Send the same curl message above. You will see that the message processor is trying to deliver it 5 times, by observing the log at
  backend service.

```bash
2019-07-02 18:24:38,119 INFO  [] - HIT!! - payload = {"Status":"SUCCESS"}Headers = [Accept: */*] , [Company: WSO2] , [connection: keep-alive] , [content-length: 27] , [Content-Type: application/json] , [host: 127.0.0.1:9095] ,
2019-07-02 18:24:41,301 INFO  [] - HIT!! - payload = {"Status":"SUCCESS"}Headers = [Accept: */*] , [Company: WSO2] , [connection: keep-alive] , [content-length: 27] , [Content-Type: application/json] , [host: 127.0.0.1:9095] ,
2019-07-02 18:24:44,317 INFO  [] - HIT!! - payload = {"Status":"SUCCESS"}Headers = [Accept: */*] , [Company: WSO2] , [connection: keep-alive] , [content-length: 27] , [Content-Type: application/json] , [host: 127.0.0.1:9095] ,
2019-07-02 18:24:47,330 INFO  [] - HIT!! - payload = {"Status":"SUCCESS"}Headers = [Accept: */*] , [Company: WSO2] , [connection: keep-alive] , [content-length: 27] , [Content-Type: application/json] , [host: 127.0.0.1:9095] ,
2019-07-02 18:24:50,340 INFO  [] - HIT!! - payload = {"Status":"SUCCESS"}Headers = [Accept: */*] , [Company: WSO2] , [connection: keep-alive] , [content-length: 27] , [Content-Type: application/json] , [host: 127.0.0.1:9095] ,
2019-07-02 18:24:53,352 INFO  [] - HIT!! - payload = {"Status":"SUCCESS"}Headers = [Accept: */*] , [Company: WSO2] , [connection: keep-alive] , [content-length: 27] , [Content-Type: application/json] , [host: 127.0.0.1:9095] ,
```

- Then message processor will forward the failing message to DLC store specified. Navigate to [ActiveMQ console](http://localhost:8161/admin/queues.jsp)
  and observe that there is one message in the queue `myDLCStore`.

```bash
2019-07-02 18:24:53,356 WARN  [wso2/messageStore] - Maximum retires breached when forwarding message to HTTP endpoint http://127.0.0.1:9095/testservice/test. Forwarding message to DLC Store
```

### Backend not available scenario

- Shutdown `backend_service.bal` service.

- Send the curl message again.

- Note that the message processor is trying to forward the message and then give up.

- Message will be moved to store `myDLCStore` same as in above step.

```bash
2019-07-02 18:27:35,034 WARN  [wso2/messageStore] - Maximum retires breached when forwarding message to HTTP endpoint http://127.0.0.1:9095/testservice/test. Forwarding message to DLC Store
```

### Deactivate message processor on forwarding failure

- In `message_forwarder.bal` edit the `ForwardingProcessorConfiguration` config as `deactivateOnFail: true`.

- Send the curl message again.

- Note that the message processor is trying to forward the message and then give up.

- This time it will be stopped with following message:

```bash
2019-07-03 16:29:38,549 WARN  [wso2/messageStore] - Maximum retires breached when forwarding message to HTTP endpoint http://127.0.0.1:9095/testservice/test. Message forwarding is stopped for http://127.0.0.1:9095/testservice/test
2019-07-03 16:29:38,550 INFO  [wso2/messageStore] - Deactivating message processor on queue = myStore
```

Note that upon forwarding failure, the message forwarding processor will behave in the following priority order:

1. If {`deactivateOnFail: true`} is configured it will be deactivated and message polling will stop. Message will remain on the store.
2. If {`DLCStore: dlcStoreClient`} is configured message will be routed to that queue pointed by that client and will be removed from
   the original store.
3. If non of the above is specified, message processor will drop the message and pick the next message in the store to try.
