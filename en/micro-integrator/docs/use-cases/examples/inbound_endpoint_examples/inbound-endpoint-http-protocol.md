# HTTP Inbound Endpoint Sample
## Example use case

This sample demonstrates how an HTTP inbound endpoint can act as a
dynamic http listener. Many http listeners can be added without
restarting the server. When a message arrives at a port it will bypass
the inbound side axis2 layer and will be sent directly to the sequence
for mediation.The response also behaves in the same way.

## Synapse configuration

Following are the integration artifacts that we can used to implement this scenario.

```xml tab='Inbound Endpoint'
<?xml version="1.0" encoding="UTF-8"?>
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                name="HttpListenerEP1"
                sequence="TestIn"
                onError="fault"
                protocol="http"
                suspend="false">
   <parameters>
    <parameter name="inbound.http.port">8085</parameter>
   </parameters>
</inboundEndpoint>
```

```xml tab='Sequence'
<?xml version="1.0" encoding="UTF-8"?>
<sequence xmlns="http://ws.apache.org/ns/synapse" name="TestIn">
    <send receive="reciveSeq">
        <endpoint>
            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
        </endpoint>
    </send>
</sequence>
<sequence xmlns="http://ws.apache.org/ns/synapse" name="reciveSeq">
    <send/>
</sequence>
```
## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Solution project
3. Create the following artifacts: Inbound endpoint, Sequence.
4. Deploy the artifacts in your Micro Integrator.

Set up the back-end service.

Invoke the inbound endpoint:

Analyze the output debug messages for the actions in the dumb client
mode.

You will see that the Micro Integrator receives a message when the Micro Integrator Inbound is set
as the ultimate receiver. You will also see the response from the back
end in the client.
