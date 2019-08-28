# Configure with Multiple Brokers

If your system has more than one existing brokers, it will be required
to configure the JMS transport with multiple brokers. In such
situations, each transport receiver should have a separate name.

The following example illustrates how to configure WSO2 EI to listen to
both ActiveMQ and WSO2 MB messages.

!!! note

From the below configurations, do the ones in the **axis2.xml** file
based on the profile you use as follows:

-   To enable the JMS transport in the Integration profile, edit the
    `          <EI_HOME>/conf/axis2/axis2.xml         ` file.
-   To enable the JMS transport in other profiles, edit the
    `          <EI_HOME>/wso2/<PROFILE_HOME>/conf/axis2/axis2.xml         `
    file. `          <PROFILE_HOME>         ` refers to the main
    directory of the profile inside the WSO2 EI distribution. For
    example, to enable the JMS transport in the Business Process
    profile, edit the
    `          <EI_HOME>/wso2/business-process/conf/axis2/axis2.xml         `
    file


1.  Download ActiveMQ (version 5.8.0 or later) from the [Apache
    ActiveMQ](http://activemq.apache.org/) site. Download the WSO2
    Message Broker from the [WSO2 Message
    Broker](http://wso2.com/products/message-broker/) site.
2.  Copy the following client libraries from
    `          <AMQ_HOME>/lib         ` directory to
    `          <EI_HOME>/lib         ` directory.  
    -   `            activemq-broker-5.8.0.jar           `
    -   `            activemq-client-5.8.0.jar           `
    -   `            geronimo-jms_1.1_spec-1.1.1.jar           `
    -   `            geronimo-j2ee-management_1.1_spec-1.0.1.jar           `
    -   `            hawtbuf-1.9.jar           `
3.  Configure the `           <EI_HOME>/conf/axis2/axis2.xml          `
    file as follows to enable ActiveMQ as a transport listener.  

    ``` xml
        <transportReceiver name="jms1" class="org.apache.axis2.transport.jms.JMSListener">
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

    Enter the following configuration in the same file to enable the
    WSO2 Message Broker.

    ``` xml
            <!--Uncomment this and configure as appropriate for JMS transport support with WSO2 MB 2.x.x -->
               <transportReceiver name="jms2" class="org.apache.axis2.transport.jms.JMSListener">
                   <parameter name="myTopicConnectionFactory" locked="false">
                      <parameter name="java.naming.factory.initial" locked="false">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                       <parameter name="java.naming.provider.url" locked="false">repository/conf/jndi.properties</parameter>
                       <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">TopicConnectionFactory</parameter>
                       <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
                   </parameter>
             
                   <parameter name="myQueueConnectionFactory" locked="false">
                       <parameter name="java.naming.factory.initial" locked="false">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                       <parameter name="java.naming.provider.url" locked="false">repository/conf/jndi.properties</parameter>
                       <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                      <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                   </parameter>
             
                   <parameter name="default" locked="false">
                       <parameter name="java.naming.factory.initial" locked="false">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                       <parameter name="java.naming.provider.url" locked="false">repository/conf/jndi.properties</parameter>
                       <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                       <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                   </parameter>
               </transportReceiver>
    ```

      
        !!! info
    
        Note that the transport receiver name is different in each
        configuration.
    

4.  Add a topic and a queue to the  jndi.properties file located in
    `           <EI_HOME>/conf          ` as follows:  

    ``` java
        <connection-factory name="QueueConnectionFactory">
              <xa>false</xa>
              <connectors>
                 <connector-ref connector-name="netty"/>
              </connectors>
              <entries>
                 <entry name="/QueueConnectionFactory"/>
              </entries>
        </connection-factory>
    
    
        <connection-factory name="TopicConnectionFactory">
              <xa>false</xa>
              <connectors>
                 <connector-ref connector-name="netty"/>
              </connectors>
              <entries>
                 <entry name="/TopicConnectionFactory"/>
              </entries>
        </connection-factory>
    ```

5.  Start both ActiveMQ and WSO2 MB.
6.  Start the WSO2 EI server and the management console. If they are
    already running, restart them.

Now a proxy service can be created with reference to transport receiver
JMS1 and/or JMS2. For example, the following proxy service is configured
with reference to both transport receivers.

``` xml
    <proxy xmlns="http://ws.apache.org/ns/synapse" 
    name="jmsMultipleListnerProxy" 
    transports="jms1,jms2" 
    statistics="disable" 
    trace="disable" 
    startOnLoad="true"> 
    <target> 
    <inSequence> 
    <log level="full"/> 
    <send> 
    <endpoint> 
    <address uri="http://localhost:9000/services/SimpleStockQuoteService"/> 
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
    <default>application/xml</default> 
    </rules> 
    </parameter> 
    <description/> 
    </proxy>
```
