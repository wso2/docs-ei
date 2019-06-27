# Configure with IBM WebSphere Application Server

This page describes how to configure the WSO2 JMS transport with IBM
<sup>®</sup> WebSphere <sup>®</sup> Application Server.

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


1\. Set up IBM WebSphere Application Server according to the instructions
provided by IBM.  

2\. Create a JMS queue (e.g., **samplequeue** ) and a JMS connection
factory (e.g., **QueueConnectionFactory** ) as described in the topics
under [Setting Up JMS in IBM WebSphere Application
Server](http://pic.dhe.ibm.com/infocenter/iisinfsv/v8r5/index.jsp?topic=%2Fcom.ibm.swg.im.iis.infoservdir.user.doc%2Ftopics%2Ft_isd_user_creating_jms_que_cx_fact.html)
in the IBM documentation.

3\. Copy the following libraries from \<WEBSHPERE\_HOME\>/java/lib
directory to \<EI\_HOME\>/lib directory .

-   com.ibm.ws.runtime.jar
-   com.ibm.ws.admin.client\_7.0.0.jar
-   com.ibm.ws.sib.client.thin.jms\_7.0.0.jar
-   com.ibm.ws.webservices.thinclient\_7.0.0.jar
-   bootstrap.jar

4\. Add the following entries to the \<EI\_HOME\>/conf/etc/launch.ini
file .

``` java
    javax.jms,\
    javax.rmi.CORBA,\
```

5\. Enable the JMS Listener and Sender in
\<EI\_HOME\>/conf/axis2/axis2.xml by un-commenting the following lines
of code.  

**JMS Listener**

``` html/xml
       <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
           <parameter name="myQueueConnectionFactory" locked="false">
                <parameter name="java.naming.factory.initial" locked="false">com.ibm.websphere.naming.WsnInitialContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">iiop://localhost:2809</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                <parameter name="transport.jms.Destination">samplequeue</parameter>
           </parameter>
     
            <parameter name="default" locked="false">
                <parameter name="java.naming.factory.initial" locked="false">com.ibm.websphere.naming.WsnInitialContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">iiop://localhost:2809</parameter>                           
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                <parameter name="transport.jms.Destination">samplequeue</parameter>
           </parameter>
       </transportReceiver>
```

**JMS Sender**

``` html/xml
    <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender"/> 
```

!!! info For details on the JMS configuration parameters used in the
code segments above, see [JMS Connection Factory
Parameters](JMS-Transport_119130301.html#JMSTransport-JMSConnectionFactoryParameters)

  
6. Start IBM WebSphere Application Server and WSO2 EI.

Y ou now have instances of IBM WebSphere Application Server and WSO2 EI
configured and running. Next, see [JMS Usecases](_JMS_Usecases_) for
implementation details of various JMS use cases.  
