# Using the Message Sampling Processor
## Synapse configuration

Shown below are the synapse artifacts that are used to define this use case.

```xml tab='Send Sequence'
<sequence name="send_seq">
    <send>
        <endpoint>
            <address uri="http://localhost:9000/services/SimpleStockQuoteService">
                <suspendOnFailure>
                <errorCodes>-1</errorCodes>
                <progressionFactor>1.0</progressionFactor>
                </suspendOnFailure>
            </address>
        </endpoint>
    </send>
</sequence>           
```

```xml tab='Main Sequence'
<sequence name="main">
    <in>
        <log level="full"/>
        <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
        <property name="OUT_ONLY" value="true"/>
        <store messageStore="MyStore"/>
    </in>
    <out/>
    <description>The main sequence for the message mediation</description>
</sequence>
```

```xml tab='Message Store'
<messageStore name="MyStore"/>
```

```xml tab='Message Processor'
<messageProcessor
     class="org.apache.synapse.message.processor.impl.sampler.SamplingProcessor"
     name="SamplingProcessor" messageStore="MyStore">
    <parameter name="interval">20000</parameter>
    <parameter name="sequence">send_seq</parameter>
</messageProcessor> 
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. Create the [mediation sequences](../../../../develop/creating-artifacts/creating-reusable-sequences), [message store](../../../../develop/creating-artifacts/creating-a-message-store), and [message processor](../../../../develop/creating-artifacts/creating-a-message-processor) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

To Execute the Client few times:

```bash
ant stockquote -Daddurl=http://localhost:9000/services/SimpleStockQuoteService -Dtrpurl=http://localhost:8280/ -Dmode=placeorder
```

When you execute the client, the message will be dispatched to the main sequence. In the Main sequence, store mediator will store the placeOrder request message in the "MyStore" Message Store. Message Processor will consume the messages, and forward to the "send_seq" sequence in configured rate. You will observe that the service invocation rate is not changing when we increase the rate of client executions.
