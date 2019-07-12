# Configure with WebLogic

The following instructions describe how to configure the JMS transport
with Oracle WebLogic 10.3.4.0.

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


### Starting WebLogic and the WSO2 EI

1.  Download, set up, and start [Oracle WebLogic
    Server](http://www.oracle.com/technetwork/middleware/weblogic/downloads/wls-main-097127.html)
    .
2.  [Start WSO2
    EI](https://docs.wso2.com/display/EI650/Running+the+Product) .
3.  Wrap the weblogic client jar and build a new OSGi bundle using the
    following
    [pom.xml](https://svn.wso2.org/repos/wso2/scratch/lasantha/weblogic-wrapper/pom.xml)
    . The exporting of `          javax.jms         ` package and
    `          javax.xml.namespace         ` package of the client jar
    should be prevented.
4.  Copy the client libraries file `          wlfullclient.jar         `
    from the `          <WEBLOGIC_HOME>/wlserver_XX/server/lib         `
    directory to the `          <EI_HOME>/dropins         ` directory.

### Configuring WebLogic server

Configure the required connection factories and queues in WebLogic.  An
entry for a JMS queue would look like the following. The configuration
files can be found in configuration inside
`          <WEBLOGIC_HOME>/user_projects/domains/<DOMAIN_NAME>/config/jms         `
file. Alternatively you can configure using the WebLogic web console
which can be accessed through
[http://localhost:7001](http://localhost:7001/) with default
configurations.

``` html/xml
      <queue name="wso2MessageQueue">
        <sub-deployment-name>jms</sub-deployment-name>
        <jndi-name>jms/wso2MessageQueue</jndi-name>
      </queue>
```

Once you start the WebLogic server with the above changes, you'll be
able to see the following on STDOUT.

``` java
    <Jun 25, 2013 11:20:02 AM IST> <Notice> <WebLogicServer> <BEA-000331> <Started WebLogic Admin Server "AdminServer" for domain "wso2" running in Development Mode> 
    <Jun 25, 2013 11:20:02 AM IST> <Notice> <WebLogicServer> <BEA-000365> <Server state changed to RUNNING> 
    <Jun 25, 2013 11:20:02 AM IST> <Notice> <WebLogicServer> <BEA-000360> <Server started in RUNNING mode> 
```

You will now need to configure the transport listener, sender, and
message store in WSO2 EI. For details on JMS configuration parameters
used in the code segments, see [JMS Connection Factory
Parameters](JMS-Transport_119130301.html#JMSTransport-JMSConnectionFactoryParameters)
.

### Setting up the JMS listener

To enable the JMS transport listener, add the following listener
configuration related to Weblogic in the
`         <EI_HOME>/conf/axis2/axis2.xml        ` file.

``` html/xml
    <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
        <parameter name="myQueueConnectionFactory" locked="false">
            <parameter name="java.naming.factory.initial" locked="false">weblogic.jndi.WLInitialContextFactory</parameter>
            <parameter name="java.naming.provider.url" locked="false">t3://localhost:7001</parameter>
            <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">jms/myconnectionFactory</parameter>
            <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
            <parameter name="transport.jms.UserName" locked="false">weblogic</parameter>
            <parameter name="transport.jms.Password" locked="false">admin123</parameter>
        </parameter>
        <parameter name="default" locked="false">
            <parameter name="java.naming.factory.initial" locked="false">weblogic.jndi.WLInitialContextFactory</parameter>
            <parameter name="java.naming.provider.url" locked="false">t3://localhost:7001</parameter>
            <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">jms/myConnectionFactory</parameter>
            <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
            <parameter name="transport.jms.UserName" locked="false">weblogic</parameter>
            <parameter name="transport.jms.Password" locked="false">admin123</parameter>
        </parameter>
    </transportReceiver>
```

### Setting up the JMS sender

To enable the JMS transport sender, un-comment the following
configuration in the `         <EI_HOME>/conf/axis2/axis2.xml        `
file.

``` html/xml
    <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender"/>
```

### Setting up a message store in WSO2 EI

To set up a message store for WebLogic messages in WSO2 EI, use a
configuration similar to the following:

``` html/xml
    <messageStore class="org.wso2.carbon.message.store.persistence.jms.JMSMessageStore"
            name="wso2MessageStore">       
        <parameter name="java.naming.factory.initial">weblogic.jndi.WLInitialContextFactory</parameter>
        <parameter name="store.jms.cache.connection">false</parameter>
        <parameter name="store.jms.password">admin123</parameter>
        <parameter name="java.naming.provider.url">t3://localhost:7001</parameter>
        <parameter name="store.jms.ConsumerReceiveTimeOut">300</parameter>
        <parameter name="store.jms.connection.factory">jms/myConnectionFactory</parameter>
        <parameter name="store.jms.username">weblogic</parameter>
        <parameter name="store.jms.JMSSpecVersion">1.1</parameter>
        <parameter name="store.jms.destination">jms/wso2MessageQueue</parameter>
    </messageStore>
```

### JMS Producer Proxy Service

Use the following proxy service configuration in WSO2 EI to publish
messages to the WebLogic queue:

``` xml
    <proxy xmlns="http://ws.apache.org/ns/synapse"
           name="WeblogicJMSSenderProxy"
           transports="http"
           statistics="disable"
           trace="disable"
           startOnLoad="true">
       <target>
          <inSequence>
             <property name="Accept-Encoding" scope="transport" action="remove"/>
             <property name="Content-Length" scope="transport" action="remove"/>
             <property name="Content-Type" scope="transport" action="remove"/>
             <property name="User-Agent" scope="transport" action="remove"/>
             <log level="custom">
                <property name="STATUS:"
                          value="------Message send by WeblogicJMSConsumerProxy--------"/>
             </log>
             <property name="OUT_ONLY" value="true"/>
             <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
             <send>
                <endpoint>
                   <address uri="jms:/jms/TestJMSQueue1?transport.jms.ConnectionFactoryJNDIName=jms/TestConnectionFactory1&amp;java.naming.factory.initial=weblogic.jndi.WLInitialContextFactory&amp;java.naming.provider.url=t3://localhost:7001&amp;transport.jms.DestinationType=queue"/>
                </endpoint>
             </send>
          </inSequence>
          <outSequence>
             <send/>
          </outSequence>
       </target>
       <description/>
    </proxy>
```

### JMS Consumer Proxy Service

Use the following proxy service configuration in WSO2 EI to read
messages from the WebLogic queue:

``` xml
    <proxy xmlns="http://ws.apache.org/ns/synapse"
           name="WeblogicJMSConsumerProxy"
           transports="jms"
           statistics="disable"
           trace="disable"
           startOnLoad="true">
       <target>
          <inSequence>
             <log level="custom">
                <property name="STATUS:"
                          value="------Message consumed by WeblogicJMSConsumerProxy--------"/>
             </log>
             <log level="full"/>
          </inSequence>
          <outSequence>
             <send/>
          </outSequence>
       </target>
       <parameter name="transport.jms.Destination">jms/TestJMSQueue1</parameter>
       <description/>
    </proxy>
```
