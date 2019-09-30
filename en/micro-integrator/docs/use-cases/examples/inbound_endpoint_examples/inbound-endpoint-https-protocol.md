# HTTPS Inbound Endpoint Sample

### Introduction

This sample demonstrates how an HTTPS inbound endpoint can act as a
dynamic https listener. Many https listeners can be added without
restarting the server. When a message arrives at a port it will bypass
the inbound side axis2 layer and will be sent directly to the sequence
for mediation.The response also behaves in the same way.

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <inboundEndpoint name="HttpsListenerEP"
                         protocol="https"
                         suspend="false" sequence="TestIn" onError="fault" >
            <p:parameters xmlns:p="http://ws.apache.org/ns/synapse">
                <p:parameter  name="inbound.http.port">8081</p:parameter>
                <p:parameter name="keystore">
                    <KeyStore>
                        <Location>repository/resources/security/wso2carbon.jks</Location>
                        <Type>JKS</Type>
                        <Password>wso2carbon</Password>
                        <KeyPassword>wso2carbon</KeyPassword>
                    </KeyStore>
                </p:parameter>
                <p:parameter name="truststore">
                    <TrustStore>
                        <Location>repository/resources/security/client-truststore.jks</Location>
                        <Type>JKS</Type>
                        <Password>wso2carbon</Password>
                    </TrustStore>
                </p:parameter>
            </p:parameters>
        </inboundEndpoint>
        <sequence xmlns="http://ws.apache.org/ns/synapse" name="TestIn">
            <send receive="reciveSeq">
                <endpoint>
                    <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                </endpoint>
            </send>
        </sequence>
        <sequence xmlns="http://ws.apache.org/ns/synapse" name="reciveSeq">
            <send/>
        </sequence>
    </definitions>
```

This configuration file `         synapse_sample_903.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start the ESB with the sample 902 configuration. For instructions on
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
    `           <ESB_HOME>/samples/axis2Client          ` directory, to
    execute the **Stock Quote Client** in the **Dumb Client Mode** .

    ``` bash
            ant stockquote -Dtrpurl=https://localhost:8081/
    ```

### Analyzing the output

Analyze the output debug messages for the actions in the Dumb Client
Mode.

You will see the ESB receiving a message for which the ESB Inbound is
set as the ultimate receiver. You will also see the response from the
back end in the Client.
