# Sample 267: Switching from UDP to HTTP/S

!!! warning

Note that WSO2 EI is shipped with the following changes to what is
mentioned in this documentation :

-   `           <PRODUCT_HOME>/          `
    `           repository/samples/          ` directory that includes
    all Integration profile samples is changed to
    `           <EI_HOME>/          `
    `           samples/service-bus/          ` .
    `                     `
-   `           <PRODUCT_HOME>/          `
    `           repository/samples/resources/          ` directory that
    includes all artifacts related to the Integration profile samples is
    changed to `           <EI_HOME>/          `
    `           samples/service-bus/resources/          ` .

TEST  

**Objective:** Demonstrate receiving SOAP messages over UDP and
forwarding them over HTTP

``` html/xml
<definitions xmlns="http://ws.apache.org/ns/synapse"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://ws.apache.org/ns/synapse http://synapse.apache.org/ns/2010/04/configuration/synapse_config.xsd">

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
</definitions>
```

**Prerequisites:**

-   [Configure Synapse to use the UDP
    transport](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ConfigureWSO2ESBforUDPTransport)
    . The sample Axis2 client should also be setup to send UDP requests.
-   Start Synapse: `          synapse -sample 267         `
-   Start Axis2 server with SimpleStockService deployed

This sample is similar to [Sample
266](_Sample_266_-_Switching_from_TCP_to_HTTP_S_) . Only difference is
instead of the TCP transport we will be using the UDP transport to
receive messages. Invoke the stockquote client using the following
command. Note the TCP URL in the command.

``` java
ant stockquote -Daddurl=udp://localhost:9999?contentType=text/xml -Dmode=placeorder
```

Since we have configured the content type as text/xml for the proxy
service, incoming messages will be processed as SOAP 1.1 messages. The
sample Axis2 server console will print a message indicating that it has
received the request:

``` java
Thu May 20 12:25:01 IST 2010 samples.services.SimpleStockQuoteService :: Accepted order #1 for : 17621 stocks of IBM at $ 73.48068475255796
```
