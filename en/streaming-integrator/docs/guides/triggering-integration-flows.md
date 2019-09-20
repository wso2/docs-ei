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



