# Configure with the Broker Profile

Follow the steps below to configure the JMS transport of the ESB profile
with the Broker profile.

!!! note

From the below configurations, do the ones in the **axis2.xml** file
based on the profile you use as follows:

-   To enable the JMS transport in the ESB profile, edit the
    `          <EI_HOME>/conf/axis2/axis2.xml         ` file.
-   To enable the JMS transport in other profiles, edit the
    `          <EI_HOME>/wso2/<PROFILE_HOME>/conf/axis2/axis2.xml         `
    file. `          <PROFILE_HOME>         ` refers to the main
    directory of the profile inside the WSO2 EI distribution. For
    example, to enable the JMS transport in the Business Process
    profile, edit the
    `          <EI_HOME>/wso2/business-process/conf/axis2/axis2.xml         `
    file


1.  To enable the JMS transport of the ESB profile to communicate with
    the Broker profile, edit the
    `           <EI_HOME>/conf/axis2/axis2.xml          ` file , find
    the commented `           <transport receiver>          ` block and
    uncomment it as shown below.

    ``` html/xml
         <!--Uncomment this and configure as appropriate for JMS transport support with WSO2 EI Broker Profile -->
           <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
               <parameter name="myTopicConnectionFactory" locked="false">
                  <parameter name="java.naming.factory.initial" locked="false">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                   <parameter name="java.naming.provider.url" locked="false">conf/jndi.properties</parameter>
                   <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">TopicConnectionFactory</parameter>
                   <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
               </parameter>
         
               <parameter name="myQueueConnectionFactory" locked="false">
                   <parameter name="java.naming.factory.initial" locked="false">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                   <parameter name="java.naming.provider.url" locked="false">conf/jndi.properties</parameter>
                   <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                  <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
               </parameter>
         
               <parameter name="default" locked="false">
                   <parameter name="java.naming.factory.initial" locked="false">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                   <parameter name="java.naming.provider.url" locked="false">conf/jndi.properties</parameter>
                   <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                   <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
               </parameter>
           </transportReceiver>
    ```

