# Using a WSDL Endpoint as the Target Endpoint

### Introduction

This sample demonstrates how you can use a WSDL endpoint as the target
endpoint. The configuration in this sample uses a WSDL endpoint inside
the send mediator. This WSDL endpoint extracts the target endpoint r
eference from the WSDL document specified in the configuration. In this
configuration the WSDL document is specified as a URI.

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <sequence name="main">
            <in>
                <send>
                    <!-- get epr from the given wsdl -->
                    <endpoint>
                        <wsdl uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"
                              service="SimpleStockQuoteService" port="SimpleStockQuoteServiceHttpSoap11Endpoint"/>
                    </endpoint>
                </send>
            </in>
            <out>
                <send/>
            </out>
        </sequence>
    </definitions>
```

This configuration file `         synapse_sample_56.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start the Axis2 server. For instructions on starting the Axis2
    server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

2.  Deploy the back-end service
    `           SimpleStockQuoteService          ` . For instructions on
    deploying sample back-end services, see [Deploying sample back-end
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
    `           <ESB_HOME>/samples/axis2Client          ` directory.

    ``` bash
            ant stockquote -Dsymbol=IBM -Dmode=quote -Daddurl=http://localhost:8280
    ```

### Analyzing the output

When the client is run, you will see the following output on the client
console:

``` java
    Standard :: Stock price = $95.26454380258552
```

According to `         synapse_sample_56.xml        ` the WSDL endpoint
inside the send mediator extracts the EPR from the WSDL document.
Since WSDL documents can have many services and many ports inside each
service, the service and port of the required endpoint has to be
specified in the configuration via the `         service        ` and
`         port        ` attributes respectively. When it comes to
address endpoints, the QoS parameters for the endpoint can be specified
in the configuration. An excerpt taken from
`         sample_proxy_1.wsdl        ` , which is the WSDL document used
in `         synapse_sample_56.xml        ` is given below.

```
    <wsdl:service name="SimpleStockQuoteService">
       <wsdl:port name="SimpleStockQuoteServiceHttpSoap11Endpoint" binding="ns:SimpleStockQuoteServiceSoap11Binding">
                <soap:address location="http://localhost:9000/services/SimpleStockQuoteService.SimpleStockQuoteServiceHttpSoap11Endpoint"/>
       </wsdl:port>
       <wsdl:port name="SimpleStockQuoteServiceHttpSoap12Endpoint" binding="ns:SimpleStockQuoteServiceSoap12Binding">
                <soap12:address location="http://localhost:9000/services/SimpleStockQuoteService.SimpleStockQuoteServiceHttpSoap12Endpoint"/>
       </wsdl:port>
    </wsdl:service>
```

According to the above WSDL, the service and port specified in the
configuration refers to the endpoint address
`                   http://localhost:9000/services/SimpleStockQuoteService.SimpleStockQuoteServiceHttpSoap11Endpoint                 `
