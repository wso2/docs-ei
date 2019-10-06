# Switching from TCP to HTTP/S

This example demonstrates how WSO2 Micro Integrator receives SOAP messages over TCP and forwards them over HTTP.

TCP is not an application layer protocol. Hence there are no application level headers available in the requests. The Micro Integrator has to simply read the XML content coming through the socket and dispatch it to the right proxy service based on the information available in the message payload itself. The TCP transport is capable of dispatching requests based on addressing headers or the first element in the SOAP body. In this sample, we will get the sample client to send WS-Addressing headers in the request. Therefore the dispatching will take place based on the addressing header values.

## Synapse configuration

Following are the integration artifacts (proxy service) that we can used to implement this scenario.

```xml
<definitions xmlns="http://ws.apache.org/ns/synapse"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://ws.apache.org/ns/synapse http://synapse.apache.org/ns/2010/04/configuration/synapse_config.xsd">

    <proxy xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteProxy" transports="tcp">
        <target>
            <endpoint>
                <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
            </endpoint>
            <inSequence>
                <log level="full"/>
                <property name="OUT_ONLY" value="true"/>
            </inSequence>
        </target>
    </proxy>
</definitions>
```
