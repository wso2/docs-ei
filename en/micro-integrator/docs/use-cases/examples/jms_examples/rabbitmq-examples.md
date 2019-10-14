# RabbitMQ Use Cases

The following are some of the main RabbitMQ use cases of WSO2 Micro Integrator.

Before executing the use cases need to [connect WSO2 Micro Integrator with RabbitMQ](https://ei.docs.wso2.com/en/latest/micro-integrator/setup/brokers/configure-with-rabbitMQ/).
                                        
## Micro Integrator as a RabbitMQ Message Consumer

This section describes how the Micro Integrator can be configured as a RabbitMQ message consumer.

The following is a sample scenario that demonstrates how WSO2 Micro Integrator is configured to listen to a rabbitMQ queue, consume messages, and send the messages to an HTTP back­-end service.

### Synapse configuration

```xml
<?xml version="1.0" encoding="UTF­8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse" name="AMQPProxy" transports="rabbitmq" statistics="disable" trace="enable" startOnLoad="true">
<target>
 <inSequence>
   <log level="full"/>
   <property name="OUT_ONLY" value="true"/>
   <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
    <send>
     <endpoint>
       <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
     </endpoint>
    </send>
 </inSequence>
</target>
 <outSequence>
   <drop/>
 </outSequence>
    <parameter name="rabbitmq.queue.name">queue</parameter>
    <parameter name="rabbitmq.exchange.name">exchange</parameter>
    <parameter name="rabbitmq.connection.factory">AMQPConnectionFactory</parameter>
   <description/>
</proxy>
```

## Micro Integrator as a RabbitMQ Message Producer

This section describes how WSO2 Micro Integrator can be
used to send messages to a RabbitMQ queue.

Following is a sample scenario that demonstrates how the Micro Integrator is
configured to listen to HTTP requests and publish them to a RabbitMQ
server (message exchange).

### Synapse configuration

```xml
<?xml version="1.0" encoding="UTF­8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse" name="AMQPProducerSample" transports="http" statistics="disable" trace="disable" startOnLoad="true">
<target>
 <inSequence>
    <property name="OUT_ONLY" value="true"/>
    <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
     <send>
      <endpoint>
       <address uri="rabbitmq:/AMQPProducerSample?rabbitmq.server.host.name=localhost&amp;rabbitmq.server.port=5672&amp;rabbitmq.queue.name=queue&amp;rabbitmq.queue.route.key=route&amp;rabbitmq.exchange.name=exchange"/>
      </endpoint>
    </send>
 </inSequence>
 <outSequence>
        <send/>
 </outSequence>
</target>
<description/>
</proxy> 
```

## Remote Procedure Call(RPC) with RabbitMQ

You can send request-response messages using the RabbitMQ transport by
implementing a Remote Procedure Call(RPC) scenario with RabbitMQ.

The following diagram illustrates a remote procedure call scenario with
RabbitMQ:

The remote procedure call works as follows:

-   When WSO2 Micro Integrator starts up, it creates an
    anonymous, exclusive callback queue.
-   For a remote procedure call request, the Micro Integrator sends a message with
    the following properties:
    -   `            reply_to           ` : This is set to the callback
        queue
    -   `            correlation_id           ` : This is set to a
        unique value for every request.
-   The request is then sent to the rpc_queue.
-   The RPC Server waits for requests on that queue. When a request
    appears, it does the job and sends a message with the result back to
    the Micro Integrator server, using the queue from the
    `           reply_to          ` field with the same
    `           correlation_id          ` .

-   WSO2 Micro Integrator waits for data on the reply_to queue. When a message
    appears, it checks the `          correlation_id         ` property.
    If it matches the value from the request, it returns the response to
    the application.

The following is a sample proxy service named
`         RabbitMQRPCProxy        ` that sends request-response messages
using the RabbitMQ transport.

### Synapse configuration

```xml
<proxy xmlns="http://ws.apache.org/ns/synapse"
    name="RabbitMQRPCProxy"
    startOnLoad="true"
    trace="enable"
       transports="http">
   <description/>
   <target>
    <inSequence>
        <log level="full">
            <property name="received" value="true"/>
        </log>
        <send>
            <endpoint>
            <address uri="rabbitmq://?rabbitmq.server.host.name=localhost&amp;rabbitmq.server.port=5672&amp;rabbitmq.server.user.name=guest&amp;rabbitmq.server.password=guest&amp;rabbitmq.queue.name=rpc_queue&amp;rabbitmq.queue.routing.key=rpc_queue&amp;rabbitmq.replyto.name=dummy"/>
            </endpoint>
        </send>
    </inSequence>
    <outSequence>
        <log level="full">
            <property name="response" value="true"/>
        </log>
        <send/>
    </outSequence>
   </target>
</proxy>
```
The following is the code for a sample RPC server:

```java
package rpc;

import com.rabbitmq.client.AMQP.BasicProperties;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.QueueingConsumer;
import java.util.concurrent.TimeoutException;
import java.io.IOException;

public class RPCServer {

    public static void main(String[] argv) {
        Connection connection = null;
        Channel channel;
        try {
            ConnectionFactory factory = new ConnectionFactory();
            factory.setHost("localhost");
            connection = factory.newConnection();
            channel = connection.createChannel();
            QueueingConsumer consumer = new QueueingConsumer(channel);
            channel.basicConsume("rpc_queue", false, consumer);

            System.out.println(" [x] Awaiting RPC requests");

            while (true) {
                String response = null;
                QueueingConsumer.Delivery delivery = consumer.nextDelivery();
                BasicProperties props = delivery.getProperties();
                BasicProperties replyProps =
                        new BasicProperties.Builder().correlationId(props.getCorrelationId()).contentType("text/xml")
                                                     .build();

                response =
                        "<soapenv:Envelope xmlns:soapenv=\"http://schemas.xmlsoap.org/soap/envelope/\" " +
                        "xmlns:ser=\"http://services.samples\" xmlns:xsd=\"http://services.samples/xsd\">\n" +
                        "   <soapenv:Header/>\n" +
                        "   <soapenv:Body>\n" +
                        "      <ser:placeOrder>\n" +
                        "         <!--Optional:-->\n" +
                        "         <ser:order>\n" +
                        "            <!--Optional:-->\n" +
                        "            <xsd:price>10</xsd:price>\n" +
                        "            <!--Optional:-->\n" +
                        "            <xsd:quantity>5</xsd:quantity>\n" +
                        "            <!--Optional:-->\n" +
                        "            <xsd:symbol>RMQ</xsd:symbol>\n" +
                        "         </ser:order>\n" +
                        "      </ser:placeOrder>\n" +
                        "   </soapenv:Body>\n" +
                        "</soapenv:Envelope>";

                String replyToQueue = props.getReplyTo();
                System.out.println("Publishing to : " + replyToQueue);
                channel.basicPublish("", replyToQueue, replyProps, response.getBytes("UTF-8"));
                channel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
            }

        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (connection != null) {
                try {
                    connection.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## Creating the RabbitMQ proxy service

Following is a sample RabbitMQ proxy service named AMQPProxy, which
consumes AMQP messages from one RabbitMQ broker and publishes them to
another:

### Synapse configuration

```xml
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
-   The proxy is defined as OUT_ONLY, because it does not expect a
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

## Rolling failed messages back

In this exmaple, messages are read from an inbound (RabbitMQ) message queue via an
Inbound Endpoint. If a failure occurs, the transaction will roll back.
This avoids the loss of the message.

!!! Tip
    If you are using a RabbitMQ Inbound Endpoint for receiving messages, set the scope of the `SET_ROLLBACK_ONLY` property to
`default` as follows:

    <property name="SET_ROLLBACK_ONLY" scope="default" type="STRING" value="true"/>

As shown in the below example, you need to set the
`           SET_ROLLBACK_ONLY          ` property to **true** in the
fault handler (e.g., the fault sequence), to roll the message back when
a failure occurs.

### Synapse configuration

Given below are the synapse artifact configuration for this use case. Note that the fault sequence contains the `SET_ROLLBACK_ONLY` property set to **true**.

```xml tab='Inbound Endpoint'
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

```xml tab='Fault Sequence'
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