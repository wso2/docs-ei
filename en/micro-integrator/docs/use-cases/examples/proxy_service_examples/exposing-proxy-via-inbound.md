# Exposing a Proxy Service via Inbound Endpoint
## Example use case

## Synapse configuration

If a proxy service is to be exposed only via inbound endpoints, the following service parameter has to be set in the proxy configuration.
```xml
<proxy xmlns="http://ws.apache.org/ns/synapse" name="InboundProxy" transports="https,http" statistics="disable" trace="disable" startOnLoad="true">
        <target>
            <outSequence>
                <send/>
            </outSequence>
            <endpoint>
                <address uri="http://localhost:9773/services/HelloService/"/>
            </endpoint>
        </target>
        <parameter name="inbound.only">true</parameter>
</proxy>
```

## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project.
3. Create a proxy service artifact with the above configuration.
5. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

........

Send the following request to the Micro Integrator: