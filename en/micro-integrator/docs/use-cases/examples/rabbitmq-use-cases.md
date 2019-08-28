# RabbitMQ Use Cases

The following are some of the main RabbitMQ use cases of WSO2 EI.

## Using the RabbitMQ Message Store

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

## WSO2 EI as a RabbitMQ Message Consumer

This section describes how WSO2 Enterprise Integrator(WSO2 EI) can be
configured as a RabbitMQ message consumer.

The following is a sample scenario that demonstrates how WSO2 EI is
configured to listen to a rabbitMQ queue, consume messages, and send the
messages to an HTTP back­-end service.

!!! Info
    To create proxy services, sequences, endpoints, message stores and message processors in WSO2 EI, you can either use the management console or copy the XML configuration to the source view. To access the source view on the WSO2 EI management console, go to **Manage** -\> **Service Bus** -\> **Source View**.

### Prerequisites

-   Configure the RabbitMQ AMQP transport. For information on how to
    configure the transport, see [Configuring the RabbitMQ AMQP
    transport](RabbitMQ-AMQP-Transport_119130371.html#RabbitMQAMQPTransport-ConfigRabbitMQ)
    .
-   Start the WSO2 EI server.

### Configure the sample

1.  Create a custom proxy service with the following configuration. For
    more information on creating proxy services, see [Working with Proxy
    Services](https://docs.wso2.com/display/EI650/Working+with+Proxy+Services)
    .

    ``` java
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

2.  WSO2 EI comes with a default Axis2 server, which you can use as the
    back-end service for this sample. To start the Axis2
    server, navigate to
    `          <EI_HOME>/samples/axis2server         ` , and run
    `          axis2Server.sh         ` on Linux or
    `          axis2Server.bat         ` on Windows.
3.  Deploy the **SimpleStockQuoteService** client by navigating to
    `          <EI_HOME>/samples/axis2Server/src/SimpleStockQuoteService         `
    , and running the **ant** command on the command prompt or shell
    script. This will build the sample and deploy the service for
    you. For more information on sample back-end services, see
    [Deploying sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

Now you have a running WSO2 EI instance with a custom proxy service and
a back­-end service deployed. Next, we will send a message to the
back­-end service through WSO2 EI using a sample client.

#### Execute the sample client

Run the following client to publish a getquote request to the RabbitMQ
server exchange that is running on port `         5672        ` .

``` java
    ConnectionFactory factory =new ConnectionFactory();
    factory.setHost("localhost");
    factory.setUsername("guest");
    factory.setPassword("guest");
    factory.setPort(5672);
    Connection connection =factory.newConnection();
    Channel channel =connection.createChannel();
    channel.queueDeclare("queue",false,false,false,null);
    channel.exchangeDeclare("exchange","direct",true);
    channel.queueBind("queue","exchange","route");
    
    //The message to be sent
    Stringmessage ="<soapenv:Envelope xmlns:soapenv=\"http://schemas.xmlsoap.org/soap/envelope/\">\n" +
                     "<soapenv:Header/>\n" +
                     "<soapenv:Body>\n" +
                       "<m:placeOrder xmlns:m=\"http://services.samples\">\n" +
                         "<m:order>\n" +
                           "<m:price>100</m:price>\n" +
                           "<m:quantity>20</m:quantity>\n" +
                           "<m:symbol>RMQ</m:symbol>\n" +
                         "</m:order>\n" +
                       "</m:placeOrder>\n" +
                     "</soapenv:Body>\n" +
                     "</soapenv:Envelope>";
    
    //PopulatetheAMQPmessageproperties
    AMQP.BasicProperties.Builderbuilder =new AMQP.BasicProperties().builder();
    builder.contentType("text/xml");
    builder.contentEncoding(contentEncoding);
    
    Map<String, Object> headers = new HashMap<String, Object>();
    headers.put("SOAP_ACTION", "urn:placeOrder");
    builder.headers(headers);
     
    //Publishthemessagetoexchange
    channel.basicPublish("exchange","queue",builder.build(),message.getBytes());    
```

#### Analyzing the output

The direct exchange is bound to the queue with route­-key
`         queue        ` that is consumed by WSO2 EI's RabbitMQ
transport receiver. From there the message will be sent to the AMQPProxy
and it will be forwarded to the given http url.

If you analyze the console running the sample Axis2 server, you will see
the following message indicating that the server has accepted an order

``` java
    Accepted order #1 for : 7078 stocks of IBM at $ 73.73786002620719
```

### WSO2 EI as a RabbitMQ Message Producer

This section describes how WSO2 Enterprise Integrator(WSO2 EI) can be
used to send messages to a RabbitMQ queue.

![](attachments/119130373/119130375.png)

Following is a sample scenario that demonstrates how WSO2 EI is
configured to listen to HTTP requests and publish them to a RabbitMQ
server (message exchange).

!!! info

Note

To create proxy services, sequences, endpoints, message stores and
message processors in WSO2 EI, you can either use the management console
or copy the XML configuration to the source view. To access the source
view on the management console, go to **Manage** -\> **Service Bus** -\>
**Source View** .


#### Prerequisites

-   Configure the RabbitMQ AMQP transport. For information on how to
    configure the transport, see [Configuring the RabbitMQ AMQP
    transport](RabbitMQ-AMQP-Transport_119130371.html#RabbitMQAMQPTransport-ConfigRabbitMQ)
    .
-   Start the WSO2 EI server.

#### Configure the sample

1.  Create a custom proxy service with the following configuration. For
    more information on creating proxy services, see [Working with Proxy
    Services](https://docs.wso2.com/display/EI650/Working+with+Proxy+Services)
    .

    ``` java
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

2.  Use the following as a RabbitMQ consumer that will consume and
    display the incoming messages to the RabbitMQ queue.

    ``` java
            ConnectionFactory factory = new ConnectionFactory();
            factory.setHost("localhost");
            factory.setUsername("guest");
            factory.setPassword("guest");
            factory.setPort(5672);
        
            Connection connection = factory.newConnection();
            Channel channel = connection.createChannel();
            channel.queueDeclare("queue",false,false,false,null);
            channel.exchangeDeclare("exchange","direct",true);
            channel.queueBind("queue","exchange","route");
        
            //Createtheconsumer
            QueueingConsumer consumer = new QueueingConsumer(channel);
            channel.basicConsume("queue",true,consumer);
        
            //Startconsumingmessages
            while(true)
            {
                QueueingConsumer.Delivery delivery = consumer.nextDelivery();
                String message = new String(delivery.getBody());
            }
    ```

#### Execute the sample client

Execute the following command from
`         <EI_HOME>/sample/axis2Client        ` , to send an HTTP
message to the WSO2 EI proxy service.

``` java
    ant stockquote -Daddurl=http://localhost:8280/services/AMQPProducerSample -Dmode=placeorder
```

#### Analyzing the output

You will see that the HTTP request is sent to the given proxy service
and that it is forwarded to the RabbitMQ server via the RabbitMQ AMQP
transport sender. You can view the messages received at the RabbitMQ
queue in the RabbitMQ SimpleProducer console.

### Remote Procedure Call(RPC) with RabbitMQ

You can send request-response messages using the RabbitMQ transport by
implementing a Remote Procedure Call(RPC) scenario with RabbitMQ.

The following diagram illustrates a remote procedure call scenario with
RabbitMQ:

  

![](attachments/119130373/119130374.png)

The remote procedure call works as follows:

-   When WSO2 Enterprise Integrator(WSO2 EI) starts up, it creates an
    anonymous, exclusive callback queue.
-   For a remote procedure call request, WSO2 EI sends a message with
    the following properties:
    -   `            reply_to           ` : This is set to the callback
        queue
    -   `            correlation_id           ` : This is set to a
        unique value for every request.
-   The request is then sent to the rpc\_queue.
-   The RPC Server waits for requests on that queue. When a request
    appears, it does the job and sends a message with the result back to
    the WSO2 EI server, using the queue from the
    `           reply_to          ` field with the same
    `           correlation_id          ` .

-   WSO2 EI waits for data on the reply\_to queue. When a message
    appears, it checks the `          correlation_id         ` property.
    If it matches the value from the request, it returns the response to
    the application.

The following is a sample proxy service named
`         RabbitMQRPCProxy        ` that sends request-response messages
using the RabbitMQ transport.

``` xml
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

``` java
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

### Sample java clients

This section describes the sample Java clients that you can use to send
and receive AMQP messages. These clients can be used to test the
scenario where the Sender publishes a message to a RabbitMQ AMQP queue,
which is consumed by WSO2 EI and published to another queue, which in
turn is consumed by the Receiver. When you run these clients, the
Receiver will get the messages sent by the Sender, confirming that you
have correctly configured the RabbitMQ AMQP transport.

#### AMQP sender

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

#### AMQP receiver

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

## Rolling failed messages back

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