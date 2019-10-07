# HTTPS Inbound Endpoint Sample

## Example use case

This sample demonstrates how an HTTPS inbound endpoint can act as a
dynamic https listener. Many https listeners can be added without
restarting the server. When a message arrives at a port it will bypass
the inbound side axis2 layer and will be sent directly to the sequence
for mediation.The response also behaves in the same way.

## Synapse configuration

Following are the integration artifacts that we can used to implement this scenario. See the instructions on how to [build and run](#build-and-run) this example.

```xml tab='Inbound Endpoint'
<?xml version="1.0" encoding="UTF-8"?>
<inboundEndpoint name="HttpsListenerEP"
                 protocol="https"
                 suspend="false" sequence="TestIn" onError="fault" >
    <p:parameters xmlns:p="http://ws.apache.org/ns/synapse">
        <p:parameter  name="inbound.http.port">8081</p:parameter>
        <p:parameter name="keystore">
            <KeyStore>
                <Location>repository/resources/security/wso2carbon.jks</Location>
                <Type>JKS</Type>
                <Password>wso2carbon</Password>
                <KeyPassword>wso2carbon</KeyPassword>
            </KeyStore>
        </p:parameter>
        <p:parameter name="truststore">
            <TrustStore>
                <Location>repository/resources/security/client-truststore.jks</Location>
                <Type>JKS</Type>
                <Password>wso2carbon</Password>
            </TrustStore>
        </p:parameter>
    </p:parameters>
</inboundEndpoint>
```

```xml tab='Sequence 1'
<?xml version="1.0" encoding="UTF-8"?>
<sequence xmlns="http://ws.apache.org/ns/synapse" name="TestIn">
    <send receive="reciveSeq">
        <endpoint>
            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
        </endpoint>
    </send>
</sequence>
```

```xml tab='Sequence 2'
<?xml version="1.0" encoding="UTF-8"?>
<sequence xmlns="http://ws.apache.org/ns/synapse" name="reciveSeq">
    <send/>
</sequence>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. See the instructions on [creating mediation sequences](../../../../develop/creating-artifacts/creating-reusable-sequences) to define the two sequences given above ('Sequence 1' and 'Sequence 2'). 
4. See the instructions on [creating an inbound endpoint](../../../../develop/creating-artifacts/creating-an-inbound-endpoint) to define the inbound endpoint given above.
5. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Set up a back-end service for the sample.

Invoke the inbound endpoint.

Analyze the output debug messages for the actions in the dumb client mode. You will see that the Micro Integrator receives a message when the Micro Integrator Inbound is set as the ultimate receiver. You will also see the response from the back
end in the client.
