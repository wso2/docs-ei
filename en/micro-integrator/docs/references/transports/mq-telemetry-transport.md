# MQ Telemetry Transport

MQ Telemetry Transport (MQTT) is a simple and lightweight network
protocol for device communication. This is an easy to implement protocol
that is based on the principle of publish/subscribe. These
characteristics make MQTT ideal for use in constrained environments.

For example,

-   When the network is expensive, has low bandwidth or is unreliable.
-   When running on an embedded device with limited processor or memory
    resources.

The MQTT transport implementation requires an MQTT server instance to be
able to send and receive messages. The recommended MQTT server is the
Mosquitto message broker.

Configuration parameters for the MQTT receiver and sender are XML
fragments that represent MQTT connection factories.

Following is a sample MQTT connection factory configuration that
consists of four connection factory parameters:

``` java
<parameter locked="false" name="mqttConFactory">
    <parameter locked="false" name="mqtt.server.host.name">localhost</parameter>
    <parameter locked="false" name="mqtt.server.port">1883</parameter>
    <parameter locked="false" name="mqtt.client.id">esb.test.listener</parameter>
    <parameter locked="false" name="mqtt.topic.name">esb.test2</parameter>
</parameter>
```

## Configuring the MQTT transport

Update the following configurations in the ei.toml file:

```toml
[transport.mqtt]
listener.enable = false
listener.parameter.hostname = "$ref{server.hostname}"
listener.parameter.connection_factory = "mqttConFactory"
listener.parameter.server_port = 1883
listener.parameter.client_id = "client-id-1234"
listener.parameter.topic_name = "esb.test"

# not reqired parameter list
listener.parameter.subscription_qos = 0
listener.parameter.session_clean = false
listener.parameter.enable_ssl = false
listener.parameter.subscription_username = ""
listener.parameter.subscription_password = ""
listener.parameter.temporary_store_directory = ""
listener.parameter.blocking_sender = false
listener.parameter.connect_type = "text/plain"
listener.parameter.message_retained = false

sender.enable = false
```