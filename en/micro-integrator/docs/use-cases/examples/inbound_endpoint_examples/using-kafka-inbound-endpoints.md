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