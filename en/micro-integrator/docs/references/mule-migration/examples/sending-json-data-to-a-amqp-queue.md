---
title: Sending JSON data to a AMQP queue
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example shows you how to use the AMQP connector to send JSON data to [RabitMQ](https://www.rabbitmq.com/#features), an open source message broker that implements AMQP.

## Assumption

This document describes the details of the example within the context of WSO2 Integration Studio, editor to develop WSO2 EI artifacts. This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

Further, this example assumes that you have [RabbitMQ](https://www.rabbitmq.com/download.html) enabled with [Management Plugin](http://www.thegeekstuff.com/2013/10/enable-rabbitmq-management-plugin/) installed on your machine. You can use [docker installation](https://registry.hub.docker.com/_/rabbitmq/) instead.

```
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```

## Example Use Case

In this example a message containing sample sales data in JSON is received through an HTTP API. This message is then sent to RabbitMQ using the RabbitMQ transport. Once this message reaches the queue, it can be viewed through he RabbitMQ web console.

<img width="80%" src="../../../assets/img/migration-examples/sending-json-data-to-a-amqp-queue-use-case.png">

## Set Up and Run the Example

1. After making sure that RabbitMQ is running, log in to the RabbitMQ web **admin console** at
   ```
    http://localhost:15672
   ```

2. Go to the **Exchanges** tab and click on **Add a new Exchange** to create an exchange called ***sales_exchange***. You may leave the other feilds as they are and then click on **Add exchange**.

3. Now go to the **Queues** tab and click on Add a new queue to create a queue called ***sales_queue***. You may leave the other fields as they are and then click on **Add queue**.

4. Click on **sales_exchange** under the **Exchanges** tab and then go to the section titled **Add binding from this exchange**. In the **To queue** field type in ***sales_queue*** and then click on **Bind**.

5. Download WSO2 Integrator Studio binary distribution (platform independent) and extract it to a known location. 

6. Navigate to <Product_HOME>/conf folder and open `deployment.toml` file. Edit it to enable `RabbitMQ Transport` and configure it. This configuration is globally applied. 
   ```
    [transport.rabbitmq]
    sender_enable = true

    [[transport.rabbitmq.sender]]
    name = "AMQPConnectionFactory"
    parameter.hostname = "localhost"
    parameter.port = 5672
    parameter.username = "guest"
    parameter.password = "guest"
   ```

7. Open the Example project in [Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). Note `RabbitMQEP` has connection details and other    information. Whatever information set at `deployment.toml` configuration can be overridden at the endpoint. 
    ```
    rabbitmq:/sales_queue?rabbitmq.server.host.name=localhost&rabbitmq.server.port=5672&rabbitmq.queue.name=sales_queue&rabbitmq.queue.route.key=sales_queue&rabbitmq.exchange.name=sales_exchange
    ```
    <img width="70%" src="../../../assets/img/migration-examples/sending-json-data-to-a-amqp-queue.png">

9. Right click `AMQPIntegrationProjectCompositeApplication` in **project explorer**  and choose **Export Composite Application Project**. Choose all the artifacts in the wizard and export CApp to <Product_HOME>/repository/deployment/server/carbonapps directory. 

10. Start the WSO2 EI server by executing  micro-integrator.sh (or micro-integrator.bat if Windows) in <Product_HOME>/bin folder. 

11. Make a HTTP POST request using Postman, curl or the Embedded HTTP4e Client to send the following JSON data:

    API Url:
    ```
    http://localhost:8290/amqp
    ```
    Data: 
    ```
    { "ITEM_ID": 1, "ITEM_NAME": "Shirt", "QTY": 1, "PRICE": 20 }
    ```

12. You will receive an `HTTP 202` response. Now, navigate back to the RabbbitMQ web admin console. You should notice an increase in the number of messages in ***sales_queue***. You may also **view  the message** by clicking on sales_queue in the **Get messages** option.

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/sending-json-data-to-a-amqp-queue.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

## Go Further

* Read more about [RabbitMQ Transport](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/transport-parameters/rabbitmq-transport-parameters/)
* How to receive AMQP messages [RabbitMQ inbound endpoint](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/inbound-endpoints/event-based-inbound-endpoints/rabbitmq-inbound-endpoint-properties/) 
