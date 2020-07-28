# Requeue a message preserving the message order with a delay in case of error

!!! Note
    <b>Work in progress!</b>

## Synapse configurations

See the instructions on how to [build and run](#build-and-run) this example.

```xml
<proxy xmlns="http://ws.apache.org/ns/synapse"
       name="StudentRegistrationService"
       transports="rabbitmq"
       startOnLoad="true">
   <description/>
   <target>
      <inSequence>
         <log level="custom">
            <property name="Message Received" expression="$body"/>
         </log>
         <call blocking="true">
            <endpoint>
               <http uri-template="http://localhost:8280/students"/>
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
         <property name="SET_REQUEUE_ON_ROLLBACK" value="true" scope="axis2"/>
      </faultSequence>
   </target>
   <parameter name="rabbitmq.queue.auto.ack">false</parameter>
   <parameter name="rabbitmq.channel.consumer.qos">1</parameter>
   <parameter name="rabbitmq.message.requeue.delay">30000</parameter>
   <parameter name="rabbitmq.queue.name">student-registration</parameter>
   <parameter name="rabbitmq.connection.factory">AMQPConnectionFactory</parameter>
</proxy>
```

## Build and run
