# Switching from UDP to HTTP/S

This example demonstrates how WSO2 Micro Integrator receives SOAP messages over UDP and forwards them over HTTP.

## Synapse configuration

Following are the integration artifacts (proxy service) that we can used to implement this scenario.

```xml 
<proxy xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteProxy" transports="udp">
    <target>
        <endpoint>
            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
        </endpoint>
        <inSequence>
            <log level="full"/>
            <property name="OUT_ONLY" value="true"/>
        </inSequence>
    </target>
    <parameter name="transport.udp.port">9999</parameter>
    <parameter name="transport.udp.contentType">text/xml</parameter>
    <publishWSDL uri="file:repository/conf/sample/resources/proxy/sample_proxy_1.wsdl"/>
</proxy>
```
