# Using the MQTT transport
## Example use case
This sample demonstrates how to run a Pub-Sub use case using MQTT as the broker.  the MQTT listener in the Micro Integrator consumes messages from a MQTT topic, and the MQTT sender publishes messages to a MQTT topic.

## Synapse configuration

```xml
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
```

## Build and run

-   Download the `org.eclipse.paho.client.mqttv3-1.1.0.jar`
    file.
-   Download the Mosquitto-clients ( <http://mosquitto.org/> )
-   Download [WSO2 MB 3.1.0](http://wso2.com/products/message-broker/) or the mosquitto MQTT broker (http://mosquitto.org/)
-   Copy the `org.eclipse.paho.client.mqttv3-1.1.0.jar` file to the `MI_HOME/lib/` directory.
-   Start the MQTT broker.
-   [Start WSO2 MB](https://docs.wso2.com/display/MB310/Running+the+Product#RunningtheProduct-Startingtheserver), [open the Management Console](https://docs.wso2.com/display/MB310/Running+the+Product#RunningtheProduct-AccessingtheManagementConsole) and [create a
    topic](https://docs.wso2.com/display/MB310/Managing+Topics+and+Sub+Topics#ManagingTopicsandSubTopics-AddingtopicsfromthemanagementconsoleAdd)
    named *esb.test2*.

Invoke the proxy service:

-   Execute the following command to start the MQTT subscriber on the
    *esb.test2* topic:

    ```bash
    mosquitto_sub -h localhost -t esb.test2
    ```

-   Execute the following command to run the MQTT publisher to publish
    to the *esb.test1* topic:

    ```bash
    mosquitto_pub -h localhost -p 1883 -t esb.test1  -m {"company":"WSO2"}
    ```

When you analyze the output messages on the MQTT subscriber console, you
will see the following log:

```bash
{"company":"WSO2"}
```
