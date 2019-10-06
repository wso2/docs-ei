# Switching from TCP to HTTP/S
## Example use case
Demonstrate receiving SOAP messages over TCP and forwarding them over HTTP.

## Synapse configuration

TCP is not an application layer protocol. Hence there are no application level headers available in the requests. The Micro Integrator has to simply read the XML content coming through the socket and dispatch it to the right proxy service based on the information available in the message payload itself. The TCP transport is capable of dispatching requests based on addressing headers or the first element in the SOAP body. In this sample, we will get the sample client to send WS-Addressing headers in the request. Therefore the dispatching will take place based on the addressing header values.

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

## Build and run

Invoke the stockquote client using the following command. Note the TCP URL in the command.

```bash
ant stockquote -Daddurl=tcp://localhost:6060/services/StockQuoteProxy -Dmode=placeorder
```

The TCP transport will receive the message and hand it over to the mediation engine. Synapse will dispatch the request to the StockQuoteProxy service based on the addressing header values. The sample Axis2 server console will print a message indicating that it
has received the request:

```bash
Thu May 20 12:25:01 IST 2010 samples.services.SimpleStockQuoteService :: Accepted order #1 for : 17621 stocks of IBM at $ 73.48068475255796
```
