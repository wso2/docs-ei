# MSMQ Transport

The **msmq:** component is a transport for working with Microsoft
Message Queuing . This component natively sends and receives direct
allocated ByteBuffer instances. This allows you to access the JNI layer
without expensive memory copying. In fact, using ByteBuffer created with
the method allocateDirect can be passed to the JNI layer, and the native
code is able to directly access the memory.

**URI format**

    msmq:msmqQueueName

**Examples**

    msmq:DIRECT=OS:localhost\\private$\\test?concurrentConsumers=1
    msmq:DIRECT=OS:localhost\\private$\\test?deliveryPersistent=true&priority=5&timeToLive=10

##### Configuring the MSMQ transport

1.  In the `            axis2.           ` xml file at location
    `            <EI_HOME>/conf/axis2           ` , define the MSMQ
    sender/listener pair as follows:

    ``` java
        <transportSender name="msmq" class="org.apache.axis2.transport.msmq.MSMQSender"/>
    
        <transportReceiver name="msmq" class="org.apache.axis2.transport.msmq.MSMQListener">
                  <parameter name="msmq.receiver.host" locked="false">localhost</parameter>
        </transportReceiver>
    ```

2.  Download the
    `                         axis2-transport-msmq-2.0.0-wso2v2.jar                       `
    file and add it to the `            <EI_HOME>/dropins           `
    directory. This file provides the JNI invocation required by MSMQ
    bridging.

3.  Make sure MQ installed and running. For more information, see
    <http://msdn.microsoft.com/en-us/library/aa967729.aspx> .

4.  Make sure that you have installed Visual C++ 2008 (VC9) and that it
    works with Microsoft Visual Studio 2008 Express.

  

!!! info

The MSMQ examples only work on Windows, since they invoke Microsoft C++
API for MSMQ via JNI invocation.


For more information, see:

-   [Configure the JMS Transport with MSMQ](_Configure_with_MSMQ_)
-   [Sample 270: Transport switching from HTTP to MSMQ and MSMQ to
    HTTP](https://docs.wso2.com/display/EI650/Sample+270%3A+Transport+switching+from+HTTP+to+MSMQ+and++MSMQ+to+HTTP)
