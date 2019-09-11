# Routing Messages that Arrive to a Proxy Service without Processing Security Headers

## Introduction

This sample demonstrates how you can route messages that arrive to a
proxy service without processing the `         MustUnderstand        `
headers.

In this sample the proxy service will receive a secure message with the
`         MustUnderstand        ` header. Since the element
`         enableSec        ` is not present in the proxy configuration,
the ESB will not engage Apache Rampart on this proxy service. It is
expected that a `         MustUnderstand        ` failure exception
should occur at the `         AxisEngine        ` before the message
reaches the ESB, but here since the ESB handles this message and gets it
in by setting all the headers that are `         MustUnderstand        `
and not processed to the processed state, this will enable the ESB to
route the messages without processing the security headers.

## Prerequisites

-   For a list of general prerequisites, see [Prerequisites to start ESB
    samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
    .
-   This sample uses Apache Rampart as the back-end security
    implementation. Therefore, you need to download and install the
    unlimited strength policy files for your JDK before using Apache
    Rampart. Follow the steps below to download and install the
    unlimited strength policy files:
    1.  Go to
        <http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html>
        , and download the unlimited strength JCE policy files for your
        JDK version.

    2.  Uncompress and extract the downloaded ZIP file. This creates a
        directory named JCE that contains the
        `            local_policy.jar           ` and
        `            US_export_policy.jar           ` files.
    3.  In your Java installation directory, go to the
        `            jre/lib/security           ` directory, and make a
        copy of the existing `            local_policy.jar           `
        and `            US_export_policy.jar           ` files. Next,
        replace the original policy files with the policy files that you
        extracted in the previous step.

## Building the sample

The XML configuration for this sample is as follows:

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <proxy name="StockQuoteProxy">
            <target>
                <inSequence>
                  <property name="preserveProcessedHeaders" value="true"/>
                    <send>
                        <endpoint>
                            <address uri="http://localhost:9000/services/SecureStockQuoteService"/>
                        </endpoint>
                    </send>
                </inSequence>
                <outSequence>
                    <send/>
                </outSequence>
            </target>
            <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
        </proxy>
    </definitions>
```

This configuration file `         synapse_sample_153.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start the Axis2 server. For instructions on starting the Axis2
    server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

2.  Deploy the back-end service
    `           SecureStockQuoteService          ` . For instructions on
    deploying sample back-end services, see [Deploying sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

  

!!! Info
    When you run this sample, the `          bouncyCastle jar         ` file
that is used for encryption does not load into the axis2 client. This is
due to an issue with the axis2Client shipped with WSO2 Enterprise
Integrator. Therefore, before running the client, you need to copy the
`          bcprov-jdk15on.jar         ` file from the
`          <EI_HOME>/repository/axis2/client/lib         ` directory to
the `          <EI_HOME>/wso2/components/plugins         ` directory.


## Executing the sample

The sample client used here is the **Stock Quote Client** , which can
operate in several modes. For further details on this sample client and
its operation modes, see [Stock Quote
Client](https://docs.wso2.org/enterprise-service-bus/Using+the+Sample+Clients#UsingtheSampleClients-StockQuoteClient)
.

**To execute the sample client**

-   Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          ` directory.

    ``` bash
        ant stockquote -Dtrpurl=http://localhost:8280/services/StockQuoteProxy -Dpolicy=./../../repository/samples/resources/policy/client_policy_3.xml
    ```

    This sends a stock quote request to the proxy service and also signs
    and encrypts the request by specifying the client side security
    policy.

## Analyzing the output

By analyzing the debug log output or the TCPMon output, you will see
that the request received by the proxy service is signed and encrypted.

You can look up the WSDL of the proxy service by requesting the URL
<http://localhost:8280/services/StockQuoteProxy?wsdl> , in order to
confirm that the security policy attachments are not available and that
security is not engaged.

When sending the message to the backend service, you can verify that the
security headers were present as in the original message to the ESB from
the client, and that the response received does use WS-Security and
forwards the message back to the client without any modification. Since
the message inside the ESB is signed and encrypted and can only be
forwarded to a secure service, you will see that this is not a security
loophole.
