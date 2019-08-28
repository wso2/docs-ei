# RabbitMQ AMQP Transport

[AMQP](http://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol)
is a wire-level messaging protocol that describes the format of the data
that is sent across the network. If a system or application can read and
write AMQP, it can exchange messages with any other system or
application that understands AMQP, regardless of the implementation
language.

The RabbitMQ AMQP transport is implemented using the [RabbitMQ Java
Client](http://www.rabbitmq.com/java-client.html) . It allows you to
send or receive AMQP messages by directly calling an AMQP broker
(RabbitMQ).

The following diagram illustrates a scenario where the ESB Profile of
WSO2 Enterprise Integrator (WSO2 EI) uses the RabbitMQ AMQP transport to
exchange messages between RabbitMQ Java clients by calling a RabbitMQ
broker.

![](attachments/119130371/119130372.png){width="500"}
  
As you can see in the diagram, the *Sender* uses the RabbitMQ Java
client to publish messages to an AMQP queue (Q1), and the *Receiver*
users it to consume messages from another AMQP queue (Q2). In this
scenario, a proxy service in the ESB Profile of WSO2 EI listens to Q1,
and when a message becomes available on the queue, the proxy service
consumes it and publishes it to Q2.

This section provides information on configuring the RabbitMQ transport
in the ESB Profile of WSO2 EI, along with information on how to enable
connection recovery, how to enable SSL support as well as a sample
RabbitMQ proxy service that describes how to use the RabbitMQ transport.

!!! Info
    - As an optional step, you can create multiple connection factories if you want this listener to connect to multiple brokers.
    - Download the "[amqp-client-5.7.0.jar](https://www.rabbitmq.com/java-client.html)" and copy it into `          <EI_HOME>/lib         ` folder.

### RabbitMQ AMQP transport parameters

Following are details on the listener parameters you can set:

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Required</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             rabbitmq.connection.factory            </code></td>
<td>The name of the connection factory.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.exchange.name            </code></td>
<td>Name of the RabbitMQ exchange to which the queue is bound. Use this parameter instead of <code>             rabbitmq.queue.routing.key            </code> , if you need to use the default exchange and publish to a queue.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.queue.name            </code></td>
<td>The queue name to send or consume messages. If you do not specify this parameter, you need to specify the <code>             rabbitmq.queue.routing.key            </code> parameter.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.queue.auto.ack            </code></td>
<td><p>Defines how the message processor sends the acknowledgement when consuming messages recived from the RabbitMQ message store. If you set this to true, the message processor automatically sends the acknowledgement to the messages store as soon as it receives messages from it. This is called an auto acknowledgement.</p>
<p>If you set it to false, the message processor waits until it receives the response from the backend to send the acknowledgement to the mssage store. This is called a client acknowledgement.</p></td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.consumer.tag            </code></td>
<td>The client­ generated consumer tag to establish context.</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.channel.consumer.qos            </code></td>
<td>The consumer qos value. You need to specify this parameter only if the <code>             rabbitmq.queue.auto.ack            </code> parameter is set to <code>             false            </code> .</td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.queue.durable            </code></td>
<td>Whether the queue should remain declared even if the broker restarts.</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.queue.exclusive            </code></td>
<td>Whether the queue should be exclusive or should be consumable by other connections.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.queue.auto.delete            </code></td>
<td>Whether to keep the queue even if it is not being consumed anymore.</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.queue.routing.key            </code></td>
<td>The routing key of the queue.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.queue.autodeclare            </code></td>
<td>Whether to create queues if they are not present. However, you should set this parameter only if queues are not declared prior on the broker. Setting this parameter in the publish URL to <code>             false            </code> improves RabbitMQ transport performance.</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.exchange.autodeclare            </code></td>
<td>Whether to create exchanges if they are not present. However, you should set this parameter only if exchanges are not declared prior on the broker. Setting this parameter in the publish URL to <code>             false            </code> improves RabbitMQ transport performance.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.exchange.type            </code></td>
<td>The type of the exchange.</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.exchange.durable            </code></td>
<td>Whether the exchange should remain declared even if the broker restarts.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.exchange.auto.delete            </code></td>
<td><p>Whether to keep the exchange even if it is not bound to any queue anymore.</p></td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.message.content.type            </code></td>
<td><div class="content-wrapper">
<p>The content type of the consumer.</p>
!!! info
<p>Note</p>
<p>If the content type is specified in the message, this parameter does not override the specified content type.</p>

</div></td>
<td>No. The default value is <code>             text/xml            </code> .</td>
</tr>
</tbody>
</table>

  

Following are details on the sender parameters you can set:

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Required</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             rabbitmq.server.host.name            </code></td>
<td>Host name of the server.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.server.port            </code></td>
<td>Port number of the server.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.exchange.name            </code></td>
<td><p>The name of the RabbitMQ exchange to which the queue is bound. Use this parameter instead of <code>              rabbitmq.queue.routing.key             </code> , if you need to use the default exchange and publish to a queue.</p></td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.queue.routing.key            </code></td>
<td>The exchange and queue binding key that will be used to route messages.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.replyto.name            </code></td>
<td>The name of the call back­ queue. Specify this parameter if you expect a response.</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.queue.delivery.mode            </code></td>
<td><p>The delivery mode of the queue. Possible values are 1 and 2.<br />
1 - Non­-persistent.<br />
2 - Persistent. This is the default value.</p></td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.exchange.type            </code></td>
<td>The type of the exchange.</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.queue.name            </code></td>
<td>The queue name to send or consume messages. If you do not specify this parameter, you need to specify the <code>             rabbitmq.queue.routing.key            </code> parameter.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.queue.durable            </code></td>
<td>Whether the queue should remain declared even if the broker restarts. The default value is <code>             false            </code> .</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.queue.exclusive            </code></td>
<td>Whether the queue should be exclusive or should be consumable by other connections. The default value is <code>             false            </code></td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.queue.auto.delete            </code></td>
<td>Whether to keep the queue even if it is not being consumed anymore. The default value is <code>             false            </code> .</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.exchange.durable            </code></td>
<td>Whether the exchange should remain declared even if the broker restarts.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.queue.autodeclare            </code></td>
<td>Whether to create queues if they are not present. However, you should set this parameter only if queues are not declared prior on the broker. Setting this parameter in the publish URL to <code>             false            </code> improves RabbitMQ transport performance.</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             rabbitmq.exchange.autodeclare            </code></td>
<td>Whether to create exchanges if they are not present. However, you should set this parameter only if exchanges are not declared prior on the broker. Setting this parameter in the publish URL to <code>             false            </code> improves RabbitMQ transport performance.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             rabbitmq.message.correlation.id            </code></td>
<td><p>The correlation ID is required to identify a message that comes through one queue and requires a response back via another queue. This ID helps you map the messages and is unique for every request.</p></td>
<td>No</td>
</tr>
<tr class="even">
<td><p><code>              rabbitmq.message.id             </code></p></td>
<td>Every message has its own unique message ID.</td>
<td><br />
</td>
</tr>
</tbody>
</table>

For the `         rabbitmq.server        ` , properties refer to the
server on which RabbitMQ is running.

### Connection recovery in RabbitMQ

In case of a network failure or broker shutdown, the WSO2 EI server will
try to recreate the connection. The following parameters need to be
configured in the transport listener to enable connection recovery in
RabbitMQ.

``` xml
<parameter name="rabbitmq.connection.retry.interval" locked="false">10000</parameter>
<parameter name="rabbitmq.connection.retry.count" locked="false">5</parameter>   
```

If the parameters specified above are set, the WSO2 EI server will retry
5 times with 10000 ms time intervals to reconnect w hen the connection
is lost . If reconnecting also fails, WSO2 EI will terminate the
connection. If you do not specify values for the above parameters, WSO2
EI uses 30000ms as the default retry interval and 3 as the default retry
count.

Optionally, you can configure the following parameter in the listener:

``` xml
    <parameter name="rabbitmq.server.retry.interval" locked="false">10000</parameter>
```

The parameter specified above sets the retry interval with which the
RabbitMQ client tries to reconnect. Generally having this value less
than the value specified as
`         rabbitmq.connection.retry.interval        ` will help
synchronize the reconnection of WSO2 EI and the RabbitMQ client.

### SSL enabled RabbitMQ transport

To enable SSL support in RabbitMQ, you need to configure the transport
listener with parameters required to enable SSL, as well as the
parameters that provide information related to keystores
and truststores.

Following is a sample SSL-enabled transport listener configuration:

``` xml
    <transportReceiver name="rabbitmq" class="org.apache.axis2.transport.rabbitmq.RabbitMQListener">                   
     <parameter name="AMQPConnectionFactoryKS" locked="false">
       <parameter name="rabbitmq.server.host.name" locked="false">localhost</parameter>
       <parameter name="rabbitmq.server.port" locked="false">5671</parameter>
       <parameter name="rabbitmq.server.user.name" locked="false"></parameter>
       <parameter name="rabbitmq.server.password" locked="false"></parameter>
       <parameter name="rabbitmq.connection.retry.interval" locked="false">10000</parameter>
       <parameter name="rabbitmq.connection.retry.count" locked="false">5</parameter>
       <parameter name="rabbitmq.connection.ssl.enabled" locked="false">true</parameter>
       <parameter name="rabbitmq.connection.ssl.version" locked="false">SSL</parameter>
       <parameter name="rabbitmq.connection.ssl.keystore.location" locked="false">../client/keycert.p12</parameter>
       <parameter name="rabbitmq.connection.ssl.keystore.type" locked="false">PKCS12</parameter>
       <parameter name="rabbitmq.connection.ssl.keystore.password" locked="false">MySecretPassword</parameter>
       <parameter name="rabbitmq.connection.ssl.truststore.location" locked="false">ssl/rabbitstore</parameter>
       <parameter name="rabbitmq.connection.ssl.truststore.type" locked="false">JKS</parameter>
       <parameter name="rabbitmq.connection.ssl.truststore.password" locked="false">rabbitstore</parameter>
     </parameter> 
    </transportReceiver>
```

### Higher Throughput of Message Delivery to RabbitMQ

For increased performance and higher throughput in message delivery,
configure the transport sender as follows:

``` xml
    <transportSender name="rabbitmq" class="org.apache.axis2.transport.rabbitmq.RabbitMQSender">
       <parameter name="CachedRabbitMQConnectionFactory" locked="false"> 
         <parameter name="rabbitmq.server.host.name" locked="false">localhost</parameter> 
         <parameter name="rabbitmq.server.port" locked="false">5672</parameter> 
         <parameter name="rabbitmq.server.user.name" locked="false">user</parameter> 
         <parameter name="rabbitmq.server.password" locked="false">abc123</parameter> 
       </parameter> 
    </transportSender>
```

!!! note

When configuring the [proxy
service](#RabbitMQAMQPTransport-proxyService) , be sure to add the
following connection factory parameter in the address URI:
`         rabbitmq.connection.factory=CachedRabbitMQConnectionFactory        `
