# Load Balancing with Message Forwarding Processor
## Synapse configuration

The XML configuration for this sample is as follows:

```xml tab='Registry Resource'
 <registry provider="org.wso2.carbon.mediation.registry.WSO2Registry">
    <parameter name="cachableDuration">15000</parameter>
 </registry>
```

```xml tab='Scheduled Task'
<taskManager provider="org.wso2.carbon.mediation.ntask.NTaskTaskManager"/>
```

```xml tab='Proxy Service'
<proxy name="StockQuoteProxy"
              transports="https http"
              startOnLoad="true">
    <description/>
    <target>
       <inSequence>
          <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
          <property name="OUT_ONLY" value="true"/>
          <store messageStore="JMSMS"/>
       </inSequence>
       <outSequence/>
       <faultSequence/>
    </target>
 </proxy>
```

```xml tab='Endpoint 1'
<endpoint name="SimpleStockQuoteService1">
  <address uri="http://localhost:9001/services/SimpleStockQuoteService"/>
</endpoint>
```

```xml tab='Endpoint 2'
<endpoint name="SimpleStockQuoteService2">
  <address uri="http://localhost:9002/services/SimpleStockQuoteService"/>
</endpoint>
```

```xml tab='Endpoint 3'
<endpoint name="SimpleStockQuoteService3">
  <address uri="http://localhost:9003/services/SimpleStockQuoteService"/>
</endpoint>
```

```xml tab='Main Sequence'
<sequence name="main">
  <in>
     <log level="full"/>
     <filter source="get-property('To')" regex="http://localhost:9000.*">
        <send/>
     </filter>
  </in>
  <out>
     <send/>
  </out>
  <description>The main sequence for the message mediation</description>
</sequence>
```

```xml tab='Fault Sequence'
<sequence name="fault">
  <log level="full">
     <property name="MESSAGE" value="Executing default 'fault' sequence"/>
     <property name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
     <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
  </log>
  <drop/>
</sequence>
```

```xml tab='Message Store'
<messageStore class="org.apache.synapse.message.store.impl.jms.JmsStore" name="JMSMS">
  <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
  <parameter name="store.jms.cache.connection">false</parameter>
  <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>
  <parameter name="store.jms.JMSSpecVersion">1.1</parameter>
  <parameter name="store.jms.destination">JMSMS</parameter>
</messageStore>
```

```xml tab='Message Processor 1'
<messageProcessor class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor"
                         name="Forwarder1"
                         targetEndpoint="SimpleStockQuoteService1"
                         messageStore="JMSMS">
  <parameter name="client.retry.interval">1000</parameter>
  <parameter name="interval">1000</parameter>
  <parameter name="is.active">true</parameter>
</messageProcessor>
```

```xml tab='Message Processor 2'
<messageProcessor class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor"
                         name="Forwarder2"
                         targetEndpoint="SimpleStockQuoteService2"
                         messageStore="JMSMS">
  <parameter name="client.retry.interval">1000</parameter>
  <parameter name="interval">1000</parameter>
  <parameter name="is.active">true</parameter>
</messageProcessor>
```

```xml tab='Message Processor 3'
<messageProcessor class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor"
                         name="Forwarder3"
                         targetEndpoint="SimpleStockQuoteService3"
                         messageStore="JMSMS">
  <parameter name="client.retry.interval">1000</parameter>
  <parameter name="interval">1000</parameter>
  <parameter name="is.active">true</parameter>
</messageProcessor>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. Create the [scheduled task](../../../../develop/creating-artifacts/creating-scheduled-task), [proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service), [registry resource](../../../../develop/creating-artifacts/creating-registry-resources), [mediation sequences](../../../../develop/creating-artifacts/creating-reusable-sequences), [endpoints](../../../../develop/creating-artifacts/creating-endpoints), [message stores](../../../../develop/creating-artifacts/creating-a-message-store), and [message processors](../../../../develop/creating-artifacts/creating-a-message-processor) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

[Configure the ActiveMQ broker](../../../../setup/brokers/configure-with-ActiveMQ).

Send the following request:

```bash
ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy -Dmode=placeorder -Dsymbol=WSO2
```

You can analyze the message sent by the Micro Integrator to the secure service using TCPMon.

On successful execution of the placeorder request, you will see thefollowing message on the back-end:

```xml
Sun Aug 18 10:58:00 IST 2013 samples.services.SimpleStockQuoteService :: Accepted order #5 for : 18851 stocks of WSO2 at $ 61.782478265721714
```

If you use the stockqoute client to send the placeorder request several times and observe the log on the back-end server, you will see that the messages are distributed randomly among the back-end nodes since you send the placeorder request randomly. However, if you send the request message evenly (such as when using soapUI), you will see that the messages are evenly distributed among the back-end nodes.
