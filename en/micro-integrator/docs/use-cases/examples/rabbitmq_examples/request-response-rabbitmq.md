# Request-Response Messaging (Dual Channel)

!!! Note
    <b>Work in progress!</b>

## Synapse configurations

See the instructions on how to [build and run](#build-and-run) this example.

```xml tab='Order Request Proxy Service'
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse"
    name="OrderRequestService"
    transports="http https"
    startOnLoad="true">
   <description/>
   <target>
    <inSequence>
      <property name="rabbitmq.message.content.type"
                value="Content-Type"
                scope="axis2"/>
      <property name="TRANSPORT_HEADERS" scope="axis2" action="remove"/>
      <send>
          <endpoint>
            <address uri="rabbitmq:/order-request?rabbitmq.server.host.name=localhost&amp;rabbitmq.server.port=5672&amp;rabbitmq.server.user.name=guest&amp;rabbitmq.server.password=guest"/>
          </endpoint>
      </send>
    </inSequence>
   </target>
</proxy>
```

```xml tab='Order Processing Proxy Service'
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse"
    name="OrderProcessingService"
    transports="rabbitmq"
    startOnLoad="true">
   <description/>
   <target>
    <inSequence>
      <call>
          <endpoint>
            <http uri-template="http://localhost:8280/orders"/>
          </endpoint>
      </call>
      <log level="custom">
          <property name="Info" value="Your order has been placed successfully."/>
      </log>
      <respond/>
    </inSequence>
    <faultSequence>
      <payloadFactory media-type="xml">
          <format>
            <Error>$1</Error>
          </format>
          <args>
            <arg evaluator="xml" expression="get-property('ERROR_MESSAGE')"/>
          </args>
      </payloadFactory>
      <respond/>
    </faultSequence>
   </target>
   <parameter name="rabbitmq.queue.name">order-request</parameter>
   <parameter name="rabbitmq.connection.factory">AMQPConnectionFactory</parameter>
</proxy>
```

## Build and run

