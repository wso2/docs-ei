---
title: Message broadcasting using topics
commitHash: a76e9f433c9db062427fd53e641e158cc9223de9
note: This is an auto-generated file do not edit this, You can edit content in "ballerina-integrator" repo
---

This guide is about broadcasting a message to several individual services in an asynchronous manner. In this example, the messages are published to a `Topic` in a message broker and there are services subscribed to that topic. Same copy of the message is received by each service and processed individually by each service. The message in fact is broadcasted to the services. 

The high level sections of this guide are as follows:

- [Message broadcasting using topics](#Message-broadcasting-using-topics)
  - [What you'll build](#What-youll-build)
  - [Prerequisites](#Prerequisites)
  - [Implementation](#Implementation)
    - [Setting up MySQL](#Setting-up-MySQL)
    - [Creating the project structure](#Creating-the-project-structure)
    - [Developing the service](#Developing-the-service)
  - [deployment](#deployment)
    - [Deploying locally](#Deploying-locally)
    - [Deploying on Docker](#Deploying-on-Docker)
  - [Testing](#Testing)
      - [Output](#Output)

## What you'll build
Let's consider a scenario where we expose a backend service via an API. Once users invoke this API we want to broadcast the response from the 
backend service to two other services. These services will consume a copy of the message and process those individually. 


Let's use `healthcare service` found [here]() (TODO: link). It processes the doctor appointments (in json format) and responds with with appointment details. This message is then forwarded to topic called `appointments` created in ActiveMQ broker. If message is forwarded without an issue, user will get successful message acceptance as the response. The service `appointment_processor_one` subscribed to the `appointment` topic, will get the appointment details, and log the message. The service `appointment_processor_two` subscribed to the `appointmentTopic` topic, will get the same message 
and it also will log the message. 

> Note in this example the two logging operations done by two services are completely independent and they are not aware of each other.  


The following diagram illustrates the scenario:

![alt text](resources/message_broadcasting_guide.svg)

## Prerequisites
- [Ballerina Distribution](https://ballerina.io/learn/getting-started/)
- A Text Editor or an IDE 
> **Tip**: For a better development experience, install one of the following Ballerina IDE plugins: [VSCode](https://marketplace.visualstudio.com/items?itemName=ballerina.ballerina), [IntelliJ IDEA](https://plugins.jetbrains.com/plugin/9520-ballerina)
- [Apache ActiveMQ](http://activemq.apache.org/getting-started.html))
  * After you install ActiveMQ, copy the .jar files from the `<AMQ_HOME>/lib` directory to the `<BALLERINA_HOME>/bre/lib` directory.
   * If you use ActiveMQ version 5.12.0, you only have to copy `activemq-client-5.12.0.jar`, `geronimo-j2ee-management_1.1_spec-1.0.1.jar`, and `hawtbuf-1.11.jar` from the `<AMQ_HOME>/lib` directory to the `<BALLERINA_HOME>/bre/lib` directory.

## Implementation

TODO: best practices

> If you want to skip the basics and move directly to the [Testing](#testing) section, you can download the project from git and skip the [Implementation](#implementation) instructions.


### Creating the project structure
Ballerina is a complete programming language that supports custom project structures. 

To implement the scenario in this guide, you can use the following package structure:

```bash
    message-broadcasting
        ├── guide
          ├── appointment_processor_one.bal
          ├── appointment_processor_two.bal
          └── http_message_receiver.bal
```
     
- Create the above directories in your local machine and also create the empty .bal files.
- Then open a terminal, navigate to project directory and run the Ballerina init command.

```bash
   $ ballerina init
```
Now that you have created the project structure, the next step is to develop the service.

### Developing the service

1. First you need to implement `http_message_receiver.bal` which will listen for HTTP messages over port 9091 and forward it to `healthcare` service. The response from the service is then sent to `appointmentTopic` topic of ActiveMQ broker. 
2. Then you need to implement `appointment_processor_one.bal` which will listen on the same ActiveMQ topic, receive JMS message, convert it back to a Json message, and log it to the log file.    
3. Finally, you need to implement `appointment_processor_two.bal` which will listen on the same ActiveMQ topic, receive JMS message, convert it back to a Json message, and log it to the log file. Logically, it is a copy of `appointment_processor_one.bal`. To differentiate, we will log the service name. 

Take a look at the code samples below to understand how to implement each service. 

**http_message_receiver.bal**
```ballerina
import ballerina/log;
import ballerina/http;
import ballerina/jms;
import ballerina/io;

// Init the HTTP client for appointment service
http:Client appointmentClient = new("http://localhost:9090");

// Initialize a JMS connection with the provider
// 'providerUrl' and 'initialContextFactory' vary based on the JMS provider you use
// 'Apache ActiveMQ' has been used as the message broker in this example
jms:Connection jmsConnection = new({
        initialContextFactory: "org.apache.activemq.jndi.ActiveMQInitialContextFactory",
        providerUrl: "tcp://localhost:61616"
    });

// Initialize a JMS session on top of the created connection
jms:Session jmsSession = new(jmsConnection, {
        acknowledgementMode: "AUTO_ACKNOWLEDGE"
    });

// Initialize a topic publisher using the created session
jms:TopicPublisher topicPublisher = new(jmsSession, topicPattern = "appointmentTopic");

// Export http listner port on 9091
listener http:Listener httpListener = new(9091);

// Healthcare Service, which allows users to channel doctors online
@http:ServiceConfig {
    basePath: "/healthcare"
}
service healthcareService on httpListener {
    // Resource that allows users to make appointments
    @http:ResourceConfig {
        methods: ["POST"],
        consumes: ["application/json"],
        produces: ["application/json"]
    }
    resource function make_appointment(http:Caller caller, http:Request request) returns error? {
        http:Response clientResponse = new;
        // Try parsing the JSON payload from the request
        var payload = request.getJsonPayload();
        if (payload is json) {
            json responseMessage;
            // Invoke HTTP service to make the appointment
            var response = appointmentClient->post("/grandoaks/categories/surgery/reserve ", untaint payload);
            if (response is http:Response) {
                var resPayload = response.getJsonPayload();
                if (resPayload is json) {
                    log:printInfo("Response is : " + resPayload.toString());
                    // Send the message to the Topic
                    var topicMessage = jmsSession.createTextMessage(resPayload.toString());
                    if (topicMessage is jms:Message) {
                        check topicPublisher->send(topicMessage);
                        // Construct a success message for the response
                        responseMessage = {                             "Message": "Your appointment is successfully placed"};
                        log:printInfo("New appointment added to the JMS topic; Patient: " + payload.patient.name.toString());
                    } else {
                        log:printError("Error occured while adding appointment to the JMS topic");
                        responseMessage = {                             "Message": "Internal error occured while storing the appointment"};
                        clientResponse.statusCode = http:INTERNAL_SERVER_ERROR_500;
                    }
                } else {
                    log:printError("Error parsing response from appointment service " + <string> resPayload.detail().message);
                    clientResponse.statusCode = http:INTERNAL_SERVER_ERROR_500;
                    responseMessage = {                         "Message": "Internal error occured while storing the appointment"};
                }
            } else {
                log:printError("Error when invoking appointment service " + <string> response.detail().message);
                clientResponse.statusCode = http:INTERNAL_SERVER_ERROR_500;
                responseMessage = {                     "Message": "Internal error occured when invoking appointment service"};
            }
            // Send response to the user
            clientResponse.setJsonPayload(responseMessage);
            check caller->respond(clientResponse);
        } else {
            clientResponse.statusCode = 400;
            clientResponse.setJsonPayload({
                "Message": "Invalid payload - Not a valid JSON payload"
            });
            check caller->respond(clientResponse);
            return;
        }

    }
}
```

**appointment_processor_one.bal**
```ballerina
import ballerina/jms;
import ballerina/log;
import ballerina/http;
import ballerina/io;

// JMS listener listening on topic
listener jms:TopicSubscriber subscriberEndpoint = new({
        initialContextFactory: "org.apache.activemq.jndi.ActiveMQInitialContextFactory",
        providerUrl: "tcp://localhost:61616",
        acknowledgementMode: "AUTO_ACKNOWLEDGE"   //remove message from broker as soon as it is received
    }, topicPattern = "appointmentTopic");


service jmsListener on subscriberEndpoint {
    // Invoked upon JMS message receive
    resource function onMessage(jms:TopicSubscriberCaller consumer,
    jms:Message message) {
        // Receive message as a text
        var messageText = message.getTextMessageContent();
        if (messageText is string) {
            io:StringReader sr = new(messageText, encoding = "UTF-8");
            json jsonMessage = checkpanic sr.readJson();
            // Write to database
            processMessage(jsonMessage);

        } else {
            log:printError("Error occurred while reading message " + messageText.reason());
        }
    }
}

function processMessage(json payload) {
    log:printInfo("service one received message: " + payload.toString());
}
```

**appointment_processor_two.bal**
```ballerina
import ballerina/jms;
import ballerina/log;
import ballerina/http;
import ballerina/io;

// JMS listener listening on topic
listener jms:TopicSubscriber subscriberEndpoint = new({
        initialContextFactory: "org.apache.activemq.jndi.ActiveMQInitialContextFactory",
        providerUrl: "tcp://localhost:61616",
        acknowledgementMode: "AUTO_ACKNOWLEDGE"   //remove message from broker as soon as it is received
    }, topicPattern = "appointmentTopic");


service jmsListener on subscriberEndpoint {
    // Invoked upon JMS message receive
    resource function onMessage(jms:TopicSubscriberCaller consumer,
    jms:Message message) {
        // Receive message as a text
        var messageText = message.getTextMessageContent();
        if (messageText is string) {
            io:StringReader sr = new(messageText, encoding = "UTF-8");
            json jsonMessage = checkpanic sr.readJson();
            // Write to database
            processMessage(jsonMessage);

        } else {
            log:printError("Error occurred while reading message " + messageText.reason());
        }
    }
}

function processMessage(json payload) {
    log:printInfo("service two received message: " + payload.toString());
}
```

## deployment

Once you are done with the development, you can deploy the services using any of the methods listed below. 

### Deploying locally

To deploy locally, navigate to asynchronous-messaging/guide, and execute the following command.

```bash
$ ballerina build
```
This builds a Ballerina executable archive (.balx) of the services that you developed in the target folder. 
You can run them by 

```bash
$ ballerina run <Exec_Archive_File_Name>
```

### Deploying on Docker

If necessary you can run the service that you developed above as a Docker container.The Ballerina language includes a Ballerina_Docker_Extension, which offers native support to run Ballerina programs on containers.

To run a service as a Docker container, add the corresponding Docker annotations to your service code.

Since ActiveMQ is a prerequisite in this guide, there are a few more steps you need to follow to run the service you developed in a Docker container. Please navigate to [ActiveMQ on Dockerhub](https://hub.docker.com/r/webcenter/activemq) and follow the instructions. 


## Testing
Follow the steps below to invoke the service.

- On a new terminal, navigate to `<AMQ_HOME>/bin`, and execute the following command to start the ActiveMQ server.
```bash
    $ ./activemq start
```

- Download `healthcare service` from [here]() (TODO: link) and start it:
```
$ ballerina run service.balx
```

- Navigate to `message-broadcasting/guide`, and execute the following commands via separate terminals to start each service:
```bash
    $ ballerina run http_message_receiver.bal
    $ ballerina run appointment_processor_one.bal
    $ ballerina run appointment_processor_two.bal
```

- Create a file called input.json with following json request to simulate placing doctor appointments:
``` json
    {
    "patient": {
        "name": "John Doe",
        "dob": "1940-03-19",
        "ssn": "234-23-525",
        "address": "California",
        "phone": "8770586755",
        "email": "johndoe@gmail.com"
    },
    "doctor": "thomas collins",
    "hospital": "grand oak community hospital",
    "appointmentDate": "2025-04-02"
    }
```
- Send the message using curl 
```bash
    curl -v -X POST --data @input.json http://localhost:9091/healthcare/make_appointment --header "Content-Type:application/json"
```

### Output
 - You will see the following log, which confirms that the `http_message_receiver` service has sent the request to the ActiveMQ queue.

    ```
    New appointment added to the JMS topic; Patient: John Doe
    ```

 - At the `appointment_processor_one` service you will see following log as message is received by it.  

    ```
    2019-07-06 10:24:15,411 INFO  [] - service one received message: {"appointmentNumber":1, "doctor":{"name":"thomas collins", "hospital":"grand oak community hospital", "category":"surgery", "availability":"9.00 a.m - 11.00 a.m", "fee":7000.0}, "patient":{"name":"John Doe", "dob":"1940-03-19", "ssn":"234-23-525", "address":"California", "phone":"8770586755", "email":"johndoe@gmail.com"}, "fee":7000.0, "confirmed":false, "appointmentDate":"2025-04-02"}
    ```
 - At the `appointment_processor_two` service you will see following log as message is received by it.  

    ```
    2019-07-06 10:24:15,411 INFO  [] - service two received message: {"appointmentNumber":1, "doctor":{"name":"thomas collins", "hospital":"grand oak community hospital", "category":"surgery", "availability":"9.00 a.m - 11.00 a.m", "fee":7000.0}, "patient":{"name":"John Doe", "dob":"1940-03-19", "ssn":"234-23-525", "address":"California", "phone":"8770586755", "email":"johndoe@gmail.com"}, "fee":7000.0, "confirmed":false, "appointmentDate":"2025-04-02"}
    ```

- Navigate to ActiveMQ console (http://localhost:8161/admin/topics.jsp). Number of enqueued and dequeued messages of 
  the topic `appointmentTopic` should be `1`.

- If you shutdown `appointment_processor_one` service and do the test again, still `appointment_processor_two` service will
  receive the message. There is no effect of `appointment_processor_one` for the message processing at `appointment_processor_two` service.