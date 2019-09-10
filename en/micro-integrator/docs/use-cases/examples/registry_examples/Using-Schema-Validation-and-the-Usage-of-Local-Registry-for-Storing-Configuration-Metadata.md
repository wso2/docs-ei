# Using Schema Validation and the Usage of Local Registry for Storing Configuration Metadata

### Introduction

This sample demonstrates how to use the [Validate
Mediator](https://docs.wso2.com/display/EI650/Validate+Mediator) for XML
schema validation and the usage of the local registry (local entries)
for storing configuration metadata.  Here a message is sent from the
sample client to the back-end service through the ESB and shows how a
static XML fragment could be made available to the ESB's local registry.
It is assumed that the resources defined in the local registry are
static (never changes over the lifetime of the configuration) and may be
specified as a source URL, inline text or inline XML.

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

``` 
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <localEntry key="validate_schema">
            <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://services.samples"
                       elementFormDefault="qualified" attributeFormDefault="unqualified"
                       targetNamespace="http://services.samples">
                <xs:element name="getQuote">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="request">
                                <xs:complexType>
                                    <xs:sequence>
                                        <xs:element name="stocksymbol" type="xs:string"/>
                                    </xs:sequence>
                                </xs:complexType>
                            </xs:element>
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>
            </xs:schema>
        </localEntry>
        <sequence name="main">
            <in>
                <validate>
                    <schema key="validate_schema"/>
                    <on-fail>
                        <!-- if the request does not validate againt schema throw a fault -->
                        <makefault>
                            <code value="tns:Receiver" xmlns:tns="http://www.w3.org/2003/05/soap-envelope"/>
                            <reason value="Invalid custom quote request"/>
                        </makefault>
                    </on-fail>
                </validate>
            </in>
            <send/>
        </sequence>
    </definitions>
```

This configuration file `         synapse_sample_7.xml        ` is
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
`         synapse_sample_7.xml        ` , the schema is made available
under the *validate\_schema* key. The [Validate
Mediator](https://docs.wso2.com/display/EI650/Validate+Mediator) by
default operates on the first child element of the SOAP body. You can
specify an XPath expression using the `         source        `
attribute to override this behaviour. Here, the Validate Mediator uses
the *validate\_schema* resource to validate the incoming message and if
the message validation fails, it invokes the *on-fail* sequence of
mediators.

**To execute the sample client**

-   Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          ` directory.

    ``` bash
            ant stockquote -Daddurl=http://localhost:9000/services/SimpleStockQuoteService -Dtrpurl=http://localhost:8280/
    ```

### Analyzing the output

When you send the stock quote request,  the schema validation fails and
a fault is generated back with the message *Invalid custom quote
request* . This is because the schema used in the example expects a
slightly different message than what is created by the stock quote
client. The schema used in the example expects a
`         stocksymbol        ` element instead of
`         symbol        ` to specify the stock symbol.
