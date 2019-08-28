# Configure with Apache Artemis

This section describes how to configure the JMS transport of the ESB
Profile of WSO2 Enterprise Integrator (WSO2 EI) with Apache Artemis
version 2.6.1 .

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


Follow the instructions below to set up and configure.

1.  Download, set up and start [Apache
    Artemis](https://activemq.apache.org/artemis/) .

2.  Set up the ESB Profile of WSO2 EI. For instructions, see
    [Installation Guide](https://docs.wso2.com/display/EI650/Installation+Guide)
    .

        !!! info
    
        Do not start the ESB Profile of WSO2 EI at this point. Apache
        Artemis should be up and running before starting the ESB Profile of
        WSO2 EI.
    

3.  Add the below transport receiver configuration to the
    `           <EI_HOME>/conf/axis2/axis2.xml          ` file.

    ``` xml
        <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
           <parameter name="myTopicConnectionFactory" locked="false">
              <parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory</parameter>
              <parameter name="java.naming.provider.url" locked="false">tcp://localhost:61616</parameter>
              <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">TopicConnectionFactory</parameter>
              <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
           </parameter>
           <parameter name="myQueueConnectionFactory" locked="false">
              <parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory</parameter>
              <parameter name="java.naming.provider.url" locked="false">tcp://localhost:61616</parameter>
              <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
              <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
           </parameter>
           <parameter name="default" locked="false">
              <parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory</parameter>
              <parameter name="java.naming.provider.url" locked="false">tcp://localhost:61616</parameter>
              <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
              <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
           </parameter>
        </transportReceiver>
    ```

4.  Add the below transport sender configuration to the
    `           <EI_HOME>/conf/axis2/axis2.xml          ` file .  

    ``` xml
            <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender">
               <parameter name="commonJmsSenderConnectionFactory" locked="false">
                  <parameter locked="false" name="java.naming.factory.initial">org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory</parameter>
                  <parameter locked="false" name="java.naming.provider.url">tcp://localhost:61616</parameter>
                  <parameter locked="false" name="transport.jms.ConnectionFactoryJNDIName">QueueConnectionFactory</parameter>
                  <parameter locked="false" name="transport.jms.ConnectionFactoryType">queue</parameter>
               </parameter>
               <parameter name="commonTopicPublisherConnectionFactory" locked="false">
                  <parameter locked="false" name="java.naming.factory.initial">org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory</parameter>
                  <parameter locked="false" name="java.naming.provider.url">tcp://localhost:61616</parameter>
                  <parameter locked="false" name="transport.jms.ConnectionFactoryJNDIName">TopicConnectionFactory</parameter>
                  <parameter locked="false" name="transport.jms.ConnectionFactoryType">topic</parameter>
               </parameter>
            </transportSender>
    ```

5.  Remove any existing Apache ActiveMQ client JAR files from the
    `          <EI_HOME>/dropins/         ` and
    `          <EI_HOME>/lib/         ` directories.  
6.  Download the
    [artemis-jms-client-all-2.6.1.jar](attachments/119130330/119130331.jar)
    file and copy it to the `          <EI_Home>/lib/         `
    directory.  
7.  Remove the below line from the
    `           <EI_HOME>/conf/etc/launch.ini          ` file.  

    ``` text
            javax.jms,\
    ```

8.  Start Apache Artemis. For instructions, go to [Apache Artemis
    Documentation](https://activemq.apache.org/artemis/docs.html) .

9.  Start the ESB Profile of WSO2 EI by navigating to the
    `           <EI_HOME>/bin          ` directory and executing
    `           ./integrator.sh          ` (on Linux/OSX) or
    `           integrator.bat          ` (on Windows).

Now you have instances of Apache Artemis and the ESB Profile of WSO2 EI
configured, up and running.  Next, let's take a look at implementation
details of various [JMS use cases](_JMS_Usecases_) .
