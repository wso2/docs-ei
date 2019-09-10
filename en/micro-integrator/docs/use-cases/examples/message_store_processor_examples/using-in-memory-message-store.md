# Sample 700: Introduction to Message Store

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


-   [Introduction](#Sample700:IntroductiontoMessageStore-Introduction)
-   [Prerequisites](#Sample700:IntroductiontoMessageStore-Prerequisites)
-   [Building the
    sample](#Sample700:IntroductiontoMessageStore-Buildingthesample)
-   [Executing the
    sample](#Sample700:IntroductiontoMessageStore-Executingthesample)
-   [Analyzing the
    output](#Sample700:IntroductiontoMessageStore-Analyzingtheoutput)

### Introduction

This sample demonstrates the basic functionality of a [message
store](https://docs.wso2.com/display/EI650/Message+Stores) .

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

``` html/xml
    <!-- Introduction to the Synapse Message Store -->
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <sequence name="fault">
            <log level="full">
                <property name="MESSAGE" value="Executing default 'fault' sequence"/>
                <property name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
                <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
            </log>
            <drop/>
        </sequence>
        <sequence name="onStoreSequence">
            <log>
                <property name="On-Store" value="Storing message"/>
            </log>
        </sequence>
        <sequence name="main">
            <in>
                <log level="full"/>
                <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
                <store messageStore="MyStore" sequence="onStoreSequence"/>
            </in>
            <description>The main sequence for the message mediation</description>
        </sequence>
        <messageStore name="MyStore"/>
    </definitions>
```

This configuration file `         synapse_sample_700.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start the ESB with the sample 700 configuration. For instructions on
    starting a sample ESB configuration, see [Starting the ESB with a
    sample
    configuration](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Startingasample)
    .  
      
    The operation log keeps running until the server starts, which
    usually takes several seconds. Wait until the server has fully
    booted up and displays a message similar to " *WSO2 Carbon started
    in n seconds.* "

2.  Start the Axis2 server. For instructions on starting the Axis2
    server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

3.  Deploy the back-end service **SimpleStockQuoteService** . For
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
    `           <ESB_HOME>/samples/axis2Client          ` directory:

    ``` bash
            ant stockquote -Daddurl=http://localhost:9000/services/SimpleStockQuoteService -Dtrpurl=http://localhost:8280/ -Dmode=placeorder
    ```

### Analyzing the output

When you execute the client you will see that the message is dispatched
to the main sequence.

In the main sequence, the store mediator will store the
`         placeorder        ` request message in the
`         MyStore        ` message store. Before storing the request,
the message store mediator will invoke the
`         onStoreSequence        ` sequence.

Analyze the output debug messages and you will see the following log:

``` java
    INFO - LogMediator To: http://localhost:9000/services/SimpleStockQuoteService, WSAction: urn:placeOrder, SOAPAction: urn:placeOrder, ReplyTo: http://www.w3.org/2005/08/addressing/none, MessageID: urn:uuid:54f0e7c6-7b43-437c-837e-a825d819688c, Direction: request, On-Store = Storing message
```

You can then use the JMX view of the Synapse message store by using the
jconsole in order to view the stored messages and delete them.
