# Switching from HTTP(S) to JMS

Demonstrate switching from HTTP to JMS.

## Synapse configuration

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

## Build and run

-   Download, install and start a JMS server.
-   Configure the Synase JMS transport.

To switch from HTTP to JMS, edit the `samples/axis2Server/repository/conf/axis2.xml` for the sample Axis2 server and enable JMS, and restart the server. Now you can
see that the simple stock quote service is available in both JMS and HTTP in the sample Axis2 server. To see this, point your browser to the
WSDL of the service at `http://localhost:9000/services/SimpleStockQuoteService?wsdl`. The JMS URL for the service is mentioned as below:

```bash
jms:/SimpleStockQuoteService?transport.jms.ConnectionFactoryJNDIName=
QueueConnectionFactory&java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&
java.naming.provider.url=tcp://localhost:61616
```

You may also notice that the simple stock quote Proxy Service exposed in the Micro Integrator is now available only in HTTP as we have specified the transport for that service as HTTP. To observe this, access the WSDL of stock quote proxy service at `http://localhost:8280/services/StockQuoteProxy?wsdl`.

This Micro Integrator configuration creates a Proxy Service Samples over HTTP and forwards received messages to the above EPR using JMS, and sends back the response to the client over HTTP once the simple stock quote service responds with the stock quote reply over JMS to the integration server. To test this, send a place order request to the Micro Integrator using HTTP as follows:

```bash
ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy -Dmode=placeorder -Dsymbol=MSFT
```

The sample Axis2 server console will print a message indicating that it has accepted the order as follows:

```bash
Accepted order for : 18406 stocks of MSFT at $ 83.58806051152119
```