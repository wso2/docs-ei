# Sample 257: Proxy Services with the FIX Transport

!!! warning

Note that WSO2 EI is shipped with the following changes to what is
mentioned in this documentation :

-   `           <PRODUCT_HOME>/          `
    `           repository/samples/          ` directory that includes
    all Integration profile samples is changed to
    `           <EI_HOME>/          `
    `           samples/service-bus/          ` .
    `                     `
-   `           <PRODUCT_HOME>/          `
    `           repository/samples/resources/          ` directory that
    includes all artifacts related to the Integration profile samples is
    changed to `           <EI_HOME>/          `
    `           samples/service-bus/resources/          ` .

TEST  

**Objective** : Demonstrate the usage of the FIX (Financial Information
eXchange) transport with Proxy Services .

###### **Prerequisites**

-   You will need the two sample FIX applications that come with
    Quickfix/J (Banzai and Executor). Configure the two applications to
    establish sessions with the ESB.

    !!! info

    For information on setting up Quickfix/J, see [Configuring WSO2
    Enterprise Integrator to use the FIX
    transport](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ConfiguringWSO2EnterpriseIntegratortousetheFIXtransport)
    .

    TEST  

-   Start Banzai and Executor.
-   Enable FIX transport in the Synapse `          axis2.xml         ` .
-   Configure Synapse for FIX samples.
-   Open the \<
    `           ESB_HOME>/repository/samples/synapse_sample_257.xml          `
    file and make sure that
    `           transport.fix.AcceptorConfigURL          ` property
    points to the `           fix-synapse.cfg          ` file you
    created. Also make sure that
    `           transport.fix. InitiatorConfigURL          ` property
    points to the `           synapse-sender.cfg          ` file you
    created.

    !!! info

    Note

    The ESB creates a new FIX session with Banzai at this point.

    TEST  

-   Send an order request from Banzai to the ESB.

WSO2 ESB will create a session with an Executor and forward the order
request. The responses coming from the Executor will be sent back to
Banzai.

``` html/xml
<definitions xmlns="http://ws.apache.org/ns/synapse">
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
</definitions>
```

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
`         <ESB_HOME>/repository/samples/resources/fix        ` folder to
`         $QFJ_HOME/etc folder        ` . Execute the sample apps from
`         <QFJ_HOME>/bin," "./banzai.sh/bat ../etc/banzai.cfg executor.sh/bat ../etc/executor.cfg        `
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

To run the FIX samples used in this guide, you need a local Quickfix/J
installation ( http://www.quickfixj.org ). Download Quickfix/J from:
http://www.quickfixj.org/downloads .

To enable the FIX transport for samples, first you must deploy the
Quickfix/J libraries into the
`         repository/components/lib        ` directory of the ESB.
Generally the  following libraries should be deployed into the ESB.

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
Alternatively if the FIX transport management bundle is in use, you can
enable the FIX transport listener and the sender from the WSO2 ESB
management console. Login to the console and navigate to " Transports "
on management menu. Scroll down to locate the sections related to the
FIX transport. Simply click on the "Enable" links to enable the FIX
listener and the sender.

### Configuring WSO2 ESB for FIX Samples

In order to configure WSO2 ESB to run the FIX samples given in this
guide, you will need to create some FIX configuration files as specified
below (you can find the config files from
`         <ESB_HOME>/repository/samples/resources/fix folder        ` ).

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
