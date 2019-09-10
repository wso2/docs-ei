# Switching from HTTP to FIX

Demonstrate switching from HTTP to FIX.

###### Prerequisites

-   You will need the Executor sample application that comes with
    Quickfix/J. Configure the Executor to establish a session with
    Synapse. Configuring Sample FIX Applications .
-   Start the Executor.
-   Enable FIX transport sender in the Synapse
    `          axis2.xml         ` .
-   Configure Synapse for FIX samples. See Configuring WSO2 ESB for FIX
    Samples . There is no need to create the
    `          fix-synapse.cfg         ` file for this sample. Having
    only the `          synapse-sender.cfg         ` file is sufficient.
-   Go to the
    `          ESB_HOME/repository/samples/synapse_sample_258.xml         `
    file and make sure that
    `          transport.fix.InitiatorConfigURL         ` property
    points to the `          synapse-sender.cfg         ` file you
    created.
-   Invoke the FIX Client as follows. This command sends a FIX message
    embedded in a SOAP message over HTTP.

``` java
ant fixclient -Dsymbol=IBM -Dqty=5 -Dmode=buy -Daddurl=http://localhost:8280/services/FIXProxy
```

Synapse will create a session with Executor and forward the order
request. The first response coming from the Executor will be sent back
over HTTP. Executor generally sends two responses for each incoming
order request. But since the response has to be forwarded over HTTP,
only one can be sent back to the client.

```
<definitions xmlns="http://ws.apache.org/ns/synapse">
    <proxy name="FIXProxy">
        <parameter name="transport.fix.InitiatorConfigURL">file:/home/synapse_user/fix-config/synapse-sender.cfg</parameter>
        <parameter name="transport.fix.InitiatorMessageStore">file</parameter>
        <parameter name="transport.fix.SendAllToInSequence">false</parameter>
        <parameter name="transport.fix.DropExtraResponses">true</parameter>
        <target>
            <endpoint>
                <address uri="fix://localhost:19876?BeginString=FIX.4.0&SenderCompID=SYNAPSE&TargetCompID=EXEC"/>
            </endpoint>
            <inSequence>
                <property name="transport.fix.ServiceName" value="FIXProxy" scope="axis2-client"/>
                <log level="full"/>
            </inSequence>
            <outSequence>
                <log level="full"/>
                <send/>
            </outSequence>
        </target>
    </proxy>
</definitions>
```

### Configuring Sample FIX Applications

If you are using a binary distribution of Quickfix/J, the two samples
and their configuration files are all packed to a single JAR file called
`         quickfixj-examples.jar        ` . You will have to extract the
JAR file, modify the configuration files and pack them to a JAR file
again under the same name.

You can pass the new configuration file as a command line parameter too,
in that case you do not need to modify the
`         quickfixj-examples.jar        ` . You can copy the config
files from `         $ESB_HOME/repository/samples/resources/fix        `
folder to `         $QFJ_HOME/etc folder        ` . Execute the sample
apps from `         $QFJ_HOME/bin        ` ,
`         ./banzai.sh/bat ../etc/banzai.cfg executor.sh/bat ../etc/executor.cfg        `
.

Locate and edit the FIX configuration file of Executor to be as follows.
This file is usually named `         executor.cfg        ` .

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

### Setting up FIX Transport

To run the FIX samples used in this guide, you need a local Quickfix/J (
http://www.quickfixj.org ) installation. Download Quickfix/J from:
http://www.quickfixj.org/downloads .

To enable the FIX transport for samples, first you must deploy the
Quickfix/J libraries into the
`         repository/components/lib        ` directory of the ESB.
Generally following libraries should be deployed into the ESB.

-   **quickfixj-core-1.4.0.jar**
-   **quickfixj-msg-fix40-1.4.0.jar**
-   **quickfixj-msg-fix42-1.4.0.jar**
-   **quickfixj-msg-fix41-1.4.0.jar**
-   **quickfixj-msg-fix43-1.4.0.jar**
-   **quickfixj-msg-fix44-1.4.0.jar**
-   **quickfixj-msg-fix50-1.4.0.jar**
-   **mina-core-1.1.0.jar**
-   **slf4j-jdk14-1.5.3.jar**
-   **slf4j-api-1.5.3.jar**

Then uncomment the FIX transport sender and FIX transport receiver
configurations in the `         repository/conf/axis2.xml        ` .
Simply locate and uncomment the `         FIXTransportSender        `
and `         FIXTransportListener        ` sample configurations.
Alternatively, if the FIX transport management bundle is in use, you can
enable the FIX transport listener and the sender from the WSO2 ESB
management console. Login to the console and navigate to " Transports "
on management menu. Scroll down to locate the sections related to the
FIX transport. Simply click on the "Enable" links to enable the FIX
listener and the sender.

### Configuring WSO2 ESB for FIX Samples

In order to configure WSO2 ESB to run the FIX samples given in this
guide, you will need to create some FIX configuration files as specified
below (you can find the config files from
`         $ESB_HOME/repository/samples/resources/fix folder        ` ).

The `         FileStorePath        ` property in the following two files
should point to two directories in your local file system. Once the
samples are executed, Synapse will create FIX message stores in these
two directories.

Put the following entries in a file called
`         fix-synapse.cfg        ` .

``` java
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

Put the following entries in a file called
`         synapse-sender.cfg        ` .

``` java
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
