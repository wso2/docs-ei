# Getting Started with Streaming Integrator in 5 mins

## Introduction

This tutorial will get you started with the Streaming Integrator (SI), in just 5 minutes.

In this tutorial, we will download the SI, start it and then try out a simple "Hello World" app.

## Downloading Streaming Integrator

Download the Streaming Integrator distribution from TODO and extract it to a location of your choice. Hereafter, we will call the extracted location {WSO2SIHome}.  

## Starting the server

Navigate to {WSO2SIHome}/bin on console and issue command "server.sh"

## Deploying a simple Siddhi app

Let's create a simple Siddhi app, to receive an HTTP message, do a simple transformation to the message, and log it on the SI console. 

Open a text file and copy-paste following app into it.
```
@App:name('MySimpleApp')

@App:description('Receive events via HTTP transport and view the output on the console')

@Source(type = 'http', receiver.url='http://localhost:8006/productionStream', basic.auth.enabled='false',
    @map(type='json'))
define stream SweetProductionStream (name string, amount double);

@sink(type='log')
define stream TransformedProductionStream (nameInUpperCase string, amount double);

from SweetProductionStream
select str:upper(name) as nameInUpperCase, amount
insert into TransformedProductionStream;
```
> **_INFO:_** Once we deploy above Siddhi app, it creates a new HTTP endpoint at `http://localhost:8006/productionStream` and starts listening to the endpoint for incoming messages. Next, we will push a message to this endpoint using a CURL command. 

Now execute following CURL command on the console:
```
curl -X POST -d "{\"event\": {\"name\":\"sugar\",\"amount\": 20.5}}"  http://localhost:8006/productionStream --header "Content-Type:application/json"
```  
You should see following output on the SI console:
```
INFO {io.siddhi.core.stream.output.sink.LogSink} - MySimpleApp : TransformedProductionStream : Event{timestamp=1563539561686, data=[SUGAR, 20.5], isExpired=false}
```
You may notice that the `name` has being converted into uppercase (from `"sugar"` to `"SUGAR"`).
 