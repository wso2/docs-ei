# Inbound HL7 with Automatic Acknowledgement

## Example use case


## Synapse configuration

Following are the integration artifacts that we can used to implement this scenario.

```xml tab='Inbound Endpoint'
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
```

```xml tab='Main Sequence'
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
```

```xml tab='Fault Sequence'
<sequence name="fault">
    <drop/>
</sequence>
```

## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the following artifacts: Inbound endpoint, Sequence.
4. Deploy the artifacts in your Micro Integrator.

Configure the ActiveMQ broker.

Set up the back-end service.

The sample client used here is the **HAPI HL7 TestPanel**:

-   Connect to the port defined in the inbound endpoint (i.e., 20000,
    which is the value of `           inbound.hl7.Port)          ` using
    the HAPI HL7 TestPanel.
-   Generate and send an HL7 message using the messages dialog frame.

You will see that the Micro Integrator receives the HL7 message and logs a
serialisation of this message in a SOAP envelope. You will also see that
the HAPI HL7 TestPanel receives an acknowledgement.
