# Sample 266 - Switching from TCP to HTTP/S

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

**Objective:** Demonstrate receiving SOAP messages over TCP and
forwarding them over HTTP

``` html/xml
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

**Prerequisites:**

-   Configure ESB to use the TCP transport and configure sample Axis2
    client to send TCP requests. See
    [here](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ConfiguringWSO2EnterpriseIntegratortousetheTCPtransport)
    for details on how to do this.
-   Start Synpase using sample 266: ie synapse -sample 266
-   Start Axis2 server with SimpleStockService deployed

This sample is similar to Sample 250. Only difference is instead of the
JMS transport we will be using the TCP transport to receive messages.
TCP is not an application layer protocol. Hence there are no application
level headers available in the requests. ESB has to simply read the XML
content coming through the socket and dispatch it to the right proxy
service based on the information available in the message payload
itself. The TCP transport is capable of dispatching requests based on
addressing headers or the first element in the SOAP body. In this
sample, we will get the sample client to send WS-Addressing headers in
the request. Therefore the dispatching will take place based on the
addressing header values.

Invoke the stockquote client using the following command. Note the TCP
URL in the command.

``` java
ant stockquote -Daddurl=tcp://localhost:6060/services/StockQuoteProxy -Dmode=placeorder
```

The TCP transport will receive the message and hand it over to the
mediation engine. Synapse will dispatch the request to the
StockQuoteProxy service based on the addressing header values.  
The sample Axis2 server console will print a message indicating that it
has received the request:

``` java
Thu May 20 12:25:01 IST 2010 samples.services.SimpleStockQuoteService :: Accepted order #1 for : 17621 stocks of IBM at $ 73.48068475255796
```
