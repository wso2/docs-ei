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

        
## Starting the server

Navigate to the `<SI_HOME>/bin` directory in the console and issue the appropriate command depending on your operating system to start the Streaming Integrator. <br/>

- For Windows: `server.bat`
- For Linux/MacOS: `./server.sh`

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
    
    -- Simple Siddhi query to transform the name to upper case.
    @info(name = 'query1')
    from SweetProductionStream 
    select name, sum(amount) as amount 
    insert into TotalProductionStream;
    ```

!!!note
    The output of this application is written into a the file, specified via the `file.uri` parameter. Change the value for this parameter accordingly so that you publish the output to a file saved in your machine.

Save this file as `MySimpleApp.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

!!!info
    Once you deploy the above Siddhi application, it creates a new `HTTP` endpoint at `http://localhost:8006/productionStream` and starts listening to the endpoint for incoming messages. The incoming messages are then published to:<br/>
        - Streaming Integrator logs<br/>
        - To a file specified by you in XML format<br/>

The next step is to publish a message to the endpoint that you created, via a CURL command.

## Testing your Siddhi application

To test the `MySimpleApp` Siddhi application you created, execute following `CURL` command on the console.

```
curl -X POST -d "{\"event\": {\"name\":\"sugar\",\"amount\": 20.5}}"  http://localhost:8006/productionStream --header "Content-Type:application/json"
```

Note that you published a message with a lower case name, i.e., `sugar`. However, the output you observe in the SI console is similar to following.

```
INFO {io.siddhi.core.stream.output.sink.LogSink} - MySimpleApp :
TransformedProductionStream :Event{timestamp=1563539561686, data=[SUGAR, 21],
isExpired=false}
```


Note that the output message has an uppercase name: `SUGAR`. In addition to that, the `amount` has being rounded. This is because of the simple message transformation carried out by the Siddhi application.

Now open `low_productions.txt` file (i.e., the file that you specified via the `file.uri` parameter). The file should contain the following text.

```
<events><event><nameInUpperCase>SUGAR</nameInUpperCase><roundedAmount>21
</roundedAmount></event></events>
``` 

## What's next?

Once you try out this quick start guide, you can proceed to one of the following sections.

- Learn the basic functionality of the Streaming Integrator in less than 30 minutes by [Creating Your First Siddhi Application](getting-started/getting-started-guide-overview.md)

- To understand how to trigger integration flows via the Micro Integrator based on the results you generate via the Streaming Integrator, see [Getting SI Running with MI in Five Minutes](hello-world-with-mi.md).

- Try out [Streaming Integrator tutorials](../examples/tutorials-overview.md).
