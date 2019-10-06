# Using the Message Forwarding Processor
## Example use case

## Synapse configuration
Shown below are the synapse artifacts that are used to define this use case.

```xml tab='Scheduled Task'
<taskManager provider="org.wso2.carbon.mediation.ntask.NTaskTaskManager"/>
```

```xml tab='Endpoint'
<endpoint name="StockQuoteServiceEp">
    <address uri="http://localhost:9000/services/SimpleStockQuoteService">
        <suspendOnFailure>
            <errorCodes>-1</errorCodes>
            <progressionFactor>1.0</progressionFactor>
        </suspendOnFailure>
    </address>
</endpoint>
```

```xml tab='Main Sequence'
<sequence name="main">
    <in>
        <log level="full"/>
        <property name="FORCE_SC_ACCEPTED" scope="axis2" value="true"/>
        <property name="OUT_ONLY" value="true"/>
        <store messageStore="MyStore"/>
    </in>
    <description>The main sequence for the message mediation</description>
</sequence>
```

```xml tab='Fault Sequence'
<sequence name="fault">
    <log level="full">
        <property name="MESSAGE" value="Executing default 'fault' sequence"/>
        <property expression="get-property('ERROR_CODE')" name="ERROR_CODE"/>
        <property expression="get-property('ERROR_MESSAGE')" name="ERROR_MESSAGE"/>
    </log>
    <drop/>
</sequence>
```

```xml tab='Message Store'
<messageStore name="MyStore"/>
```

```xml tab='Message Processor'
<messageProcessor
    class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor"
    messageStore="MyStore" name="ScheduledProcessor" targetEndpoint="StockQuoteServiceEp">
    <parameter name="interval">10000</parameter>
    <parameter name="throttle">false</parameter>
    <parameter name="target.endpoint">StockQuoteServiceEp</parameter>
</messageProcessor>
```

## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create integration artifacts with the above configuration.
4. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

........


Configure the Micro Integrator with Apache ActiveMQ and set up the JMS Sender.

Invoke the service:

```xml
ant stockquote -Daddurl=http://localhost:9000/services/SimpleStockQuoteService -Dtrpurl=http://localhost:8280/ -Dmode=placeorder
```

Now Start the SimpleStockQuoteService. When you Start the service you will see message getting delivered to the service. Even though service is down when we invoke it from the client. Here in the Main sequence store mediator will store the placeOrder request message in the "MyStore" Message Store. Message Processor will send the message to the endpoint configured as a message context property. Message processor will remove the message from the store only if message delivered successfully.
