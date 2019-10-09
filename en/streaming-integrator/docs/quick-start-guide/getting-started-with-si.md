# Getting Started with Streaming Integrator in Five Minutes

## Introduction

This quick start guide gets you started with the Streaming Integrator (SI), in just 5 minutes.


In this guide, you will download the SI distribution, start it and then try out a simple Siddhi application.

## Tutorial Outline

- [Downloading Streaming Integrator](#downloading-streaming-integrator)
- [Starting the server](#starting-the-server)
- [Deploying a simple Siddhi app](#deploying-a-simple-siddhi-app)

## Downloading Streaming Integrator

Download the Streaming Integrator distribution from [WSO2 Enterprise Integrator site](https://wso2.com/integration/#) and extract it to a location of your choice. Hereafter, the extracted location is referred to as `<SI_HOME>`.

## Starting the server

Navigate to the `<SI_HOME>/bin` directory in the console and issue one of the following commands: <br/>
    - For Windows: `server.bat`
    - For Linux: `./serverg.sh`

## Deploying a simple Siddhi application

Let's create a simple Siddhi application that receives an HTTP message, does a simple transformation to the message, and then publishes the output to the SI console and to a user-specified file.

![Scenario](../../images/quick-start-guide-101/quick-start.png)

Open a text file and copy-paste following Siddhi application into it.

```
@App:name('MySimpleApp')

@App:description('Receive events via HTTP transport and view the output on the console')

@Source(type = 'http', receiver.url='http://localhost:8006/productionStream', basic.auth.enabled='false',
    @map(type='json'))
define stream SweetProductionStream (name string, amount double);

@sink(type='file', @map(type='xml'), file.uri='/Users/foo/low_productions.txt')
@sink(type='log')
define stream TransformedProductionStream (nameInUpperCase string, roundedAmount long);

-- Simple Siddhi query to transform the name to upper case.
from SweetProductionStream
select str:upper(name) as nameInUpperCase, math:round(amount) as roundedAmount
insert into TransformedProductionStream;
```

!!!note
    The output of this application is written into a the file, specified via the `file.uri` parameter. Change the value for this parameter accordingly.

Save this file as `MySimpleApp.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

!!!info
    Once you deploy the above Siddhi application, it creates a new `HTTP` endpoint at `http://localhost:8006/productionStream` and starts listening to the endpoint for incoming messages. The incoming messages are then published to:<br/>
    1. Streaming Integrator logs<br/>
    2. To a file specified by you in XML format<br/>
    The next step is to publish a message to the endpoint that you created, via a CURL command.

## Testing your Siddhi application

To test the `MySimpleApp` Siddhi application you created, execute following `CURL` command on the console.

```
curl -X POST -d "{\"event\": {\"name\":\"sugar\",\"amount\": 20.5}}"  http://localhost:8006/productionStream --header "Content-Type:application/json"
```

Note that you published a message with a lower case name, i.e., `sugar`. However, the output you observe in the SI console is similar to following.

```
INFO {io.siddhi.core.stream.output.sink.LogSink} - MySimpleApp : TransformedProductionStream : Event{timestamp=1563539561686, data=[SUGAR, 21], isExpired=false}
```

Note that the output message has an uppercase name: `SUGAR`. In addition to that, the `amount` has being rounded. This is because of the simple message transformation carried out by the Siddhi application.

In addition to this, open `low_productions.txt` file (i.e., the file that you specified via the `file.uri` parameter). The file should contain the following text.

```
<events><event><nameInUpperCase>SUGAR</nameInUpperCase><roundedAmount>21</roundedAmount></event></events>
``` 

## What's next?

The Streaming Integrator works seamlessly with the Micro Integrator to trigger integration flows based on the output it generates for streaming data. To try out a scenario where you process streaming data and trigger an integration flow via the Micro Integrator in five minutes, see [Getting SI Running with MI in Five Minutes](hello-world-with-mi.md).
