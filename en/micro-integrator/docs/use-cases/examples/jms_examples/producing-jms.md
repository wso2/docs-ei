# Producing JMS Messages

This section describes how to configure WSO2 Micro Integrator to send messages to a JMS Queue. In this example, the Micro Integrator accepts messages via HTTP and sends them to a JMS queue.

## Synapse configuration

Given below is the synapse configuration of the proxy service that mediates the above use case. Note that you need to update the JMS connection URL according to your broker as explained below.

```xml
<proxy xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteProxy" transports="http">
        <target>
            <inSequence>
                <property action="set" name="OUT_ONLY" value="true"/>
                <send>
                    <endpoint>
                        <address uri=""/> <!-- Specify the JMS connection URL here -->
                    </endpoint>
                </send>
            </inSequence>
            <outSequence/>
        </target>
        <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
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
            A proxy service is used to receive messages and to define the message flow.
        </td>
    </tr>
    <tr>
        <td>Property Mediator</td>
        <td>
            The <b>OUT ONLY</b> property is set to <b>true</b> , which indicates that the message exchange is one-way. 
        </td>
    </tr>
    <tr>
        <td>Send Mediator</td>
        <td>
           To send a message to a JMS queue, you should define the JMS connection URL as the endpoint address (which should be invoked via the <b>Send</b> mediator). There are two ways to specify the endpoint URL: 
           <ul>
               <li>
                    Specify the JNDI name of the JMS queue and the connection factory parameters in the JMS connection URL as shown in the exampe below. Values of connection factory parameters depend on the type of the JMS broker. </br></br>
                    <b>When the broker is ActiveMQ</b></br>
                    <code>jms:/SimpleStockQuoteService?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&java.naming.provider.url=tcp://localhost:61616&transport.jms.DestinationType=queue</code></br></br>
                    <b>When the broker is WSO2 Message Broker</b></br>
                    <code>jms:/StockQuotesQueue?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.wso2.andes.jndi.PropertiesFileInitialContextFactory&amp;java.naming.provider.url=conf/jndi.properties&transport.jms.DestinationType=queue</code>
               </li></br>
               <li>
                    If you have already specified the endpoint's connection factory parameters (for the JMS sender configuration) in the axis2.xml file, the connection URL in the proxy service should be as shown below. In this example, the endpoint URL of the proxy service refers the relevant connection factory in the axis2.xml file: </br></br>
                    <b>When the broker is ActiveMQ</b></br>
                    <code>jms:transport.jms.ConnectionFactory=QueueConnectionFactory</code></br></br>
                    <b>When the broker is WSO2 Message Broker</b></br>
                    <code>jms:/StockQuotesQueue?transport.jms.ConnectionFactory=QueueConnectionFactory</code>
               </li>
           </ul>
        </td>
    </tr>
</table>
      
!!! Note
    Be sure to replace the ' `& ` ' character in the endpoint URL with '`&amp;`' to avoid the following exception:
    ``` java
    com.ctc.wstx.exc.WstxUnexpectedCharException: Unexpected character '=' (code 61); expected a semi-colon after the reference for entity 'java.naming.factory.initial' at [row,col {unknown-source}
    ```  