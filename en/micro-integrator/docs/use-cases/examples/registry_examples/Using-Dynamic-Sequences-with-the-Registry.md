# Sample 9: Introduction to Dynamic Sequences with the Registry

### Introduction

This sample demonstrates how you can achieve dynamic behavior of the
WSO2 ESB by the use of a registry.

The ESB supports dynamic definitions for sequences, endpoints and
configuration resources. Here a Synapse configuration is defined which
references a sequence definition specified as a registry key. The
registry key resolves to the actual content of the sequence which is
loaded dynamically by the ESB at runtime and cached appropriately as per
its definition in the registry. Once the cache expires, ESB rechecks the
meta information for the definition, reloads the sequence definition if
necessary, and caches it again.

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <registry provider="org.wso2.carbon.mediation.registry.ESBRegistry">
            <parameter name="root">file:./repository/samples/resources/</parameter>
            <parameter name="cachableDuration">15000</parameter>
        </registry>
        <sequence name="main">
            <sequence key="sequence/dynamic_seq_1.xml"/>
        </sequence>
    </definitions>
```

This configuration file `         synapse_sample_9.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start the Axis2 server. For instructions on starting the Axis2
    server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

2.  Deploy the back-end service **SimpleStockQuoteService** . For
    instructions on deploying sample back-end services, see [Deploying
    sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

    Now you have a running ESB instance and a back-end service deployed.
    In the next section, we will send a message to the back-end service
    through the ESB using a sample client.

### Executing the sample

The sample client used here is the **Stock Quote Client** , which can
operate in several modes. For further details on this sample client and
its operation modes, see [Stock Quote
Client](https://docs.wso2.com/display/EI650/Using+the+Sample+Clients#UsingtheSampleClients-StockQuoteClient)
.

**To execute the sample client**

1.  Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          ` directory.

    ``` bash
            ant stockquote -Daddurl=http://localhost:9000/services/SimpleStockQuoteService -Dtrpurl=http://localhost:8280/
    ```

2.  Execute the client immediately again (within 15 seconds of the last
    execution) using the above command to make sure that the sequence is
    not reloaded.
3.  Edit the sequence definition in
    `          <ESB_HOME>/repository/samples/resources/sequence/dynamic_seq_1.xml,         `
    change the log message to *Test Message 2* and execute the client
    again.
4.  Wait for more than 15 seconds since the original caching of the
    sequence and execute the client again.

### Analyzing the output

When you run the client for the first time, ESB fetches the definition
of the sequence from the registry and executes its rules. Analyze the
debug log output on the ESB console, you will see the following:

``` java
    [HttpServerWorker-1] DEBUG SequenceMediator - Sequence mediator <dynamic_sequence> :: mediate()
    ...
    [HttpServerWorker-1] INFO  LogMediator - message = *** Test Message 1 ***
```

When you execute the client immediately again (within 15 seconds of the
last execution), you will notice that the sequence is not reloaded.

Once you edit the sequence definition and execute the client again, you
will see that the new message is not displayed if you executed the
client within 15 seconds of loading the resource for the first time.
However, after the elapse of 15 seconds since the original caching of
the sequence, you will notice that the new sequence is loaded and
executed by Synapse from the following log message.

However, when you wait for 15 seconds since the original caching of the
sequence and then execute the client,  you will see that the new
sequence is loaded and executed by the ESB. This can be seen by
analyzing the following debug log output on the ESB console.

``` java
    [HttpServerWorker-1] DEBUG SequenceMediator - Sequence mediator <dynamic_sequence> :: mediate()
    ...
    [HttpServerWorker-1] INFO  LogMediator - message = *** Test Message 2 ***
```

The cache timeout could be tuned appropriately by configuring the URL
registry to suit the environment and the needs.
