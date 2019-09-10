# Using Kafka Inbound Endpoints

In order to use the kafka inbound endpoint, you need to download and install [Apache Kafka](http://kafka.apache.org/downloads.html) . The recommended version is `         kafka_2.9.2-0.8.1.1        ` .
      To configure the kafka inbound endpoint, copy the following client libraries from the `<KAFKA_HOME>/libs        ` directory to the `<EI_HOME>/lib` directory.
      -   `          kafka_2.9.2-0.8.1.1.jar         `
      -   `           scala-library-2.9.2.jar          `
      -   `           zkclient-0.3.jar          `
      -   `           zookeeper-3.3.4.jar          `
      -   `           metrics-core-2.2.0.jar          `
      **Note**:    
      -   If you are using `          kafka_2.x.x-0.8.2.0         ` or later, you also need to add the `          kafka-clients-0.8.x.x.jar         ` file to the `          <EI_HOME>/lib         ` directory.
        -   If you are using a newer version of ZooKeeper, follow the steps below:
            1.  Create a directory named `             conf            ` inside the `             <EI_HOME>/repository            ` directory.
            2.  Create a directory named identity inside the `             <EI_HOME>/repository/conf            ` directory.
            3.  Add the [jaas.conf](attachments/119130492/119130493.conf) file to the `             <EI_HOME>/repository/conf/identity            ` directory. This is required because Kerberos authentication is enforced on newer versions of ZooKeeper.
            
## High-level Kafka Configuration

Following are two high-level Kafka configurations that can be used to consume messages in two ways: Using **specific topics** or using a **topic filter**.

``` java tab='Using Specific Topics'
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

``` java tab='Using a Topic Filter'
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

```
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

##Inbound Endpoint Kafka Protocol Sample

!!! warning

Note that WSO2 EI is shipped with the following changes to what is
mentioned in this documentation :

-   `           <PRODUCT_HOME>/          `
    `           repository/samples/          ` directory that includes
    all Integration profile samples is changed to
    `           <EI_HOME>/          `
    `           samples/service-bus/          ` .
    `                     `
-   `           <PRODUCT_HOME>/          `
    `           repository/samples/resources/          ` directory that
    includes all artifacts related to the Integration profile samples is
    changed to `           <EI_HOME>/          `
    `           samples/service-bus/resources/          ` .


-   [Introduction](#Sample904:InboundEndpointKafkaProtocolSample-Introduction)
-   [Prerequisites](#Sample904:InboundEndpointKafkaProtocolSample-Prerequisites)
-   [Building the
    sample](#Sample904:InboundEndpointKafkaProtocolSample-Buildingthesample)
-   [Executing the
    sample](#Sample904:InboundEndpointKafkaProtocolSample-Executingthesample)
-   [Analyzing the
    output](#Sample904:InboundEndpointKafkaProtocolSample-Analyzingtheoutput)

### Introduction

This sample demonstrates how one way message bridging from Kafka to HTTP
can be done using the inbound kafka endpoint.

### Prerequisites

-   Download and install [Apache
    Kafka](http://kafka.apache.org/downloads.html) . For more
    information, see [Apache Kafka
    documentation](http://kafka.apache.org/documentation.html) .
-   Copy the following client libraries from the
    `           <KAFKA_HOME>/lib          ` directory to the
    `           <EI_HOME>/lib          ` directory.

    -   `            kafka_2.9.2-0.8.1.1.jar           `
    -   `            scala-library-2.9.2.jar           `
    -   `            zkclient-0.3.jar           `
    -   `            zookeeper-3.3.4.jar           `
    -   `            metrics-core-2.2.0.jar           `

        !!! info
    
        Note
    
        -   If you are using `            kafka_2.x.x-0.8.2.0           ` or
            later, you also need to add the
            `            kafka-clients-0.8.x.x.jar           ` file to the
            `            <EI_HOME>/lib           ` directory.
        -   If you are using a newer version of ZooKeeper, follow the steps
            below:
    
            1.  Create a directory named `               conf              `
                inside the
                `               <EI_HOME>/repository              `
                directory.
            2.  Create a directory named identity inside the
                `               <EI_HOME>/repository/conf              `
                directory.
            3.  Add the
                [jaas.conf](https://docs.wso2.com/download/attachments/119130492/jaas.conf?version=1&modificationDate=1558091429000&api=v2)
                file to the
                `               <EI_HOME>/repository/conf/identity              `
                directory. This is required because Kerberos authentication
                is enforced on newer versions of ZooKeeper.
    

-   Navigate to `           <KAFKA_HOME>          ` and run the
    following command to start the ZooKeeper server:

    ``` java
        bin/zookeeper-server-start.sh config/zookeeper.properties
    ```

    You will see the following log:

    ![](attachments/119129714/119129715.png){width="800"}

-   Run the following command to start the Kafka server

    ``` java
            bin/kafka-server-start.sh config/server.properties
    ```

    You will see the following log:

    ![](attachments/119129714/119129718.png){width="800"}

### Building the sample

The XML configuration for this sample is as follows:

``` html/xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
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
     
    <sequence xmlns="http://ws.apache.org/ns/synapse" name="TestIn">
     <log level="full"/>
     <drop/>
    </sequence>
     
    </definitions>
```

This configuration file `         synapse_sample_904.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

-   Start the ESB with the sample 904 configuration. For instructions on
    starting a sample ESB configuration, see [Starting the ESB with a
    sample
    configuration](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Startingasample)
    .  
    The operation log keeps running until the server starts, which
    usually takes several seconds. Wait until the server has fully
    booted up and displays a message similar to " *WSO2 Carbon started
    in n seconds.* "

### Executing the sample

-   Run the following on the Kafka command line to create a topic named
    `           test          ` with a single partition and only one
    replica:

    ``` java
            bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
    ```

-   Run the following on the Kafka command line to send a message to the
    Kafka brokers. You can also use the WSO2 ESB Kafka producer
    connector to send the message to the Kafka brokers.

    ``` java
            bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
    ```

    ![](attachments/119129714/119129717.png){width="800"}

### Analyzing the output

  
`         You will see the following Message        ` content:

``` html/xml
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsa="http://www.w3.org/2005/08/addressing"><soapenv:Body><m0:getQuote xmlns:m0="http://services.samples">
    <m0:request><m0:symbol>IBM</m0:symbol></m0:request></m0:getQuote></soapenv:Body></soapenv:Envelope>
```

The Kafka inbound gets the messages from the Kafka brokers and logs the
messages in the ESB
