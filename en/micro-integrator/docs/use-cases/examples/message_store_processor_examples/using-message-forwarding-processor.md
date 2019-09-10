# Introduction to Message Forwarding Processor

Introduction to Message Forwarding Processor.

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <taskManager provider="org.wso2.carbon.mediation.ntask.NTaskTaskManager"/>
        <endpoint name="StockQuoteServiceEp">
            <address uri="http://localhost:9000/services/SimpleStockQuoteService">
                <suspendOnFailure>
                    <errorCodes>-1</errorCodes>
                    <progressionFactor>1.0</progressionFactor>
                </suspendOnFailure>
            </address>
        </endpoint>
        <sequence name="fault">
            <log level="full">
                <property name="MESSAGE" value="Executing default 'fault' sequence"/>
                <property expression="get-property('ERROR_CODE')" name="ERROR_CODE"/>
                <property expression="get-property('ERROR_MESSAGE')" name="ERROR_MESSAGE"/>
            </log>
            <drop/>
        </sequence>
        <sequence name="main">
            <in>
                <log level="full"/>
                <property name="FORCE_SC_ACCEPTED" scope="axis2" value="true"/>
                <property name="OUT_ONLY" value="true"/>
                <store messageStore="MyStore"/>
            </in>
            <description>The main sequence for the message mediation</description>
        </sequence>
        <messageStore name="MyStore"/>
        <messageProcessor
            class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor"
            messageStore="MyStore" name="ScheduledProcessor" targetEndpoint="StockQuoteServiceEp">
            <parameter name="interval">10000</parameter>
            <parameter name="throttle">false</parameter>
            <parameter name="target.endpoint">StockQuoteServiceEp</parameter>
        </messageProcessor>
    </definitions>
```

**Prerequisites**:

-   Start the configuration numbered 702: i.e. wso2esb-samples -sn 702

To Execute the Client:

``` java
    ant stockquote -Daddurl=http://localhost:9000/services/SimpleStockQuoteService -Dtrpurl=http://localhost:8280/ -Dmode=placeorder
```

Now Start the SimpleStockQuoteService. When you Start the service you
will see message getting delivered to the service. Even though service
is down when we invoke it from the client. Here in the Main sequence
store mediator will store the placeOrder request message in the
"MyStore" Message Store. Message Processor will send the message to the
endpoint configured as a message context property. Message processor
will remove the message from the store only if message delivered
successfully.
