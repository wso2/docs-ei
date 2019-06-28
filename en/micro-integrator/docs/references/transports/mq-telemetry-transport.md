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

### MQTT connection factory parameters

Following are details on the MQTT parameters that you can set:

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
<th><p>Possible Values</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             mqtt.server.host.name            </code></td>
<td>The name of the host.</td>
<td><p>Yes</p></td>
<td><p>A valid host name</p></td>
</tr>
<tr class="even">
<td><code>             mqtt.server.port            </code></td>
<td>The port ID.</td>
<td>Yes</td>
<td><code>             1883            </code> , <code>             1885            </code></td>
</tr>
<tr class="odd">
<td><code>             mqtt.client.id            </code></td>
<td>The client ID.</td>
<td>Yes</td>
<td>A valid client ID</td>
</tr>
<tr class="even">
<td><code>             mqtt.topic.name            </code></td>
<td>The name of the topic</td>
<td>Yes</td>
<td>A valid topic name</td>
</tr>
<tr class="odd">
<td><code>             mqtt.subscription.qos            </code></td>
<td>The QoS value.</td>
<td>No</td>
<td><p><code>              0             </code> , <code>              1             </code> , <code>              2             </code></p></td>
</tr>
<tr class="even">
<td><code>             mqtt.session.clean            </code></td>
<td>Whether session clean should be enabled or not.</td>
<td>No</td>
<td><code>             true            </code> , <code>             false            </code></td>
</tr>
<tr class="odd">
<td><code>             mqtt.ssl.enable            </code></td>
<td>Whether ssl should be enabled or not.</td>
<td>No</td>
<td><code>             true            </code> , <code>             false            </code></td>
</tr>
<tr class="even">
<td><code>             mqtt.subscription.username            </code></td>
<td>The username for the subscription.</td>
<td>No</td>
<td>A valid user name</td>
</tr>
<tr class="odd">
<td><code>             mqtt.subscription.password            </code></td>
<td>The password for the subscription.</td>
<td>No</td>
<td>A valid password</td>
</tr>
<tr class="even">
<td><code>             mqtt.temporary.store.directory            </code></td>
<td>The path of the directory to be used as the persistent data store for quality of service purposes.</td>
<td>No</td>
<td>A valid local path. The default value is the ESB temp path.</td>
</tr>
<tr class="odd">
<td><code>             mqtt.blocking.sender            </code></td>
<td>Whether blocking sender should be enabled or not.</td>
<td>No</td>
<td><code>             true            </code> , <code>             false            </code></td>
</tr>
<tr class="even">
<td><code>             mqtt.content.type            </code></td>
<td>The content type.</td>
<td>No</td>
<td>A valid content type. the default content type is <code>             text/plain            </code> .</td>
</tr>
<tr class="odd">
<td><code>             mqtt.message.retained            </code></td>
<td>Whether the messaging engine should retain a published message or not. This parameter can be used only in the transport sender.</td>
<td>No</td>
<td><code>             true            </code> , <code>             false            </code><br />
The default value is <code>             false            </code> .</td>
</tr>
</tbody>
</table>

For a sample that demonstrates how Axis2 publishes a message on a
particular topic, and how an MQTT client subscribed to that topic
receives it, see [Sample 272:Publishing and Subscribing using EI's MQ
Telemetry
Transport](https://docs.wso2.com/display/EI650/Sample+272%3A+Publishing+and+Subscribing+using+WSO2+ESB%27s+MQ+Telemetry+Transport)
.
