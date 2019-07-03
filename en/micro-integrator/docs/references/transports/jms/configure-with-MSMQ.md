# Configure with MSMQ

This section describes how to configure the WSO2 Enterprise Integrator's
JMS transport with Microsoft Message Queuing (MSMQ) .

!!! info

The setup instruction here are only applicable for Windows environments
since we invoke Microsoft C++ API for MSMQ via JNI invocations.

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


The **msmq:** component in WSO2 EI is a transport for working with MSMQ.
This component natively sends and receives directly allocated ByteBuffer
instances, allowing access to the JNI layer without memory copying.
Using the **ByteBuffer** created with the method **allocateDirect** ,
the native code can directly access the memory. URI format is
[msmq:msmqQueueName](http://msmqmsmqQueueName) .

Follow the steps below to set up and configure WSO2 EI with MSMQ.

1. Download the
`                   axis2-transport-msmq-2.0.0-wso2v2.jar                 `
file and add it to the `         <EI_HOME>/dropins        ` directory.
This file provides the JNI invocation required by MSMQ bridging.

2\. Install Visual C++ 2008 (VC9). It works with Microsoft Visual Studio
2008 Express.

3\. Set up MSMQ on a Windows environment. For setup instructions, refer
to : http://msdn.microsoft.com/en-us/library/aa967729.aspx .

4\. If you haven't already, download and install WSO2 EI as described in
[Getting
Started](https://docs.wso2.com/display/EI650/Installation+Guide) .  

#### Setting up the JMS Listener

5\. Add the following configuration to \<EI\_HOME\>/conf/axis2/axis2.xml
file.  

``` html/xml
    <transportReceiver name="msmq" class="org.apache.axis2.transport.msmq.MSMQListener">
         <parameter name="msmq.receiver.host" locked="false">localhost</parameter>
    </transportReceiver>
```

#### Setting up the JMS Sender

6\. To enable the JMS transport sender, add the following JMS transport
listener configuration in \<EI\_HOME\>/conf/axis2/axis2.xml file.  

``` html/xml
    <transportSender name="msmq" class="org.apache.axis2.transport.msmq.MSMQSender"/>
```

!!! info

If you get an error message similar to the following when you start the
Integration Profile after configuring the JMS transport with MSMQ, you
must check whether the `         msvcr100d.dll        ` file is
available in the `         C:\Windows\System32        ` directory. If
the file is not available, you need to download the file and copy it to
the `         C:\Windows\System32        ` directory.

``` java
    ERROR - NativeWorkerPool Uncaught exception
    java.lang.UnsatisfiedLinkError: no msmq_native_support in java.library.path
```
    
!!! tip

For details on the JMS configuration parameters used in the code
segments above, see [JMS Connection Factory
Parameters](JMS-Transport_119130301.html#JMSTransport-JMSConnectionFactoryParameters)
.


Y ou now have instances of MSMQ and WSO2 EI configured, up and running.
Next, refer to section [JMS Usecases](_JMS_Usecases_) for implementation
details of various JMS use cases.
