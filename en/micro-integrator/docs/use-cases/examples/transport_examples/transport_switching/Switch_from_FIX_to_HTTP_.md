# Switch from FIX to HTTP

Demonstrate the capability of switching form FIX to
HTTP.

###### Prerequisites

-   You will need the sample FIX blotter that come with Quickfix/J
    (Banzai). Configure the blotter to establish sessions with Synapse.
-   Start the Axis2 server and deploy the
    `          SimpleStockQuoteService         ` if not already
    deployed.
-   Start Banzai.
-   Enable FIX transport in the `          Synapse axis2.xml         ` .
-   Configure ESB for FIX samples.
-   Open up the
    `           ESB_HOME/repository/samples/synapse_sample_259.xml          `
    file and make sure that
    `           transport.fix.AcceptorConfigURL          ` property
    points to the `           fix-synapse.cfg          ` file you
    created.

    !!! Note
        Synapse creates a new FIX session with Banzai at this point.

-   Send an order request from Banzai to Synapse. For example, Buy DELL
    1000 @ 100. User has to send a "Limit" Order because price is a
    mandatory field for `          placeOrder         ` operation.

ESB will forward the order request to one-way
`         placeOrder        ` operation on the
`         SimpleStockQuoteService        ` . ESB uses a simple XSLT
Mediator to transform the incoming FIX to a SOAP message.

```
<xsl:stylesheet version="2.0"
        xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
        xmlns:fn="http://www.w3.org/2005/02/xpath-functions">
    <xsl:output method="xml" omit-xml-declaration="yes" indent="yes" />
    <xsl:template match="/">
        <m0:placeOrder xmlns:m0="http://services.samples">
            <m0:order>
                <m0:price><xsl:value-of select="//message/body/field[]"/></m0:price>
                <m0:quantity><xsl:value-of select="//message/body/field[]"/></m0:quantity>
                <m0:symbol><xsl:value-of select="//message/body/field[]"/></m0:symbol>
            </m0:order>
        </m0:placeOrder>
    </xsl:template>
</xsl:stylesheet>
```

To get an idea about the various transport parameters being used in this
sample, see FIX Transport .

``` 
<definitions xmlns="http://ws.apache.org/ns/synapse">
    <localEntry key="xslt-key-req" src="file:repository/samples/resources/transform/transform_fix_to_http.xslt" />
    <proxy name="FIXProxy" transports="fix">
        <target>
            <endpoint>
                <address
                    uri="http://localhost:9000/services/SimpleStockQuoteService" />
            </endpoint>
            <inSequence>
                <log level="full" />
                <xslt key="xslt-key-req" />
                <log level="full" />
            </inSequence>
            <outSequence>
                <log level="full" />
            </outSequence>
        </target>
    <parameter name="transport.fix.AcceptorConfigURL">
        file:repository/samples/resources/fix/fix-synapse.cfg
    </parameter>
    <parameter name="transport.fix.AcceptorMessageStore">file</parameter>
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
files from `         $ESB_HOME/repository/samples/resources/fix        `
folder to `         $QFJ_HOME/etc folder        ` . Execute the sample
applications from
`         $QFJ_HOME/bin," "./banzai.sh/bat ../etc/banzai.cfg executor.sh/bat ../etc/executor.cfg        `
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
http://www.quickfixj.org/downloads ) installation. Download Quickfix/J
from: http://www.quickfixj.org/downloads .

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
below (You can find the config files from
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
