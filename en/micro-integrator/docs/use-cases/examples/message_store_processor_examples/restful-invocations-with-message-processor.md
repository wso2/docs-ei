# RESTful Invocations with Message Forwarding Processor

Invoke REST messages with a plain-old XML (POX) back end
using the message store and [Message Forwarding
Processor](https://docs.wso2.com/display/EI650/Scheduled+Message+Forwarding+Processor)
.

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
       <registry provider="org.wso2.carbon.mediation.registry.WSO2Registry">
          <parameter name="cachableDuration">15000</parameter>
       </registry>
       <proxy name="StockQuoteProxy" transports="https http" startOnLoad="true" trace="disable">
          <target>
             <inSequence>
                <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2" />
                <property name="OUT_ONLY" value="true" />
                <property name="messageType" value="application/xml" scope="axis2" />
                <store messageStore="MSG_STORE" />
             </inSequence>
             <outSequence>
                <send />
             </outSequence>
          </target>
       </proxy>
       <endpoint name="SecureStockQuoteService">
          <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="rest" />
       </endpoint>
       <sequence name="fault">
          <log level="full">
             <property name="MESSAGE" value="Executing default 'fault' sequence" />
             <property name="ERROR_CODE" expression="get-property('ERROR_CODE')" />
             <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')" />
          </log>
          <drop />
       </sequence>
       <sequence name="main">
          <in>
             <log level="full" />
             <filter source="get-property('To')" regex="http://localhost:9000.*">
                <send />
             </filter>
          </in>
          <out>
             <send />
          </out>
          <description>The main sequence for the message mediation</description>
       </sequence>
       <messageStore name="MSG_STORE" />
       <messageProcessor class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor" name="SecureForwardingProcessor" targetEndpoint="SecureStockQuoteService" messageStore="MSG_STORE">
          <parameter name="client.retry.interval">1000</parameter>
          <parameter name="interval">1000</parameter>
          <parameter name="is.active">true</parameter>
       </messageProcessor>
    </definitions>
```

**Prerequisites**

-   Start the Synapse configuration numbered 704, e.g.,
    `          wso2esb-samples.sh -sn 704         `
-   Start the Axis2 server and deploy the SecureStockQuoteService if you
    have not done so already.

This sample demonstrate how to do RESTful invocations with the message
store and message forwarding processor. Though in this sample we have
used an In memory message store, these instructions are valid for any
available message store implementations. Use the stockqoute client to
send the following placeorder request:

``` java
    ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy -Dmode=placeorder -Dsymbol=WSO2
```

You can see the message sent by the ESB to the secure service using a
TCPMon. Upon successful execution, there should be a message on the back
end as follows:

``` java
    Sun Aug 18 10:58:00 IST 2013 samples.services.SimpleStockQuoteService  :: Accepted order #5 for : 18851 stocks of WSO2 at $ 61.782478265721714
```
