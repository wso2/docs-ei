# Sample 905: Inbound HL7 with Automatic Acknowledgement

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


-   [Introduction](#Sample905:InboundHL7withAutomaticAcknowledgement-Introduction)
-   [Prerequisites](#Sample905:InboundHL7withAutomaticAcknowledgement-Prerequisites)
-   [Building the
    sample](#Sample905:InboundHL7withAutomaticAcknowledgement-Buildingthesample)
-   [Executing the
    sample](#Sample905:InboundHL7withAutomaticAcknowledgement-Executingthesample)
-   [Analyzing the
    output](#Sample905:InboundHL7withAutomaticAcknowledgement-Analyzingtheoutput)

### Introduction

This sample illustrates how the [HL7 Inbound
Protocol](https://docs.wso2.com/display/EI650/HL7+Inbound+Protocol) can
be used to receive a simple HL7 message.

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

**Inbound HL7 Automatic Acknowledgement**

``` xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                     name="Sample1"
                     sequence="main"
                     onError="fault"
                     protocol="hl7"
                     suspend="false">
       <parameters>
          <parameter name="inbound.hl7.AutoAck">true</parameter>
          <parameter name="inbound.hl7.Port">20000</parameter>
          <parameter name="inbound.hl7.TimeOut">3000</parameter>
          <parameter name="inbound.hl7.CharSet">UTF-8</parameter>
          <parameter name="inbound.hl7.ValidateMessage">false</parameter>
          <parameter name="transport.hl7.BuildInvalidMessages">false</parameter>
       </parameters>
    </inboundEndpoint>
    
    <sequence name="main">
       <in>
           <log level="full"/>
           <drop/>
       </in>
       <out>
           <send/>
       </out>
       <description>The main sequence for the message mediation</description>
    </sequence>
    
    <sequence name="fault">
        <drop/>
    </sequence>
    </definitions>
```

This configuration file `         synapse_sample_905.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start the ESB with the sample 905 configuration. For instructions on
    starting a sample ESB configuration, see [Starting the ESB with a
    sample
    configuration](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Startingasample)
    .  
    The operation log keeps running until the server starts, which
    usually takes several seconds. Wait until the server has fully
    booted up and displays a message similar to " *WSO2 Carbon started
    in n seconds.* "

2.  Download and install the [HAPI HL7
    TestPanel](https://sourceforge.net/projects/hl7api/files/hapi-testpanel/)
    .

### Executing the sample

The sample client used here is the **HAPI HL7 TestPanel** .

**To execute the sample**

-   Connect to the port defined in the inbound endpoint (i.e., 20000,
    which is the value of `           inbound.hl7.Port)          ` using
    the HAPI HL7 TestPanel.

-   Generate and send an HL7 message using the messages dialog frame.

### Analyzing the output

You will see that the ESB receives the HL7 message and logs a
serialisation of this message in a SOAP envelope. You will also see that
the HAPI HL7 TestPanel receives an acknowledgement.

  
