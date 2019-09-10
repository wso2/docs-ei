# Sample 253: Bridging from JMS to HTTP and Replying with a 202 Accepted Response

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

-   [Introduction](#Sample253:BridgingfromJMStoHTTPandReplyingwitha202AcceptedResponse-Introduction)
-   [Prerequisites](#Sample253:BridgingfromJMStoHTTPandReplyingwitha202AcceptedResponse-Prerequisites)
-   [Building the
    sample](#Sample253:BridgingfromJMStoHTTPandReplyingwitha202AcceptedResponse-Buildingthesample)
-   [Executing the
    sample](#Sample253:BridgingfromJMStoHTTPandReplyingwitha202AcceptedResponse-Executingthesample)
-   [Analyzing the
    output](#Sample253:BridgingfromJMStoHTTPandReplyingwitha202AcceptedResponse-Analyzingtheoutput)

### Introduction

This sample demonstrates how the ESB performs transport switching
between JMS and HTTP, and also demonstrates how to configure a one-way
HTTP proxy.

### Prerequisites

-   Configure WSO2 ESB's JMS transport with ActiveMQ and enable the JMS
    transport listener. For information on how to configure WSO2 ESB's
    JMS transport with ActiveMQ, see [Configure with
    ActiveMQ](https://docs.wso2.com/display/EI650/Configure+with+ActiveMQ)
    .

    !!! note

    Note

    If you are using ActiveMQ version 5.8.0 or above, you need to copy
    the
    [hawtbuf-1.2.jar](http://repo1.maven.org/maven2/org/fusesource/hawtbuf/hawtbuf/1.2/hawtbuf-1.2.jar)
    file from the `           <ActiveMQ>/lib          ` directory to the
    `           <ESB_HOME>/repository/components/lib          `
    directory.

    TEST  

-   For a list of prerequisites, see [Prerequisites to Start the ESB
    Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
    .

### Building the sample

The XML configuration for this sample is as follows:

``` html/xml
<definitions xmlns="http://ws.apache.org/ns/synapse">
    <proxy name="JMStoHTTPStockQuoteProxy" transports="jms">
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
        <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
    </proxy>
    <proxy name="OneWayProxy" transports="http">
        <target>
            <inSequence>
                <log level="full"/>
            </inSequence>
            <endpoint>
                <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
            </endpoint>
            <outSequence>
                <send/>
            </outSequence>
        </target>
        <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
    </proxy>
</definitions>
```

This configuration file `         synapse_sample_253.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start the Axis2 server. For instructions on starting the Axis2
    server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

2.  Deploy the back-end service **SimpleStockQuoteService** . For
    instructions on deploying sample back-end services, see [Deploying
    sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

    Now you have a running ESB instance and a back-end service deployed.
    In the next section, we will send a message to the back-end service
    through the ESB using a sample client.

### Executing the sample

This example invokes the one-way `         placeOrder        ` operation
on the `         SimpleStockQuoteService        ` using the Axis2
`         ServiceClient.fireAndForget()        ` API at the client. To
test this, run the following command from the
`         <ESB_HOME>/samples/axis2Client        ` directory:

``` java
ant stockquote -Dmode=placeorder -Dtrpurl="jms:/JMStoHTTPStockQuoteProxy?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&java.naming.provider.url=tcp://localhost:61616&transport.jms.ContentTypeProperty=Content-Type&transport.jms.DestinationType=queue"
```

To test how the ESB responds with an *HTTP 202 Accepted* response to a
request received, run the following command from the
`         <ESB_HOME>/samples/axis2Client        ` directory:

``` java
ant stockquote -Dmode=placeorder -Dtrpurl=http://localhost:8280/services/OneWayProxy
```

### Analyzing the output

When you run the first command specified above, you will see the one-way
JMS message flowing through the ESB into the sample Axis2 server
instance over HTTP, and you will also see how Axis2 acknowledges it with
a HTTP 202 Accepted response.

``` java
SimpleStockQuoteService :: Accepted order for : 7482 stocks of IBM at $ 169.27205579038733
```

When you run the next command, you will see that the proxy service
simply logs the message received and acknowledges it. On the ESB
console, you will see the logged message, and if
[TCPMon](https://ws.apache.org/tcpmon/) is used at the client, you will
also see the 202 Accepted response that is sent back to the client from
the ESB.

``` java
HTTP/1.1 202 Accepted
Content-Type: text/xml; charset=UTF-8
Host: 127.0.0.1
SOAPAction: "urn:placeOrder"
Date: Sun, 06 May 2007 17:20:19 GMT
Server: Synapse-HttpComponents-NIO
Transfer-Encoding: chunked

0
```
