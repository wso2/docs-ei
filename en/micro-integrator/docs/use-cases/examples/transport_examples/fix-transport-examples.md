# Using the FIX Transport

Demonstrate the usage of the FIX (Financial Information eXchange) transport with Proxy Services .

## Synapse configuration

WSO2 Micro Integrator will create a session with an Executor and forward the order request. The responses coming from the Executor will be sent back to
Banzai.

```xml
<proxy name="FIXProxy" transports="fix">
    <parameter name="transport.fix.AcceptorConfigURL">file:/home/synapse_user/fix-config/fix-synapse.cfg</parameter>
    <parameter name="transport.fix.InitiatorConfigURL">file:/home/synapse_user/fix-config/synapse-sender.cfg</parameter>
    <parameter name="transport.fix.AcceptorMessageStore">file</parameter>
    <parameter name="transport.fix.InitiatorMessageStore">file</parameter>
    <target>
        <endpoint>
            <address uri="fix://localhost:19876?BeginString=FIX.4.0&amp;SenderCompID=SYNAPSE&amp;TargetCompID=EXEC"/>
        </endpoint>
    <inSequence>
    <log level="full"/>
    </inSequence>
        <outSequence>
            <log level="full"/>
            <send/>
        </outSequence>
    </target>
</proxy>
```

## Build and run

-   You will need the two sample FIX applications that come with
    Quickfix/J (Banzai and Executor). Configure the two applications to
    establish sessions with the Micro Integrator.
-   Start Banzai and Executor.
-   Be sure that the
    `           transport.fix.AcceptorConfigURL          ` property
    points to the `           fix-synapse.cfg          ` file you
    created. Also make sure that
    `           transport.fix. InitiatorConfigURL          ` property
    points to the `           synapse-sender.cfg          ` file you
    created.

    !!! Note
        The ESB creates a new FIX session with Banzai at this point.

-   Send an order request from Banzai to the Micro Integrator.

### Configuring Sample FIX Applications

If you use a binary distribution of Quickfix/J, the two samples and
their configuration files are all packed to a single JAR file called
`         quickfixj-examples.jar        ` . You will have to extract the
JAR file, modify the configuration files and pack them to a JAR file
again under the same name.

You can pass the new configuration file as a command line parameter too,
in that case you do not need to modify the
`         quickfixj-examples.jar        ` . You can copy the config
files from
`MI_HOME/repository/samples/resources/fix        ` folder to
`$QFJ_HOME/etc folder        ` . Execute the sample apps from
`<QFJ_HOME>/bin," "./banzai.sh/bat ../etc/banzai.cfg executor.sh/bat ../etc/executor.cfg        `
.

Locate and edit the FIX configuration file of Executor to be as follows.
This file is usually named `executor.cfg` .

``` java
[default]
    FileStorePath=examples/target/data/executor
    ConnectionType=acceptor
    StartTime=00:00:00
    EndTime=00:00:00
    HeartBtInt=30
    ValidOrderTypes=1,2,F
    SenderCompID=EXEC
    TargetCompID=SYNAPSE
    UseDataDictionary=Y
    DefaultMarketPrice=12.30

    [session]
    BeginString=FIX.4.0
    SocketAcceptPort=19876
```

Locate and edit the FIX configuration file of Banzai to be as follows.
This file is usually named `         banzai.cfg        ` .

``` java
[default]
    FileStorePath=examples/target/data/banzai
    ConnectionType=initiator
    SenderCompID=BANZAI
    TargetCompID=SYNAPSE
    SocketConnectHost=localhost
    StartTime=00:00:00
    EndTime=00:00:00
    HeartBtInt=30
    ReconnectInterval=5

    [session]
    BeginString=FIX.4.0
    SocketConnectPort=9876
```

The `         FileStorePath        ` property in the above two files
should point to two directories in your local file system. The launcher
scripts for the sample application can be found in the bin directory of 
Quickfix/J distribution.

### Configuring the Micro Integrator for FIX Samples

Create the FIX configuration files as specified below. The `FileStorePath` property in the following two files should point to two directories in your local file system. Once the samples are executed, Synapse will create FIX message stores in these two directories.

```java tab='fix-synapse.cfg'
[default]
    FileStorePath=repository/logs/fix/data
    ConnectionType=acceptor
    StartTime=00:00:00
    EndTime=00:00:00
    HeartBtInt=30
    ValidOrderTypes=1,2,F
    SenderCompID=SYNAPSE
    TargetCompID=BANZAI
    UseDataDictionary=Y
    DefaultMarketPrice=12.30

    [session]
    BeginString=FIX.4.0
    SocketAcceptPort=9876
```

```java tab='synapse-sender.cfg'
[default]
    FileStorePath=repository/logs/fix/data
    SocketConnectHost=localhost
    StartTime=00:00:00
    EndTime=00:00:00
    HeartBtInt=30
    ReconnectInterval=5
    SenderCompID=SYNAPSE
    TargetCompID=EXEC
    ConnectionType=initiator

    [session]
    BeginString=FIX.4.0
    SocketConnectPort=19876
```