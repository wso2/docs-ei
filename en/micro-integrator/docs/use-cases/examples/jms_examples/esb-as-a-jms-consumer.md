# Micro Integrator as a JMS Consumer
This section describes how to configure WSO2 Micro Integrator to listen to a JMS Queue.

## Example 1: One-way messaging

In this example, the Micro Integrator listens to a JMS queue, consume messages, and sends them to an HTTP back-end service.

![](attachments/119130303/119130304.png){width="570"}

### Synape configuration

Given below is the synapse configuration of the proxy service that mediates the above use case. Note that you need to update the JMS connection URL according to your broker as explained below.

```
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

The Synapse artifacts used are explained below.

<table>
    <tr>
        <th>Artifact Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>
            Proxy Service
        </td>
        <td>
            A proxy service is used to receive messages and to define the message flow. You can make the proxy service a JMS listener by setting its transport as <code>jms</code>. Once the JMS transport is enabled for a proxy service, the Micro Integrator listens on a JMS queue for the same name as the proxy service.</br>
            In the sample configuration above, the Micro Integrator listens to a JMS queue named <code>JMStoHTTPStockQuoteProxy</code>. To make the proxy service listen to a different JMS queue, define the <code>transport.jms.Destination</code> parameter with the name of the destination queue.
        </td>
    </tr>
    <tr>
        <td>Endpoint Mediator</td>
        <td>
           To send a message to the HTTP backend, you should define the connection URL as the endpoint address. 
        </td>
    </tr>
</table>

### Run the Example

1.  Configure the Micro Integrator with Apache ActiveMQ and set up the JMS Sender.
2.  Start WSO2 Integration Studio and create a proxy service with the above configuration. You can copy the synapse configuration given above to the **Source View** of your proxy service.
3.  To test this scenario you need an HTTP back-end service. Deploy the SimpleStockQuoteService and start the Axis2 server.
4.  Place a message in the ActiveMQ queue by executing the following command from the `MI_HOME/samples/axis2Client` folder.

    ```
    ant stockquote -Dmode=placeorder -Dtrpurl="jms:/JMStoHTTPStockQuoteProxy?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&java.naming.provider.url=tcp://localhost:61616&transport.jms.ContentTypeProperty=Content-Type&transport.jms.DestinationType=queue"
    ```

The Micro Integrator reads the message from the ActiveMQ queue and sends it to the back-end service. You will see the following response on the Axis2 Server console:

```
Fri Dec 16 10:21:11 GST 2016 samples.services.SimpleStockQuoteService  :: Accepted order #1 for : 7424 stocks of IBM at $ 156.74347214873563
```

## Example 2: Two-way HTTP back-end call

In addition to one-way invocations, the proxy service can listen to the queue, pick up a message and do a two-way HTTP call as well. It allows the response to be delivered to a queue specified by the client. This is done by specifying a `         ReplyDestination        ` element when placing a request message to a JMS queue. The scenario is depicted in
the diagram below.

![](attachments/33136192/33348782.png){width="600"}

### Synapse configuration

We can have a proxy service similar to the following to simulate a two-way invocation:

```
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
The Synapse artifacts used are explained below.

<table>
    <tr>
        <th>Artifact Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>
            Proxy Service
        </td>
        <td>
            A proxy service is used to receive messages and to define the message flow. You can make the proxy service a JMS listener by setting its transport as <code>jms</code>. Once the JMS transport is enabled for a proxy service, the Micro Integrator listens on a JMS queue for the same name as the proxy service.</br>
            In the sample configuration above, the Micro Integrator listens to a JMS queue named <code>JMStoHTTPStockQuoteProxy1</code>. To make the proxy service listen to a different JMS queue, define the <code>transport.jms.Destination</code> parameter with the name of the destination queue.
        </td>
    </tr>
    <tr>
        <td>Send Mediator</td>
        <td>
           To send a message to the HTTP backend, you should define the connection URL as the endpoint address. 
        </td>
    </tr>
</table>

### Run the Example

1.  Configure the Micro Integrator with Apache ActiveMQ and set up the JMS Sender.
2.  Start WSO2 Integration Studio and create a proxy service with the above configuration. You can copy the synapse configuration given above to the **Source View** of your proxy service.
3.  To test this scenario you need an HTTP back-end service. Deploy the SimpleStockQuoteService and start the Axis2 server.
4.  Place a message in the ActiveMQ queue by executing the following command from the `MI_HOME/samples/axis2Client` folder. Note how the `transport.jms.ReplyDestination` element is specified.

    ```
    ant stockquote -Dsymbol=WSO2 -Dtrpurl="jms:/JMStoHTTPStockQuoteProxy1?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&java.naming.provider.url=tcp://localhost:61616&transport.jms.ContentTypeProperty=Content-Type&transport.jms.DestinationType=queue&transport.jms.ReplyDestination=ResponseQueue" 
    ```
    
You can view the responses from the back-end service in the `ResponseQueue` by accessing the ActiveMQ management console using the URL <http://0.0.0.0:8161/admin> and using `admin` as both the username and password.
    
## Example 3: Set content type of incoming JMS messages

By default, the Micro Integrator considers all messages consumed from a queue as SOAP messages. To consider messages consumed from a queue as a different format, define the **transport.jms.ContentType** parameter with the respective content type as a proxy service parameter.  
  
To demonstrate this, let's modify the above configuration as follows:

```
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

The Synapse artifacts used are explained below.

<table>
    <tr>
        <th>Artifact Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>
            Proxy Service
        </td>
        <td>
            You can specify a different content type within the <code>transport.jms.ContentType</code> parameter. In the sample configuration above, the content type defined is <code>application/xml</code>.
            If you want the proxy service to listen to a queue where the queue name is different from the proxy service name, you can specify the queue name using <code>transport.jms.Destination</code> parameter. In the sample configuration above, the Micro Integrator listens to a JMS queue named <b>MyJMSQueue</b>.
        </td>
    </tr>
    <tr>
        <td>Send Mediator</td>
        <td>
           To send a message to the HTTP backend, you should define the connection URL as the endpoint address. 
        </td>
    </tr>
</table>

## Performance Tuning

You can improve the performance of this scenario by following the steps
below.

-   If the queue gets filled up at a high rate, and the queue is long,
    you can improve the performance by increasing the number of
    concurrent consumers. Add the following parameters to the JMS
    listener configuration of the esb.toml file to increasing the number of concurrent consumers:  

    ``` java
    [mediation]
    transport.jms.ConcurrentConsumers=50
    transport.jms.MaxConcurrentConsumers=50
    ```

-   Add the following parameter to the JMS listener configuration to enable
    JMS listener caching:  

    ```java
    [mediation]
     transport.jms.CacheLevel=consumer
    ```

    The possible values for the cache level are
    `           none          ` , `           auto          ` ,
    `           connection          ` , `           session          `
    and `           consumer          ` . Out of the possible values,
    `           consumer          ` is the highest level that provides
    maximum performance.

