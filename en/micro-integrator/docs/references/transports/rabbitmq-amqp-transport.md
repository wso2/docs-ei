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

------------------------------------------------------------------------

\[ [Configuring the RabbitMQ AMQP
transport](#RabbitMQAMQPTransport-ConfigRabbitMQConfiguringtheRabbitMQAMQPtransport)
\] \[ [RabbitMQ AMQP transport
parameters](#RabbitMQAMQPTransport-RabbitMQAMQPtransportparameters) \]
\[ [Connection recovery in
RabbitMQ](#RabbitMQAMQPTransport-ConnectionrecoveryinRabbitMQ) \] \[
[SSL enabled RabbitMQ
transport](#RabbitMQAMQPTransport-SSLenabledRabbitMQtransport) \] \[
[Higher Throughput of Message Delivery to
RabbitMQ](#RabbitMQAMQPTransport-HigherThroughputofMessageDeliverytoRabbitMQ)
\] \[ [Creating the RabbitMQ proxy
service](#RabbitMQAMQPTransport-proxyServiceCreatingtheRabbitMQproxyservice)
\] \[ [Rolling failed messages
back](#RabbitMQAMQPTransport-Rollingfailedmessagesback) \]

------------------------------------------------------------------------

### Configuring the RabbitMQ AMQP transport

1.  Open `          <EI_HOME>/conf/axis2/axis2.xml         ` for
    editing.
2.  In the transport **listeners** section, add the following RabbitMQ
    transport listener, replacing the values with your host, port,
    username, and password to connect to the AMQP broker.

    ``` html/xml
        <transportReceiver name="rabbitmq" class="org.apache.axis2.transport.rabbitmq.RabbitMQListener">
           <parameter name="AMQPConnectionFactory" locked="false">
              <parameter name="rabbitmq.server.host.name" locked="false">192.168.0.3</parameter>
              <parameter name="rabbitmq.server.port" locked="false">5672</parameter>
              <parameter name="rabbitmq.server.user.name" locked="false">user</parameter>
              <parameter name="rabbitmq.server.password" locked="false">abc123</parameter>
           </parameter>
        </transportReceiver>
    ```

    As an optional step, you can create multiple connection factories if
    you want this listener to connect to multiple brokers.

3.  In the transport **senders** section, add the following RabbitMQ
    transport sender, which will be used for sending AMQP messages to a
    queue:

    ``` html/xml
            <transportSender name="rabbitmq" class="org.apache.axis2.transport.rabbitmq.RabbitMQSender"/>
    ```

4.  Download the "
    [amqp-client-5.7.0.jar](https://www.rabbitmq.com/java-client.html) "
    and copy it into `          <EI_HOME>/lib         ` folder
5.  Start WSO2 EI.

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

Configuring parameters that provide information related to keystores
and truststores can be optional based on your broker configuration.

For example, if `         fail_if_no_peer_cert        ` is set to
`         false        ` in the RabbitMQ broker configuration, then you
only need to specify
`         <parameter name="rabbitmq.connection.ssl.enabled" locked="false">true</parameter>        `
. Additionally, you can also set
`         <parameter name="rabbitmq.connection.ssl.version" locked="false">true</parameter>        `
parameter to specify the SSL version. If
`         fail_if_no_peer_cert        ` is set to
`         true        ` , you need to provide keystore and truststore
information.

Following is a sample broker configuration where
`         fail_if_no_peer_cert        ` is set to
`         false        ` :

``` java
    {ssl_options, [{cacertfile,"/path/to/testca/cacert.pem"},
                   {certfile,"/path/to/server/cert.pem"},
                   {keyfile,"/path/to/server/key.pem"},
                   {verify,verify_peer},
                   {fail_if_no_peer_cert,false}]}   
```

The same parameter names are applicable for transport sender
configurations.

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


### Creating the RabbitMQ proxy service

Following is a sample RabbitMQ proxy service named AMQPProxy, which
consumes AMQP messages from one RabbitMQ broker and publishes them to
another:

**Sample Proxy Service**

``` java
    <proxy xmlns="http://ws.apache.org/ns/synapse" name="AMQPProxy" transports="rabbitmq" statistics="disable" trace="disable" startOnLoad="true">
       <target>
          <inSequence>
             <log level="full"/>
             <property name="OUT_ONLY" value="true"/>
             <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
          </inSequence>
          <endpoint>
             <address
             uri="rabbitmq:/AMQPProxy?rabbitmq.server.host.name=192.168.0.3&rabbitmq.server.port=5672&rabbitmq.server.user.name=user&rabbitmq.server.password=abc123&rabbitmq.queue.name=queue2&rabbitmq.exchange.name=exchange2"/>
          </endpoint>
       </target>
       <parameter name="rabbitmq.queue.name">queue1</parameter>
       <parameter name="rabbitmq.exchange.name">exchange1</parameter>
       <parameter name="rabbitmq.connection.factory">AMQPConnectionFactory</parameter>
       <description></description>
    </proxy>
```

Note the following:

-   The transport key name is `          rabbitmq         ` . You need
    to specify this key name in the transports parameter (ie.,
    `          transports="rabbitmq"         ` ).
-   The proxy is defined as OUT\_ONLY, because it does not expect a
    response from the endpoint.
-   The endpoint specifies where the messages will be published. The URI
    prefix is `          rabbbitmq         ` so that the RabbitMQ AMQP
    transport will be used to publish the message. Be sure to specify
    the rest of the parameters in the URI as shown in the sample proxy
    service above. (NOTE: if you are configuring the URI through the
    management console instead of entering the configuration directly in
    the configuration file, you must encode the ampersands in the URI as
    "&amp;" instead of "&".) If you do not know which [RabbitMQ
    exchange](http://www.rabbitmq.com/tutorials/tutorial-three-python.html)
    to use, leave the value blank to use the default exchange.
-   The `          rabbitmq.queue.name         ` parameter specifies the
    queue on which the proxy service listens and consumes messages. If
    you do not specify a name for this parameter, the name of the proxy
    service will be used as the queue name.
-   The `          rabbitmq.exchange.name         ` parameter specifies
    the RabbitMQ exchange to which the queue is bound. If you do not
    want to use a specific exchange, leave this value blank to use the
    default exchange.
-   The `          rabbitmq.connection.factory         ` parameter
    specifies the listener that listens on the queue and consumes
    messages. In this example, the connection factory is set to the name
    of the listener we created earlier (ie.,
    `          AMQPConnectionFactory         ` ).

You can modify the sample proxy service above to handle scenarios where
you only want to receive AMQP messages but need to send messages in a
different format, or you want to receive messages in a different format
and send only AMQP messages. You can also modify the proxy service to
work with a different transport. For example, you can create a proxy
that uses the RabbitMQ AMQP transport to listen to messages and then
sends them over HTTP or JMS.

#### Sample java clients

This section describes the sample Java clients that you can use to send
and receive AMQP messages. These clients can be used to test the
scenario where the Sender publishes a message to a RabbitMQ AMQP queue,
which is consumed by WSO2 EI and published to another queue, which in
turn is consumed by the Receiver. When you run these clients, the
Receiver will get the messages sent by the Sender, confirming that you
have correctly configured the RabbitMQ AMQP transport.

##### AMQP sender

The following Java client sends SOAP XML messages to a RabbitMQ queue:

``` java
    ConnectionFactory factory = new ConnectionFactory();
    factory.setHost(host);
    factory.setUsername(username);
    factory.setPassword(password);
    factory.setPort(port);
    Connection connection = factory.newConnection();
    Channel channel = connection.createChannel();
    channel.queueDeclare(queueName, false, false, false, null);
    channel.exchangeDeclare(exchangeName, "direct", true);
    channel.queueBind(queueName, exchangeName, routingKey);
    
    // The message to be sent
    String message = "<soapenv:Envelope xmlns:soapenv=\"http://schemas.xmlsoap.org/soap/envelope/\">\n" +
                     "<soapenv:Header/>\n" +
                     "<soapenv:Body>\n" +
                     "  <p:greet xmlns:p=\"http://greet.service.kishanthan.org\">\n" +
                     "     <in>" + name + "</in>\n" +
                     "  </p:greet>\n" +
                     "</soapenv:Body>\n" +
                     "</soapenv:Envelope>";
    
    // Populate the AMQP message properties
    AMQP.BasicProperties.Builder builder = new
    AMQP.BasicProperties().builder();
    builder.messageId(messageID);
    builder.contentType("text/xml");
    builder.replyTo(replyToAddress);
    builder.correlationId(correlationId);
    builder.contentEncoding(contentEncoding);
    
    // Custom user properties
    Map<String, Object> headers = new HashMap<String, Object>();
    headers.put("SOAP_ACTION", "greet");
    builder.headers(headers);
    
    // Publish the message to exchange
    channel.basicPublish(exchangeName, queueName, builder.build(), message.getBytes());
```

This client was tested with SOAP messages sent and consumed from AMQP
broker queues with the content type “text/xml”. When specifying the
queue name for publishing messages, be sure to specify the same queue
where the RabbitMQ transport listener is listening.

##### AMQP receiver ** **

When specifying the queue name for consuming messages, be sure to
specify the same queue configured in the proxy service endpoint.

``` java
    ConnectionFactory factory = new ConnectionFactory();
    factory.setHost(hostName);
    factory.setUsername(userName);
    factory.setPassword(password);
    factory.setPort(port);
    Connection connection = factory.newConnection();
    Channel channel = connection.createChannel();
    channel.queueDeclare(queueName, false, false, false, null);
    channel.exchangeDeclare(exchangeName, "direct", true);
    channel.queueBind(queueName, exchangeName, routingKey);
    
    // Create the consumer
    QueueingConsumer consumer = new QueueingConsumer(channel);
    channel.basicConsume(queueName, true, consumer);
    
    // Start consuming messages
    while (true) {
       QueueingConsumer.Delivery delivery = consumer.nextDelivery();
       String message = new String(delivery.getBody());
    }
```

### Rolling failed messages back

When a message is read from an inbound (RabbitMQ) message queue via an
Inbound Endpoint, it will be sent to a service running on the Axis2
back-end server. If a failure occurs, the transaction will roll back.
This avoids the loss of the message.

  

!!! tip

If you are using a RabbitMQ Inbound Endpoint for receiving messages, set
the scope of the `           SET_ROLLBACK_ONLY          ` property to
`           default          ` as follows:

    <property name="SET_ROLLBACK_ONLY" scope="default" type="STRING" value="true"/>


As shown in the below example, you need to set the
`           SET_ROLLBACK_ONLY          ` property to **true** in the
fault handler (e.g., the fault sequence), to roll the message back when
a failure occurs.

**The inbound endpoint configuration**

``` xml
    <?xml version="1.0" encoding="UTF-8"?><inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="rabbit-mq-dec41-inbound-endpoint" sequence="rabbit-mq-dec41-inbound-sequence" onError="rabbitmq_fault" protocol="rabbitmq" suspend="false">
        <parameters>
            <parameter name="sequential">true</parameter>
            <parameter name="coordination">true</parameter>
            <parameter name="rabbitmq.connection.factory">AMQPConnectionFactory</parameter>
            <parameter name="rabbitmq.server.host.name">localhost</parameter>
            <parameter name="rabbitmq.server.port">5672</parameter>
            <parameter name="rabbitmq.server.user.name">rabbit</parameter>
            <parameter name="rabbitmq.server.password">rabbit</parameter>
            <parameter name="rabbitmq.queue.name">RABBITMQ-INBOUND-QUEUE</parameter>
            <parameter name="rabbitmq.exchange.name">RABBITMQ-INBOUND-EXCHANGE</parameter>
            <parameter name="rabbitmq.queue.durable">true</parameter>
            <parameter name="rabbitmq.queue.exclusive">false</parameter>
            <parameter name="rabbitmq.queue.auto.delete">false</parameter>
            <parameter name="rabbitmq.queue.auto.ack">false</parameter>
            <parameter name="rabbitmq.exchange.durable">true</parameter>
            <parameter name="rabbitmq.exchange.auto.delete">false</parameter>
            <parameter name="rabbitmq.connection.ssl.enabled">false</parameter>
        </parameters>
    </inboundEndpoint>
```

The below is the fault sequence with the
`           SET_ROLLBACK_ONLY          ` property set to **true** .

**The fault sequence**

``` xml
    <?xml version="1.0" encoding="UTF-8"?><sequence xmlns="http://ws.apache.org/ns/synapse" name="rabbitmq_fault">
        <log level="full">
            <property name="MESSAGE" value="Executing default 'fault' sequence"/>
            <property xmlns:ns="http://org.apache.synapse/xsd" name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
            <property xmlns:ns="http://org.apache.synapse/xsd" name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
        </log>
            <property name="SET_ROLLBACK_ONLY" value="true" scope="default" type="STRING"/>
        <drop/>
    </sequence>
```

For more information on the implementation details of common
RabbitMQ use cases, see the following topics:

-   [RabbitMQ Use Cases](_RabbitMQ_Use_Cases_)
