# Sample 261: Switching between FIX Versions

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

-   [Introduction](#Sample261:SwitchingbetweenFIXVersions-Introduction)
-   [Prerequisites](#Sample261:SwitchingbetweenFIXVersions-Prerequisites)
-   [Building the
    sample](#Sample261:SwitchingbetweenFIXVersions-Buildingthesample)
-   [Executing the
    sample](#Sample261:SwitchingbetweenFIXVersions-Executingthesample)
-   [Analyzing the
    output](#Sample261:SwitchingbetweenFIXVersions-Analyzingtheoutput)

### Introduction

This sample demonstrates how you can use WSO2 ESB to accept FIX input
via the FIX transport layer and dispatch to another FIX acceptor that
accept messages in a different FIX version. Here you will see how the
ESB receives a FIX 4.0 messages and simply forwards it to the FIX 4.1
endpoint.

### Prerequisites

-   For a list of general prerequisites, see [Prerequisites to Start the
    ESB
    Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
    .
-   To configure the ESB to use the FIX transport, see [Configure the
    ESB to use the FIX
    transport](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-FIX)
    .
-   You will need the two sample FIX applications ( *Banzai* and
    *Executor* ) that are provided with Quickfix/J. Configure the two
    applications to establish sessions with the ESB.
-   Add the following line to the
    `           fix-synapse-m40.cfg          ` and
    `           synapse-sender-m.cfg          ` configuration files.

    ``` java
    DataDictionary=~/etc/spec/FIX40-synapse.xml
    ```

    !!! info

    Note

    The `           FIX40-synapse.xml          ` file can be found in
    the
    `           <ESB_HOME>/repository/samples/resources/fix          `
    directory. This is a custom FIX data dictionary file that adds the
    tag 150 and 151 to the execution message(35=8) of FIX4.0. Make sure
    the `           DataDictionary          ` property in the
    `           banzai.cfg          ` file points to this data
    dictionary file.

    TEST  

-   Add the following lines to `           executor.cfg          ` ,
    which is the *Executor* configuration file:

    ``` java
    [session]
    BeginString=FIX.4.1
    SocketAcceptPort=19877
    ```

-   Start *Banzai* and *Executor* using the custom configuration files.
-   You need to use two custom configuration files for the ESB in this
    sample. The two custom configuration files can be found in the \<
    `          ESB_HOME>/repository/samples/resources/fix         `
    directory. The two files are
    `          fix-synapse-m40.cfg         ` and
    `          synapse-sender-m.cfg         ` . You need to point your
    ESB configuration to these two files (this is already done in the
    provided `          synapse_sample_261.xml         ` file) or you
    should create copies of them and point the ESB configuration to the
    copies. In either case, make sure that the properties
    `          FileStorePath         ` and
    `          FileLogPath         ` in the two files point to valid
    locations in your local file system.
-   Open the \<
    `           ESB_HOME>/repository/samples/synapse_sample_261.xml          `
    file and make sure that the
    `           transport.fix.AcceptorConfigURL          ` property
    points to the `           fix-synapse-m40.cfg          ` file and
    the `           transport.fix.InitiatorConfigURL          ` property
    points to the `           synapse-sender-m.cfg          ` file.

    !!! info

    Note

    The ESB creates a new FIX session with *Banzai* at this point.

    TEST  

### Building the sample

The XML configuration for this sample is as follows:

``` html/xml
<definitions xmlns="http://ws.apache.org/ns/synapse">
     <proxy name="OrderProcesserProxy41" transports="fix">
        <target>
           <endpoint>
              <address uri="fix://localhost:19877?BeginString=FIX.4.1&SenderCompID=SYNAPSE&TargetCompID=EXEC"/>
           </endpoint>
           <inSequence>
                <log level="full"/>
           </inSequence>
           <outSequence>
                <log level="full"/>
                <send/>
           </outSequence>
        </target>
        <parameter name="transport.fix.AcceptorConfigURL">file:repository/samples/resources/fix/fix-synapse-m40.cfg</parameter>
        <parameter name="transport.fix.AcceptorMessageStore">file</parameter>
        <parameter name="transport.fix.InitiatorConfigURL">file:repository/samples/resources/fix/synapse-sender-m.cfg</parameter>
        <parameter name="transport.fix.InitiatorMessageStore">file</parameter>
     </proxy>
</definitions>
```

This configuration file `         synapse_sample_261.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

-   Start the ESB with the sample 261 configuration. For instructions on
    starting a sample ESB configuration, see [Starting the ESB with a
    sample
    configuration](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Startingasample)
    .  
    The operation log keeps running until the server starts, which
    usually takes several seconds. Wait until the server has fully
    booted up and displays a message similar to " *WSO2 Carbon started
    in n seconds.* "

### Executing the sample

Send an order request from *Banzai* to the ESB. For example, Buy DELL
1000 @ MKT.

### Analyzing the output

You will see that the ESB forwards the FIX4.0 order request to the
*Executor* that accepts FIX4.1 messages, and that the *Executor*
processes the request and sends a response back to *Banzai* .
