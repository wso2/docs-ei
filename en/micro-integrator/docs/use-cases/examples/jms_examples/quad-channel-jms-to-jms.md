# JMS Synchronous Invocations : Quad Channel JMS-to-JMS

The following diagram depicts quad-channel JMS synchronous invocations of the WSO2 Micro Integrator.

![](attachments/119130326/119130327.png)

## Synapse configuration 

The proxy service configuration.

```
<proxy name="QuadJMS" transports="jms" xmlns="http://ws.apache.org/ns/synapse">
          <target>
              <inSequence>
                  <property action="set" name="transport.jms.ContentTypeProperty" value="Content-Type" scope="axis2"/>
                  <log level="full" xmlns="http://ws.apache.org/ns/synapse"/>
                  <send>
                      <endpoint>
                          <address uri="jms:/BEReq?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;java.naming.provider.url=tcp://localhost:61616&amp;transport.jms.DestinationType=queue&amp;transport.jms.ReplyDestination=BERes"/>
                      </endpoint>
                  </send>
              </inSequence>
              <outSequence>
                  <send/>
              </outSequence>
          </target>
          <parameter name="transport.jms.ContentType">
              <rules>
                  <jmsProperty>contentType</jmsProperty>
                  <default>text/xml</default>
              </rules>
          </parameter>
          <parameter name="transport.jms.Destination">ClientReq</parameter>
</proxy>
```

The message flow of the above sample configuration is as follows:

1.  The **JMSReplyTo** property of the JMS message is set to **ClientRes** . Therefore, the c lient sends a JMS message to the
    **ClientReq** queue.  
2.  The **transport.jms.ReplyDestination** value is set to **BERes**. This enables the WSO2 EI proxy to pick messages from **ClientReq** queue, and send to **BEReq** queue .  
3.  The back-end picks messages from the **BEReq** queue, processes and places response messages to **BERes** queue.  
4.  Once a response is available in **BERes** queue, the proxy service picks it and sends back to **ClientRes** queue.  
5.  The client picks it as the response message.  

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
            ... 
        </td>
    </tr>
    <tr>
        <td>Send Mediator</td>
        <td>
           To send a message to a JMS queue, you should define the JMS connection URL as the endpoint address (which should be invoked via the **Send** mediator). There are two ways to specify the endpoint URL: 
           <ul>
               <li>
                    Specify the JNDI name of the JMS queue and the connection factory parameters in the JMS connection URL as shown in the exampe. Values of connection factory parameters depend on the type of the JMS broker.
               </li></br>
               <li>
                    If you have already specified the endpoint's connection factory parameters (for the JMS sender configuration) in the axis2.xml file, the connection URL in the proxy service should be as shown below. In this example, the endpoint URL of the proxy service refers the relevant connection factory in the axis2.xml file: </br></br>
                    <b>When the broker is ActiveMQ</b></br>
                    <code>jms:transport.jms.ConnectionFactory=QueueConnectionFactory</code></br></br>
                    <b>When the broker is WSO2 Message Broker</b></br>
                    <code>jms:/BEReq?transport.jms.ConnectionFactory=QueueConnectionFactory</code>
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

## Run the Example

1.  Configure the Micro Integrator with Apache ActiveMQ and set up the JMS Sender.
2.  Start WSO2 Integration Studio and create a proxy service with the above configuration. You can copy the synapse configuration given above to the **Source View** of your proxy service.
3.  Send a message to the Micro Integrator by executing the following command from the `MI_HOME/samples/axis2Client          `
    folder.

    ``` java
    .....
    ```

    !!! Info
        You can view the ActiveMQ queue by accessing the ActiveMQ management console using the URL `http://0.0.0.0:8161/admin`and using `admin` as both the username and password.