# Store and Forward Messaging using RabbitMQ

!!! Note
    <b>Work in progress!</b>

## Synapse configurations

See the instructions on how to [build and run](#build-and-run) this example.

```xml tab='Sales Delivery - Message store'
<?xml version="1.0" encoding="UTF-8"?>
<messageStore xmlns="http://ws.apache.org/ns/synapse"
            class="org.apache.synapse.message.store.impl.rabbitmq.RabbitMQStore"
            name="sales-delivery-store">
   <parameter name="store.rabbitmq.host.name">localhost</parameter>
   <parameter name="store.producer.guaranteed.delivery.enable">false</parameter>
   <parameter name="store.rabbitmq.host.port">5672</parameter>
   <parameter name="store.rabbitmq.username">guest</parameter>
   <parameter name="store.rabbitmq.queue.name">sales-delivery</parameter>
   <parameter name="store.rabbitmq.password">guest</parameter>
</messageStore>
```

```xml tab='Sales Delivery - Message Processor'
<?xml version="1.0" encoding="UTF-8"?>
<messageProcessor xmlns="http://ws.apache.org/ns/synapse"
                class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor"
                name="sales-delivery-processor"
                targetEndpoint="DeliveryEndpoint"
                messageStore="sales-delivery-store">
   <parameter name="client.retry.interval">1000</parameter>
   <parameter name="throttle">false</parameter>
   <parameter name="max.delivery.attempts">4</parameter>
   <parameter name="member.count">1</parameter>
   <parameter name="store.connection.retry.interval">1000</parameter>
   <parameter name="max.store.connection.attempts">-1</parameter>
   <parameter name="max.delivery.drop">Disabled</parameter>
   <parameter name="interval">1000</parameter>
   <parameter name="message.processor.failMessagesStore">sales-store</parameter>
   <parameter name="is.active">true</parameter>
   <parameter name="target.endpoint">DeliveryEndpoint</parameter>
</messageProcessor>
```

## Build and run

