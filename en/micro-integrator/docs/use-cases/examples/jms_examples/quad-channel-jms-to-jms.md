# JMS Synchronous Invocations : Quad Channel JMS-to-JMS

-   [Introduction](#JMSSynchronousInvocations:QuadChannelJMS-to-JMS-Introduction)
-   [Prerequisites](#JMSSynchronousInvocations:QuadChannelJMS-to-JMS-Prerequisites)
-   [Sample
    configuration](#JMSSynchronousInvocations:QuadChannelJMS-to-JMS-Sampleconfiguration)

### Introduction

The following diagram depicts quad-channel JMS synchronous invocations
of the WSO2 Enterprise Integrator (WSO2 EI).

![](attachments/119130326/119130327.png)

### Prerequisites

Follow the steps below to set the prerequisites up before you start.

1.  Download and set up [Apache ActiveMQ](http://activemq.apache.org/) .
    For instructions, see [Installation
    Prerequisites](https://docs.wso2.com/display/EI650/Installation+Prerequisites)
    .
2.  C opy the following client libraries from the
    `          <AMQ_HOME>/lib         ` directory to the
    `          <         ` `          EI_HOME>/lib         `
    directory.  
    **ActiveMQ 5.8.0 and above**  
    -   -   `              activemq-broker-5.8.0.jar             `
        -   `              activemq-client-5.8.0.jar             `
        -   `              geronimo-jms_1.1_spec-1.1.1.jar             `
        -   `              geronimo-j2ee-management_1.1_spec-1.0.1.jar             `
        -   `              hawtbuf-1.9.jar             `

    **Earlier versions of ActiveMQ**

    -   -   `              activemq-core-5.5.1.jar             `
        -   `              geronimo-j2ee-management_1.0_spec-1.0.jar             `
        -   `              geronimo-jms_1.1_spec-1.1.1.jar             `

3.  Add the following properties to the
    `           <EI_HOME>/conf/jndi.properties          ` file. For more
    information, see [Setting up WSO2 EI and
    ActiveMQ](_Configure_with_ActiveMQ_) .

    ``` powershell
        queue.ClientReq = ClientReq
        queue.BEReq = BEReq
        queue.BERes = BERes
    ```

4.  Uncomment the following sections in the
    `           <EI_HOME>/conf/axis2/axis2.xml          ` file.

    1.  To enable the JMS transport sender:

        ``` xml
                <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender"/>
        ```

    2.  To enable the JMS transport listener:

        ``` xml
                    <!--Uncomment this and configure as appropriate for JMS transport support, after setting up your JMS environment (e.g. ActiveMQ)--><transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
                           <parameter name="myTopicConnectionFactory" locked="false">
                               <parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
                               <parameter name="java.naming.provider.url" locked="false">tcp://localhost:61616</parameter>
                               <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">TopicConnectionFactory</parameter>
                                <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
                           </parameter>
                      
                           <parameter name="myQueueConnectionFactory" locked="false">
                               <parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
                               <parameter name="java.naming.provider.url" locked="false">tcp://localhost:61616</parameter>
                               <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                           </parameter>
                      
                           <parameter name="default" locked="false">
                               <parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
                               <parameter name="java.naming.provider.url" locked="false">tcp://localhost:61616</parameter>
                               <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                           </parameter>
                       </transportReceiver>
        ```

### Sample configuration 

Following is a sample configuration of WSO2 EI for quad-channel  JMS
synchronous invocations.

**Example Code 5**

``` html/xml
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

1.  The **JMSReplyTo** property of the JMS message is set to
    **ClientRes** . Therefore, the c lient sends a JMS message to the
    **ClientReq** queue.  
2.  The **transport.jms.ReplyDestination** value is set to **BERes** .
    This enables the WSO2 EI proxy to pick messages from **ClientReq**
    queue, and send to **BEReq** queue .  
3.  The back-end picks messages from the **BEReq** queue, processes and
    places response messages to **BERes** queue.  
4.  Once a response is available in **BERes** queue, the proxy service
    picks it and sends back to **ClientRes** queue.  
5.  The client picks it as the response message.  

!!! tip

Note that there are two ways to define the endpoint URL:

-   Specify the JNDI name of the JMS queue and the [connection factory
    parameters](JMS-Transport_119130301.html#JMSTransport-ConnectionFactoryParams)
    in the connection URL as shown in the sample configuration given
    above.

-   If you have already specified the endpoint's connection factory
    parameters (for the JMS sender configuration) in the
    `           axis2.xml          ` file (stored in the
    `           <EI_HOME>/conf/axis2/          ` directory), the
    connection URL in the proxy service should be as shown below. In
    this example, the endpoint URL of the proxy service refers the
    relevant connection factory in the axis2.xml file:

    **When the broker is ActiveMQ**

    ``` java
        jms:transport.jms.ConnectionFactory=QueueConnectionFactory
    ```
    
        **When the broker is WSO2 MB**
    
    ``` java
            jms:/BEReq?transport.jms.ConnectionFactory=QueueConnectionFactory
    ```
        
        !!! note
        
        Be sure to replace the ' `         &        ` ' character in the
        endpoint URL with ' `         &amp;        ` ' to avoid the following
        exception:
        
``` java
    com.ctc.wstx.exc.WstxUnexpectedCharException: Unexpected character '=' (code 61); expected a semi-colon after the reference for entity 'java.naming.factory.initial' at [row,col {unknown-source}
```
    
