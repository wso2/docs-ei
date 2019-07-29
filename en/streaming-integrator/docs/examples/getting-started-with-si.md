# Getting Started with Streaming Integrator in 5 mins

## Introduction

This tutorial will get you started with the Streaming Integrator (SI), in just 5 minutes.

In this tutorial, you will download the SI distribution, start it and then try out a simple Siddhi application.

## Tutorial Outline

- [Downloading Streaming Integrator](#downloading-streaming-integrator)
- [Starting the server](#starting-the-server)
- [Deploying a simple Siddhi app](#deploying-a-simple-siddhi-app)

## Downloading Streaming Integrator

Download the Streaming Integrator distribution from TODO and extract it to a location of your choice. Hereafter, we will call the extracted location as `<SI_HOME>`.  

## Starting the server

Navigate to `<SI_HOME>/bin` on console and issue command `server.sh`

## Deploying a simple Siddhi app

Let's create a simple Siddhi application, to receive an HTTP message, do a simple transformation to the message, and log it on the SI console. 

Open a text file and copy-paste following Siddhi application into it.
```
@App:name('MySimpleApp')

@App:description('Receive events via HTTP transport and view the output on the console')

@Source(type = 'http', receiver.url='http://localhost:8006/productionStream', basic.auth.enabled='false',
    @map(type='json'))
define stream SweetProductionStream (name string, amount double);

@sink(type='log')
define stream TransformedProductionStream (nameInUpperCase string, amount double);

-- Simple Siddhi query to transform the name to upper case.
from SweetProductionStream
select str:upper(name) as nameInUpperCase, amount
insert into TransformedProductionStream;
```
Save this file as `MySimpleApp.siddhi` into <SI_HOME>/wso2/server/deployment/siddhi-files directory.

!!info
    Once you deploy above Siddhi application, it creates a new `HTTP` endpoint at `http://localhost:8006/productionStream` and starts listening to the endpoint for incoming messages. Next, we will push a message to this endpoint using a CURL command. 

Now execute following `CURL` command on the console:
```
curl -X POST -d "{\"event\": {\"name\":\"sugar\",\"amount\": 20.5}}"  http://localhost:8006/productionStream --header "Content-Type:application/json"
```  
Notice that you published a message with a lower case name: `sugar`. 

You should see an output, similar to following, on the SI console:
```
INFO {io.siddhi.core.stream.output.sink.LogSink} - MySimpleApp : TransformedProductionStream : Event{timestamp=1563539561686, data=[SUGAR, 20.5], isExpired=false}
```
Notice that the output message has an uppercase name: `SUGAR`. This is because of the simple message transformation done using the Siddhi application. 

 