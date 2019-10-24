# Switching from HTTP(S) to JMS

This example demonstrates how WSO2 Micro Integrator receives messages in HTTP and passes the messages through JMS.

This Micro Integrator configuration creates a Proxy Service Samples over HTTP and forwards received messages to the above EPR using JMS, and sends back the response to the client over HTTP once the simple stock quote service responds with the stock quote reply over JMS to the integration server.

## Synapse configuration

Following are the integration artifacts (proxy service) that we can used to implement this scenario.

```xml
<proxy name="StockQuoteProxy" transports="http">
    <target>
        <endpoint>
            <address uri="jms:/SimpleStockQuoteService?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;
               java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;java.naming.provider.url=tcp://localhost:61616"/>
        </endpoint>
        <inSequence>
            <property action="set" name="OUT_ONLY" value="true"/>
        </inSequence>
        <outSequence>
            <send/>
        </outSequence>
    </target>
    <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
</proxy>
```