# Introduction to Dynamic and Static Registry Keys

### Introduction

This Sample demonstrates the use of dynamic keys with mediators. Here
the [XSLT Mediator](https://docs.wso2.com/display/EI650/XSLT+Mediator)
is used to demonstrate the difference between the static and dynamic
usage of keys.

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <!-- the SimpleURLRegistry allows access to a URL based registry (e.g. file:/// or http://) -->
        <registry provider="org.wso2.carbon.mediation.registry.ESBRegistry">
            <!-- the root property of the simple URL registry helps resolve a resource URL as root + key -->
            <parameter name="root">file:repository/samples/resources/</parameter>
            <!-- all resources loaded from the URL registry would be cached for this number of milli seconds -->
            <parameter name="cachableDuration">15000</parameter>
        </registry>
        <sequence name="main">
            <in>
                <!-- define the request processing XSLT resource as a property value -->
                <property name="symbol" value="transform/transform.xslt"/>
                <!-- {} denotes that this key is a dynamic one and it is not a static key -->
                <!-- use Xpath expression "get-property()" to evaluate real key from property -->
                <xslt key="{get-property('symbol')}"/>
            </in>
            <out>
                <!-- transform the standard response back into the custom format the client expects -->
                <!-- the key is looked up in the remote registry using a static key -->
                <xslt key="transform/transform_back.xslt"/>
            </out>
            <send/>
        </sequence>
    </definitions>
```

This configuration file `         synapse_sample_16.xml        ` is
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

According to the configuration file
`         synapse_sample_16.xml,        ` the first registry resource
`         transform/transform.xslt        ` is set as a property value.

Inside the XSLT mediator, the local property value is looked up using
the Xpath expression `         get-property()        ` . Similarly, any
XPath expression can be enclosed within curly braces to denote that it
is a dynamic key. Then the mediator evaluates the real value for that
expression.

The second XSLT resource
`         transform/transform_back.xslt        ` is simply used as a
static key. It is not included within curly braces since the mediator
directly uses the static value as the key.

**To execute the sample client**

-   Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          ` directory.

    ``` bash
            ant stockquote -Daddurl=http://localhost:9000/services/SimpleStockQuoteService -Dtrpurl=http://localhost:8280/ -Dmode=customquote
    ```

### Analyzing the output

When you analyze the debug log output on the ESB console, you will see
an output similar to that of [Sample 8: Introduction to Static and
Dynamic Registry Resources and Using XSLT
Transformations](https://docs.wso2.com/display/EI610/Sample+8%3A+Introduction+to+Static+and+Dynamic+Registry+Resources+and+Using+XSLT+Transformations)
.

!!! Info
    You can try this sample with different local entries as the source with
the correct target XPath values.

