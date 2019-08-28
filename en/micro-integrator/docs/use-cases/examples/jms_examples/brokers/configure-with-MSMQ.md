# Configure with MSMQ

This section describes how to configure WSO2 Micro Integrator's
JMS transport with Microsoft Message Queuing (MSMQ) .

!!! Info
    The setup instruction here are only applicable for Windows environments since we invoke Microsoft C++ API for MSMQ via JNI invocations.

The **msmq:** component in WSO2 EI is a transport for working with MSMQ.
This component natively sends and receives directly allocated ByteBuffer
instances, allowing access to the JNI layer without memory copying.
Using the **ByteBuffer** created with the method **allocateDirect** ,
the native code can directly access the memory. URI format is
[msmq:msmqQueueName](http://msmqmsmqQueueName) .

Follow the steps below to set up and configure WSO2 EI with MSMQ.

1. Download the `                   axis2-transport-msmq-2.0.0-wso2v2.jar                 `
file and add it to the `MI_HOME/dropins        ` directory.
This file provides the JNI invocation required by MSMQ bridging.

2. Install Visual C++ 2008 (VC9). It works with Microsoft Visual Studio
2008 Express.

3. Set up MSMQ on a Windows environment. For setup instructions, refer
to : http://msdn.microsoft.com/en-us/library/aa967729.aspx .

4. If you haven't already, download and install WSO2 EI as described in
[Getting
Started](https://docs.wso2.com/display/EI650/Installation+Guide) .  


!!! Info
    If you get an error message similar to the following when you start the Micro Integrator after configuring the JMS transport with MSMQ, you must check whether the `         msvcr100d.dll        ` file is available in the `         C:\Windows\System32        ` directory. If the file is not available, you need to download the file and copy it to the `         C:\Windows\System32        ` directory.

    ``` java
    ERROR - NativeWorkerPool Uncaught exception
    java.lang.UnsatisfiedLinkError: no msmq_native_support in java.library.path
    ```