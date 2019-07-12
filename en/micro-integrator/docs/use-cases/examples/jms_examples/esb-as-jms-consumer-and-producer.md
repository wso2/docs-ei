# Using the ESB as JMS Consumer and Producer

This section describes how to configure the ESB Profile of WSO2
Enterprise Integrator (WSO2 EI) to work as a JMS-to-JMS proxy service.

![](attachments/119130309/119130312.png){width="570"}

Follow the steps below to configure the ESB Profile of WSO2 EI to listen
to a JMS queue and consume messages, and then send those messages to
another JMS queue.

1.  Configure the ESB Profile of WSO2 EI with Apache ActiveMQ and set up
    the JMS listener and sender. For instructions, see [Configure with
    ActiveMQ](_Configure_with_ActiveMQ_) .  
2.  Create a proxy service with the following configuration. To create a
    proxy service using Tooling, see [Working with Proxy Services via
    Tooling](https://docs.wso2.com/display/EI650/Creating+a+Proxy+Service)
    .

    ``` java
        <proxy name="StockQuoteProxy" transports="jms">
           <target>
               <inSequence>
                   <property action="set" name="OUT_ONLY" value="true"/>
                   <send>
                       <endpoint>
                          <address uri=""/> <!-- Specify the JMS connection URL here -->
                       </endpoint>
                   </send>
               </inSequence>
           </target>
        </proxy>
    ```

        !!! tip
    
        Proxy service configuration
    
        In the sample configuration above, the 'transports' property is set
        to 'jms', which allows the ESB to receive JMS messages. This proxy
        service listens to a JMS queue named
        `           StockQuoteProxy          ` and sends messages to another
        queue named `           SimpleStockQuoteService.          ` The
        `           OUT_ONLY          ` property is set to
        `           true          ` , which indicates that the message
        exchange is one-way. To send a message to a JMS queue, you should
        define the JMS connection URL as the endpoint address (which should
        be invoked via the **Send** mediator). There are two ways to specify
        the endpoint URL
    
        -   Specify the JNDI name of the JMS queue and the [connection
            factory
            parameters](JMS-Transport_119130301.html#JMSTransport-ConnectionFactoryParams)
            in the JMS connection URL in the exampe below. Values of
            connection factory parameters depend on the type of the JMS
            broker .
    
            **When the broker is ActiveMQ**
    
        ``` java
                jms:/SimpleStockQuoteService?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&java.naming.provider.url=tcp://localhost:61616&transport.jms.DestinationType=queue
        ```
        
                **When the broker is WSO2 Message Broker**
        
        ``` java
                    jms:/StockQuotesQueue?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.wso2.andes.jndi.PropertiesFileInitialContextFactory&amp;java.naming.provider.url=conf/jndi.properties&transport.jms.DestinationType=queue
        ```
            
                      
            
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
        

3.  To place a message into a JMS queue, execute following command from
    the `          <EI_HOME>/samples/axis2Client         ` directory.

``` java
    ant stockquote -Dmode=placeorder -Dtrpurl="jms:/StockQuoteProxy?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&java.naming.provider.url=tcp://localhost:61616&transport.jms.ContentTypeProperty=Content-Type&transport.jms.DestinationType=queue"
```

!!! tip

You can view the ActiveMQ queues by accessing the ActiveMQ management
console using the URL
`                                                       http://0.0.0.0:8161/admin                                                  `
and using `         admin        ` as both the username and password.


Generally, JMS is used for one-way, asynchronous message exchange.
However you can perform synchronous messaging also with JMS. For more
information, see [JMS Synchronous Invocations : Dual Channel
HTTP-to-JMS](https://docs.wso2.com/display/EI611/JMS+Synchronous+Invocations+%3A+Dual+Channel+HTTP-to-JMS)
and [JMS Synchronous Invocations : Quad Channel
JMS-to-JMS](https://docs.wso2.com/display/EI611/JMS+Synchronous+Invocations+%3A+Quad+Channel+JMS-to-JMS)
.
