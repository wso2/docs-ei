# Using Sequences and Endpoints as Local Registry Items

### Introduction

This sample demonstrates how sequences and endpoints can be fetched from
a local registry so that it is possible to have the sequences and
endpoints as local registry entries including file entries.

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

``` 
    <definitions xmlns="http://ws.apache.org/ns/synapse"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://ws.apache.org/ns/synapse http://synapse.apache.org/ns/2010/04/configuration/synapse_config.xsd">
        <localEntry key="local-enrty-ep-key"
                    src="file:repository/samples/resources/endpoint/dynamic_endpt_1.xml"/>
        <localEntry key="local-enrty-sequence-key">
            <sequence name="dynamic_sequence">
                <log level="custom">
                    <property name="message" value="*** Test Message 1 ***"/>
                </log>
            </sequence>
            <description/>
        </localEntry>
        <sequence name="main">
            <in>
                <sequence key="local-enrty-sequence-key"/>
                <send>
                    <endpoint key="local-enrty-ep-key"/>
                </send>
            </in>
            <out>
                <send/>
            </out>
        </sequence>
    </definitions>
```

This configuration file `         synapse_sample_14.xml        ` is
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
            ant stockquote -Dtrpurl=http://localhost:8280/
    ```

### Analyzing the output

When you analyze the debug log output, you will see that the log
statement for the fetched sequence of the local entry and the endpoint
is fetched from the specified file at runtime and is cached in the
system.