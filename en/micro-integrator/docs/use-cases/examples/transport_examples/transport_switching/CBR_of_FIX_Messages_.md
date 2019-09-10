# Sample 262: CBR of FIX Messages

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

**Objective** : Demonstrate the capability of CBR FIX messages.

###### **Prerequisites**

-   You will need the two sample FIX applications that come with
    Quickfix/J (Banzai and Executor). Configure the two applications to
    establish sessions with Synapse.
-   Add the following lines to `          banzai.cfg         ` .

``` java
DataDictionary=~/etc/spec/FIX40-synapse.xml
```

!!! info

Note

`         FIX40-synapse.xml        ` can be found at \<
`         EI_HOME>/samples/service-bus/resources/fix        ` . This is
a custom FIX data dictionary file that has added tag 150,151 to the
execution messages (35=8) of FIX4.0. Make sure the
`         DataDictionary        ` property of the
`         banzai.cfg        ` points to this data dictionary file.

TEST  

-   Add the following lines to `          executor.cfg         ` .

``` java
[session]
BeginString=FIX.4.1
SocketAcceptPort=19877
```

-   Start Banzai and Executor using the custom config files.
-   Enable FIX transport in the Synapse `          axis2.xml         ` .
-   Configure Synapse for FIX samples.
-   Open up the \<
    `          EI_HOME>/samples/service-bus/synapse_sample_262.xml         `
    file and make sure that
    `          transport.fix.AcceptorConfigURL         ` property points
    to the `          fix-synapse.cfg         ` file you created and
    `          transport.fix.InitiatorConfigURL         ` points to the
    `          synapse-sender.cfg         ` file you created.

!!! info

Note

Synapse creates a new FIX session with Banzai at this point.

TEST  

-   Send an order request from Banzai to Synapse. For example, Buy GOOG
    1000 @ MKT, Buy MSFT 3000 @ MKT, Buy SUNW 500 @ 100.20.
-   Synapse will forward the order requests with symbol "GOOG" to FIX
    endpoint FIX-4.0 @ localhost:19876.
-   Synapse will forward the order requests with symbol "MSFT" to FIX
    endpoint FIX-4.1 @ localhost:19877.
-   Synapse will not forward the orders with other symbols to any
    endpoint (default case has kept blank in the configuration).

``` html/xml
<definitions xmlns="http://ws.apache.org/ns/synapse">
    <sequence name="CBR_SEQ">
        <in>
            <switch source="//message/body/field@id='55'">
                <case regex="GOOG">
                    <send>
                        <endpoint>
                            <address
                                uri="fix://localhost:19876?BeginString=FIX.4.0&SenderCompID=SYNAPSE&TargetCompID=EXEC" />
                        </endpoint>
                    </send>
                </case>
                <case regex="MSFT">
                    <send>
                        <endpoint>
                            <address
                                uri="fix://localhost:19877?BeginString=FIX.4.1&SenderCompID=SYNAPSE&TargetCompID=EXEC" />
                        </endpoint>
                    </send>
                </case>
                <default></default>
            </switch>
        </in>
        <out>
            <send />
        </out>
    </sequence>
    <proxy name="FIXProxy" transports="fix">
        <target inSequence="CBR_SEQ" />
        <parameter name="transport.fix.AcceptorConfigURL">
            file:repository/samples/resources/fix/fix-synapse.cfg
        </parameter>
        <parameter name="transport.fix.AcceptorMessageStore">
            file
        </parameter>
        <parameter name="transport.fix.InitiatorConfigURL">
            file:repository/samples/resources/fix/synapse-sender.cfg
        </parameter>
        <parameter name="transport.fix.InitiatorMessageStore">
            file
        </parameter>
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
files from \< `         EI_HOME>/samples/service-bus/        `
`         resources/fix        ` folder to \<
`         QFJ_HOME>/etc folder        ` . Execute the sample apps from
\< `         QFJ_HOME>/bin        ` ,
`         ./banzai.sh/bat ../etc/banzai.cfg executor.sh/bat ../etc/executor.cfg        `
.

Locate and edit the FIX configuration file of Executor to be as follows.
This file is usually named `         executor.cfg.        `

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

### Configuring WSO2 EI for FIX Samples

In order to configure WSO2 EI to run the FIX samples given in this
guide, you will need to create some FIX configuration files as specified
below (you can find the config files from \<
`         EI_HOME>/samples/service-bus        `
`         /resources/fix folder        ` ).

The `         FileStorePath        ` property in the following two files
should point to two directories in your local file system. Once the
samples are executed, Synapse will create FIX message stores in these
two directories.

Put the following entries in the file called
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

Put the following entries in the file called
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