2.  Uncomment the following `           <transport sender>          `
    block for JMS in the same file:

    ``` html/xml
            <!-- uncomment this and configure to use connection pools for sending messages>
            <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender"/>
    ```

        !!! info
    
        For more information on the JMS configuration parameters used in the
        code segments above, see [JMS Connection Factory
        Parameters](https://docs.wso2.com/display/EI600/JMS+Transport#JMSTransport-JMSConnectionFactoryParameters)
        .
    

3.  `                     Open                       <EI_HOME>/conf/jndi.properties           `
    file and make a reference to the running Broker profile as specified
    below: `                               `
    -   Use **carbon** as the virtual host.

    -   Define a queue named `            JMSMS           ` .
    -   Comment out the topic, since it is not required in this
        scenario. However, in order to avoid getting the
        `             javax.naming.NameNotFoundException:TopicConnectionFactory            `
        exception during server startup, make a reference to the Broker
        profile from the
        `             TopicConnectionFactory            ` as well.  
        For example:

        ``` java
                # register some connection factories
                # connectionfactory.[jndiname] = [ConnectionURL]
                connectionfactory.QueueConnectionFactory = amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'
                connectionfactory.TopicConnectionFactory = amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'
                # register some queues in JNDI using the form
                # queue.[jndiName] = [physicalName]
                queue.JMSMS=JMSMS
                queue.StockQuotesQueue = StockQuotesQueue
        ```

                !!! note
        
                For instructions on configuring the JMS transport in **a
                cluster** of the WSO2 EI Broker profile, see [Running the Broker
                instances](https://docs.wso2.com/display/EI600/Handling+Failover#HandlingFailover-Runningthebrokerinstances)
                .
        

4.  Ensure that the Broker profile is running, and then open a command
    prompt (or a shell in Linux) and go to the
    `          <EI_HOME>/bin/         ` directory.
5.  Start the WSO2 EI server by executing the following commands:
    `          sh integrator.sh         ` (on Linux/OS X) or
    `          integrator.bat         ` (on Windows).  

Now, you have both the Broker and the ESB profile of WSO2 EI configured
and running with the JMS transport enabled.

### Managing security of the configuration

JMS is an integral part of enterprise integration solutions that are
highly-reliable, loosely-coupled and asynchronous. As a result,
implementing proper security to your JMS deployments is vital. The below
sections discuss some of the best practices of an effective JMS security
implementation when used in combination with WSO2 Enterprise Integrator.

Let's see how some of the key concepts of system security such as
authentication, authorization and availability are implemented in
different types of broker servers as follows.

!!! note

You can apply the same information mentioned in this section when
configuring JMS with [Apache QPid](https://qpid.apache.org/) .


Given below is an overview of how some common security concepts are
implemented in EI-Broker runtime.

| Security Concept                                        | How it is Implemented in EI-Broker                      |
|---------------------------------------------------------|---------------------------------------------------------|
| [Authentication](#ConfigurewiththeBrokerProfile-AuthMB) | Andes Authenticator connected entities to authenticate. |
| [Authorization](#ConfigurewiththeBrokerProfile-AuthrMB) | Creation and use of role-based permissions.             |
| Availability                                            | Clustering using Apache Zookeeper.                      |
| [Integrity](#ConfigurewiththeBrokerProfile-InMB)        | Message-level encryption using WS-Security.             |

Let's see how each concept in the table above is implemented in
EI-Broker .

After setting up EI-Broker runtime with the ESB runtime in WO2 EI, open
`         <EI_HOME>/wso2/broker/conf/advanced/qpid-config.xml        `
file and add the following line as a child element of \<tuning\> .

``` html/xml
    <messageBatchSizeForBrowserSubscriptions>100000</messageBatchSizeForBrowserSubscriptions>
```

#### Authentication: Plain Text

EI-Broker requires all its incoming connections to be authenticated. The
`         <EI_HOME>/conf/jndi.properties        ` file contains lines
similar to the following. They contain the username and password
credentials used to authenticate connections made to the EI-Broker
runtime. This is plain text authentication.  

``` java
    connectionfactory.TopicConnectionFactory = amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'
    connectionfactory.QueueConnectionFactory = amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675' 
```

In the EI-Broker authentication example below, we send a request to the
proxy service **testJMSProxy** , which adds a message to the
**example.MyQueue** queue.

``` html/xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
      <registry provider="org.wso2.carbon.mediation.registry.WSO2Registry">
         <parameter name="cachableDuration">15000</parameter>
      </registry>
      <proxy name="testJMSProxy"
             transports="https http"
             startOnLoad="true"
             trace="disable">
         <target>
            <inSequence>
               <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
               <property name="target.endpoint" value="jmsEP" scope="default"/>
               <store messageStore="testMsgStore"/>
            </inSequence>
         </target>
      </proxy>
      <endpoint name="jmsEP">
         <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
      </endpoint>
      <sequence name="fault">
         <log level="full">
            <property name="MESSAGE" value="Executing default 'fault' sequence"/>
            <property name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
            <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
         </log>
         <drop/>
      </sequence>
      <sequence name="main">
         <in>
            <log level="full"/>
            <filter source="get-property('To')" regex="http://localhost:9000.*">
               <send/>
            </filter>
         </in>
         <out>
            <send/>
         </out>
         <description>The main sequence for the message mediation</description>
      </sequence>
      <messageStore class="org.wso2.carbon.message.store.persistence.jms.JMSMessageStore"
                    name="testMsgStore">
         <parameter name="java.naming.factory.initial">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
         <parameter name="java.naming.provider.url">repository/conf/jndi.properties</parameter>
         <parameter name="store.jms.destination">MyQueue</parameter>
      </messageStore>
    </definitions>
```

If you change the authentication credentials of
the jndi.properties file, the connection will not be authenticated. You
will see an error similar to:

``` java
    ERROR - AMQConnection Throwable Received but no listener set: org.wso2.andes.AMQDisconnectedException: Server closed connection and reconnection not permitted. 
```

#### Authentication: Encrypted

In the [previous authentication
example](#ConfigurewiththeBrokerProfile-plain) , the user names and
passwords are stored in plain text inside the WSO2 EI’s jndi.properties
file. These credentials can be stored in an encrypted manner for added
security.

#### Authorization

EI-Broker runtime allows user-based authorization as seen in the
[example on WSO2 MB Authentication](#ConfigurewiththeBrokerProfile-Auth)
. To set up users, follow the instructions in [User Management
section](https://docs.wso2.com/display/ADMIN44x/Managing+Users%2C+Roles+and+Permissions)
of the WSO2 Admin Guide.  
  
EI-Broker provides role-based authorization for topics, where
public/subscribe access can be assigned to user groups. For more
information on setting up role-based authorization for topics, refer to
section [Managing Topics and Subscriptions
section](http://docs.wso2.org/message-broker/Managing+Topics+and+Subscriptions)
of the WSO2 MB documentation.

#### Integrity

Integrity is part of message-level security, and can be implemented
using a standard like WS-Security. For information on how message-level
security works in JMS, see [Managing Security of the
configuration](Configure-with-ActiveMQ_119130329.html#ConfigurewithActiveMQ-Managingsecurityoftheconfiguration)
.
