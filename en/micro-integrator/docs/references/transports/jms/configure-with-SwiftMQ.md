# Configure with SwiftMQ

This section describes how to configure the WSO2 Enterprise Integrator's
JMS transport with SwiftMQ. Follow the instructions below to set up and
configure.

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


1\. Download and set up SwiftMQ . Instructions can be found in SwiftMQ
documentation.

2\. If you have not already done so, download and install WSO2 EI as
described in [Installation
Guide](https://docs.wso2.com/display/EI650/Installation+Guide) .

!!! info

SwiftMQ should be up and running before starting WSO2 EI.


3\. Copy the following client libraries from \<SMQ\_HOME\>/lib directory
to \< EI\_HOME\>/lib directory.

-   jms.jar
-   jndi.jar
-   swiftmq.jar

!!! info

Always use the standard client libraries that come with a particular
version of SwiftMQ, in order to avoid version incompatibility issues. We
recommend you to remove old client libraries, if any, from all locations
including \< EI\_HOME\>/lib and \< EI\_HOME\>/droppins before copying
the ones relevant to a given version.


4\. Next, configure transport listeners and senders in WSO2 EI.

#### Setting up the JMS Listener

To enable the JMS transport listener, add the following listener
configuration related to SwiftMQ to \<EI\_HOME\>/conf/axis2/axis2.xml
file.  

``` html/xml
    <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
       <parameter name="myTopicConnectionFactory" locked="false">
           <parameter name="java.naming.factory.initial" locked="false">com.swiftmq.jndi.InitialContextFactoryImpl</parameter>
           <parameter name="java.naming.provider.url" locked="false">smqp://localhost:4001/timeout=10000</parameter>
           <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">TopicConnectionFactory</parameter>
           <parameter name="transport.jms.JMSSpecVersion" locked="false">1.0</parameter>
           <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
       </parameter>
       <parameter name="myQueueConnectionFactory" locked="false">
           <parameter name="java.naming.factory.initial" locked="false">com.swiftmq.jndi.InitialContextFactoryImpl</parameter>
           <parameter name="java.naming.provider.url" locked="false">smqp://localhost:4001/timeout=10000</parameter>
           <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
           <parameter name="transport.jms.JMSSpecVersion" locked="false">1.0</parameter>
           <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
       </parameter>
       <parameter name="default" locked="false">
           <parameter name="java.naming.factory.initial" locked="false">com.swiftmq.jndi.InitialContextFactoryImpl</parameter>
           <parameter name="java.naming.provider.url" locked="false">smqp://localhost:4001/timeout=10000</parameter>
           <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
           <parameter name="transport.jms.JMSSpecVersion" locked="false">1.0</parameter>
           <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
       </parameter>
    </transportReceiver>
```

#### Setting up the JMS Sender

To enable the JMS transport sender, add the following configuration in
\<EI\_HOME\>/conf/axis2/axis2.xml file.

``` html/xml
    <!-- uncomment this and configure to use connection pools for sending messages -->
    <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender"/>
```

!!! info For details on the JMS configuration parameters used in the
code segments above, see [JMS Connection Factory
Parameters](JMS-Transport_119130301.html#JMSTransport-JMSConnectionFactoryParameters)

  
You now have instances of SwiftMQ and WSO2 EI configured, up and
running. Next, refer to section [JMS Usecases](_JMS_Usecases_) for
implementation details of various JMS use cases.  
