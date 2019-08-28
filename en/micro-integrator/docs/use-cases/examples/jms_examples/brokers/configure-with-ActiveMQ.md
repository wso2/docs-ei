# Configure with ActiveMQ

This section describes how to configure the JMS transport of the ESB
Profile of WSO2 Enterprise Integrator (WSO2 EI) with ActiveMQ. The
following topics are covered:

!!! note

From the below configurations, do the ones in the **axis2.xml** file
based on the profile you use as follows:

-   To enable the JMS transport in the ESB profile, edit the
    `          <EI_HOME>/conf/axis2/axis2.xml         ` file.
-   To enable the JMS transport in other profiles, edit the
    `          <EI_HOME>/wso2/<PROFILE_HOME>/conf/axis2/axis2.xml         `
    file. `          <PROFILE_HOME>         ` can be a main directory of
    a profile inside the WSO2 EI distribution. For example, to enable
    the JMS transport of the Business Process Profile, edit the
    `          <EI_HOME>/wso2/business-process/conf/axis2/axis2.xml         `
    file


-   [Setting up WSO2 EI and
    ActiveMQ](#ConfigurewithActiveMQ-SettingupWSO2EIandActiveMQ)
-   [Configuring redelivery in ActiveMQ
    queues](#ConfigurewithActiveMQ-ConfiguringredeliveryinActiveMQqueues)
-   [Managing security of the
    configuration](#ConfigurewithActiveMQ-Managingsecurityoftheconfiguration)

### Setting up WSO2 EI and ActiveMQ

Follow the instructions below to set up and configure.

1.  Download, set up and start [Apache
    ActiveMQ](http://activemq.apache.org/) .
2.  Follow the [Installation
    Guide](https://docs.wso2.com/display/EI650/Installation+Guide) and
    set up the ESB Profile of WSO2 EI.

        !!! info
    
        Do not start the ESB Profile of WSO2 EI at this point. ActiveMQ
        should be up and running before starting the ESB Profile of WSO2 EI.
    

3.  Copy the following client libraries from the
    `          <ACTIVEMQ_HOME>/lib         ` directory to the
    `          <         ` `          EI_HOME>/lib         `
    directory.  

    **ActiveMQ 5.8.0 and above**

    -   activemq-broker-5.8.0.jar
    -   activemq-client-5.8.0.jar
    -   activemq-kahadb-store-5.8.0.jar  
    -   geronimo-jms\_1.1\_spec-1.1.1.jar
    -   geronimo-j2ee-management\_1.1\_spec-1.0.1.jar
    -   geronimo-jta\_1.0.1B\_spec-1.0.1.jar
    -   hawtbuf-1.9.jar
    -   Slf4j-api-1.6.6.jar
    -   activeio-core-3.1.4.jar (available in the
        `            <ACTIVEMQ_HOME>/lib/optional           `
        directory)  

    **Earlier version of ActiveMQ**

    -   activemq-core-5.5.1.jar

    -   geronimo-j2ee-management\_1.0\_spec-1.0.jar

    -   geronimo-jms\_1.1\_spec-1.1.1.jar

4.  Next, configure the [JMS transport
    listeners](#ConfigurewithActiveMQ-JMSListener) and
    [senders](#ConfigurewithActiveMQ-JMSSender) in the ESB Profile of
    WSO2 EI based on your requirement. When you need to listen to a JMS
    queue you need to configure the [JMS transport
    listener](#ConfigurewithActiveMQ-JMSListener) , and when you need to
    send messages to a JMS queue you need to configure the [JMS
    transport sender](#ConfigurewithActiveMQ-JMSSender) .

        !!! note
    
        When configuring the JMS transport with ActiveMQ, you can append
        [ActiveMQ-specific
        properties](http://activemq.apache.org/connection-configuration-uri.html)
        to the value of the `           java.naming.provider.url          `
        property. For example, you can set the
        `           redeliveryDelay          ` and
        `           initialRedeliveryDelay          ` properties when
        configuring a JMS inbound endpoint as follows:
    
        `           <parameter name="java.naming.provider.url">tcp://localhost:61616?jms.redeliveryPolicy.redeliveryDelay=10000&amp;jms.redeliveryPolicy.initialRedeliveryDelay=10000</parameter>          `
    

5.  Start ActiveMQ by navigating to the
    `           <ACTIVEMQ_HOME>/bin          ` directory and executing
    `           ./activemq console          ` (on Linux/OSX) or
    `           activemq start          ` (on Windows).

6.  Now you have instances of ActiveMQ and the ESB Profile of WSO2 EI
    configured, up and running.  Next, let's take a look
    at implementation details of various [JMS use cases](_JMS_Usecases_)
    .

#### Setting up the JMS listener

To enable the JMS transport listener, un-comment the following listener
configuration related to ActiveMQ in
`         <EI_HOME>/conf/axis2/axis2.xml        ` file.

``` html/xml
    <!--Uncomment this and configure as appropriate for JMS transport support, after setting up your JMS environment (e.g. ActiveMQ)-->
    <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
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

#### Setting up the JMS sender

To enable the JMS transport sender, un-comment the following
configuration in `         <EI_HOME>/conf/axis2/axis2.xml        ` file.

``` html/xml
    <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender"/>
```

!!! info For details on the JMS configuration parameters used in the
code segments above, see [JMS Connection Factory
Parameters](JMS-Transport_119130301.html#JMSTransport-JMSConnectionFactoryParameters)
!!! info

The above configurations do not address the problem of transient
failures of the ActiveMQ message broker. Let's say for some reason
ActiveMQ goes down and becomes active again after a while. The ESB
Profile of WSO2 EI will not reconnect to ActiveMQ, instead it will throw
an error when requests are sent to the ESB Profile of WSO2 EI until it
is restarted. In order to tackle this issue you need to add the
following configuration in place of the *java.naming.provider.url,*

failover:tcp://localhost:61616

This simply makes sure that re-connection takes place. Failover prefix
is associated with the Failover Transport of ActiveMQ. For more
information on this, see [The Failover
Transport](http://activemq.apache.org/failover-transport-reference.html)
.


Connecting multiple ActiveMQ brokers

The ESB Profile of WSO2 EI can be configured as described below to work
with two ActiveMQ brokers. In this example, port 61616 is used for one
ActiveMQ instance and port 61617 is used for another ActiveMQ instance.

1.  Configure the ESB Profile of WSO2 EI to work with one ActiveMQ
    broker as described above.
2.  Start another ActiveMQ instance.
3.  Add another transport receiver to the
    `           <EI_Home>/conf/axis2/axis2.xml          ` file as
    follows. Note that the name of the transport receiver is different
    to that of the transport receiver that you already entered. The port
    specified is `           61617          ` .  

    ``` xml
        <transportReceiver name="jms1" class="org.apache.axis2.transport.jms.JMSListener">
               <parameter name="myTopicConnectionFactory" locked="false">
                   <parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
                   <parameter name="java.naming.provider.url" locked="false">tcp://localhost:61617</parameter>
                   <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">TopicConnectionFactory</parameter>
                    <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
               </parameter>
         
               <parameter name="myQueueConnectionFactory" locked="false">
                   <parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
                   <parameter name="java.naming.provider.url" locked="false">tcp://localhost:61617</parameter>
                   <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                    <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
               </parameter>
         
               <parameter name="default" locked="false">
                   <parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
                   <parameter name="java.naming.provider.url" locked="false">tcp://localhost:61617</parameter>
                   <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                    <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
               </parameter>
           </transportReceiver>
    ```

4.  Set up the JMS sender as described in [Setting up the JMS
    sender](#ConfigurewithActiveMQ-JMSSender) .

### Configuring redelivery in ActiveMQ queues

When the ESB Profile of WSO2 EI is configured to consume messages from
an ActiveMQ queue, you have the option to configure message re-delivery.
This is useful when messages are unable to be processed by the ESB
Profile of WSO2 EI due to failures.

1.  Enable the JMS listener as explained in [Setting up the JMS
    listener](#ConfigurewithActiveMQ-JMSListener) .
2.  Add the following JMS parameters into the proxy service
    configuration in the ESB Profile of WSO2 EI:

    ``` xml
            <parameter name="redeliveryPolicy.maximumRedeliveries">1</parameter>
            <parameter name="transport.jms.DestinationType">queue</parameter>
            <parameter name="transport.jms.SessionTransacted">true</parameter>
            <parameter name="transport.jms.Destination">JMStoHTTPStockQuoteProxy</parameter>
            <parameter name="redeliveryPolicy.redeliveryDelay">2000</parameter>
            <parameter name="transport.jms.CacheLevel">consumer</parameter>
    ```

    **Parameters**

    -   `            redeliveryPolicy.maximumRedeliveries           ` :
        Maximum number of retries for delivering the message. If set to
        -1 ActiveMQ will retry inifinitely.
    -   `            transport.jms.SessionTransacted           ` : When
        set to `            true           ` , this enables the JMS
        session transaction for the proxy service.
    -   `            redeliveryPolicy.redeliveryDelay           ` :
        Delay time in milliseconds between retries.
    -   `            transport.jms.CacheLevel           ` : This needs
        to be set to `            consumer           ` for the ActiveMQ
        redelivery mechanism to work.

3.  Add the following line in your fault sequence:

        <property name="SET_ROLLBACK_ONLY" value="true" scope="axis2"/>

        !!! info
    
        SET\_ROLLBACK\_ONLY
    
        This parameter must be defined for ActiveMQ to redeliver the
        message.
    
        When the ESB Profile of WSO2 EI is unable to deliver a message to
        the back-end service due to an error, it will be routed to the fault
        sequence in the configuration. When the
        `           SET_ROLLBACK_ONLY          ` property is set in the
        fault sequence, the ESB Profile of WSO2 EI informs ActiveMQ to
        redeliver the message.
    

Following is a sample proxy service configuration:

``` xml
    <proxy xmlns="http://ws.apache.org/ns/synapse"
           name="JMStoHTTPStockQuoteProxy"
           transports="jms"
           statistics="disable"
           trace="disable"
           startOnLoad="true">
       <target>
          <inSequence>
             <property name="transactionID"
                       expression="get-property('MessageID')"
                       scope="default"/>
             <property name="sourceMessageID"
                       expression="get-property('MessageID')"
                       scope="default"/>
             <property name="proxyMessageID"
                       expression="get-property('MessageID')"
                       scope="default"/>
             <log level="full">
                <property name="transactionID" expression="get-property('transactionID')"/>
                <property name="sourceMessageID" expression="get-property('sourceMessageID')"/>
                <property name="MessageID" expression="get-property('proxyMessageID')"/>
             </log>
             <property name="SET_ROLLBACK_ONLY" value="true" scope="axis2"/>
             <drop/>               
          </inSequence>
          <faultSequence name="jms_fault"/>
       </target>
       <parameter name="redeliveryPolicy.maximumRedeliveries">1</parameter>
       <parameter name="transport.jms.DestinationType">queue</parameter>
       <parameter name="transport.jms.SessionTransacted">true</parameter>
       <parameter name="transport.jms.Destination">JMStoHTTPStockQuoteProxy</parameter>
       <parameter name="redeliveryPolicy.redeliveryDelay">2000</parameter>
       <parameter name="transport.jms.CacheLevel">consumer</parameter>
       <description/>
    </proxy>
```

### Managing security of the configuration

  

JMS is an integral part of enterprise integration solutions that are
highly-reliable, loosely-coupled and asynchronous. As a result,
implementing proper security to your JMS deployments is vital. The below
sections discuss some of the best practices of an effective JMS security
implementation when used in combination with WSO2 Enterprise Integrator.

Let's see how some of the key concepts of system security such as
authentication, authorization and availability are implemented in
different types of broker servers as follows. Given below is an overview
of how some common security concepts are implemented in Apache ActiveMQ.

  

| Security Concept                                        | How it is Implemented                                                                                           |
|---------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Authentication](#ConfigurewithActiveMQ-Authentication) | Simple authentication and JAAS plugins.                                                                         |
| [Authorization](#ConfigurewithActiveMQ-Authorization)   | Built-in authorization mechanism using XML configuration.                                                       |
| [Availability](#ConfigurewithActiveMQ-Availability)     | Master/Slave configurations using fail-over transport in ActiveMQ (not to be confused with WSO2 EI transports). |
| [Integrity](#ConfigurewithActiveMQ-Integrity)           | WS-Security                                                                                                     |

#### Authentication

Simple Authentication: ActiveMQ comes with an authentication plugin,
which provides basic authentication between the ActiveMQ JMS and WSO2
EI. The steps below describe how to configure.

1. Add the following configuration in
\<ACTIVEMQ\_HOME\>/conf/activemq-security.xml file.

``` html/xml
    <simpleAuthenticationPlugin anonymousAccessAllowed="true">
               <users>
                   <authenticationUser username="system" password="${activemq.password}"
                       groups="users,admins"/>
                   <authenticationUser username="user" password="${guest.password}"
                       groups="users"/>
                   <authenticationUser username="guest" password="${guest.password}" groups="guests"/>
               </users> 
    
    </simpleAuthenticationPlugin>
```

2. Edit \<ACTIVEMQ\_HOME\>/conf/credentials.properties file for
plain-text version or \<ACTIVEMQ\_HOME\>/conf/credentials-enc.properties
file for encrypted version to define the username and password lists
referenced in the configuration above.  
  
Th e **anonymousAccessAllowed** attribute defines whether or not to
allow anonymous access. The groups and users defined in step 1 are used
to provide authorization schemes. Refer to section Authorization for
more information.

3\. Ensure that the \<transportReceiver\> element below is added in
`          <EI_HOME>/conf/axis2/axis2.xml         ` file.

``` html/xml
    <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
           <parameter name="myTopicConnectionFactory" locked="false">
               <parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
               <parameter name="java.naming.provider.url" locked="false">tcp://localhost:61616</parameter>
                  <parameter name="transport.jms.UserName">system</parameter>
                   <parameter name="transport.jms.Password">manager</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">TopicConnectionFactory</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
           </parameter>
    
           <parameter name="myQueueConnectionFactory" locked="false">
               <parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
               <parameter name="java.naming.provider.url" locked="false">tcp://localhost:61616</parameter>
                   <parameter name="transport.jms.UserName">system</parameter>
                   <parameter name="transport.jms.Password">manager</parameter>
               <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
           </parameter>
    
           <parameter name="default" locked="false">
               <parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
               <parameter name="java.naming.provider.url" locked="false">tcp://localhost:61616</parameter>
                  <parameter name="transport.jms.UserName">system</parameter>
                   <parameter name="transport.jms.Password">manager</parameter>
               <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
           </parameter>
    </transportReceiver>
```

Lines similar to the following contain the username and password
configured in ActiveMQ.

``` html/xml
    <parameter name="transport.jms.UserName">system</parameter> 
    <parameter name="transport.jms.Password">manager</parameter>
```

!!! info

Infor

For more advanced authentication schemes that use JAAS which are
supported in ActiveMQ, refer to the official ActiveMQ documentation
here: <http://activemq.apache.org/security.html>


#### Authorization

ActiveMQ provides authorization schemes using simple XML configurations,
which you can apply to the users defined in the [authentication
plugin](#ConfigurewithActiveMQ-Authentication) . To setup authorization,
ensure you have the following configuration in
\<ACTIVEMQ\_HOME\>/conf/activemq-sequrity.xml file.

``` html/xml
    <authorizationPlugin>
     <map>
      <authorizationMap>
        <authorizationEntries>
          <authorizationEntry queue=">" read="admins" write="admins" admin="admins" />
          <authorizationEntry queue="USERS.>" read="users" write="users" admin="users" />
          <authorizationEntry queue="GUEST.>" read="guests" write="guests,users" admin="guests,users" />
     
 
      <authorizationEntry topic=">" read="admins" write="admins" admin="admins" />
      <authorizationEntry topic="USERS.>" read="users" write="users" admin="users" />
      <authorizationEntry topic="GUEST.>" read="guests" write="guests,users" admin="guests,users" />
 
      <authorizationEntry topic="ActiveMQ.Advisory.>" read="guests,users" write="guests,users" admin="guests,users"/>
    </authorizationEntries>
  </authorizationMap>
 </map>
</authorizationPlugin> 
```
    
!!! info

Infor

This configuration defines role-based authorization on queues and
topics, and uses ActiveMQ wildcards. For information on    wildcards,
refer to ActiveMQ documentation here:

#### Availability

ActiveMQ supports the use of master/slave and fail-over transport to
provide high-availability. ActiveMQ supports two types of master/slave
configurations as follows:

-   Master/slave using shared file systems
-   Master/slave using JDBC

!!! info

Infor

For more information on either model, refer to ActiveMQ documentation on
master/slave here: <http://activemq.apache.org/masterslave.html> .

We explore the second option here.

###### Master/slave using JDBC

ActiveMQ uses a special URI similar to the following to facilitate
fail-over functionality:
failover://(tcp://127.0.0.1:61616,tcp://127.0.0.1:61617,tcp://127.0.0.1:61618)?initialReconnectDelay=100
. Use this URI inside WSO2 EI for a highly-available JMS solution.

To create proxy services, sequences, endpoints, message stores,
processors etc. in WSO2 EI, you can either use the management console or
copy the XML configuration to the source view. You can find the source
view under menu **Manage \> Service Bus \> Source View** in the left
navigation pane of the WSO2 EI management console. Alternatively, you
can add an XML file  to
`          <EI_HOME>/repository/deployment/server/synapse-configs/default/proxy-services         `
.

A sample WSO2 EI Proxy service for this setup is given below.

``` html/xml
    <proxy xmlns="http://ws.apache.org/ns/synapse" name="FailOverJMS"
    transports="http" startOnLoad="true" trace="disable">
       <target>
           <inSequence>
               <log level="full"/>
               <property name="OUT_ONLY" value="true" scope="default"/>
               <clone>
                   <target>
                       <endpoint>
                   <address                         uri="jms:/OMS?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;java.naming.provider.url=failover:(tcp://localhost:61616,tcp://localhost:61617)?randomize=false&amp;transport.jms.DestinationType=queue"/>
               </endpoint>
                   </target>
               </clone>
           </inSequence>
       </target>
       <publishWSDL key="gov:/services/FileService.wsdl">
           <resource location="Message.xsd" key="gov:/services/Message.xsd"/>
       </publishWSDL>
    </proxy> 
```

  
Note
`          java.naming.provider.url=failover:(                     tcp://localhost:61616,tcp://localhost:61617                    )?randomize=false         `
inside the address endpoint uri attribute. The
`          randomize=false         ` parameter makes this setup follow a
prioritized fail-over configuration, which means when the first instance
fails, it moves to the next. For more information on ActiveMQ fail-over
transport and its parameters, refer to ActiveMQ documentation here:
<http://activemq.apache.org/failover-transport-reference.html> .

#### Integrity

Integrity is part of message-level security and can be implemented using
a standard like WS-Security. Following sample shows the application of
WS-Security for message-level encryption where messages are stored in a
message store in WSO2 EI.

``` html/xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <localEntry key="sec_policy" src="file:repository/samples/resources/policy/policy_3.xml"/>
        <proxy name="FailOverJMS" startOnLoad="true" transports="http" xmlns="http://ws.apache.org/ns/synapse">
            <target>
                <inSequence>
                    <send>
                        <endpoint>
                            <address uri="jms:/StockQuoteJmsProxy2?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;java.naming.provider.url=tcp://localhost:61616">
                                <enableAddressing version="submission"/>
                                <enableSec policy="sec_policy"/>
                        </address>
                    </endpoint>
                </send>
                </inSequence>
                <outSequence>
                    <header action="remove" name="wsse:Security" scope="default" xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"/>
                    <send/>
                </outSequence>
                <faultSequence/>
            </target>
        </proxy>
    </definitions>
```
