# Publish-Subscribe with JMS

JMS supports two models for messaging as follows:

-   Queues: point-to-point
-   Topics: publish and subscribe  

There are many business use cases that can be implemented using the
publisher-subscriber (pub-sub) pattern. For example, consider a blog
with subscribed readers. The blog author posts a blog entry that the
subscribers of the blog can view. In other words, the blog author
publishes a message (the blog post content) and the subscribers (the
blog readers) receive that message. Popular publisher/subscriber
patterns like these can be implemented using JMS topics as described in
the following sections:

-   [Scenario overview](#Publish-SubscribewithJMS-Scenariooverview)
-   [Configuring the broker
    server](#Publish-SubscribewithJMS-Configuringthebrokerserver)
-   [Configuring the
    publisher](#Publish-SubscribewithJMS-Configuringthepublisher)
-   [Configuring the
    subscribers](#Publish-SubscribewithJMS-Configuringthesubscribers)
-   [Publishing to the
    topic](#Publish-SubscribewithJMS-Publishingtothetopic)

### Scenario overview

In this sample scenario, you create a JMS topic in a broker server such
as ActiveMQ or the Message Broker Profile of WSO2 Enterprise Integrator
(WSO2 EI), and then you add proxy services using the ESB Profile of WSO2
EI to act as the publisher and subscribers. This example assumes you
have already downloaded and installed the ESB Profile of WSO2 EI (see
[Installation
Guide](https://docs.wso2.com/display/EI650/Installation+Guide) ).  

![](attachments/119130316/119130317.png){width="380"}  

### Configuring the broker server

For this example, you will use ActiveMQ as the broker server. Follow the
instructions in [Configure with ActiveMQ](_Configure_with_ActiveMQ_) to
set up the ESB Profile of WSO2 EI with ActiveMQ as the broker.

### Configuring the publisher

1.  Open the `           <EI_HOME>/conf/JNDI.properties          ` file
    and specify the JNDI designation of the topic (in this example it is
    `           SimpleStockQuoteService          ` ):

    ``` java
        # register some queues in JNDI using the form
        # queue.[jndiName] = [physicalName]
        queue.MyQueue = example.MyQueue
         
        # register some topics in JNDI using the form
        # topic.[jndiName] = [physicalName]
        topic.MyTopic = example.MyTopic
        topic.SimpleStockQuoteService = SimpleStockQuoteService
    ```

2.  Next, [add a proxy
    service](https://docs.wso2.com/display/EI650/Creating+a+Proxy+Service)
    named `           StockQuoteProxy          ` and configure it to
    publish to the topic `           SimpleStockQuoteService          `
    . You can add the proxy service to the ESB either by building the
    proxy service in the design view via the Management Console of the
    ESB Profile or by copying the XML configuration into the source
    view. Alternatively, you can add an XML file named
    `           StockQuoteProxy.xml          ` to the
    `           <EI_HOME>/repository/deployment/server/synapse-configs/default/proxy-services          `
    . A sample XML code segment that defines the proxy service is given
    below. You will see that the address URI specifies properties to
    configure the [JMS transport](_JMS_Transport_) .

    ``` html/xml
            <definitions xmlns="http://ws.apache.org/ns/synapse">
               <proxy name="StockQuoteProxy"
                      transports="http"
                      startOnLoad="true"
                      trace="disable">
                  <target>
                     <endpoint>
        
                        <address 
            uri="jms:/SimpleStockQuoteService?transport.jms.ConnectionFactoryJNDIName=TopicConnectionFactory&amp;java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;java.naming.provider.url=tcp://localhost:61616&amp;transport.jms.DestinationType=topic"/>
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

        !!! tip
    
        Note that there are two ways to define the endpoint URL:
    
        -   Specify the JNDI name of the JMS topic and the [connection
            factory
            parameters](JMS-Transport_119130301.html#JMSTransport-ConnectionFactoryParams)
            in the connection URL as shown in the above example.
    
        -   If you have already specified the endpoint's connection factory
            parameters (for the JMS sender configuration) in the
            `             axis2.xml            ` file (stored in the
            `             <EI_HOME>/conf/axis2/            ` directory), the
            connection URL in the proxy service should be as shown below. In
            this example, the endpoint URL of the proxy service refers the
            relevant connection factory in the axis2.xml file:
    
            **When the broker is ActiveMQ**
    
        ``` java
                jms:transport.jms.ConnectionFactory=QueueConnectionFactory
        ```
        
                **When the broker is WSO2 Message Broker**
        
        ``` java
                    jms:/StockQuotesQueue?transport.jms.ConnectionFactory=QueueConnectionFactory
        ```
            
                !!! note
            
                Be sure to replace the ' `           &          ` ' character in the
                endpoint URL with ' `           &amp;          ` ' to avoid the
                following exception:
            
    ``` java
            com.ctc.wstx.exc.WstxUnexpectedCharException: Unexpected character '=' (code 61); expected a semi-colon after the reference for entity 'java.naming.factory.initial' at [row,col {unknown-source}
    ```
        

### Configuring the subscribers

Next, you need to configure two proxy services that subscribe to the JMS
topic `         SimpleStockQuoteService        ` , so that whenever the
topic receives a message, it is sent to the subscribing proxy services.
Following is the sample configuration for the proxy services:

``` html/xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <proxy name="SimpleStockQuoteService1"
              transports="jms"
              startOnLoad="true"
              trace="disable">
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
    
       <proxy name="SimpleStockQuoteService2"
              transports="jms"
              startOnLoad="true"
              trace="disable">
          <description/>
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
    </definitions>
```

### Publishing to the topic

1.  Open a command prompt (or a shell in Linux), go to the
    `           <EI_HOME>\bin          ` directory and execute one of
    the following commands to start the ESB Profile of WSO2 EI:

    -   On Windows: `            integrator.bat --run           `
    -   On Linux/Mac OS: `             sh integrator.sh            `

2.  A log message similar to the following will appear:

    ``` java
            INFO {org.wso2.andes.server.store.CassandraMessageStore} -  Created Topic : SimpleStockQuoteService
            INFO {org.wso2.andes.server.store.CassandraMessageStore} -  Registered Subscription tmp_127_0_0_1_44759_1 for Topic SimpleStockQuoteService
    ```

3.  To invoke the publisher, use the sample
    `           stock quote          ` client service by navigating to
    the `           <EI_HOME>/samples/axis2Client          ` directory
    and running the following command:

    ``` java
            ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy -Dmode=placeorder -Dsymbol=MSFT 
    ```

    The message flow is executed as follows:

-   -   When the `             stockquote            ` client sends the
        message to the StockQuoteProxy service, the publisher is invoked
        and sends the message to the JMS topic.

    -   The topic delivers the message to all the subscribers of that
        topic. In this case, the subscribers are ESB proxy services.

!!! info There can be many types of publishers and subscribers for a
given JMS topic. The following article in the WSO2 library provides more
information on different types of publishers and subscribers:
<http://wso2.org/library/articles/2011/12/wso2-esb-example-pubsub-soa> .
