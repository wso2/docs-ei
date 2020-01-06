# Using the Kafka Inbound Endpoint

!!! Warning
    **Please note that the contents on this page are under review!**

## Example use case

This sample demonstrates how one way message bridging from Kafka to HTTP can be done using the inbound kafka endpoint.

### Synapse configuration

Following are the integration artifacts that we can used to implement this scenario. See the instructions on how to [build and run](#build-and-run) this example.

```xml tab='Inbound Endpoint'
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
 name="KAFKAListenerEP"
 sequence="TestIn"
 onError="fault"
 protocol="kafka"
 suspend="false">
 <parameters>
     <parameter name="interval">10</parameter>
     <parameter name="consumer.type">highlevel</parameter>
     <parameter name="content.type">application/xml</parameter>
     <parameter name="coordination">false</parameter>
     <parameter name="sequential">false</parameter>
     <parameter name="topics">test</parameter>
     <parameter name="zookeeper.connect">localhost:2181</parameter>
     <parameter name="group.id">test-1</parameter>
 </parameters>
</inboundEndpoint>
```

```xml tab='Sequence'
<sequence xmlns="http://ws.apache.org/ns/synapse" name="TestIn">
 <log level="full"/>
 <drop/>
</sequence>
```

### Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. Create a [mediation sequence](../../../../develop/creating-artifacts/creating-reusable-sequences) and [inbound endpoint](../../../../develop/creating-an-inbound-endpoint) with configurations given in the above example.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Set up the back-end service.

-   Apache Kafka inbound endpoint should be configured. The recommended version for the customized kafka inbound endpoint is `kafka_2.9.2-0.8.1.1`. See [Configuring Kafka](../../../../setup/feature_configs/configuring-kafka) for more information. 

-   Go to the [WSO2 Connector Store](https://store.wso2.com/store/assets/esbconnector/details/kafka) and click **Download Inbound Endpoint** to download the inbound JAR file. Add the downloaded JAR file to the <MI_HOME>/dropins directory.

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

You will see the following Message content in the Micro Integrator:

```bash
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsa="http://www.w3.org/2005/08/addressing"><soapenv:Body><m0:getQuote xmlns:m0="http://services.samples">
    <m0:request><m0:symbol>IBM</m0:symbol></m0:request></m0:getQuote></soapenv:Body></soapenv:Envelope>
```

The Kafka inbound gets the messages from the Kafka brokers and logs the messages in the Micro Integrator.

## High-level Kafka Configuration

Following are two high-level Kafka configurations that can be used to consume messages in two ways: Using **specific topics** or using a **topic filter**.

```xml tab='Using Specific Topics'
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                     name="KakfaListenerEP"
                     sequence="requestHandlerSeq"
                     onError="inFaulte"
                     protocol="kafka"
                     suspend="false">
       <parameters>
          <parameter name="interval">100</parameter> 
          <parameter name="coordination">true</parameter>
          <parameter name="sequential">true</parameter>
          <parameter name="zookeeper.connect">localhost:2181</parameter>
          <parameter name="consumer.type">highlevel</parameter>
          <parameter name="content.type">application/xml</parameter>
          <parameter name="topics">test,sampletest</parameter>
          <parameter name="group.id">test-group</parameter>
       </parameters>
</inboundEndpoint>
```

```xml tab='Using a Topic Filter'
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                     name="KakfaListenerEP"
                     sequence="requestHandlerSeq"
                     onError="inFaulte"
                     protocol="kafka"
                     suspend="false">
       <parameters>
          <parameter name="interval">100</parameter> 
          <parameter name="coordination">true</parameter>
          <parameter name="sequential">true</parameter>
          <parameter name="zookeeper.connect">localhost:2181</parameter>
          <parameter name="consumer.type">highlevel</parameter>
          <parameter name="content.type">application/xml</parameter>
          <parameter name="topic.filter">test</parameter>
          <parameter name="filter.from.whitelist">true</parameter>
          <parameter name="group.id">test-group</parameter>      
       </parameters>
</inboundEndpoint>
```

!!! Info
    In high-level kafka configurations, the follwing parameters are used instead of the `topics` paramater.
    <parameter name="topic.filter">test</parameter>
    <parameter name="filter.from.whitelist">true</parameter>


## Low-Level Kafka Inbound Endpoint Properties

Following is a sample low-level Kafka configuration that can be used to consume messages from a specific server in a specific partition, so that the messages are limited:

```xml
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                     name="KakfaListenerEP"
                     sequence="requestHandlerSeq"
                     onError="inFaulte"
                     protocol="kafka"
                     interval="1000"
                     suspend="false">
       <parameters>     
          <parameter name="zookeeper.connect">localhost:2181</parameter>
          <parameter name="group.id">test-group</parameter>  
          <parameter name="content.type">application/xml</parameter>
          <parameter name="consumer.type">simple</parameter>
          <parameter name="simple.max.messages.to.read">5</parameter>
          <parameter name="simple.topic">test</parameter>
          <parameter name="simple.brokers">localhost</parameter>
          <parameter name="simple.port">9092</parameter>
          <parameter name="simple.partition">1</parameter>
          <parameter name="interval">1000</parameter>
       </parameters>
</inboundEndpoint>
```
