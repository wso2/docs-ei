# Configure with Tibco EMS

This section describes how to configure the WSO2 Enterprise Integrator's
JMS transport with Tibco EMS . Follow the steps below to set up and
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


1.  Download and set up Tibco EMS in your environment.
2.  If you have not done so already, download and install WSO2 EI as
    described in [Installation
    Guide](https://docs.wso2.com/display/EI650/Installation+Guide) .
3.  Copy the Tibco EMS client jar files that are shipped with the
    distribution to the `          <EI_HOME>/extensions         `
    directory.
    -   tibcrypt.jar or jms-2.0.jar (If Tibco EMS 8.4 version is used,
        be sure to use jms-2.0.jar)
    -   tibjms.jar
    -   tibjmsadmin.jar
    -   tibjmsapps.jar
    -   tibrvjms.jar
4.  WSO2 EI does not have a default configuration script for TIBCO EMS.
    Therefore, add the following configuration to the
    `          <EI_HOME>/conf/axis2/axis2.xml         ` file.

### Setting up the JMS Listener

``` html/xml
    <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
        <parameter name="TopicConnectionFactory" locked="false">
           <parameter locked="false" name="java.naming.factory.initial">
             com.tibco.tibjms.naming.TibjmsInitialContextFactory
           </parameter>
           <parameter locked="false" name="java.naming.provider.url">tcp://127.0.0.1:37222</parameter>
           <parameter locked="false" name="java.naming.security.principal">admin</parameter>
           <parameter locked="false" name="java.naming.security.credentials"/>
           <parameter locked="false" name="transport.jms.ConnectionFactoryJNDIName">TopicConnectionFactory</parameter>
           <parameter locked="false" name="transport.jms.JMSSpecVersion">1.0.2b</parameter>
           <parameter locked="false" name="transport.jms.ConnectionFactoryType">topic</parameter>
           <parameter locked="false" name="transport.jms.UserName">admin</parameter>
           <parameter locked="false" name="transport.jms.Password">admin</parameter>
           <parameter locked="false" name="transport.jms.CacheLevel">session</parameter>
        </parameter>
        <parameter locked="false" name="QueueConnectionFactory">
           <parameter locked="false" name="java.naming.factory.initial">
             com.tibco.tibjms.naming.TibjmsInitialContextFactory
           </parameter>
           <parameter locked="false" name="java.naming.provider.url">tcp://127.0.0.1:37222</parameter>
           <parameter locked="false" name="java.naming.security.principal">admin</parameter>
           <parameter locked="false" name="java.naming.security.credentials"/>
           <parameter locked="false" name="transport.jms.ConnectionFactoryJNDIName">QueueConnectionFactory</parameter>
           <parameter locked="false" name="transport.jms.JMSSpecVersion">1.0.2b</parameter>
           <parameter locked="false" name="transport.jms.ConnectionFactoryType">queue</parameter>
           <parameter locked="false" name="transport.jms.UserName">admin</parameter>
           <parameter locked="false" name="transport.jms.Password">admin</parameter>
           <parameter locked="false" name="transport.jms.CacheLevel">session</parameter>
        </parameter>
        <parameter name="default" locked="false">
           <parameter locked="false" name="java.naming.factory.initial">
              com.tibco.tibjms.naming.TibjmsInitialContextFactory
           </parameter>
           <parameter locked="false" name="java.naming.provider.url">tcp://127.0.0.1:37222</parameter>
           <parameter locked="false" name="java.naming.security.principal">admin</parameter>
           <parameter locked="false" name="java.naming.security.credentials"/>
           <parameter locked="false" name="transport.jms.ConnectionFactoryJNDIName">QueueConnectionFactory</parameter>
           <parameter locked="false" name="transport.jms.JMSSpecVersion">1.0.2b</parameter>
           <parameter locked="false" name="transport.jms.ConnectionFactoryType">queue</parameter>
           <parameter locked="false" name="transport.jms.UserName">admin</parameter>
           <parameter locked="false" name="transport.jms.Password">admin</parameter>
           <parameter locked="false" name="transport.jms.CacheLevel">session</parameter>
       </parameter>
    </transportReceiver>
```

### Setting up the JMS Sender

To enable the JMS transport sender, add the following JMS transport
listener configuration in \<EI\_HOME\>/conf/axis2/axis2.xml file.

``` html/xml
    <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender">
       <parameter locked="false" name="QueueConnectionFactory">
          <parameter locked="false" name="java.naming.factory.initial">
             com.tibco.tibjms.naming.TibjmsInitialContextFactory
          </parameter>
          <parameter locked="false" name="java.naming.provider.url">tcp://127.0.0.1:37222</parameter>
          <parameter locked="false" name="transport.jms.ConnectionFactoryJNDIName">QueueConnectionFactory</parameter>
          <parameter locked="false" name="transport.jms.JMSSpecVersion">1.0.2b</parameter>
          <parameter locked="false" name="transport.jms.ConnectionFactoryType">queue</parameter>
       </parameter>
    </transportSender>
```

For details on the JMS configuration parameters used in the code
segments above, see [JMS Connection Factory
Parameters](JMS-Transport_119130301.html#JMSTransport-JMSConnectionFactoryParameters)
.

You now have instances of Tibco EMS and WSO2 EI configured, up and
running. Next, see [JMS Usecases](_JMS_Usecases_) for implementation
details of various JMS use cases.  
