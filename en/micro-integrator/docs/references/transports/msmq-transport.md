# MSMQ Transport

The **msmq:** component is a transport for working with Microsoft
Message Queuing . This component natively sends and receives direct
allocated ByteBuffer instances. This allows you to access the JNI layer
without expensive memory copying. In fact, using ByteBuffer created with
the method allocateDirect can be passed to the JNI layer, and the native
code is able to directly access the memory.

!!! info

The MSMQ examples only work on Windows, since they invoke Microsoft C++
API for MSMQ via JNI invocation.

**URI format**

    msmq:msmqQueueName

**Examples**

    msmq:DIRECT=OS:localhost\\private$\\test?concurrentConsumers=1
    msmq:DIRECT=OS:localhost\\private$\\test?deliveryPersistent=true&priority=5&timeToLive=10

## Configuring the MSMQ transport

Update the following configurations in the ei.toml file:

```toml
[transport.msmq]
listener.enable = false
listener.hostname = "$ref{server.hostname}"
sender.enable = false
```

!!! Info
    -   Download the `                         axis2-transport-msmq-2.0.0-wso2v2.jar                       ` file and add it to the `            <EI_HOME>/dropins           ` directory. This file provides the JNI invocation required by MSMQ bridging.
    -   Make sure MQ installed and running. For more information, see
    <http://msdn.microsoft.com/en-us/library/aa967729.aspx> .
    -   Make sure that you have installed Visual C++ 2008 (VC9) and that it
    works with Microsoft Visual Studio 2008 Express.

