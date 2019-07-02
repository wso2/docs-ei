#Working with Kafka
Streaming integrator could be used to consume from a Kafka topic as well as publish to a Kafka topic, in a streaming way.

This tutorial takes you through, consuming from a Kafka topic, processing the messages and finally publishing output to a Kafka topic. 

####Tutorial Outline
- Preparing the server
- Consuming from a Kafka topic
- Configuring more Kafka Consumer parameters
    - Specifying Kafka Offsets
    - Etc
- Publishing to a Kafka topic
- Configuring more Kafka Publisher parameters

####Preparing the server

Follow these steps to prepare the server in order to consume/publish from/to Kafka.

1. Download the Kafka broker from here: https://www.apache.org/dyn/closer.cgi?path=/kafka/2.3.0/kafka_2.12-2.3.0.tgz and extract it.
We will call the extracted location as {KafkaHome}

2. Create a directory named {Source} in a preferred location in your machine and copy the following JARs to it from the {KafkaHome}/libs directory.
* kafka_2.12-2.3.0.jar
* kafka-clients-2.3.0.jar
* metrics-core-2.2.0.jar
* scala-library-2.12.8.jar
* zkclient-0.11.jar
* zookeeper-3.4.14.jar

3. Create another directory named {Destination} in a preferred location in your machine.

4. Issue following command:
sh {WSO2SPHome}/bin/jartobundle.sh <{Source} Directory Path> <{Destination} Directory Path>

5. Copy all the jars from the {Destination} directory to {WSO2SPHome}/lib directory.

6. Copy all the jars from the {Source} directory to {WSO2SPHome}/samples/sample-clients/lib directory. 

####Starting Kafka 

1. Navigate to {KafkaHome} and start zookeeper node using "sh bin/zookeeper-server-start.sh config/zookeeper.properties" command.

2. Navigate to {KafkaHome} and start Kafka server node using "sh bin/kafka-server-start.sh config/server.properties" command.

####Starting Stream Processor
Navigate to {WSO2SPHome}/bin directory and issue command "sh worker.sh" 

####Consuming from a Kafka topic

Let's create a basic Siddhi app to consume messages from a Kafka topic.

Open a text file and copy-paste following app into it.

```
@App:name("HelloKafka")

@App:description('Consume events from a Kafka Topic and log the messages on the console.')

@source(type='kafka',
        topic.list='kafka_topic',
        threading.option='single.thread',
        group.id="group",
        bootstrap.servers='localhost:9092',
        @map(type='json'))
@sink(type='log')        
define stream SweetProductionStream (name string, amount double);
```

Save this file as HelloKafka.siddhi into {WSO2SPHome}/wso2/worker/deployment/siddhi-files directory.

> **_INFO:_**  We just created a Siddhi app which listens to Kafka topic 'kafka_topic' and log any incoming messages. However, we still either have not created such a Kafka topic or have pushed any messages to it.  We will do that in the following section.

####Generating Kafka messages

Now let's generate some Kafka messages so that the Stream Processor would receive those. 

First, let's create a topic named "kafka_topic" in the Kafka server.

Navigate to {KafkaHome} and run following command:
```
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic kafka_topic
```
Now let's run the Kafka command line client to push a few messages to the Kafka server.
```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic kafka_topic
```
Now you are prompted to type the messages on the console. Type following on the command prompt:
```
{"event":{ "name":"KitKat", "amount":100.0}} 
```
This will push a message to Kafka Server which will then be consumed by the Siddhi app we deployed in the Stream Processor. As a result, you should now see following log on the Stream Processor log:
```
INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - HelloKafka : SweetProductionStream : Event{timestamp=1562069868006, data=[KitKat, 100.0], isExpired=false}
```


