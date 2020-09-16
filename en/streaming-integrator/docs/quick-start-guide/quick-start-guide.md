!!! note
    This guide is still a work in progress and not tested.

# Streaming Integrator Quick Start Guide

## Introduction

This quick start guide gets you started with the Streaming Integrator (SI), in just 5 minutes.


In this guide, you will download the SI distribution, start it and then try out a simple Siddhi application.

The outline of this quick start guide is shown in the diagram below.

![Quick Start Guide Outline](../../images/qsg/qsg-outline.png)


## Downloading Streaming Integrator

Download the Streaming Integrator distribution from [WSO2 Streaming Integrator site](https://wso2.com/integration/streaming-integrator/) and extract it to a location of your choice. Hereafter, the extracted location is referred to as `<SI_HOME>`.

## Enabling the Streaming Integrator to work with Kafka

To enable WSO2 Streaming Integrator to consume from Kafka topics and to publish to Kafka topics, follow the steps below:

1. Download the Kafka broker from [the Apache site](https://www.apache.org/dyn/closer.cgi?path=/kafka/2.3.0/kafka_2.12-2.3.0.tgz) and extract it.
From here onwards, this directory is referred to as `<KAFKA_HOME>`. <br/>
<br/>
2. Create a directory named `Source` in a preferred location in your machine and copy the following JARs to it from the `<KAFKA_HOME>/libs` directory.<br/>
<br/>
    - `kafka_2.12-2.3.0.jar`<br/>
    <br/>
    - `kafka-clients-2.3.0.jar`<br/>
    <br/>
    - `metrics-core-2.2.0.jar`<br/>
    <br/>
    - `scala-library-2.12.8.jar`<br/>
    <br/>
    - `zkclient-0.11.jar`<br/>
    <br/>
    - `zookeeper-3.4.14.jar`<br/>
    <br/>
3. Create another directory named `Destination` in a preferred location in your machine.<br/>
<br/>
4. To convert the Kafka JARS you copied to the `Source` directory, issue the following command:<br/>
   ```
   sh <SI_HOME>/bin/jartobundle.sh <{Source}_Directory_Path> <{Destination}_Directory_Path>
   ```<br/>
   <br/>
5. Copy all the jars from the `Destination` directory to the `<SI_HOME>/lib` directory.


## Deploying a simple Siddhi application

Let's create a simple Siddhi application that reads data from a CSV file, does a simple transformation to the data, and then publishes the results to a Kafka topic so that multiple subscriber applications can have access to that data.

![Scenario](../../images/quick-start-guide-101/quick-start.png)

1. Download `productions.csv` file from [here](https://github.com/wso2/docs-ei/tree/master/en/streaming-integrator/docs/examples/resources/productions.csv) and save it in a location of your choice.

    !!! info
        In this example, the file is located in the `Users/foo`directory.

2. Open a text file and copy-paste following Siddhi application into it.

    ```
    @App:name('ManageProductionStats')
    @App:description('Receive events via file and publish to Kafka topic')
    
    @source(type = 'file', mode = "LINE", file.uri = "file:/Users/foo/productions.csv", tailing = "true",
        @map(type = 'csv'))
    define stream SweetProductionStream (name string, amount double);
    
    @sink(type = 'kafka', bootstrap.servers = "localhost:9092", topic = "total_production", is.binary.message = "false", partition.no = "0",
        @map(type = 'json'))
    define stream TotalProductionStream (name string, amount long);
    
    -- Simple Siddhi query to calculate production totals.
    @info(name = 'query1')
    from SweetProductionStream 
    select name, sum(amount) as amount 
    group by name
    insert into TotalProductionStream;
    ```

    The above Siddhi application reads input data from a file named `production.csv` in the CSV format, processes it and publishes the resulting output in a Kafka topic named `total_production`. As a result, any application that cannot read streaming data, but is capable of subscribing to a Kafka topic can access the output. The each input event reports the amount of a specific sweet produced in a production run. The Streaming Integrator calculates the total produced of each sweet with each event. Therefore, each output event reports the total amount produced for a sweet from the time you started running the Siddhi application. 

3. Save this file as `ManageProductionStats.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

## Starting Kafka and creating a topic

Let's start the Kafka server and create a Kafka topic so that the `ManageProductionStats.siddhi` application you created can publish its output to it.

To start Kafka:

1. Navigate to the `<KAFKA_HOME>` directory and start a zookeeper node by issuing the following command.

    `sh bin/zookeeper-server-start.sh config/zookeeper.properties`

2. Navigate to the `<KAFKA_HOME>` directory and start Kafka server node by issuing the following command.

    `sh bin/kafka-server-start.sh config/server.properties`
    
To create a Kafka topic named `total_production`:

1. Navigate to `<KAFKA_HOME>` directory and issue the following command:

    `bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic total_production`
    
## Starting the Streaming Integrator server

Navigate to the `<SI_HOME>/bin` directory in the console and issue the appropriate command depending on your operating system to start the Streaming Integrator. <br/>

- For Windows: `server.bat`
- For Linux/MacOS: `./server.sh`

## Testing your Siddhi application

To test the `ManageProductionStats` Siddhi application you created, follow the steps below.
 
1. Open the `/Users/foo/productions.csv` file and add five rows in it as follows:

    `Toffee,40.0`
    `Almond cookie, 70.0`
    `Baked alaska, 30.0`
    `Toffee, 60.0`
    `Baked alaska, 20.0`
    
    Save your changes.
    
2. To observe the messages in the `total_production` topic, navigate to the `<KAFKA_HOME>` directory and issue the following command:

    `bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic bulk-orders --from-beginning`
    
    
You can see the following message in the Kafka Consumer log. 


## What's next?

Once you try out this quick start guide, you can proceed to one of the following sections.

- Learn the basic functionality of the Streaming Integrator in less than 30 minutes by [Creating Your First Siddhi Application](getting-started/getting-started-guide-overview.md)

- To understand how to trigger integration flows via the Micro Integrator based on the results you generate via the Streaming Integrator, see [Getting SI Running with MI in Five Minutes](hello-world-with-mi.md).

- Try out [Streaming Integrator tutorials](../examples/tutorials-overview.md).
