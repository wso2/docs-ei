# A topic used to broadcast a message to all consumers

!!! Note
    <b>Work in progress!</b>

## Synapse configurations

See the instructions on how to [build and run](#build-and-run) this example.

```xml tab='RabbitMQ Subscriber'
<?xml version="1.0" encoding="UTF-8"?><proxy xmlns="http://ws.apache.org/ns/synapse" name="TopicSubscriber" transports="rabbitmq" startOnLoad="true">
  <description/>
  <target>
      <inSequence>
          <log level="custom">
              <property name="Message Received" expression="//Message"/>
          </log>
          <call>
              <endpoint>
                  <http uri-template="http://localhost:8280/employees" method=”post”/>
              </endpoint>
          </call>
      </inSequence>
  </target>
  <parameter name="rabbitmq.queue.routing.key">topic1</parameter>
  <parameter name="rabbitmq.exchange.name">amq.topic</parameter>
  <parameter name="rabbitmq.queue.name">queue2</parameter>
  <parameter name="rabbitmq.connection.factory">AMQPConnectionFactory</parameter>
</proxy>
```

```xml tab='RabbitMQ Publisher'
<?xml version="1.0" encoding="UTF-8"?><proxy xmlns="http://ws.apache.org/ns/synapse" name="TopicPublisher" transports="http https" startOnLoad="true">
  <description/>
  <target>
      <inSequence>
          <property name="OUT_ONLY" value="true"/>
          <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
          <send>
              <endpoint>
                  <address uri="rabbitmq:/topic1?rabbitmq.server.host.name=localhost&amp;rabbitmq.server.port=5672&amp;rabbitmq.server.user.name=guest&amp;rabbitmq.server.password=guest&amp;rabbitmq.exchange.name=amq.topic"/>
              </endpoint>
          </send>
      </inSequence></target></proxy>
```

## Build and run
