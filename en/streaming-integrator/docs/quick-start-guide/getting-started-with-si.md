# Getting Started with Streaming Integrator in 5 mins

## Introduction

This quick start guide gets you started with the Streaming Integrator (SI), in just 5 minutes.

In this guide, you will download the SI distribution, start it and then try out a simple Siddhi application.

## Tutorial Outline

- [Downloading Streaming Integrator](#downloading-streaming-integrator)
- [Starting the server](#starting-the-server)
- [Deploying a simple Siddhi app](#deploying-a-simple-siddhi-app)

## Downloading Streaming Integrator

Download the Streaming Integrator distribution from TODO and extract it to a location of your choice. Hereafter, the extracted location is referred to as `<SI_HOME>`.

## Starting the server

Navigate to the `<SI_HOME>/bin` directory in the console and issue the following command. <br/> `server.sh`

## Deploying a simple Siddhi application

Let's create a simple Siddhi application that receives an HTTP message, does a simple transformation to the message, and then logs it in the SI console.

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

Save this file as `MySimpleApp.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

!!info
    Once you deploy the above Siddhi application, it creates a new `HTTP` endpoint at `http://localhost:8006/productionStream` and starts listening to the endpoint for incoming messages.
    Therefore, the next step is to publish a message to this endpoint via a CURL command.

Now execute following `CURL` command on the console.
```
curl -X POST -d "{\"event\": {\"name\":\"sugar\",\"amount\": 20.5}}"  http://localhost:8006/productionStream --header "Content-Type:application/json"
```  
Notice that you published a message with a lower case name: `sugar`. 

However, the output you observe in the SI console is similar to following.
```
INFO {io.siddhi.core.stream.output.sink.LogSink} - MySimpleApp : TransformedProductionStream : Event{timestamp=1563539561686, data=[SUGAR, 20.5], isExpired=false}
```
Notice that the output message has an uppercase name: `SUGAR`. This is because of the simple message transformation carried out by the Siddhi application.
 