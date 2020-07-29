# A queue used to deliver a message to a consumer

This example demonstrates how the WSO2 Micro-Integrator can be used to implement a point-to-point messaging scenario
using queues and a RabbitMQ broker instance.

## Synapse configurations

See the instructions on how to [build and run](#build-and-run) this example.

```xml tab='RabbitMQ Consumer'
<?xml version="1.0" encoding="UTF-8"?><proxy xmlns="http://ws.apache.org/ns/synapse" name="QueueConsumer" transports="rabbitmq" startOnLoad="true">
  <description/>
  <target>
      <inSequence>
          <log level="custom">
               <property name="Message Received" expression="//Message"/>
          </log>
          <call>
              <endpoint>
                  <http uri-template="http://localhost:8280/employees" method="post"/>
              </endpoint>
          </call>
      </inSequence>
  </target>
  <parameter name="rabbitmq.queue.name">queue1</parameter>
  <parameter name="rabbitmq.connection.factory">AMQPConnectionFactory</parameter>
</proxy>
```

```xml tab='RabbitMQ Producer'
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse"
    name="QueueProducer"
    transports="http https"
    startOnLoad="true">
   <description/>
   <target>
    <inSequence>
      <property name="OUT_ONLY" value="true"/>
      <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
      <send>
          <endpoint>
            <address uri="rabbitmq:/queue1?rabbitmq.server.host.name=localhost&amp;rabbitmq.server.port=5672&amp;rabbitmq.server.user.name=guest&amp;rabbitmq.server.password=guest”/>
          </endpoint>
      </send>
    </inSequence>
  </target>
</proxy>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. Create the [proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service) with the configurations given above.
4. Enable the RabbitMQ sender and receiver in the Micro-Integrator from the deployment.toml. Refer the 
 [configuring RabbitMQ documentation](../../../setup/brokers/configure-with-rabbitMQ.md) for more information.
5. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.
6. Make sure you have a RabbitMQ broker instance running.
7. Configure a queue named `queue1` with required exchanges and routing keys.
8. Send the following payload to the RabbitMQ publisher proxy (QueueProducer).

    ```xml
    <Message>
    <Name>John Doe</Name>
    <Age>27</Age>
    </Message>
    ```
