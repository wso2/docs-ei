#Triggering Integration 

## Introduction

## Triggering integration flows via Streaming Integrator

### Triggering integration via Streaming Integrator as fire and forget manner

gRPC sink is used to send messages in a fire and forget manner from SI to MI and trigger a sequence.

Following is a sample siddhi application which contains gRPC sink which triggers the “inSeq” in the micro integrator. This 
```siddhi
@App:name("grpc-call")
@App:description("This siddhi application will trigger inSeq in the MicroIntegrator")

@sink(
    type='grpc', 
    publisher.url = 'grpc://localhost:8888/org.wso2.grpc.EventService/consume/inSeq',
    headers='Content-Type:json', 
    metadata='Authorization:Basic YWRtaW46YWRtaW4=',
    @map(type='json')
)
define stream FooStream (message String);

@Source(type = 'http',
        receiver.url='http://localhost:8006/productionStream',
        basic.auth.enabled='false',
        @map(type='json'))
define stream InputStream(message string, headers string);

from InputStream
select *
insert into FooStream;

```
`consume`: this parameter in `publisher.url` implies that, gRPC request invokes the ‘consume’ method of MI's gRPC inbound endpoint which is not sending a response back to the client.

`headers`: it is compulsory to pass the type of content as a header parameter, in-order to read and construct the message from MI.

The user should Deploy a GRPC inbound endpoint to <MicroIntegratorHome>/repository/deployment/server/synapse-configs/default/inbound-endpoints to start a gRPC server in the MI to receive the gRPC event sent by the SI.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                 name="GrpcInboundEndpoint"
                 sequence="inSeq"
                 onError="errSeq"
                 protocol="grpc"
                 suspend="false">
   <parameters>
      <parameter name="inbound.grpc.port">8888</parameter>
   </parameters>
</inboundEndpoint>
```
The following sample `inSeq` sequence should be added to <MIHome>/repository/deployment/server/synapse-configs/default/sequences folder
```xml
<?xml version="1.0" encoding="UTF-8"?>
<sequence xmlns="http://ws.apache.org/ns/synapse" name="inSeq">
   <log level="full"/>
</sequence>

```
### Triggering integration via Streaming Integrator and receiving a response from MI

`gRPC-call` sink uses to send messages from SI to MI and trigger a sequence and get a response back from MI. When using this particular sink, `grpc-call-response` source should be defined to get the corresponding response from the MI.

Shown below is a sample siddhi application which contains gRPC-call sink which triggers the “inSeq” in the micro integrator and processes the response received by the MI using the grpc-call-response source
```siddhi
@App:name("grpc-call-response")
@App:description("Description of the plan")

@sink(
    type='grpc-call', 
    publisher.url = 'grpc://localhost:8888/org.wso2.grpc.EventService/process/inSeq', 
    sink.id= '1', 
    headers='Content-Type:json', 
    @map(type='json')) 
define stream FooStream (message string, headers string);

@source(type='grpc-call-response', sink.id= '1', @map(type='json'))
define stream BarStream (message string);

@Source(type = 'http',
        receiver.url='http://localhost:8006/productionStream',
        basic.auth.enabled='false',
        @map(type='json'))
define stream InputStream(message string, headers string);

from InputStream
select *
insert into FooStream;

from BarStream
select *
insert into TempStream;
```
`process`: this parameter in `publisher.url` implies that, gRPC request invokes the process method of the gRPC server in MI's gRPC inbound endpoint which will be sending a response back to the client.

`sink.id`:  this parameter is a compulsory parameter when using the gRPC-call sink. This is used to map the request with its corresponding response.

The user should Deploy a GRPC inbound endpoint to <MicroIntegratorHome>/repository/deployment/server/synapse-configs/default/inbound-endpoints to start a gRPC server in the MI to receive the gRPC event sent by the SI.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                 name="GrpcInboundEndpoint"
                 sequence="inSeq"
                 onError="errSeq"
                 protocol="grpc"
                 suspend="false">
   <parameters>
      <parameter name="inbound.grpc.port">8888</parameter>
   </parameters>
</inboundEndpoint>
```
The following sample `inSeq` sequence should be added to <MIHome>/repository/deployment/server/synapse-configs/default/sequences folder. `org.wso2.carbon.mediator.grpcresponse.ResponseMediator` will send the response back to SI.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<sequence xmlns="http://ws.apache.org/ns/synapse" name="inSeq">
   <log level="full"/>
   <class name="org.wso2.carbon.mediator.grpcresponse.ResponseMediator"/>
</sequence>
```

### Tutorial

In this tutorial, lets look at how the Streaming Integrator generates an alert based on the events received and how that particular alert can trigger an integration flow in Micro Integrator and get a response back to the Streaming Integrator for further processing.

#### Tutorial steps
##### Configuring Streaming integrator
* Start the WSO2 Streaming Integrator tooling login. Then open a new Siddhi application.
* Lets add the following source to receive events from external into the streaming integrator. The following http source will start listening to the events published to 'http://localhost:8006/productionStream' in JSON format.
```siddhi
@source(type = 'http',
        receiver.url='http://localhost:8006/productionStream',
        basic.auth.enabled='false',
        @map(type='json'))
