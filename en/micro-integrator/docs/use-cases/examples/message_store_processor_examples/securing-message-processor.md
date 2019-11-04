# Securing the Message Forwarding Processor
## Synapse configuration
Add security policies to the [Message Forwarding Processor](https://docs.wso2.com/display/EI650/Scheduled+Message+Forwarding+Processor).

```xml tab='Proxy Service'
<proxy xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteProxy" transports="https http" startOnLoad="true" trace="disable">
          <description />
    <target>
       <inSequence>
          <property name="OUT_ONLY" value="true" />
          <store messageStore="MSG_STORE" />
       </inSequence>
       <outSequence>
          <send />
       </outSequence>
    </target>
 </proxy>
```

```xml tab='Local Registry Entry'
<localEntry xmlns="http://ws.apache.org/ns/synapse" key="sec_policy" src="file:/path/to/policy1.xml" />
```

```xml tab='Endpoint'
<endpoint xmlns="http://ws.apache.org/ns/synapse" name="SecureStockQuoteService">
    <address uri="http://localhost:9000/services/SecureStockQuoteService">
       <enableSec policy="sec_policy" />
    </address>
 </endpoint>
```

```xml tab='Message Store'
<messageStore xmlns="http://ws.apache.org/ns/synapse" name="MSG_STORE" />
```

```xml tab='Message Processor'
<messageProcessor xmlns="http://ws.apache.org/ns/synapse" class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor" name="SecureForwardingProcessor" targetEndpoint="SecureStockQuoteService" messageStore="MSG_STORE">
          <parameter name="client.retry.interval">1000</parameter>
          <parameter name="interval">1000</parameter>
          <parameter name="is.active">true</parameter>
</messageProcessor>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. Create the [proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service), [registry resource](../../../../develop/creating-artifacts/creating-registry-resources), [local entry](../../../../develop/creating-artifacts/registry/creating-local-registry-entries), [message store](../../../../develop/creating-artifacts/creating-a-message-store) and [message processor](../../../../develop/creating-artifacts/creating-a-message-processor) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Use the stockquote client to send a request without WS-Security. The Micro Integrator is
configured to enable WS-Security as per the policy specified by
'policy_3.xml' for the outgoing messages to the SecureStockQuoteService
endpoint hosted on the Axis2 instance. The debug log messages on the Micro Integrator
shows the encrypted message flowing to the service and the encrypted
response being received by the Micro Integrator.

The security policy file `policy1.xml` can be downloaded from  [policy1.xml](https://github.com/wso2-docs/WSO2_EI/blob/master/sec-policies/policy1.xml). 
The security policy file uri needs to be updated with the path to the policy1.xml file.
This sample security policy file validates username token and admin role is allowed to invoke the service. 

<!--
You can see the message sent by the Micro Integrator to the secure service using a TCPMon. Upon successful execution, there should be a message on the back end as follows:

```xml
Sun Aug 18 10:58:00 IST 2013 samples.services.SimpleStockQuoteService  :: Accepted order #5 for : 18851 stocks of WSO2 at $ 61.782478265721714
```

Start the SimpleStockQuoteService. When you start the service, you will see the message getting delivered to the service, even though the
service was down when we invoked it from the client. The Main sequence store mediator stores the placeOrder request message in the "MyStore" message store, and the message processor sends the message to the endpoint configured as a message context property. The message processor will remove the message from the store only if the message is delivered successfully.
-->