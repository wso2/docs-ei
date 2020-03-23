# Kafka Connector Example

Given below is a sample scenario that demonstrates how to send messages to a Kafka broker via Kafka topics.The publishMessages operation allows you to publish messages to the Kafka brokers via Kafka topics.

## What you'll build
Given below is a sample proxy service that illustrates how you can connect to a Kakfa broker with the `init` operation and then use the `publishMessages` operation to publish messages via the topic.

**publishMessages**
````xml
<kafkaTransport.publishMessages>
    <topic>topicName</topic>
    <partitionNo>partitionNo</partitionNo>
</kafkaTransport.publishMessages>
````
**Properties**

| Property        | Description |
| ------------- |-------------|
| topic    | The name of the topic. |
| partitionNo      | The partition number of the topic. |

If required, you can add [custom headers](https://cwiki.apache.org/confluence/display/KAFKA/A+Case+for+Kafka+Headers) to the records in the `publishMessage` operation:

````
<topic.Content-Type>Value</topic.Content-Type>
````
You can add the parameter as follows in the publishMessage operation:

````
<kafkaTransport.publishMessage configKey="kafka_init">
    <topic>topicName</topic>
    <partitionNo>partitionNo</partitionNo>
    <topicName.Content-Type>Value</topicName.Content-Type>
</kafkaTransport.publishMessage>

````
The following diagram illustrates all the required functionality of the Kafka service that you are going to build.

<img src="/assets/img/connectors/KafkaConnector.png" title="KafkaConnector" width="800" alt="KafkaConnector"/>

## Configure the connector in WSO2 Integration Studio

Follow these steps to set up the ESB Solution Project and the Connector Exporter Project.

{!references/connectors/importing-connector-to-integration-studio.md!}

1. Our project would look similar to the following (source view).

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <proxy name="KafkaTransport" startOnLoad="true" transports="https http" xmlns="http://ws.apache.org/ns/synapse">
        <target>
           <inSequence>
                <kafkaTransport.init>
                   <bootstrapServers>localhost:9092</bootstrapServers>
                   <keySerializerClass>org.apache.kafka.common.serialization.StringSerializer</keySerializerClass>
                   <valueSerializerClass>org.apache.kafka.common.serialization.StringSerializer</valueSerializerClass>
                   <maxPoolSize>100</maxPoolSize>
                </kafkaTransport.init>
                <kafkaTransport.publishMessages>
                    <topic>test</topic>
                </kafkaTransport.publishMessages>
            </inSequence>
            <outSequence/>
            <faultSequence/>
        </target>
    </proxy>
    ```
2. Right-click on the Composite Application Project and click on **Export Project Artifacts and Run**. Select **Run on Micro Integrator**.

3. Micro Integrator will be started and the composite application will be deployed. You can further refer to the application deployed through the CLI tool. Make sure you first export the PATH as below.

    ```
    $ export PATH=/path/to/mi/cli/directory/bin:$PATH\
    ```

{! /references/connectors/exporting-artifacts.md !}

## Deployment
Follow these steps to deploy the exported CApp in the Enterprise Integrator Runtime. 

{!references/connectors/deploy-capp.md!}
    
## Testing

1. Login in to the Micro Integrator CLI tool.

    ```
    ./mi remote login
    ```
2. Provide default credentials admin for both username and password.

3. In order to view the proxy services deployed, execute the following command.

    ```
    ./mi proxyservice show
    ```
4. Let’s create a topic named “test” with a single partition and only one replica.
   Navigate to the <KAFKA_HOME> and run following command.
   
   ```
   bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test     
   ```
5. Send a message to the Kafka broker using a CURL command or sample client.

   ```
   curl -v POST -d '{"name":"sample"}' "http://localhost:8290/services/KafkaTransport" -H "Content-Type:application/json"
   ```
6. Navigate to the <KAFKA_HOME> and run the following command to verify the messages:
   ```
   bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
   ```
7. See the following message content:
   ```
   {"name":"sample"}
   ```   
This demonstrates how the Kafka connector publishes messages to the Kafka brokers.
   
## What's next

* You can deploy and run your project on [Docker](../../../setup/installation/run_in_docker.md) or [Kubernetes](../../../setup/installation/run_in_kubernetes.md).
* To customize this example for your own scenario, see [kafka Connector Configuration](kafka-connector-config.md) documentation.