# Inbound Endpoint RabbitMQ Protocol Sample

### Introduction

This sample demonstrates how one way message bridging from RabbitMQ to
HTTP can be done using the inbound RabbitMQ endpoint.

### Prerequisites

-   Download and install
    [RabbitMQ](https://www.rabbitmq.com/download.html) . For more
    information, see [RabbitMQ
    documentation](https://www.rabbitmq.com/documentation.html) .

### Building the sample

The XML configuration for this sample is as follows:

```
    <?xml version="1.0" encoding="UTF-8"?>
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <taskManager provider="org.wso2.carbon.mediation.ntask.NTaskTaskManager"/>
        <sequence name="TestIn">
            <log level="full"/>
            <drop/>
        </sequence>  
        <inboundEndpoint name="test" onError="fault" protocol="rabbitmq"
            sequence="TestIn" suspend="false">
            <parameters>
                <parameter name="sequential">true</parameter>
                <parameter name="coordination">true</parameter>
                <parameter name="rabbitmq.server.host.name">localhost</parameter>
                <parameter name="rabbitmq.server.port">5672</parameter>
                <parameter name="rabbitmq.connection.factory">AMQPConnectionFactory</parameter>
                <parameter name="rabbitmq.server.user.name">guest</parameter>
                <parameter name="rabbitmq.server.password">guest</parameter>
                <parameter name="rabbitmq.queue.name">queue</parameter>
                <parameter name="rabbitmq.exchange.name">exchange</parameter>
            </parameters>
        </inboundEndpoint>
    </definitions>
```

This configuration file `         synapse_sample_907.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

-   Start the ESB with the sample 907 configuration. For instructions on
    starting a sample ESB configuration, see [Starting the ESB with a
    sample
    configuration](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Startingasample)
    .  
    The operation log keeps running until the server starts, which
    usually takes several seconds. Wait until the server has fully
    booted up and displays a message similar to " *WSO2 Carbon started
    in n seconds.* "

### Executing the sample

-   Use the following [Java
    client](https://mvnrepository.com/artifact/com.rabbitmq/amqp-client)
    to publish a request to the RabbitMQ broker.

    ``` java
            ConnectionFactory factory = new ConnectionFactory();
            factory.setHost("localhost");
            factory.setUsername("guest");
            factory.setPassword("guest");
            factory.setPort(5672);
            Channel channel = null;
            Connection connection = factory.newConnection();
            channel = connection.createChannel();
            channel.queueDeclare("queue", false, false, false, null);
            channel.exchangeDeclare("exchange", "direct", true);
            channel.queueBind("queue", "exchange", "route");
        
            // The message to be sent
            String message = "<m:placeOrder xmlns:m=\"http://services.samples\">" +
                            "<m:order>" +
                            "<m:price>100</m:price>" +
                            "<m:quantity>20</m:quantity>" +
                            "<m:symbol>RMQ</m:symbol>" +
                            "</m:order>" +
                            "</m:placeOrder>";
        
            // Populate the AMQP message properties
            AMQP.BasicProperties.Builder builder = new AMQP.BasicProperties().builder();
            builder.contentType("application/xml");
        
            // Publish the message to exchange
            channel.basicPublish("exchange", "queue", builder.build(), message.getBytes());
    ```

### Analyzing the output

You will see the following Message content:

``` 
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"><soapenv:Body><m:placeOrder xmlns:m="http://services.samples"><m:order><m:price>100</m:price><m:quantity>20</m:quantity><m:symbol>RMQ</m:symbol></m:order></m:placeOrder></soapenv:Body></soapenv:Envelope>
```

The RabbitMQ inbound endpoint gets the messages from the RabbitMQ broker
and logs the messages in the ESB.