define stream InputStream(symbol string, amount double);
```
* The following code segment will be responsible for triggering the integration flow (grpc-call) in the micro integrator and retrieving the response back (grpc-call-response) from the micro integrator
The json mapper will get the two attributes from the stream and generates the JSON message before handing over the siddhi event to grpc-call sink to send the event to the micro integrator. 
grpc-call-response source will retrieve the JSON message from micro integrator.
in the `publisher.url` parameter value contains `process` and `inSeq` which means, the streaming integrator will call the process method of the gRPC Listener server in the micro integrator and inject the message to the inSeq which then provides a response back to the client.
```siddhi
@sink(
        type='grpc-call', 
        publisher.url = 'grpc://localhost:8888/org.wso2.grpc.EventService/process/inSeq', 
        sink.id= '1', headers='Content-Type:json', 
        @map(type='json', @payload("""{"symbol":"{{symbol}}","avgAmount":{{avgAmount}}}"""))
    ) 
define stream FooStream (symbol string, avgAmount double);

@source(type='grpc-call-response', sink.id= '1', @map(type='json'))
define stream BarStream (message string);
```
* The following code segment will print the received message using the console.
```siddhi
@sink(type='log', prefix='response_from_mi: ')
define stream LogStream (message string);
```
* The following siddhi queries will trigger an alert using the defined grpc-call sink attached to the FooStream if the average amount is larger than 100 within 1 minute. The response will retrieved by grpc-call-response attached to the BarStream.
The retrieved response will be logged to the console using the LogStream.
```siddhi
from InputStream#window.timeBatch(1 min)
select avg(amount) as avgAmount, symbol
group by symbol
insert into AVGStream;

from AVGStream[avgAmount > 100]
select symbol, avgAmount
insert into FooStream;

@info(name = 'query')
from BarStream 
select *  
insert into LogStream;
```
* The following is the complete siddhi application. Navigate to <SIHome>/wso2/server/deployment/siddhi-apps and deploy the siddhi application.
```siddhi
@App:name("grpc-call-response")
@App:description("This application triggers integration process in the micro integrator using gRPC calls")

@sink(
        type='grpc-call', 
        publisher.url = 'grpc://localhost:8888/org.wso2.grpc.EventService/process/inSeq', 
        sink.id= '1', headers='Content-Type:json', 
        @map(type='json', @payload("""{"symbol":"{{symbol}}","avgAmount":{{avgAmount}}}"""))
    ) 
define stream FooStream (symbol string, avgAmount double);

@source(type='grpc-call-response', sink.id= '1', @map(type='json'))
define stream BarStream (message string);

@source(type = 'http',
        receiver.url='http://localhost:8006/productionStream',
        basic.auth.enabled='false',
        @map(type='json'))
define stream InputStream(symbol string, amount double);

@sink(type='log', prefix='response_from_mi: ')
define stream OutputStream (message string);

from InputStream#window.timeBatch(1 min)
select avg(amount) as avgAmount, symbol
group by symbol
insert into AVGStream;

from AVGStream[avgAmount > 100]
select symbol, avgAmount
insert into FooStream;

@info(name = 'query')
from BarStream 
select *  
insert into OutputStream;
```
##### Configuring Micro integrator
* Lets configure the micro integrator to receive the gRPC event from the streaming integrator and send the response back.
* Deploy a GRPC inbound endpoint to <MicroIntegratorHome>/repository/deployment/server/synapse-configs/default/inbound-endpoints to start a gRPC server in the MI to receive the gRPC event sent by the SI.
This contains a configurable paramter to start the gRPC server and the default sequences that it will inject the messages accordingly.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
               name="GrpcInboundEndpoint"
               sequence="inSeq"
               onError="errSeq"
               protocol="grpc"
               suspend="false">
 <parameters>
    <parameter name="inbound.grpc.port">8888</parameter>
 </parameters>
</inboundEndpoint>
```
* Deploy the following sequence which 
1. Calls a REST endpoint which returns a JSON object. 
1. Logs the response
1. Sends the response back to the gRPC client.

This sequence is defined as "inSeq" because the "grpc-call" sink's "publisher.url" value contains "inSeq" as the url parameter value as the sequence name.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<sequence name="inSeq" xmlns="http://ws.apache.org/ns/synapse">
    <call>
        <endpoint>
            <http method="GET" uri-template="http://localhost:8080/TestService/rest/symbol/info"/>
        </endpoint>
    </call>
    <log level="full"/>
    <class name="org.wso2.carbon.mediator.grpcresponse.ResponseMediator"/>
</sequence>
```
##### Executing and getting results
* Send event to defined "http" source hosted in "http://localhost:8006/productionStream". 
* Sample cURL command is as follows.
`curl -X POST -d "{\"event\":{\"symbol\":\"soap\",\"amount\":110.23}}" http://localhost:8006/productionStream --header "Content-Type:application/json"`
* Execute the above command or a command similar to the above where an average value for "amount" attribute is larger than 100 within a minute.
* After a minute, if the average amount is larger than 100.00 for a particular symbol, the streaming integrator will trigger an integration flow in the micro integrater, which will send a response back after executing the integration flows. This response will be printed on the carbon console of the streaming integrator.


## Triggering integration flows through micro integrator
   
## Triggering integration flows through external services
