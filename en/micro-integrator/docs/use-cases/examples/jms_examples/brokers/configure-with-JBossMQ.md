# Configure with JBossMQ

The following instructions describe how to set up the [JMS
transport](_JMS_Transport_) with
[JBossMQ](https://community.jboss.org/wiki/JBossMQ) , the default JMS
provider in JBoss Application Server 4.2. (JBossMQ was replaced by
[JBoss Messaging](http://www.jboss.org/jbossmessaging) in JBoss
Application Server 5.0.)

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


To configure the JMS transport with JBossMQ:

1.  Copy the following client libraries to the
    `          <EI_HOME>/lib         ` directory.  
    -   `            <JBOSS_HOME>/lib/jboss­system.jar           `
    -   `            <JBOSS_HOME>/client/jbossall­client.jar           `
2.  Enable the JMS transport listener by adding the following listener
    configuration to the
    `           <EI_HOME>/conf/axis2/axis2.xml          ` file:

    ``` java
        <!­­ Configuration for JBoss 4.2.2 GA MQ ­­>
        <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
          <parameter name="MyQueueConnectionFactory" locked="false">
            <parameter name="java.naming.factory.initial" locked="false">org.jnp.interfaces.NamingContextFactory</parameter>
            <parameter name="java.naming.factory.url.pkgs" locked="false">org.jnp.interfaces:org.jboss.naming</parameter>
            <parameter name="java.naming.provider.url" locked="false">jnp://localhost:1099</parameter>
            <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">/ConnectionFactory</parameter>
            <parameter name="transport.jms.Destination" locked="true">queue/susaQueue</parameter>
          </parameter>
        </transportReceiver>
    ```

3.  Enable the JMS transport sender by uncommenting the following line
    in the `           <EI_HOME>/conf/axis2/axis2.xml          ` file:

    ``` java
            <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender"/>
    ```

4.  Start the WSO2 EI and ensure that the logs prints messages
    indicating that the JMS listener and sender are started and that the
    JMS transport is initialized.
