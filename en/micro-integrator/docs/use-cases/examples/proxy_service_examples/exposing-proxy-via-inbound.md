# Exposing a Proxy Service via Inbound Endpoint

Following is a sample proxy configuration:

```
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

If a proxy service is to be exposed only via inbound endpoints, the following service parameter has to be set in the proxy configuration.