# Sample 10: Introduction to Dynamic Endpoints with the Registry

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


-   [Introduction](#Sample10:IntroductiontoDynamicEndpointswiththeRegistry-Introduction)
-   [Prerequisites](#Sample10:IntroductiontoDynamicEndpointswiththeRegistry-Prerequisites)
-   [Building the
    sample](#Sample10:IntroductiontoDynamicEndpointswiththeRegistry-Buildingthesample)
-   [Executing the
    sample](#Sample10:IntroductiontoDynamicEndpointswiththeRegistry-Executingthesample)
-   [Analyzing the
    output](#Sample10:IntroductiontoDynamicEndpointswiththeRegistry-Analyzingtheoutput)

### Introduction

This sample demonstrates the functionality of dynamic endpoints.
Here you store your endpoints in the registry and refer to them.

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

``` html/xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <registry provider="org.wso2.carbon.mediation.registry.ESBRegistry">
            <parameter name="root">file:repository/samples/resources/</parameter>
            <parameter name="cachableDuration">15000</parameter>
        </registry>
        <sequence name="main">
            <in>
                <send>
                    <endpoint key="endpoint/dynamic_endpt_1.xml"/>
                </send>
            </in>
            <out>
                <send/>
            </out>
        </sequence>
    </definitions>
```

This configuration file `         synapse_sample_10.xml        ` is
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

3.  Run the following command to start a second Axis2 server on HTTP
    port 9001 and HTTPS port 9003.

    ``` bash
            ./axis2server.sh -http 9001 -https 9003
    ```

### Executing the sample

The sample client used here is the **Stock Quote Client** , which can
operate in several modes. For further details on this sample client and
its operation modes, see [Stock Quote
Client](https://docs.wso2.com/display/EI650/Using+the+Sample+Clients#UsingtheSampleClients-StockQuoteClient)
.

**To execute the sample client**

1.  Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          ` directory.

    ``` bash
            ant stockquote -Dtrpurl=http://localhost:8280/
    ```

2.  Execute the client immediately again (within 15 seconds of the last
    execution) using the above command.
3.  Edit the endpoint definition in
    `          <ESB_HOME>/repository/samples/resources/endpoint/dynamic_endpt_1.xml         `
    by changing the endpoint address to
    `                                 http://localhost:9001/services/SimpleStockQuoteService                              `
    .  
      

### Analyzing the output

When you execute the client for the first time, the message is routed to
the SimpleStockQuoteService on the default Axis2 instance on HTTP port
9000.

When you execute the client immediately again,  you will see that the
endpoint is cached and reused by the ESB. A similar scenario is
explained in [Sample 9: Introduction to Dynamic Sequences with the
Registry](https://docs.wso2.com/display/EI610/Sample+9%3A+Introduction+to+Dynamic+Sequences+with+the+Registry)
.

When you edit the endpoint definition and execute the client again, you
will see that the registry loads the new definition of the endpoint once
the cache expires.

Once the registry loads the new definition of the endpoint,  you will
see that the messages are routed to the second sample Axis2 server on
HTTP port 9001.
