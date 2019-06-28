# RabbitMQ Use Cases

The following are some of the main RabbitMQ use cases of WSO2 EI.

-   [WSO2 EI as a RabbitMQ Message
    Consumer](#RabbitMQUseCases-WSO2EIasaRabbitMQMessageConsumer)
    -   [Prerequisites](#RabbitMQUseCases-Prerequisites)
    -   [Configure the sample](#RabbitMQUseCases-Configurethesample)
    -   [Execute the sample
        client](#RabbitMQUseCases-Executethesampleclient)
    -   [Analyzing the output](#RabbitMQUseCases-Analyzingtheoutput)
-   [WSO2 EI as a RabbitMQ Message
    Producer](#RabbitMQUseCases-WSO2EIasaRabbitMQMessageProducer)
    -   [Prerequisites](#RabbitMQUseCases-Prerequisites.1)
    -   [Configure the sample](#RabbitMQUseCases-Configurethesample.1)
    -   [Execute the sample
        client](#RabbitMQUseCases-Executethesampleclient.1)
    -   [Analyzing the output](#RabbitMQUseCases-Analyzingtheoutput.1)
-   [Remote Procedure Call(RPC) with
    RabbitMQ](#RabbitMQUseCases-RemoteProcedureCall(RPC)withRabbitMQ)

### WSO2 EI as a RabbitMQ Message Consumer

This section describes how WSO2 Enterprise Integrator(WSO2 EI) can be
configured as a RabbitMQ message consumer.

![](attachments/119130373/119130376.png)

The following is a sample scenario that demonstrates how WSO2 EI is
configured to listen to a rabbitMQ queue, consume messages, and send the
messages to an HTTP back­-end service.

!!! info

Note

To create proxy services, sequences, endpoints, message stores and
message processors in WSO2 EI, you can either use the management console
or copy the XML configuration to the source view. To access the source
view on the WSO2 EI management console, go to **Manage** -\> **Service
Bus** -\> **Source View** .


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
