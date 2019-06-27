# RabbitMQ Inbound Protocol

[AMQP](http://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol)
is a wire-level messaging protocol that describes the format of the data
that is sent across the network. If a system or application can read and
write AMQP, it can exchange messages with any other system or
application that understands AMQP regardless of the implementation
language.

Configuration parameters for a RabbitMQ inbound endpoint are XML
fragments that represent various properties.

Following is a sample RabbitMQ inbound endpoint configuration that
consumes messages from a queue:

``` html/xml
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

### RabbitMQ inbound endpoint parameters

<table>
<thead>
<tr class="header">
<th><p>Parameter Name</p>
<p><br />
</p></th>
<th><p>Description</p>
<p><br />
</p></th>
<th><p>Required</p>
<p><br />
</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>sequential</code></pre></td>
<td>The behavior when executing the given sequence.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.connection.factory</code></pre></td>
<td>Name of the connection factory.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><pre><code>rabbitmq.server.host.name</code></pre></td>
<td><p>Host name of the server.</p></td>
<td>Yes</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.server.port</code></pre></td>
<td>The port number of the server.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><pre><code>rabbitmq.server.user.name</code></pre></td>
<td>The user name to connect to the server.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.server.password</code></pre></td>
<td>The password to connect to the server.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><pre><code>rabbitmq.queue.name</code></pre></td>
<td>The queue name to send or consume messages.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.exchange.name</code></pre></td>
<td>The name of the RabbitMQ exchange to which the queue is bound.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><pre><code>rabbitmq.server.virtual.host</code></pre></td>
<td>The virtual host name of the server, if available.</td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.connection.ssl.enabled</code></pre></td>
<td>Whether to use TCP connection or SSL connection.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>rabbitmq.connection.ssl.keystore.location</code></pre></td>
<td>The keystore location.</td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.connection.ssl.keystore.type</code></pre></td>
<td><p>The keystore type.</p></td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>rabbitmq.connection.ssl.keystore.password</code></pre></td>
<td>The keystore password.</td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.connection.ssl.truststore.location</code></pre></td>
<td>The truststore location.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>rabbitmq.connection.ssl.truststore.type</code></pre></td>
<td>The truststore type.</td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.connection.ssl.truststore.password</code></pre></td>
<td>The truststore password.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>rabbitmq.connection.ssl.version</code></pre></td>
<td>The SSL version.</td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.queue.durable</code></pre></td>
<td>Whether the queue should remain declared even if the broker restarts.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>rabbitmq.queue.exclusive</code></pre></td>
<td>Whether the queue should be exclusive or should be consumable by other connections.</td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.queue.auto.delete</code></pre></td>
<td>Whether to keep the queue even if it is not being consumed anymore.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>rabbitmq.queue.auto.ack</code></pre></td>
<td>Whether to send back an acknowledgment.</td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.queue.routing.key</code></pre></td>
<td>The routing key of the queue.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>rabbitmq.queue.delivery.mode</code></pre></td>
<td>The delivery mode of the queue (ie., Whether it is persistent or not).</td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.exchange.type</code></pre></td>
<td>The type of the exchange.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>rabbitmq.exchange.durable</code></pre></td>
<td>Whether the exchange should remain declared even if the broker restarts.</td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.exchange.auto.delete</code></pre></td>
<td>Whether to keep the queue even if it is not used anymore.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>rabbitmq.factory.heartbeat</code></pre></td>
<td><p>The period of time after which the connection should be considered dead.</p></td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>rabbitmq.message.content.type</code></pre></td>
<td>The content type of the message.</td>
<td>No</td>
</tr>
</tbody>
</table>

### Connection recovery parameters

In case of a network failure or broker shutdown, the ESB can try to
recreate the connection.

If you want to enable connection recovery, you should configure the
following parameters in the inbound endpoint:

``` xml
    <parameter name="rabbitmq.connection.retry.interval" locked="false">10000</parameter>
    <parameter name="rabbitmq.connection.retry.count" locked="false">5</parameter>   
```

If the parameters are configured with sample values as given above, the
ESB makes 5 retry attempts with a time interval of 10000 ms between each
retry attempt to reconnect when the connection is lost. If reconnecting
fails after 5 retry attempts, the ESB terminates the connection.

In addition to the above parameters, if you want to set the interval
between retry attempts with which the RabbitMQ client should try
reconnecting to the ESB, you can configure the following parameter:

``` xml
    <parameter name="rabbitmq.server.retry.interval" locked="false">10000</parameter>
```

Setting the value of `         rabbitmq.server.retry.interval        `
to be less than the value of
`         rabbitmq.connection.retry.interval        ` helps synchronize
the reconnection of the ESB and the RabbitMQ client.

### Samples

For a sample that demonstrates how one-way message bridging from
RabbitMQ to HTTP can be done using the RabbitMQ inbound endpoint, see
[Sample 907: Inbound Endpoint RabbitMQ Protocol
Sample](https://docs.wso2.com/display/EI6xx/Sample+907%3A+Inbound+Endpoint+RabbitMQ+Protocol+Sample)
.
