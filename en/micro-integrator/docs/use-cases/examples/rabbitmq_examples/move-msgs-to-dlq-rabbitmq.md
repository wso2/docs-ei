# Publish unacked messages to Dead Letter Exchange

!!! Note
    <b>Work in progress!</b>

## Synapse configurations

See the instructions on how to [build and run](#build-and-run) this example.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse"
    name="OrdersService"
    startOnLoad="true"
    statistics="disable"
    trace="disable"
    transports="rabbitmq">
   <target>
    <inSequence>
      <log level="custom">
          <property expression="$body" name="Message Received"/>
      </log>
      <call blocking="true">
          <endpoint>
            <http uri-template="http://localhost:8280/orders"/>
          </endpoint>
      </call>
      <log level="custom">
          <property name="Status" value="The message process successfully"/>
      </log>
    </inSequence>
    <faultSequence>
      <log level="custom">
          <property name="Status" value="Could not process the message"/>
      </log>
      <property name="SET_ROLLBACK_ONLY" scope="axis2" value="true"/>
    </faultSequence>
   </target>
   <parameter name="rabbitmq.exchange.name">orders-exchange</parameter>
   <parameter name="rabbitmq.queue.auto.ack">false</parameter>
   <parameter name="rabbitmq.queue.name">orders</parameter>
   <parameter name="rabbitmq.connection.factory">AMQPConnectionFactory</parameter>
   <description/>
</proxy>
```

## Build and run
