# Getting SI Running with MI in Five Minutes

This quick start guide explains how to trigger an integration flow using a message received by the Streaming Integrator.

In this example, the same message you send to the Micro Integrator goes through the `inSeq` defined, and uses the `respond` mediator (to be changed with a `grpcResponseMediator`) to send back the response to the Streaming integrator.

!!!tip "Before you begin:"
    - Download and install both the Streaming Integrator and the Sttreaming Integrator Tooling from [here](Link).

    - Start the Streaming Integrator Tooling by issuing one of the following commands from the `<SI_HOME>/bin` directory:
        -   For Windows: `server.bat`
        -   For Linux: `./server.sh`

    - Start the Streaming Integrator by issuing one of the following commands from the `<SI_TOOLING_HOME>/bin` directory:
        - For Windows: `tooling.bat`
        - For Linux: `./tooling.sh`


Navigate to <SteamingIntegratorHome>/wso2/server/deployment/siddhi-apps and deploy the following siddhi application in streaming integrator. This siddhi application will receive an HTTP message and trigger a sequence in the micro integrator and log the received response.

@App:name("grpc-call-response")
@App:description("This siddhi app triggers integration flow from SI to MI")

-- Please refer to https://docs.wso2.com/display/SP400/Quick+Start+Guide on getting started with SI editor.

@sink(type='grpc-call', publisher.url = 'grpc://localhost:8888/org.wso2.grpc.EventService/process/mySeq', sink.id= '1', headers='{{headers}}', @map(type='json'))
define stream FooStream (message string, headers string);

@sink(type='log')
define stream outputStream(message string);

@source(type='grpc-call-response', sink.id= '1', @map(type='json'))
define stream BarStream (message string);

@source(type='http', @map(type='json'))

Deploy the following GRPC inbound endpoint to <MicroIntegratorHome>/repository/deployment/server/synapse-configs/default/inbound-endpoints
<?xml version="1.0" encoding="UTF-8"?>
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                 name="GrpcInboundEndpoint"
                 sequence="inSeq"
                 onError="errSeq"
                 protocol="grpc"
                 suspend="false">
   <parameters>
      <parameter name="inbound.grpc.port">8888</parameter>
      <parameter name="payload.type">true</parameter>
   </parameters>
</inboundEndpoint>


Use the following curl command to send an event to the streaming integrator’s http source which will trigger the integration flow with the micro integrator.
curl -X POST -d "{\"event\":{\"message\":\"http_curl\",\"headers\":\"'Content-Type:json'\"}}" http://localhost:8006/productionStream --header "Content-Type:application/json"define stream InputStream(message string, headers string);

from InputStream
select *
insert into FooStream;

@info(name = 'query')
from BarStream
select *
insert into outputStream;

Deploy the following artefacts in the Micro Integrator

Deploy the following sequence with “respond” mediator (to be changed with a grpcResponseMediator) to <MicroIntegratorHome>/repository/deployment/server/synapse-configs/default/sequences folder.
<?xml version="1.0" encoding="UTF-8"?>
<sequence xmlns="http://ws.apache.org/ns/synapse" name="inSeq">
   <log level="full"/>
   <respond/>
</sequence>
