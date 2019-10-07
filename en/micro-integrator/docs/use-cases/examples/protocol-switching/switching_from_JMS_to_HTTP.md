# Switching from JMS to HTTP(S)

This example demonstrates how the Micro Integrator receives a messages over the JMS transport and forwards it over an HTTP/S transport. In this sample, the client sends a request message to the proxy service exposed in JMS. The Micro Integrator forwards this message to the HTTP endpoint and returns the reply back to the client through a JMS temporary queue.

## Synapse configuration

Following are the integration artifacts (proxy service) that we can used to implement this scenario.

```xml
<proxy name="StockQuoteProxy" transports="jms">
    <target>
        <inSequence>
            <property action="set" name="OUT_ONLY" value="true"/>
        </inSequence>
        <endpoint>
            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
        </endpoint>
        <outSequence>
            <send/>
        </outSequence>
    </target>
    <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
    <parameter name="transport.jms.ContentType">
        <rules>
            <jmsProperty>contentType</jmsProperty>
            <default>application/xml</default>
        </rules>
    </parameter>
</proxy>
```