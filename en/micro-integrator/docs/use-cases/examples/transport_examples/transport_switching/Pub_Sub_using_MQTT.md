# Publishing and Subscribing using WSO2 ESB's MQ Telemetry Transport

### Introduction

This sample demonstrates how WSO2 ESB's MQ Telemetry Transport
(MQTT) listener consumes messages from a MQTT topic, and how the MQ
Telemetry Transport (MQTT) sender publishes messages to a MQTT topic.

### Prerequisites

-   Download the
    `                       org.eclipse.paho.client.mqttv3-1.1.0.jar                     `
    file.

-   Download the Mosquitto-clients ( <http://mosquitto.org/> )

-   Download [WSO2 MB 3.1.0](http://wso2.com/products/message-broker/)
    or the mosquitto MQTT broker ( <http://mosquitto.org/> )

### Building the sample

1.  Copy the
    `          org.eclipse.paho.client.mqttv3-1.1.0.jar         ` file
    to the `          <ESB_HOME>/repository/components/lib         `
    directory.
2.  Edit the
    `           <ESB_HOME>/repository/conf/axis2/axis2.xml          `
    file and change the MQTT sender and listener configuration to be as
    follows:

    ```
    <transportReceiver class="org.apache.axis2.transport.mqtt.MqttListener" name="mqtt">
            <parameter locked="false" name="mqttConFactory">
                    <parameter locked="false" name="mqtt.server.host.name">localhost</parameter>
                    <parameter locked="false" name="mqtt.server.port">1883</parameter>
                    <parameter locked="false" name="mqtt.client.id">esb.test.listener</parameter>
                    <parameter locked="false" name="mqtt.topic.name">esb.test1</parameter>
            </parameter>
        </transportReceiver>
     
    <transportsender class="org.apache.axis2.transport.mqtt.MqttSender" name="mqtt">
    </transportsender>
    ```

3.  Start the MQTT broker. If you are using WSO2 MB as the MQTT broker,
    you should set the WSO2 ESB port offset to 1 before running the ESB.
    To set the port offset in WSO2 ESB, open the
    `           <ESB_HOME>/repository/conf/carbon.xml          ` file
    and set the offset to 1 as follows:

    ```
    <Offset>1</Offset>
    ```

4.  [Start WSO2
    MB](https://docs.wso2.com/display/MB310/Running+the+Product#RunningtheProduct-Startingtheserver)
    , [open the Management
    Console](https://docs.wso2.com/display/MB310/Running+the+Product#RunningtheProduct-AccessingtheManagementConsole)
    and [create a
    topic](https://docs.wso2.com/display/MB310/Managing+Topics+and+Sub+Topics#ManagingTopicsandSubTopics-AddingtopicsfromthemanagementconsoleAdd)
    named *esb.test2* .
5.  The XML configuration for this sample is as follows:

    ```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <proxy xmlns="http://ws.apache.org/ns/synapse" name="SampleProxy" transports="mqtt" startOnLoad="true" trace="disable">
            <description/>
            <target>
                <endpoint>
                    <address uri="mqtt:/SampleProxy?mqtt.server.host.name=localhost&amp;mqtt.server.port=1883&amp;mqtt.client.id=esb.test.sender&amp;mqtt.topic.name=esb.test2&amp;mqtt.subscription.qos=2&amp;mqtt.blocking.sender=true"/>
                </endpoint>
                <inSequence>
                    <property name="OUT_ONLY" value="true"/>
                    <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2" type="STRING"/>
                </inSequence>
                <outSequence>
                    <send/>
                </outSequence>
            </target>
            <parameter name="mqtt.connection.factory">mqttConFactory</parameter>
            <parameter name="mqtt.topic.name">esb.test1</parameter>
            <parameter name="mqtt.subscription.qos">2</parameter>
            <parameter name="mqtt.content.type">text/plain</parameter>
            <parameter name="mqtt.session.clean">false</parameter>
        </proxy>
    </definitions>
     
    ```

    This configuration file
    `           synapse_sample_272.xml          ` is available in the
    `           <ESB_HOME>/repository/samples          ` directory.

### Executing the sample

-   Execute the following command to start the MQTT subscriber on the
    *esb.test2* topic:

    ``` java
    mosquitto_sub -h localhost -t esb.test2
    ```

-   Execute the following command to run the MQTT publisher to publish
    to the *esb.test1* topic:

    ``` java
    mosquitto_pub -h localhost -p 1883 -t esb.test1  -m {"company":"WSO2"}
    ```

### Analyzing the output

When you analyze the output messages on the MQTT subscriber console, you
will see the following log:

``` java
{"company":"WSO2"}
```
