# Setting up Kafka

The Kafka connector allows you to access the Kafka Producer API through WSO2 EI and acts as a message producer that facilitates message publishing. The Kafka connector sends messages to the Kafka brokers.

Kafka is a distributed publish-subscribe messaging system that maintains feeds of messages in topics. Producers write data to topics and consumers read from topics. For more information on Apache Kafka, see [Apache Kafka documentation](http://kafka.apache.org/documentation.html).

Kafka mainly operates based on a topic model. A topic is a category or feed name to which records get published. Topics in Kafka are always multi-subscriber.

## Setting up the environment

To use the Kafka connector, download and install [Apache Kafka](http://kafka.apache.org/downloads.html). Before you start configuring the Kafka you also need WSO2 MI and we refer to that location as <PRODUCT_HOME>.

> **Note**: The recommended version is [kafka](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.0.0/kafka_2.12-1.0.0.tgz). For all available versions of Kafka that you can download, see https://kafka.apache.org/downloads. The recommended Java version is 1.8.

To configure the Kafka connector, copy the following client libraries from the `<KAFKA_HOME>/lib` directory to the `<PRODUCT_HOME>/repository/components/lib` directory.

* [kafka_2.12-1.0.0.jar](https://mvnrepository.com/artifact/org.apache.kafka/kafka_2.12/1.0.0)  
* [kafka-clients-1.0.0.jar](https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients/1.0.0)
* [metrics-core-2.2.0.jar](https://mvnrepository.com/artifact/com.yammer.metrics/metrics-core/2.2.0)
* [scala-library-2.12.3.jar](https://mvnrepository.com/artifact/org.scala-lang/scala-library/2.12.3)
* [zkclient-0.10.jar](https://mvnrepository.com/artifact/com.101tec/zkclient/0.10)
* [zookeeper-3.4.10.jar](https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper/3.4.10)

> **Note**: When configuring the Kafka Inbound Operation, navigate to https://store.wso2.com/store/assets/esbconnector/details/kafka, and click **Download Inbound Endpoint** to download the inbound JAR file and add the downloaded .jar file to the <PRODUCT_HOME>/dropins directory.

Run the following command to start the ZooKeeper server:

```
bin/zookeeper-server-start.sh config/zookeeper.properties
```

Run the following command to start the Kafka server:

```
bin/kafka-server-start.sh config/server.properties
```

Now that you have connected to Kafka, you can start publishing and consuming messages using the Kafka brokers. For more information, see [Publishing Messages using Kafka](kafka-connector-producer-example.md) and [Consuming Messages using Kafka](kafka-inbound-endpoint-example.md).