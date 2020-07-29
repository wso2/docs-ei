# Requeue a message preserving the message order with a delay in case of error

This sample demonstrates how you can requeue a message to the RabbitMQ queue which was consumed by a consumer proxy,
when an error occurs. In the sample given below, if the HTTP endpoint becomes unavailable the message will be put back
to the student-registration queue, until it becomes available.

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

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. Create the [proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service) with the configurations given above.
4. Enable the RabbitMQ sender and receiver in the Micro-Integrator from the deployment.toml. Refer the 
 [configuring RabbitMQ documentation](../../../setup/brokers/configure-with-rabbitMQ.md) for more information.
5. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.
6. Make the `http://localhost:8280/students` endpoint unavailable temporarily. 
7. Make sure you have a RabbitMQ broker instance running.
8. Publish a message to the student-registration queue.
