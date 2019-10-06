# Introduction to Switching Transports
## Example use case

This sample demonstrates how the Micro Integrator receives a messages over the JMS transport and forwards it over a HTTP/S transport.. In this sample, the client sends a request message to the proxy service exposed in JMS. The ESB forwards this message to the HTTP EPR of the simple stock quote service hosted on the sample Axis2 server, and returns the reply back to the client through a JMS temporary queue.

## Building the sample

The XML configuration for this sample is as follows:

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

## Build and run

```bash
ant jmsclient -Djms_type=pox -Djms_dest=dynamicQueues/StockQuoteProxy -Djms_payload=MSFT
```

When you run the client and analyze the Micro Integrator debug log, you will see the following entry, which shows that the JMS listener received the request message:

When you run the client, it sends a plain XML formatted place order request to a JMS queue named `StockQuoteProxy`. The Micro Integrator polls on this queue for any incoming messages and picks up the request. If you run the Micro Integrator in the DEBUG mode, you will see the following entry on the console.

```bash
[JMSWorker-1] DEBUG ProxyServiceMessageReceiver -Proxy Service StockQuoteProxy received a new message...
```

Then, the Micro Integrator mediated the request and forwards it to the sample Axis2 server over HTTP. If you analyze the console running the sample Axis2 server, you will see the following message indicating that the server has accepted an order:

```bash
Accepted order for : 16517 stocks of MSFT at $ 169.14622538721846
```

!!! Note
    It is possible to instruct a JMS proxy service to listen to an already existing destination without creating a new one. To do this, use the property elements on the proxy service definition to specify the destination and connection factory.

    ```xml
    <property name="transport.jms.Destination" value="dynamicTopics/something.TestTopic"/>
    ```