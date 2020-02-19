# Using the Kafka Inbound Endpoint

## Example use case

This sample demonstrates how one way message bridging from Kafka to HTTP can be done using the inbound kafka endpoint.

### Synapse configuration

Following are the integration artifacts that we can used to implement this scenario. See the instructions on how to [build and run](#build-and-run) this example.

```xml tab='Inbound Endpoint'
<?xml version="1.0" encoding="UTF-8"?>
<inboundEndpoint name="KAFKAListenerEP" sequence="kafka_process_seq" onError="fault" class="org.wso2.carbon.inbound.kafka.KafkaMessageConsumer" suspend="false" 
    xmlns="http://ws.apache.org/ns/synapse">
    <parameters>
        <parameter name="sequential">true</parameter>
        <parameter name="interval">10</parameter>
        <parameter name="coordination">true</parameter>
        <parameter name="inbound.behavior">polling</parameter>
        <parameter name="value.deserializer">org.apache.kafka.common.serialization.StringDeserializer</parameter>
        <parameter name="topic.names">test</parameter>
        <parameter name="poll.timeout">100</parameter>
        <parameter name="bootstrap.servers">localhost:9092</parameter>
        <parameter name="group.id">hello</parameter>
        <parameter name="contentType">application/json</parameter>
        <parameter name="key.deserializer">org.apache.kafka.common.serialization.StringDeserializer</parameter>
    </parameters>
</inboundEndpoint>
```

```xml tab='Sequence'
<?xml version="1.0" encoding="ISO-8859-1"?>
<sequence xmlns="http://ws.apache.org/ns/synapse" name="kafka_process_seq">
   <log level="full"/>
   <log level="custom">
      <property xmlns:ns="http://org.apache.synapse/xsd" name="partitionNo" expression="get-property('partitionNo')"/>
   </log>
   <log level="custom">
      <property xmlns:ns="http://org.apache.synapse/xsd" name="messageValue" expression="get-property('messageValue')"/>
   </log>
   <log level="custom">
      <property xmlns:ns="http://org.apache.synapse/xsd" name="offset" expression="get-property('offset')"/>
   </log>
</sequence>
```

### Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. Create a [mediation sequence](../../../../develop/creating-artifacts/creating-reusable-sequences) and [inbound endpoint](../../../../develop/creating-an-inbound-endpoint) with configurations given in the above example.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Set up the back-end service.

-   Apache Kafka inbound endpoint should be configured. The recommended version for the customized kafka inbound endpoint is `kafka_2.12-2.2.1`. See [Configuring Kafka](../../../../setup/feature_configs/configuring-kafka) for more information. 

-   Go to the [WSO2 Connector Store](https://store.wso2.com/store/assets/esbconnector/details/b15e9612-5144-4c97-a3f0-179ea583be88) and click **Download Inbound Endpoint** to download the inbound JAR file. Add the downloaded JAR file to the <MI_HOME>/dropins directory.

Run the following commands in the {KAFKA_HOME} directory to invoke the service.
    
-   Run the following on the Kafka command line to create a topic named `test` with a single partition and only one
    replica:

    ```bash
    bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
    ```

-   Run the following on the Kafka command line to send a message to the Kafka brokers. You can also use the WSO2 ESB Kafka producer connector to send the message to the Kafka brokers.

    ```bash
    bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
    ```
    
    Executing the above command will open up the console producer, send the following message using it
    
```json
{"test":"wso2"}
```

You can see the following Message content in the Micro Integrator:

```bash
[2020-02-19 12:39:59,331]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: , MessageID: d130fb8f-5d77-43f8-b6e0-85b98bf0f8c1, Direction: request, Payload: {"test":"wso2"}
[2020-02-19 12:39:59,335]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - partitionNo = 0
[2020-02-19 12:39:59,336]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - messageValue = {"test":"wso2"}
[2020-02-19 12:39:59,336]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - offset = 6
```

The Kafka inbound gets the messages from the Kafka brokers and logs the messages in the Micro Integrator.

## High-level Kafka Configuration

Following are two high-level Kafka configurations that can be used to consume messages in two ways: Using **specific 
topics** or using a **topic patterns**.

```xml tab='Using Specific Topics'
<?xml version="1.0" encoding="UTF-8"?>
<inboundEndpoint name="KAFKAListenerEP" sequence="kafka_process_seq" onError="fault" class="org.wso2.carbon.inbound.kafka.KafkaMessageConsumer" suspend="false" 
    xmlns="http://ws.apache.org/ns/synapse">
    <parameters>
        <parameter name="sequential">true</parameter>
        <parameter name="interval">10</parameter>
        <parameter name="coordination">true</parameter>
        <parameter name="consumer.type">highlevel</parameter>
        <parameter name="inbound.behavior">polling</parameter>
        <parameter name="value.deserializer">org.apache.kafka.common.serialization.StringDeserializer</parameter>
        <parameter name="topic.names">test,sampletest</parameter>
        <parameter name="poll.timeout">100</parameter>
        <parameter name="bootstrap.servers">localhost:9092</parameter>
        <parameter name="group.id">hello</parameter>
        <parameter name="contentType">application/json</parameter>
        <parameter name="key.deserializer">org.apache.kafka.common.serialization.StringDeserializer</parameter>
    </parameters>
</inboundEndpoint>
```

```xml tab='Using a Topic Pattern'
<?xml version="1.0" encoding="UTF-8"?>
<inboundEndpoint name="KAFKAListenerEP" sequence="kafka_process_seq" onError="fault" class="org.wso2.carbon.inbound.kafka.KafkaMessageConsumer" suspend="false" 
    xmlns="http://ws.apache.org/ns/synapse">
    <parameters>
        <parameter name="sequential">true</parameter>
        <parameter name="interval">10</parameter>
        <parameter name="coordination">true</parameter>
        <parameter name="consumer.type">highlevel</parameter>
        <parameter name="inbound.behavior">polling</parameter>
        <parameter name="value.deserializer">org.apache.kafka.common.serialization.StringDeserializer</parameter>
        <parameter name="topic.pattern">.*test</parameter>
        <parameter name="poll.timeout">100</parameter>
        <parameter name="bootstrap.servers">localhost:9092</parameter>
        <parameter name="group.id">hello</parameter>
        <parameter name="contentType">application/json</parameter>
        <parameter name="key.deserializer">org.apache.kafka.common.serialization.StringDeserializer</parameter>
    </parameters>
</inboundEndpoint>
```

You can see the following Message content in the Micro Integrator:


```bash
[2020-02-19 19:30:48,161]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: , MessageID: 1224bef2-73fb-4953-96a0-c0f3c92c3338, Direction: request, Payload: {"test":"high-level"}
[2020-02-19 19:30:48,164]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - partitionNo = 0
[2020-02-19 19:30:48,165]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - messageValue = {"test":"high-level"}
[2020-02-19 19:30:48,165]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - offset = 10
```

```bash
[2020-02-19 19:33:03,401]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: , MessageID: a33f2a58-51c6-4bef-96f7-52e6ff5f3ec1, Direction: request, Payload: {"sampletest":"high-level"}
[2020-02-19 19:33:03,402]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - partitionNo = 0
[2020-02-19 19:33:03,403]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - messageValue = {"sampletest":"high-level"}
[2020-02-19 19:33:03,404]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - offset = 1
```

## Low-Level Kafka Inbound Endpoint Properties

Following is a sample low-level Kafka configuration that can be used to consume messages from a specific server in a specific partition, so that the messages are limited

```xml
<?xml version="1.0" encoding="UTF-8"?>
<inboundEndpoint name="KAFKAListenerEP" sequence="kafka_process_seq" onError="fault" class="org.wso2.carbon.inbound.kafka.KafkaMessageConsumer" suspend="false" 
    xmlns="http://ws.apache.org/ns/synapse">
    <parameters>
        <parameter name="sequential">true</parameter>
        <parameter name="interval">10</parameter>
        <parameter name="coordination">true</parameter>
        <parameter name="consumer.type">simple</parameter>
        <parameter name="inbound.behavior">polling</parameter>
        <parameter name="bootstrap.servers">localhost:9092</parameter>
        <parameter name="value.deserializer">org.apache.kafka.common.serialization.StringDeserializer</parameter>
        <parameter name="poll.timeout">100</parameter>
        <parameter name="contentType">application/json</parameter>
        <parameter name="key.deserializer">org.apache.kafka.common.serialization.StringDeserializer</parameter>
        <parameter name="zookeeper.connect">localhost:2181</parameter>
        <parameter name="group.id">test-group</parameter>
        <parameter name="consumer.type">simple</parameter>
        <parameter name="simple.max.messages.to.read">5</parameter>
        <parameter name="topic.names">test</parameter>
        <parameter name="simple.partition">1</parameter>
    </parameters>
</inboundEndpoint>
```

```bash
[2020-02-19 19:42:35,052]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: , MessageID: 6a8b1add-5e16-43df-afc9-04062dd4e9b1, Direction: request, Payload: {"test":"simple"}
[2020-02-19 19:42:35,054]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - partitionNo = 0
[2020-02-19 19:42:35,056]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - messageValue = {"test":"simple"}
[2020-02-19 19:42:35,058]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - offset = 14
```