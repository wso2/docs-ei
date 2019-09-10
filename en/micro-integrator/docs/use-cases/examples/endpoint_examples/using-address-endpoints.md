# Sample 50: POX to SOAP conversion

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


-   [Introduction](#Sample50:POXtoSOAPconversion-Introduction)
-   [Prerequisites](#Sample50:POXtoSOAPconversion-Prerequisites)
-   [Building the
    sample](#Sample50:POXtoSOAPconversion-Buildingthesample)
-   [Executing the
    sample](#Sample50:POXtoSOAPconversion-Executingthesample)
-   [Analyzing the
    output](#Sample50:POXtoSOAPconversion-Analyzingtheoutput)

### Introduction

This sample demonstrates how you can convert a POX message to a SOAP
request.

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

``` html/xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
       <sequence name="main">
         <in>
           <!-- filtering of messages with XPath and regex matches -->
           <filter source="get-property('To')" regex=".*/StockQuote.*">
               <header name="Action" value="urn:getQuote"/>
               <send>
                   <endpoint>
                       <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="soap11"/>
                   </endpoint>
               </send>
           </filter>
         </in>
         <out>
              <send/>
         </out>
       </sequence>
    </definitions>
```

This configuration file `         synapse_sample_50.xml        ` is
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

### Executing the sample

The sample client used here is the **Stock Quote Client** , which can
operate in several modes. For further details on this sample client and
its operation modes, see [Stock Quote
Client](https://docs.wso2.com/display/EI650/Using+the+Sample+Clients#UsingtheSampleClients-StockQuoteClient)
.

**To execute the sample client**

-   Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          `
    directory, specifying that the request should be a REST request.

    ``` bash
            ant stockquote -Dtrpurl=http://localhost:8280/services/StockQuote -Drest=true
    ```

### Analyzing the output

The request sent by the client is as follows:

``` java
    POST /services/StockQuote HTTP/1.1
    Content-Type: application/xml; charset=UTF-8;action="urn:getQuote";
    SOAPAction: urn:getQuote
    User-Agent: Axis2
    Host: 127.0.0.1
    Transfer-Encoding: chunked
    
    75
    <m0:getQuote xmlns:m0="http://services.samples/xsd">
       <m0:request>
          <m0:symbol>IBM</m0:symbol>
       </m0:request>
    </m0:getQuote>0
```

It is a HTTP REST request, which will be transformed into a SOAP request
and forwarded to the stock quote service.

  
