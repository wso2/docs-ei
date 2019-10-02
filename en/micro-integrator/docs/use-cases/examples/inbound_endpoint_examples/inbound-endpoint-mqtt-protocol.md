# Inbound Endpoint MQTT Protocol Sample
## Example use case

This sample demonstrates how the MQTT connector publishes a message on a
particular topic and how a MQTT client that is subscribed to that topic
receives it. 
Following sections demonstrate how you can try this sample using the
Mosquitto server as the Message Broker.

### Prerequisites

Follow the steps below before starting the MQTT sample configurations.

1.  Download and install WSO2 EI. For instructions, see [Installation
    Guide](https://docs.wso2.com/display/EI650/Installation+Guide) .
2.  Install Mosquitto. (This sample is tested for
    [Mosquitto1.6.7 version](https://mosquitto.org/download/) .)
    After installing, Mosquitto server will run automatically in the background.
3.  Download [MQTT client
    library](http://repo.spring.io/plugins-release/org/eclipse/paho/mqtt-client/0.4.0/)
    (i.e. `          mqtt-client-0.4.0.jar         ` ) and add it to the
    `<EI_HOME>/lib/` directory.

### Synapse configuration
```<?xml version="1.0" encoding="UTF-8"?>
<inboundEndpoint name="SampleInbound" onError="fault" protocol="mqtt" sequence="TestIn" statistics="enable" suspend="false" trace="enable" xmlns="http://ws.apache.org/ns/synapse">
    <parameters>
        <parameter name="sequential">true</parameter>
        <parameter name="mqtt.connection.factory">mqttConFactory</parameter>
        <parameter name="mqtt.server.host.name">localhost</parameter>
        <parameter name="mqtt.server.port">1883</parameter>
        <parameter name="mqtt.topic.name">esb.test</parameter>
        <parameter name="content.type">application/xml</parameter>
        <parameter name="mqtt.subscription.qos">0</parameter>
        <parameter name="mqtt.session.clean">false</parameter>
        <parameter name="mqtt.ssl.enable">false</parameter>
        <parameter name="mqtt.reconnection.interval">1000</parameter>
    </parameters>
</inboundEndpoint>
```
```<?xml version="1.0" encoding="UTF-8"?>
   <sequence name="TestIn" trace="disable" xmlns="http://ws.apache.org/ns/synapse">
       <log level="full"/>
       <drop/>
   </sequence>

```

### Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the following artifacts: Inbound endpoint, Sequence.
4. Deploy the artifacts in your Micro Integrator.

### Sending a sample message 

Open a new terminal and enter the below command to send an MQTT message using mosquitto-pub.<br>
Note: Enter the MQTT Topic Name you entered when creating the inbound endpoint as \<MQTT Topic Name> below.

`mosquitto_pub -t <MQTT Topic Name>  -m "<msg><a>Testing123</a></msg>`

You will see that the Micro Integrator receives a message when the Micro Integrator Inbound is set
as the ultimate receiver.
