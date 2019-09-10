# Switching Transports and Message Format from SOAP to REST POX

## Introduction

This sample demonstrates how a proxy service can be exposed on a subset
of available transports and how it could switch from one transport to
another.

## Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

## Building the sample

The XML configuration for this sample is as follows:

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <proxy name="StockQuoteProxy" transports="https">
            <target>
                <endpoint>
                    <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="pox"/>
                </endpoint>
                <outSequence>
                    <property name="messageType" value="text/xml" scope="axis2"/>
                    <send/>
                </outSequence>
            </target>
            <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
        </proxy>
    </definitions>
```

This configuration file `         synapse_sample_152.xml        ` is
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

## Executing the sample

The sample client used here is the **Stock Quote Client** , which can
operate in several modes. For further details on this sample client and
its operation modes, see [Stock Quote
Client](https://docs.wso2.com/display/EI650/Using+the+Sample+Clients#UsingtheSampleClients-StockQuoteClient)
.

**To execute the sample client**

-   Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          ` directory.

    ``` bash
             
            ant stockquote -Dtrpurl=https://localhost:8243/services/StockQuoteProxy
    ```

## Analyzing the output

This example exposes the created proxy service only on HTTPS. Therefore,
if you try to access it over HTTP, it would result in a fault.

``` java
    ant stockquote -Dtrpurl=http://localhost:8280/services/StockQuoteProxy
    ...
         [java] org.apache.axis2.AxisFault: The service cannot be found for the endpoint reference (EPR) /services/StockQuoteProxy
```

Accessing this over HTTPS using the command
`         ant stock quote -Dtrpurl= href="https://localhost:8243/services/StockQuoteProxy">https://localhost:8243/services/StockQuoteProxy        `
causes the proxy service to access the
`         SimpleStockQuoteService        ` on the sample Axis2 server
using REST/POX.

If you capture the message exchange using TCPMon, you will see that the
REST/POX response is transformed back into a SOAP message and returned
to the client as follows:

``` java
    POST http://localhost:9000/services/SimpleStockQuoteService HTTP/1.1
    Host: 127.0.0.1
    SOAPAction: urn:getQuote
    Content-Type: application/xml; charset=UTF-8;action="urn:getQuote";
    Transfer-Encoding: chunked
    Connection: Keep-Alive
    User-Agent: Synapse-HttpComponents-NIO
    
    75
    <m0:getQuote xmlns:m0="http://services.samples/xsd">
       <m0:request>
          <m0:symbol>IBM</m0:symbol>
       </m0:request>
    </m0:getQuote>
```

``` java
    HTTP/1.1 200 OK
    Content-Type: application/xml; charset=UTF-8;action="http://services.samples/SimpleStockQuoteServicePortType/getQuoteResponse";
    Date: Tue, 24 Apr 2007 14:42:11 GMT
    Server: Synapse-HttpComponents-NIO
    Transfer-Encoding: chunked
    Connection: Keep-Alive
    
    2b3
    <ns:getQuoteResponse xmlns:ns="http://services.samples/xsd">
       <ns:return>
          <ns:change>3.7730036841862384</ns:change>
          <ns:earnings>-9.950236235550818</ns:earnings>
          <ns:high>-80.23868444613285</ns:high>
          <ns:last>80.50750970812187</ns:last>
          <ns:lastTradeTimestamp>Tue Apr 24 20:42:11 LKT 2007</ns:lastTradeTimestamp>
          <ns:low>-79.67368355714606</ns:low>
          <ns:marketCap>4.502043663670823E7</ns:marketCap>
          <ns:name>IBM Company</ns:name>
          <ns:open>-80.02229531286982</ns:open>
          <ns:peRatio>25.089295161182022</ns:peRatio>
          <ns:percentageChange>4.28842665653824</ns:percentageChange>
          <ns:prevClose>87.98107059692451</ns:prevClose>
          <ns:symbol>IBM</ns:symbol>
          <ns:volume>19941</ns:volume>
       </ns:return>
    </ns:getQuoteResponse>
```
