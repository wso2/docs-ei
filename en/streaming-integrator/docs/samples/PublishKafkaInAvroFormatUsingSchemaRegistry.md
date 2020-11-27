# Publishing Avro Events via Kafka

## Purpose:

This application demonstrates how to configure WSO2 Streaming Integrator Tooling to send sweet production events via the Kafka transport in Avro format using Confluent Schema Registry.

## Prerequisites:

1. To install Kafka, follow the steps below:

	1. In Streaming Integrator Tooling, click **Tools**, and then click **Extension Installer**.
	
	2. In the **Extension Installer** dialog box that opens, search for `Kafka`. Then click **Install** in the row that appears. A message appears to confirm whether you want to proceed to install the extension. Click **Install**.
	
	3. Restart Streaming Integrator Tooling.

2. Download confluent-5.2.1 from the [Confluent website](https://www.confluent.io/download/). Then unzip the file you downloaded.

    !!! tip
        Download the product Confluent PLatform. For this sample, the deployment type selected was **Manual**.

3. Save the sample `PublishKafkaInAvroFormatUsingSchemaRegistry` Siddhi application.

    If there is no syntax error, the following message is logged in the terminal.
    
    ```
    * -Siddhi App PublishKafkaInAvroFormatUsingSchemaRegistry successfully deployed.
    ```

## Executing the Sample:

To execute the sample, follow the steps below:

1. First, start a zoo keeper node. To do this, navigate to the `<KAFKA_HOME>` directory and issue the following command.

    `sh bin/zookeeper-server-start.sh config/zookeeper.properties`

2. Next, start a Kafka server node. To do this, issue the following command from the same directory.

    `sh bin/kafka-server-start.sh config/server.properties`

3. Start the schema registry node by navigating to the `<CONFLUENT_HOME>` directory and issuing the following command:

    `sh bin/schema-registry-start ./etc/schema-registry/schema-registry.properties`
    
    This starts the Confluent client in `localhost:8081` port.

4. Post the avro schema to the schema registry by issuing the following CURL command.

    ```
    curl -X POST -H "Content-Type: application/json" --data '{ "schema": "{ \"type\": \"record\", \"name\": \"sweetProduction\",\"namespace\": \"sweetProduction\", \"fields\":[{ \"name\": \"name\", \"type\": \"string\" },{ \"name\": \"amount\", \"type\": \"double\" }]}"}' http://localhost:8081/subjects/sweet-production/versions
    ```
   
   The sample Siddhi application specifies `http://localhost:8081/subjects/sweet-production/versions` as the URI of the schema registry. The above CURL command defines the Avro schema and posts it to this schema registry so that the schema is applied to the output events generated in the `LowProductionAlertStream` when they are published to the `kafka_result_topic` kafka topic.
   
   For more information about how to configure an Avro mapper, see [Siddhi Documentation - Avro Sink Mapper](https://siddhi-io.github.io/siddhi-map-avro/api/latest/#avro-sink-mapper)

5. Navigate to the `<SI_TOOLING_HOME>/samples/sample-clients/kafka-avro-consumer` directory and run the `ant` command without arguments.

6. Start the `PublishKafkaInAvroFormatUsingSchemaRegistry` Siddhi application you saved by opening it in Streaming Integrator Tooling and clicking the **Start** button in the toolbar.
    
    If the Siddhi application starts successfully, the following messages are logged in the terminal:
    
    - `PublishKafkaInAvroFormatUsingSchemaRegistry.siddhi - Started Successfully!`
      
    - `Kafka version : 2.2.0`
    
    - `Kafka commitId : 05fcfde8f69b0349`
    
    - `Kafka producer created.`

## Testing the Sample:

To test this sample, send events following one or more of the methods given below:

**Option 1 - Send events to the kafka sink via the Event Simulator:**

1. In Streaming Integrator Studio, open the Event Simulator by clicking on the **Event Simulator** icon in the left panel or pressing Ctrl+Shift+I.

2. In the **Single Simulation** tab of the panel, specify the values as follows:

    | **Field**           | **Value**                                     |
    |---------------------|-----------------------------------------------|
    | **Siddhi App Name** | `PublishKafkaInAvroFormatUsingSchemaRegistry` |
    | **Stream Name**     | `SweetProductionStream`                       |

3. Once you select the stream, the **name** and **amount** fields appear. Enter `chocolate cake` as the name and `50.50` as the value. Then click **Send** to send the event.

4. Simulate a few more events for the `SweetProductionStream` stream by repeating the above steps.

**Option 2 - Publish events with Curl to the simulator HTTP endpoint:**

1. Open a new terminal and issue the following command:

    ```
    curl -X POST -d '{"streamName": "SweetProductionStream", "siddhiAppName": "PublishKafkaInAvroFormatUsingSchemaRegistry","data": ["chocolate cake", 50.50]}' http://localhost:9390/simulation/single -H 'content-type: text/plain'
    ```
   
    When the message is successfully sent, the following message is logged in the terminal:
    
    ```
    "status":"OK","message":"Single Event simulation started successfully"
    ```

**Option 3 - Publish events with Postman to the simulator HTTP endpoint:**

1. Install the Postman application from the Chrome web store.

2. Launch the Postman application.

3. Make a 'Post' request to the 'http://localhost:9390/simulation/single' endpoint. Set the Content-Type to `text/plain` and set the request body in text as follows:

    `{"streamName": "SweetProductionStream", "siddhiAppName": "PublishKafkaInAvroFormatUsingSchemaRegistry","data": ['chocolate cake', 50.50]}`
    
4. Click **send**. When the message is successfully sent, the following messages are logged in the terminal.

    *  `"status": "OK",`
    *  `"message": "Single Event simulation started successfully"`

## Viewing the Results:

You can view the following output in the terminal in which you ran the ant build for `<SI_HOME>/samples/sample-clients/kafka-avro-consumer`.
```
[java] [org.wso2.extension.siddhi.io.kafka.source.KafkaConsumerThread] : Event received in Kafka Event Adaptor with offSet: 2, key: null, topic: kafka_result_topic, partition: 0
[java] [io.siddhi.core.stream.output.sink.LogSink] : KafkaSample : logStream : Event{timestamp=1546973831995, data=[chocolate cake, 50.5], isExpired=false}
```

!!! note
    If the message `'Kafka' sink at 'LowProductionAlertStream' has successfully connected to http://localhost:9092` does not appear, the reason can be that port 9092 defined in the Siddhi application is already being used by a different program. To resolve this issue, do as follows:<br/><br/>
    1. Stop the Siddhi application (i.e., by clicking the stop button for the Siddhi application in the top panel).<br/><br/>
    2. In the source configuration of the Siddhi application, change port 9092 to an unused port.<br/><br/>
    3. Start the Siddhi application and check whether the specified messages appear in the terminal.

The complete Siddhi application used in this sample is as follows:

```sql
@App:name("PublishKafkaInAvroFormatUsingSchemaRegistry")

@App:description('Send events via Kafka transport using Avro format')


define stream SweetProductionStream (name string, amount double);

@sink(type='kafka',
topic='kafka_result_topic',
bootstrap.servers='localhost:9092',
is.binary.message='true',
@map(type='avro',schema.registry='http://localhost:8081',schema.id='1'))
define stream LowProductionAlertStream (name string, amount double);

@info(name='EventsPassthroughQuery')
from SweetProductionStream
select *
insert into LowProductionAlertStream;
```