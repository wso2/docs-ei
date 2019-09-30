# Publish-Subscribe with JMS

JMS supports two models for messaging as follows:

- Queues: point-to-point
- Topics: publish and subscribe  

There are many business use cases that can be implemented using the publisher-subscriber (pub-sub) pattern. For example, consider a blog with subscribed readers. The blog author posts a blog entry that the subscribers of the blog can view. In other words, the blog author publishes a message (the blog post content) and the subscribers (the blog readers) receive that message. Popular publisher/subscriber patterns like these can be implemented using JMS topics.

In this sample scenario, two proxy services in the Micro Integrator acts as the publisher and subscriber to a topic defined in the message broker. 

![](attachments/119130316/119130317.png){width="380"}  

When the `stockquote` client sends the message to the StockQuoteProxy service, the publisher is invoked and sends the message to the JMS topic. The topic delivers the message to all the subscribers of that topic. In this case, the subscribers are Micro Integrator proxy services.

## Synapse configurations

Shown below are the synapse artifacts that are used to define this use case.

``` java tab="Proxy Service (Publisher)"
<definitions xmlns="http://ws.apache.org/ns/synapse">
    <proxy name="StockQuoteProxy" transports="http" startOnLoad="true" trace="disable">
          <target>
            <endpoint>
              <address uri="jms:/SimpleStockQuoteService?transport.jms.ConnectionFactoryJNDIName=TopicConnectionFactory&amp;java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;java.naming.provider.url=tcp://localhost:61616&amp;transport.jms.DestinationType=topic"/>
            </endpoint>
            <inSequence>
              <property name="OUT_ONLY" value="true"/>
            </inSequence>
            <outSequence>
              <send/>
             </outSequence>
          </target>
     </proxy>
</definitions>
```

``` java tab="Proxy Service (Subscriber 1)"
<proxy name="SimpleStockQuoteService1" transports="jms" startOnLoad="true" trace="disable">
   <description/>
      <target>
        <inSequence>
          <property name="OUT_ONLY" value="true"/>
          <log level="custom">
            <property name="Subscriber1" value="I am Subscriber1"/>
          </log>
          <drop/>
        </inSequence>
        <outSequence>
           <send/>
        </outSequence>
      </target>
  <parameter name="transport.jms.ContentType">
      <rules>
        <jmsProperty>contentType</jmsProperty>
        <default>application/xml</default>
      </rules>
  </parameter>
  <parameter name="transport.jms.ConnectionFactory">myTopicConnectionFactory</parameter>
  <parameter name="transport.jms.DestinationType">topic</parameter>
  <parameter name="transport.jms.Destination">SimpleStockQuoteService</parameter>
</proxy>
```

``` java tab="Proxy Service (Subscriber 2)"
<proxy name="SimpleStockQuoteService2" transports="jms" startOnLoad="true" trace="disable">
  <target>
    <inSequence>
      <property name="OUT_ONLY" value="true"/>
      <log level="custom">
        <property name="Subscriber2" value="I am Subscriber2"/>
      </log>
      <drop/>
    </inSequence>
    <outSequence>
      <send/>
    </outSequence>
  </target>
  <parameter name="transport.jms.ContentType">
    <rules>
      <jmsProperty>contentType</jmsProperty>
      <default>application/xml</default>
    </rules>
  </parameter>
  <parameter name="transport.jms.ConnectionFactory">myTopicConnectionFactory</parameter>
  <parameter name="transport.jms.DestinationType">topic</parameter>
  <parameter name="transport.jms.Destination">SimpleStockQuoteService</parameter>
</proxy>
```

See the descriptions of the above configurations:

<table>
  <tr>
    <th>Artifact</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Publisher</td>
    <td>
      This proxy service (StockQuoteProxy) isd configure to publish message to the `SimpleStockQuoteService` topic in the broker. Note that there are two ways to define the endpoint URL:
      <ul>
        <li>
          Specify the JNDI name of the JMS topic and the connection factory parameters in the connection URL as shown in the above example.
        </li>
        <li>
          If you have already specified the endpoint's connection factory parameters (for the JMS sender configuration) in the axis2.xml file, the connection URL in the proxy service should be as shown below. In this example, the endpoint URL of the proxy service refers the relevant connection factory in the axis2.xml file. </br></br>
          <b>When the broker is ActiveMQ</b></br>
          <code>jms:transport.jms.ConnectionFactory=QueueConnectionFactory</code></br></br>
          <b>When the broker is WSO2 Message Broker</b></br>
          <code>jms:/StockQuotesQueue?transport.jms.ConnectionFactory=QueueConnectionFactory</code></br>
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Subscriber 1</td>
    <td>Proxy service that consumes messages from the broker.</td>
  </tr>
  <tr>
    <td>Subscriber 2</td>
    <td>Proxy service that consumes messages from the broker.</td>
  </tr>
</table>

## Running the Example

Publishing to the topic:

1. Configure the Micro Integrator (Publisher) with Apache ActiveMQ.
2. Start the Broker.
3. Start WSO2 Integration Studio and create artifacts with the above configuration. You can copy the synapse configuration given above to the **Source View** of your proxy service.
4. Start WSO2 Micro Integrator. A log message similar to the following will appear:
   ``` java
   INFO {org.wso2.andes.server.store.CassandraMessageStore} -  Created Topic : SimpleStockQuoteService
   INFO {org.wso2.andes.server.store.CassandraMessageStore} -  Registered Subscription tmp_127_0_0_1_44759_1 for Topic SimpleStockQuoteService
   ```
3.  To invoke the publisher, use the sample `stock quote` client service by navigating to the `MI_HOME/samples/axis2Client` directory and running the following command:

    ``` java
    ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy -Dmode=placeorder -Dsymbol=MSFT 
    ```

## Related Examples

There can be many types of publishers and subscribers for a given JMS topic. The following article in the WSO2 library provides more information on different types of publishers and subscribers:
<http://wso2.org/library/articles/2011/12/wso2-esb-example-pubsub-soa> .