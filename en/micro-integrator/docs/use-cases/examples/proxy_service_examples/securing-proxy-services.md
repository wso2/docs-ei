# Securing Proxy Services
## Example use case

This sample demonstrates how you can use WS-Security signing and encryption with proxy services through WS-Policy.

In this sample the proxy service expects to receive a signed and encrypted message as specified by the security policy. To understand the format of the policy file, have a look at the Apache Rampart and Axis2 documentation. The element   engageSec  specifies that Apache Rampart should be engaged on this proxy service. Hence if Rampart rejects any request message that does not conform to the specified policy, that message will never reach the   inSequence  in order to be processed. Since the proxy service is forwarding the received request to the simple stock quote service that does not use WS-Security, you are instructing the ESB to remove the wsse:Security header from the outgoing message.

## Synapse configuration

```xml tab='Proxy Service'
<proxy name="StockQuoteProxy">
    <target>
        <inSequence>
            <header name="wsse:Security" action="remove"
                    xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"/>
            <send>
                <endpoint>
                    <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence>
            <send/>
        </outSequence>
    </target>
    <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
    <policy key="sec_policy"/>
    <enableSec/>
</proxy>
```

```xml tab='Local Entry'
<localEntry key="sec_policy" src="file:repository/samples/resources/policy/policy_3.xml"/>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. Create the [proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service) and [security policy](../../../../develop/creating-artifacts/registry/creating-local-registry-entries) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Set up the back-end service.

By analyzing the debug log output or the TCPMon output, you will see that the request received by the proxy service is signed and encrypted.

You can look up the WSDL of the proxy service by requesting the URL http://localhost:8280/services/StockQuoteProxy?wsdl , in order to confirm the security policy attachment to the supplied base WSDL.

When sending the message to the backend service, you can verify that the security headers were removed and that the response received does not use WS-Security, but that the response being forwarded back to the client is signed and encrypted as expected by the client.