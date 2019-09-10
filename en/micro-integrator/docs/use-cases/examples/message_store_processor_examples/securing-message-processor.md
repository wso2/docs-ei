# Adding Security to Message Forwarding Processor

Add security policies to the [Message Forwarding
Processor](https://docs.wso2.com/display/EI650/Scheduled+Message+Forwarding+Processor)
.

``` 
    <definitions xmlns="http://ws.apache.org/ns/synapse">
       <registry provider="org.wso2.carbon.mediation.registry.WSO2Registry">
          <parameter name="cachableDuration">15000</parameter>
       </registry>
       <proxy name="StockQuoteProxy" transports="https http" startOnLoad="true" trace="disable">
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
       <localEntry key="sec_policy" src="file:repository/samples/resources/policy/policy_3.xml" />
       <endpoint name="SecureStockQuoteService">
          <address uri="http://localhost:9000/services/SecureStockQuoteService">
             <enableSec policy="sec_policy" />
          </address>
       </endpoint>
       <messageStore name="MSG_STORE" />
       <messageProcessor class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor" name="SecureForwardingProcessor" targetEndpoint="SecureStockQuoteService" messageStore="MSG_STORE">
          <parameter name="client.retry.interval">1000</parameter>
          <parameter name="interval">1000</parameter>
          <parameter name="is.active">true</parameter>
       </messageProcessor>
    </definitions>
```

**Prerequisites**

-   Start the Synapse configuration numbered 703, e.g.,
    `          wso2ei-samples.sh -sn 703         `
-   Start the Axis2 server and deploy the SecureStockQuoteService if you
    have not done so already.

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

Use the stockquote client to send a request without WS-Security. EI is
configured to enable WS-Security as per the policy specified by
'policy\_3.xml' for the outgoing messages to the SecureStockQuoteService
endpoint hosted on the Axis2 instance. The debug log messages on EI
shows the encrypted message flowing to the service and the encrypted
response being received by EI.

``` java
    ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy -Dmode=placeorder -Dsymbol=WSO2
```

You can see the message sent by the EI to the secure service using a
TCPMon. Upon successful execution, there should be a message on the back
end as follows:

``` java
    Sun Aug 18 10:58:00 IST 2013 samples.services.SimpleStockQuoteService  :: Accepted order #5 for : 18851 stocks of WSO2 at $ 61.782478265721714
```

Start the SimpleStockQuoteService. When you start the service, you will
see the message getting delivered to the service, even though the
service was down when we invoked it from the client. The Main sequence
store mediator stores the placeOrder request message in the "MyStore"
message store, and the message processor sends the message to the
endpoint configured as a message context property. The message processor
will remove the message from the store only if the message is delivered
successfully.
