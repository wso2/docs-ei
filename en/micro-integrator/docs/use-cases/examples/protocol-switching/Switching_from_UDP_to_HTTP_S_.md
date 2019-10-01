# Switching from UDP to HTTP/S

Demonstrate receiving SOAP messages over UDP and forwarding them over HTTP.

## Synapse configuration

This sample uses the UDP transport to
receive messages. Invoke the stockquote client using the following
command. Note the TCP URL in the command.

``` java
ant stockquote -Daddurl=udp://localhost:9999?contentType=text/xml -Dmode=placeorder
```

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

## Build and run

-   [Configure Synapse to use the UDP transport](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ConfigureWSO2ESBforUDPTransport). The sample Axis2 client should also be setup to send UDP requests.
-   Start Synapse: `synapse -sample 267`
-   Start Axis2 server with SimpleStockService deployed

Since we have configured the content type as text/xml for the proxy
service, incoming messages will be processed as SOAP 1.1 messages. The
sample Axis2 server console will print a message indicating that it has
received the request:

```bash
Thu May 20 12:25:01 IST 2010 samples.services.SimpleStockQuoteService :: Accepted order #1 for : 17621 stocks of IBM at $ 73.48068475255796
```
