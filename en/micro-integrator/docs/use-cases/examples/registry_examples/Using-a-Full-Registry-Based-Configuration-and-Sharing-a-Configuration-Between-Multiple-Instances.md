# Using a Full Registry-Based Configuration and Sharing a Configuration Between Multiple Instances

### Introduction

This sample d emonstrates the use of a full registry-based ESB
configuration, to start a remote configuration from multiple ESB
instances in a clustered environment. Here the Synapse configuration of
a given node hosting the ESB simply points to the registry and looks up
the actual configuration by requesting the key
`         synapse.xml        ` .

!!! Info
    Full registry-based configuration is not dynamic at the moment; it is
not reloading itself.


### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <registry provider="org.wso2.carbon.mediation.registry.ESBRegistry">
            <parameter name="root">file:./repository/samples/resources/</parameter>
            <parameter name="cachableDuration">15000</parameter>
        </registry>
    </definitions>
```

This configuration file `         synapse_sample_11.xml        ` is
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

The sample client used here is the **Stock Quote Client** , which can
operate in several modes. For further details on this sample client and
its operation modes, see [Stock Quote
Client](https://docs.wso2.com/display/EI650/Using+the+Sample+Clients#UsingtheSampleClients-StockQuoteClient)
.

**To execute the sample client**

-   Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          ` directory.

    ``` bash
            ant stockquote -Daddurl=http://localhost:9000/services/SimpleStockQuoteService -Dtrpurl=http://localhost:8280/
    ```

### Analyzing the output

When you a nalyze the debug log output on the ESB console once the
client is executed, you will see the following:

``` java
    [HttpServerWorker-1] INFO LogMediator - message = This is a dynamic ESB configuration
```

The actual `         synapse.xml        ` that is loaded when the sample
is run is:

```
    <!-- a registry based Synapse configuration -->
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <sequence name="main">
            <log level="custom">
                <property name="message" value="This is a dynamic ESB configuration"/>
            </log>
            <send/>
        </sequence>
    </definitions>
```
