# Using the ESB as a JMS Consumer

This section describes how to configure the ESB Profile of WSO2
Enterprise Integrator (WSO2 EI) to listen to a JMS Queue.

-   [One-way messaging](#UsingtheESBasaJMSConsumer-One-waymessaging)
-   [Two-way HTTP back-end
    call](#UsingtheESBasaJMSConsumer-Two-wayHTTPback-endcall)
    -   [Defining the content type of incoming JMS
        messages](#UsingtheESBasaJMSConsumer-DefiningthecontenttypeofincomingJMSmessages)

### One-way messaging

![](attachments/119130303/119130304.png){width="570"}

Follow the steps below to configure the ESB Profile of WSO2 EI to listen
to a JMS queue, consume messages, and send them to a HTTP back-end
service.

1.  Configure the ESB Profile of WSO2 EI with Apache ActiveMQ and set up
    the JMS listener. For instructions, see [Configure with
    ActiveMQ](_Configure_with_ActiveMQ_) .

2.  Create a proxy service with the following configuration. To create a
    proxy service using Tooling, see [Working with Proxy Services via
    Tooling](https://docs.wso2.com/display/EI650/Creating+a+Proxy+Service)
    .

    ``` xml
        <proxy xmlns="http://ws.apache.org/ns/synapse" name="JMStoHTTPStockQuoteProxy" transports="jms">
               <target>
                   <inSequence>
                       <property action="set" name="OUT_ONLY" value="true"/>
                   </inSequence>
                   <endpoint>
                       <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                   </endpoint>
                   <outSequence/>
               </target>
        </proxy>
    ```

        !!! tip
    
        Proxy Service Configuration
    
        The `           OUT_ONLY          ` property is set to
        `           true          ` to indicate that the message exchange is
        one-way.
    
        You can make the proxy service a JMS listener by setting its
        transport as `           jms          ` . Once the JMS transport is
        enabled for a proxy service, the ESB Profile of WSO2 EI starts
        listening on a JMS queue with the same name as the proxy service.
    
        In the sample configuration above, the ESB Profile of WSO2 EI
        listens to a JMS queue named
        `           JMStoHTTPStockQuoteProxy          ` . To make the proxy
        service listen to a different JMS queue, define the
        `           transport.jms.Destination          ` parameter with the
        name of the destination queue. For details, see
        [below](#UsingtheESBasaJMSConsumer-differentQueueName) .
    

3.  To test this scenario you need an HTTP back-end service.
    [Deploy](https://docs.wso2.com/display/EI600/Setting+Up+the+Service+Bus+Samples#SettingUptheServiceBusSamples-Deployingsampleback-endservices)
    the SimpleStockQuoteService and [start the Axis2
    server](https://docs.wso2.com/display/EI600/Setting+Up+the+Service+Bus+Samples#SettingUptheServiceBusSamples-StartingtheAxis2server)
    .
4.  Place a message in the ActiveMQ queue by executing the following
    command from the
    `           <EI_HOME>/samples/axis2Client          ` folder.

    ``` xml
        ant stockquote -Dmode=placeorder -Dtrpurl="jms:/JMStoHTTPStockQuoteProxy?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&java.naming.provider.url=tcp://localhost:61616&transport.jms.ContentTypeProperty=Content-Type&transport.jms.DestinationType=queue"
    ```

    The ESB Profile of WSO2 EI will read the message from the ActiveMQ
    queue and send it to the back-end service. You will see the
    following response on the Axis2 Server console:

    ``` xml
            Fri Dec 16 10:21:11 GST 2016 samples.services.SimpleStockQuoteService  :: Accepted order #1 for : 7424 stocks of IBM at $ 156.74347214873563
    ```

### Two-way HTTP back-end call

In addition to one-way invocations, the proxy service can listen to the
queue, pick up a message and do a two-way HTTP call as well. It allows
the response to be delivered to a queue specified by the client. This is
done by specifying a `         ReplyDestination        ` element when
placing a request message to a JMS queue. The scenario is depicted in
the diagram below.

![](attachments/33136192/33348782.png){width="600"}

We can have a proxy service similar to the following to simulate a
two-way invocation:

  

``` html/xml
    <proxy xmlns="http://ws.apache.org/ns/synapse"
           name="JMStoHTTPStockQuoteProxy1"
           transports="jms"
           startOnLoad="true"
           trace="disable">
       <description/>
       <target> 
          <inSequence>
             <send>
                <endpoint>
                   <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                </endpoint>
             </send>
          </inSequence>
          <outSequence/>
       </target>
       <parameter name="transport.jms.ContentType">
          <rules>
             <jmsProperty>contentType</jmsProperty>
             <default>text/xml</default>
          </rules>
       </parameter>
    </proxy>
```

!!! tip

For a two-way JMS scenario the `          OUT_ONLY         ` property is
not used.

Use the following command from the
`          <EI_HOME>/samples/axis2Client         ` folder. Note how the
`          transport.jms.ReplyDestination         ` element is
specified.

``` xml
    ant stockquote -Dsymbol=WSO2 -Dtrpurl="jms:/JMStoHTTPStockQuoteProxy1?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&java.naming.provider.url=tcp://localhost:61616&transport.jms.ContentTypeProperty=Content-Type&transport.jms.DestinationType=queue&transport.jms.ReplyDestination=ResponseQueue" 
```
    
    You can view the responses from the back-end service in the
    `          ResponseQueue         ` by accessing the ActiveMQ management
    console using the URL <http://0.0.0.0:8161/admin> and using
    `          admin         ` as both the username and password.
    

#### Defining the content type of incoming JMS messages

By default, the ESB Profile of WSO2 EI considers all messages consumed
from a queue as SOAP messages. To consider messages consumed from a
queue as a different format, define the **transport.jms.ContentType**
parameter with the respective content type as a proxy service
parameter.  
  
To demonstrate this, let's modify the [above
configuration](#UsingtheESBasaJMSConsumer-sample) as follows:

``` java
     <proxy xmlns="http://ws.apache.org/ns/synapse" name="JMStoHTTPStockQuoteProxy" transports="jms">
           <target>
               <inSequence>
                   <property action="set" name="OUT_ONLY" value="true"/>
                   <send>
                       <endpoint>
                           <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                       </endpoint>
                   </send>
               </inSequence>
               <outSequence/>
           </target>
           <parameter name="transport.jms.ContentType">
               <rules>
                   <jmsProperty>contentType</jmsProperty>
                   <default>application/xml</default>
               </rules>
           </parameter>
           <parameter name="transport.jms.Destination">MyJMSQueue</parameter>
       </proxy>
```

!!! tip

Proxy Service Configuration

You can specify a different content type within the
`         transport.jms.ContentType        ` parameter. In the sample
configuration above, the content type defined is
`         application/xml        ` .

If you want the proxy service to listen to a queue where the queue name
is different from the proxy service name, you can specify the queue name
using `         transport.jms.Destination        ` parameter. In the
sample configuration above, the ESB Profile of WSO2 EI listens to a JMS
queue named `         MyJMSQueue        ` .

