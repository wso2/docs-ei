# RabbitMQ Inbound 
## Introduction

<a href="http://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol">AMQP</a> is a wire-level messaging protocol that describes the format of the data that is sent across the network. If a system or application can read and write AMQP, it can exchange messages with any other system or application that understands AMQP regardless of the implementation language.

## Syntax

```xml
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="RabbitMQConsumer" sequence="amqpSeq" onError="amqpErrorSeq" protocol="rabbitmq" suspend="false">
   <parameters>
      <parameter name="sequential">true</parameter>
      <parameter name="coordination">true</parameter>
      <parameter name="rabbitmq.connection.factory">AMQPConnectionFactory</parameter>
      <parameter name="rabbitmq.server.host.name">localhost</parameter>
      <parameter name="rabbitmq.server.port">5672</parameter>
      <parameter name="rabbitmq.server.user.name">guest</parameter>
      <parameter name="rabbitmq.server.password">guest</parameter>
      <parameter name="rabbitmq.queue.name">queue</parameter>
      <parameter name="rabbitmq.exchange.name">exchange</parameter>
      <parameter name="rabbitmq.connection.ssl.enabled">false</parameter>
   </parameters>
</inboundEndpoint>
```

## Properties

Listed below are the properties used for [creating a RabbitMQ inbound endpiont](../../../../../develop/creating-artifacts/creating-an-inbound-endpoint).

### Required Properties

The following properties are required when [creating a RabbitMQ inbound endpiont](../../../../../develop/creating-artifacts/creating-an-inbound-endpoint).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
         <td>
            sequential
         </td>
         <td>The behavior when executing the given sequence.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.factory
         </td>
         <td>Name of the connection factory.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.server.host.name
         </td>
         <td>
            Host name of the server.
         </td>
      </tr>
      <tr>
         <td>
            rabbitmq.server.port
         </td>
         <td>The port number of the server.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.server.user.name
         </td>
         <td>The user name to connect to the server.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.server.password
         </td>
         <td>The password to connect to the server.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.name
         </td>
         <td>The queue name to send or consume messages.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.exchange.name
         </td>
         <td>The name of the RabbitMQ exchange to which the queue is bound.</td>
      </tr>
      <tr>
         <td>coordination</td>
         <td>This parameter is only applicable in a clustered environment.<br />
            In a cluster environment an inbound endpoint will only be executed in worker nodes. If this parameter is set to <code>true</code> in a clustered environment, the inbound will only be executed in a single worker node. Once the running worker node is down, the inbound will start on another available worker node in the cluster. By default, this setting is <code>true</code>.
         </td>
      </tr>
      <tr>
      <td>
         sequential
      </td>
      <td>The behaviour when executing the given sequence.</td>
   </tr>
   <tr>
         <td>sequential</td>
         <td>The behavior when executing the given sequence.<br />
            When set as <code>true</code> , mediation will happen within the same thread. When set as <code>false</code> , the mediation engine will use the inbound thread pool. The default thread pool values can be found in the <code>MI_HOME/conf/deployment.toml</code> file, under the `[mediation]` section. The default setting is <code>true</code>.
         </td>
      </tr>
      <tr>
        <td>Suspend</td>
        <td>
          If the inbound listener should pause when accepting incoming requests, set this to <code>true</code>. If the inbound listener should not pause when accepting incoming requests, set this to <code>false</code>.
        </td>
      </tr>
</table>

### Optional Properties

The following optional properties can be configured when [creating an RabbitMQ inbound endpiont](../../../../../develop/creating-artifacts/creating-an-inbound-endpoint).

<table>
   <thead>
      <tr>
         <th>
          Property Name
         </th>
         <th>
          Description
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>
            rabbitmq.server.virtual.host
         </td>
         <td>The virtual host name of the server (if available).</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.enabled
         </td>
         <td>Whether to use TCP connection or SSL connection.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.keystore.location
         </td>
         <td>The keystore location.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.keystore.type
         </td>
         <td>
          The keystore type.
         </td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.keystore.password
         </td>
         <td>The keystore password.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.truststore.location
         </td>
         <td>The truststore location.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.truststore.type
         </td>
         <td>The truststore type.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.truststore.password
         </td>
         <td>The truststore password.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.version
         </td>
         <td>The SSL version.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.durable
         </td>
         <td>Whether the queue should remain declared even if the broker restarts.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.exclusive
         </td>
         <td>Whether the queue should be exclusive or should be consumable by other connections.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.auto.delete
         </td>
         <td>Whether to keep the queue even if it is not being consumed anymore.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.auto.ack
         </td>
         <td>Whether to send back an acknowledgment.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.routing.key
         </td>
         <td>The routing key of the queue.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.delivery.mode
         </td>
         <td>The delivery mode of the queue (whether or not the queue is persistent).</td>
      </tr>
      <tr>
         <td>
            rabbitmq.exchange.type
         </td>
         <td>The type of the exchange.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.exchange.durable
         </td>
         <td>Whether the exchange should remain declared even if the broker restarts.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.exchange.auto.delete
         </td>
         <td>Whether to keep the queue even if it is not used anymore.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.factory.heartbeat
         <td>
            <p>The period of time after which the connection should be considered dead.</p>
         </td>
      </tr>
      <tr>
         <td>
            rabbitmq.message.content.type
         </td>
         <td>The content type of the message.</td>
      </tr>
   </tbody>
</table>

### Connection Recovery Properties

In case of a network failure or broker shutdown, the Micro Integrator can try to
recreate the connection.

If you want to enable connection recovery, you should configure the
following parameters in the inbound endpoint:

```
<parameter name="rabbitmq.connection.retry.interval" locked="false">10000</parameter>
<parameter name="rabbitmq.connection.retry.count" locked="false">5</parameter>   
```

If the parameters are configured with sample values as given above, the
server makes 5 retry attempts with a time interval of 10000 miliseconds between each
retry attempt to reconnect when the connection is lost. If reconnecting
fails after 5 retry attempts, the Micro Integrator terminates the connection.

In addition to the above parameters, if you want to set the interval
between retry attempts with which the RabbitMQ client should try
reconnecting to the Micro Integrator, you can configure the following parameter:

```
<parameter name="rabbitmq.server.retry.interval" locked="false">10000</parameter>
```

Setting the value of `rabbitmq.server.retry.interval` to be less than the value of `rabbitmq.connection.retry.interval` helps synchronize the reconnection of the Micro Integrator and the RabbitMQ client.