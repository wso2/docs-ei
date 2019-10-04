---
title: Notifying a Fire Alarm using Amazon SQS
commitHash: 9be2bde276ae340075a85d504719e5f8831bfe6b
note: This is an auto-generated file do not edit this, You can edit content in "ballerina-integrator" repo
---

This guide demonstrates how to use Amazon SQS Connector to notify alerts.

The high level sections of this guide are as follows:

- [What you'll build](#What-youll-build)
- [Prerequisites](#Prerequisites)
- [Implementation](#Implementation)
  - [Creating the project structure](#Creating-the-project-structure)
  - [Developing the scenario](#Developing-the-scenario)
- [Deployment](#Deployment)
  - [Deploying locally](#Deploying-locally)

## What you'll build

The following diagram illustrates the scenario:

![Message flow diagram image](../../../../../../assets/img/sqs-alert.png)

Let's consider a scenario where a fire alarm sends fire alerts to an Amazon SQS queue. As message duplication is not an issue, an Amazon SQS Standard Queue is used as the queue. A fire alarm listener is polling the messages in the SQS queue. When fire alerts are available, it will consume the messages from the queue and remove the messages from the queue. Consumed messages are showed in the console to the User.

Here, the fire alarm is sending fire alerts periodically from a Ballerina worker where listener polls in another worker. Both sent messages and received messages are printed in the console.

As there can be multiple alert messages available in the queue, the listener is configured to consume more than one message at a time.

## Prerequisites
- [Ballerina Distribution](https://ballerina.io/learn/getting-started/)
- A Text Editor or an IDE
  > **Tip**: For a better development experience, install one of the following Ballerina IDE plugins: [VSCode](https://marketplace.visualstudio.com/items?itemName=ballerina.ballerina), [IntelliJ IDEA](https://plugins.jetbrains.com/plugin/9520-ballerina)
- [Amazon SQS Account](https://aws.amazon.com/sqs/)


## Implementation

Take a look at the code samples below to understand how to implement the scenario.

The following code creates a new queue in Amazon SQS with the configuration provided in a file.

**create_notification_queue.bal**
```ballerina
import ballerina/config;
import wso2/amazonsqs;

function createNotificationQueue(string queueName) returns @tainted string|error {
    // Amazon SQS client configuration
    amazonsqs:Configuration configuration = {
        accessKey: config:getAsString("ACCESS_KEY_ID"),
        secretKey: config:getAsString("SECRET_ACCESS_KEY"),
        region: config:getAsString("REGION"),
        accountNumber: config:getAsString("ACCOUNT_NUMBER")
    };

    // Amazon SQS client
    amazonsqs:Client sqsClient = new(configuration);

    // Create SQS Standard Queue for notifications
    string|error queueURL = sqsClient->createQueue(queueName, {});
    return queueURL;
}
```

The following code generates fire alert notifications periodically and these are sent to the above created SQS queue.

**notify_fire.bal**
```ballerina
import ballerina/config;
import ballerina/log;
import ballerina/runtime;
import wso2/amazonsqs;

// periodicallySendFireNotifications, which periodically send fire alerts to Amazon SQS queue
function periodicallySendFireNotifications(string queueResourcePath) {

    // Amazon SQS client configuration
    amazonsqs:Configuration configuration = {
        accessKey: config:getAsString("ACCESS_KEY_ID"),
        secretKey: config:getAsString("SECRET_ACCESS_KEY"),
        region: config:getAsString("REGION"),
        accountNumber: config:getAsString("ACCOUNT_NUMBER")
    };

    // Amazon SQS client
    amazonsqs:Client sqsClient = new(configuration);

    while (true) {

        // Wait for 5 seconds
        runtime:sleep(5000);
        string queueUrl = "";

        // Send a fire notification to the queue
        amazonsqs:OutboundMessage|error response = sqsClient->sendMessage("There is a fire!",
            queueResourcePath, {});
        // When the response is valid
        if (response is amazonsqs:OutboundMessage) {
            log:printInfo("Sent an alert to the queue. MessageID: " + response.messageId);
        } else {
            log:printError("Error occurred while trying to send an alert to the SQS queue!");
        }
    }

}
```

The following code listens to the SQS queue and if there are any notifications, it would receive from the queue and delete the existing messages in the queue.

**listen_to_fire_alarm.bal**
```ballerina
import ballerina/config;
import ballerina/log;
import ballerina/runtime;
import wso2/amazonsqs;

// listenToFireAlarm, which listens to the Amazon SQS queue for fire notifications with polling
function listenToFireAlarm(string queueResourcePath) {

    // Amazon SQS client configuration
    amazonsqs:Configuration configuration = {
        accessKey: config:getAsString("ACCESS_KEY_ID"),
        secretKey: config:getAsString("SECRET_ACCESS_KEY"),
        region: config:getAsString("REGION"),
        accountNumber: config:getAsString("ACCOUNT_NUMBER")
    };

    // Amazon SQS client
    amazonsqs:Client sqsClient = new(configuration);
    string receivedReceiptHandler = "";

    // Receive a message from the queue
    map<string> attributes = {};
    // MaxNumberOfMessages, the maximum number of messages that can be received per request
    attributes["MaxNumberOfMessages"] = "10";
    // VisibilityTimeout, time allowed to delete after received, in seconds
    attributes["VisibilityTimeout"] = "2";
    // WaitTimeSeconds, waits for this time (in seconds) till messages are collected before received
    attributes["WaitTimeSeconds"] = "1";

    while(true) {

        // Wait for 5 seconds
        runtime:sleep(5000);
        // Receive messages from the queue
        amazonsqs:InboundMessage[]|error response = sqsClient->receiveMessage(queueResourcePath, attributes);

        // When the response is not an error
        if (response is amazonsqs:InboundMessage[]) {

            // When there are messages available in the queue
            if (response.length() > 0) {
                log:printInfo("************** Received fire alerts! ******************");
                int deleteMssageCount = response.length();
                log:printInfo("Going to delete " + deleteMssageCount.toString() + " messages from queue.");

                // Iterate on each message
                foreach var eachResponse in response {

                    // Keep receipt handle for deleting the message from the queue
                    receivedReceiptHandler = eachResponse.receiptHandle;

                    // Delete the received the messages from the queue
                    boolean|error deleteResponse = sqsClient->deleteMessage(queueResourcePath, receivedReceiptHandler);

                    // When the response from the delete operation is valid
                    if (deleteResponse is boolean && deleteResponse) {
                        if (deleteResponse) {
                            log:printInfo("Deleted the fire alert \"" + eachResponse.body + "\" from the queue.");
                        }
                    } else {
                        log:printError("Error occurred while deleting the message.");
                    }
                }
            } else {
                log:printInfo("Queue is empty. No messages to be deleted.");
            }

        } else {
            log:printError("Error occurred while receiving the message.");
        }
    }
}
```

In the following code, the `main` method would implement the workers related to creating a queue, sending a message to the queue, and consuming and receiving/deleting messages from the queue.

**main.bal**
```ballerina
import ballerina/log;
import wso2/amazonsqs;

// Executes the workers in the guide
public function main(string... args) {

    // queueCreator, creates a new queue
    worker queueCreator {
        log:printInfo("queueCreator started ....");
        string queueResourcePath = "";
        // Create the queue
        string|error queueURL = createNotificationQueue("fireNotifications");
        // When the queue creation operation is successful
        if (queueURL is string) {
            // Extract the SQS queue resource path from the URL
            queueResourcePath = amazonsqs:splitString(queueURL, "amazonaws.com", 1);
            log:printInfo("Queue Resource Path: " + queueResourcePath);
        } else {
            log:printError("Error occurred while creating the queue.");
        }

        // Send the resource path of the queue to the fire notifier
        queueResourcePath -> fireNotifier;
        // Send the resource path of the queue to the fire listener
        queueResourcePath -> fireListener;
    }

    // Fire notifier worker which publishes to the SQS queue
    worker fireNotifier {
        log:printInfo("fireNotifier started ....");
        // Get the resource path from the queue creator
        string queueResourcePath = <- queueCreator;
        // Starts to periodically send fire alerts
        periodicallySendFireNotifications(queueResourcePath);
    }

    // Fire listener which listens to the SQS queue
    worker fireListener {
        log:printInfo("fireListener started ....");
        // Get the resource path from the queue creator
        string queueResourcePath = <- queueCreator;
        // Starts to listen for the fire alerts via polling
        listenToFireAlarm(queueResourcePath);
    }

    // Starts from the queue creator worker
    wait queueCreator;

}
```

### Creating the project structure

1. Create a new project.

    ```bash
    $ ballerina new notifying-fire-alarm-using-sqs
    ```

2. Create a module.

    ```bash
    $ ballerina add guide
    ```

To implement the scenario in this guide, you can use the following package structure:

```
  notifying-fire-alarm-using-sqs
  ├── Ballerina.toml
  └── src
      └── guide
          ├── Module.md
          ├── create_notification_queue.bal
          ├── notify_fire.bal
          ├── listen_to_fire_alarm.bal
          └── main.bal

```

Now that you have created the project structure, the next step is to develop the scenario.

### Developing the scenario

1. Configure parameters in `create_notification_queue.bal`, which will create a new queue. In order to create a queue initialize the `amazonsqs:Client` with configuration parameters and invoke the `createQueue` method of it. `ACCESS_KEY_ID` and `SECRET_ACCESS_KEY` can be obtained from the Amazon account you have created. When a queue is created you can find the `ACCOUNT_NUMBER` of the SQS account.

2. Configure/develop `notify_fire.bal`, which will send periodic fire alerts to the created SQS queue. Instead of the `while` loop added, you can add some custom logic to trigger fire alarm. Create a client as described in step 1 and invoke `sendMessage` method to send alert message to the SQS queue.

3. Configure/develop `listen_to_fire_alarm.bal`, which will listen to the above created queue with polling. `sleep` method in the `while` loop can be called according to the polling interval. Then create the client as described in step 1 and invoke `receiveMessage` method. Depending on the `MaxNumberOfMessages` parameter set in the `attributes` array, maximum number of messages received per API invocation will be restricted. Each message can be accessed with `receiptHandle` value in the response. Once the message is read it can be deleted by invoking the `deleteMessage` method.

4. Configure/develop `main.bal`, which will implement the different workers for each of the above cases and run the system. There the workers can be replaced with the relevant code. `queueCreator` code should be called once to setup the queue. Code in the `fireNotifier` can be called from the fire alarm triggering side while `fireListener` should reside in the alarm polling/listening code.

## Deployment

Once you are done with the development, you can deploy the scenario using any of the methods listed below.

### Deploying locally

To deploy locally, navigate to `notifying-fire-alarm-using-sqs` directory, and execute the following command.

```bash
  $ ballerina build guide
```
This builds a JAR file (.jar) in the target folder. You can run this by using the `java -jar` command.

```bash
  $ java -jar target/bin/guide.jar
```

You see the SQS queue creation, sending fire alerts to the queue, consuming process of queues and subsequent deletion process on console.
