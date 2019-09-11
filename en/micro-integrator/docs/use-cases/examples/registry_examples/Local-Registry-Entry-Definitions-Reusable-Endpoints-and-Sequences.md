# Local Registry Entry Definitions, Reusable Endpoints and Sequences

### Introduction

This sample demonstrates the functionality of local registry entry
definitions, reusable endpoints and sequences.

### Prerequisites

For a list of prerequisites, see [Prerequisites to start
samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

``` 
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <!-- define a string resource entry to the local registry -->
        <localEntry key="version">0.1</localEntry>
        <!-- define a reuseable endpoint definition -->
        <endpoint name="simple">
            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
        </endpoint>
        <!-- define a reusable sequence -->
        <sequence name="stockquote">
            <!-- log the message using the custom log level. illustrates custom properties for log -->
            <log level="custom">
                <property name="Text" value="Sending quote request"/>
                <property name="version" expression="get-property('version')"/>
                <property name="direction" expression="get-property('direction')"/>
            </log>
            <!-- send message to real endpoint referenced by key "simple" endpoint definition -->
            <send>
                <endpoint key="simple"/>
            </send>
        </sequence>
        <sequence name="main">
            <in>
                <property name="direction" value="incoming"/>
                <sequence key="stockquote"/>
            </in>
            <out>
                <send/>
            </out>
        </sequence>
    </definitions>
```

This configuration file `         synapse_sample_3.xml        ` is
available in the `         <EI_HOME>/samples/service-bus        `
directory.

**To build the sample**

1.  Start the Axis2 server.For instructions on starting the Axis2
    server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

2.  Deploy the back-end service **SimpleStockQuoteService** . For
    instructions on deploying sample back-end services, see [Deploying
    sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

    Now you have a running WSO2 EI instance and a back-end service
    deployed. In the next section, we will send a message to the
    back-end service through WSO2 EI using a sample client.

### Executing the sample

This example uses a sequence named *main* that specifies the main
mediation rules to be executed. This is equivalent to directly
specifying the mediators of the main sequence within the \<
`         definitions        ` \> tag. This sample scenario is a
recommended approach for non-trivial configurations.

The sample client used here is the **Stock Quote Client** , which can
operate in several modes. For further details on this sample client and
its operation modes, see [Stock Quote
Client](https://docs.wso2.com/display/EI650/Using+the+Sample+Clients#UsingtheSampleClients-StockQuoteClient)
.

**To execute the sample client**

-   Run the following command from the
    `           <EI_HOME>/samples/axis2Client          ` directory, to
    trigger a sample message to the back-end service.

    ``` bash
            ant stockquote -Daddurl=http://localhost:9000/services/SimpleStockQuoteService -Dtrpurl=http://localhost:8280/
    ```

### Analyzing the output

Analyze the mediation log on the WSO2 EI start-up console.

You will see that a sequence named *main* is executed. Then, for the
incoming message flow, the In Mediator executes and it calls the
sequence named *stockquote* .

``` java
    DEBUG SequenceMediator - Sequence mediator <main> :: mediate()
    DEBUG InMediator - In mediator mediate()
    DEBUG SequenceMediator - Sequence mediator <stockquote> :: mediate()
```

You will also see that the *stockquote* sequence is executed. and that
the l og mediator dumps a simple *text/string* property which is the
result of an XPath evaluation, that picks up the key named *version* ,
and a second result of an XPath evaluation that picks up a local message
property set previously by the p roperty mediator . The
`         get-property()        ` XPath extension function is able to
read message properties local to the current message, local or remote
registry entries, Axis2 message context properties as well as transport
headers. The local entry definition for *version* defines a simple
*text/string* registry entry, which is visible to all messages that pass
through WSO2 EI.

``` java
    [HttpServerWorker-1] INFO LogMediator - Text = Sending quote request, version = 0.1, direction = incoming
    [HttpServerWorker-1] DEBUG SendMediator - Send mediator :: mediate()
    [HttpServerWorker-1] DEBUG AddressEndpoint - Sending To: http://localhost:9000/services/SimpleStockQuoteService
```
