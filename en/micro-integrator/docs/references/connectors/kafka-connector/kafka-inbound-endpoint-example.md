# Kafka Inbound Endpoint Example

The Kafka inbound endpoint of WSO2 EI acts as a message consumer. It creates a connection to ZooKeeper and requests messages for either a topic/s or topic filters.

## What you'll build
This sample demonstrates how one way message bridging from Kafka to HTTP can be done using the inbound Kafka endpoint.
See [Configuring kafka connector](kafka-connector-configuration.md) for more information.

The following diagram illustrates all the required functionality of the Kafka service that you are going to build. In this example, you only need to consider about the scenario of message consuming.

<img src="/assets/img/connectors/KafkaConnector.png" title="KafkaConnector" width="800" alt="KafkaConnector"/>

## Configure the connector in WSO2 Integration Studio

Follow these steps to set up the ESB Solution Project and the Connector Exporter Project.

{!references/connectors/importing-connector-to-integration-studio.md!}

1. Our project would look similar to the following (source view).

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <inboundEndpoint name="KAFKAListenerEP" sequence="kafka_process_seq" onError="fault" class="org.wso2.carbon.inbound.kafka.KafkaMessageConsumer" suspend="false" xmlns="http://ws.apache.org/ns/synapse">
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
   Sequence

   ```
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

2. Right-click on the Composite Application Project and click on **Export Project Artifacts and Run**. Select **Run on Micro Integrator**.

3. Micro Integrator will be started and the composite application will be deployed. You can further refer to the application deployed through the CLI tool. Make sure you first export the PATH as below.

    ```
    $ export PATH=/path/to/mi/cli/directory/bin:$PATH\
    ```

{!references/connectors/exporting-artifacts.md !}

## Deployment
Follow these steps to deploy the exported CApp in the Enterprise Integrator Runtime. 

{!references/connectors/deploy-capp.md!}

## Testing  
1. Run the following on the Kafka command line to create a topic named test with a single partition and only one replica:
   ```
   bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
   ```           
2. Run the following on the Kafka command line to send a message to the Kafka brokers. You can also use the WSO2 Kafka Producer connector to send the message to the Kafka brokers.
   ```
   bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
   ```   
3. Executing the above command will open up the console producer. Send the following message using the console:
   ```
   {"test":"wso2"}
   ```
   You can see the following Message content in the Micro Integrator:
   
   ```  
   [2020-02-19 12:39:59,331]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: , MessageID: d130fb8f-5d77-43f8-b6e0-85b98bf0f8c1, Direction: request, Payload: {"test":"wso2"}
   [2020-02-19 12:39:59,335]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - partitionNo = 0
   [2020-02-19 12:39:59,336]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - messageValue = {"test":"wso2"}
   [2020-02-19 12:39:59,336]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - offset = 6  
   ```
   The Kafka inbound endpoint gets the messages from the Kafka brokers and logs the messages in the Micro Integrator.
   
   ## What's next
   
   * You can deploy and run your project on [Docker](../../../setup/installation/run_in_docker.md) or [Kubernetes](../../../setup/installation/run_in_kubernetes.md).
   * To customize this example for your own scenario, see [kafka Connector Configuration](kafka-connector-config.md) documentation.