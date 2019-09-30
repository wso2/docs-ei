# Guaranteed Delivery with Failover

WSO2 Micro Integrator ensures guaranteed delivery with the failover message store and scheduled failover message forwarding processor. The topics in the following section describe how you can setup guaranteed message delivery with failover configurations.

The following diagram illustrates a scenario where a failover message
store and a scheduled failover message forwarding processor is used
to ensure guaranteed delivery:

![](/assets/img/tutorials/guaranteed-delivery-failover/Guaranteed_Delivery.png)

In this scenario, the original message store fails due to either network
failure, message store crash or system shutdown for maintenance, and the
failover message store is used as the solution for the original message
store failure. So now the store mediator sends messages to the failover
message store. Then, when the original message store is available again,
the messages that were sent to the failover message store need to be
forwarded to the original message store. The **scheduled failover message forwarding processor**
is used for this purpose. The scheduled failover message
forwarding processor is almost the same as the scheduled message
forwarding processor, the only difference is that the scheduled message
forwarding processor forwards messages to a defined endpoint, whereas
the scheduled failover message forwarding processor forwards messages to
the original message store that the message was supposed to be
temporarily stored.

## Synapse Configuration

``` java tab='Failover message store'
<messageStore name="failover"/>  
```

``` java tab=''
<messageStore  
    class="org.apache.synapse.message.store.impl.jms.JmsStore" name="Orginal">  
    <parameter name="store.failover.message.store.name">failover</parameter>  
    <parameter name="store.producer.guaranteed.delivery.enable">true</parameter>  
    <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>  
    <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>  
    <parameter name="store.jms.JMSSpecVersion">1.1</parameter>  
</messageStore>
```

``` java tab='Endpoint'
<endpoint name="SimpleStockQuoteService">  
  <address uri="http://127.0.0.1:9000/services/SimpleStockQuoteService"/>  
</endpoint>
```

``` java tab='Scheduled message forwarding processor'
<messageProcessor name="ForwardMessageProcessor" class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor" targetEndpoint="SimpleStockQuoteService" messageStore="Orginal" xmlns="http://ws.apache.org/ns/synapse">
       <parameter name="interval">1000</parameter>
       <parameter name="client.retry.interval">1000</parameter>
       <parameter name="max.delivery.attempts">4</parameter>
       <parameter name="is.active">true</parameter>
       <parameter name="max.delivery.drop">Disabled</parameter>
       <parameter name="member.count">1</parameter>
</messageProcessor>
```

``` java tab='Scheduled failover message forwarding processor'
<messageProcessor name="FailoverMessageProcessor" class="org.apache.synapse.message.processor.impl.failover.FailoverScheduledMessageForwardingProcessor" messageStore="failover" xmlns="http://ws.apache.org/ns/synapse">
       <parameter name="interval">1000</parameter>
       <parameter name="client.retry.interval">60000</parameter>
       <parameter name="max.delivery.attempts">1000</parameter>
       <parameter name="is.active">true</parameter>
       <parameter name="max.delivery.drop">Disabled</parameter>
       <parameter name="member.count">1</parameter>
       <parameter name="message.target.store.name">Orginal</parameter>
</messageProcessor> 
```

``` java tab='Proxy Service'
<proxy name="Proxy1" transports="https http" startOnLoad="true" trace="disable" xmlns="http://ws.apache.org/ns/synapse">    
      <target>  
        <inSequence>  
         <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>  
         <property name="OUT_ONLY" value="true"/>  
         <log level="full"/>  
         <store messageStore="Orginal"/>  
        </inSequence>  
      </target>  
</proxy>   
```

- **Failover message store**
  In this example an in-memory message store is used to create the failover message store. If you have a cluster setup, it will not be possible to use an in-memory message store since it is not possible tomshare in-memory stores among nodes in a cluster. This step does not involve any special configuration.

- **Original message store**
  In this example a JMS message store is used to create the original message store.  When creating the original message store, you need to enable guaranteed delivery on the producer side. To do this, set the following parameters in the message store configuration:</br>
  `<parameter name="store.failover.message.store.name">failover</parameter>`  
  `<parameter name="store.producer.guaranteed.delivery.enable">true</parameter>`

- **Endpoint for the scheduled message forwarding processor**
  In this example, the SimpleStockquate service is used as the back-end service. Follow the instructions below to define the address endpoint.

- **Scheduled failover message forwarding processor**
  When creating the scheduled failover message forwarding processor, you need to specify the following two mandatory parameters that are important in the failover scenario.
  The scheduled failover message forwarding processor sends messages from the failover store to the original store when it is available in the failover scenario. In this configuration, the source message store should be the failover message store and target message store should be the original message store.

- **Proxy service**
  A proxy service is used to send messages to the original message store via the store mediator. Following is the proxy service used in this example:

## Run the Example

To test the failover scenario, shut down the JMS broker(i.e., the
original message store) and send a few messages to the proxy service.

You will see that the messages are not sent to the back-end since the
original message store is not available. You will also see that the
messages are stored in the failover message store.

If you analyze the the Console log, you will see the failover
message processor trying to forward messages to the original message
store periodically. Once the original message store is available, you
will see that the scheduled failover message forwarding processor sends
the messages to the original store and then that the scheduled message
forwarding processor forwards the messages to the back-end service.
