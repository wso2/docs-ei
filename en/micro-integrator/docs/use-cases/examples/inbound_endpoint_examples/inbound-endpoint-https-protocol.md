# HTTPS Inbound Endpoint Sample

## Example use case

This sample demonstrates how an HTTPS inbound endpoint can act as a
dynamic https listener. Many https listeners can be added without
restarting the server. When a message arrives at a port it will bypass
the inbound side axis2 layer and will be sent directly to the sequence
for mediation.The response also behaves in the same way.

## Synapse configuration

Following are the integration artifacts that we can used to implement this scenario.

```xml tab='Inbound Endpoint'
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
<sequence xmlns="http://ws.apache.org/ns/synapse" name="TestIn">
    <send receive="reciveSeq">
        <endpoint>
            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
        </endpoint>
    </send>
</sequence>
```

```xml tab='Sequence 2'
<sequence xmlns="http://ws.apache.org/ns/synapse" name="reciveSeq">
    <send/>
</sequence>
```

## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the following artifacts: Inbound endpoint, Sequence.
4. Deploy the artifacts in your Micro Integrator.

Configure the ActiveMQ broker.

Set up the back-end service.

Invoke the proxy service:

Analyze the output debug messages for the actions in the d umb client
mode.

You will see that the Micro Integrator receives a message when the Micro Integrator Inbound is set
as the ultimate receiver. You will also see the response from the back
end in the client.

Analyze the output debug messages for the actions in the Dumb Client
Mode.

You will see the ESB receiving a message for which the ESB Inbound is
set as the ultimate receiver. You will also see the response from the
back end in the Client.
