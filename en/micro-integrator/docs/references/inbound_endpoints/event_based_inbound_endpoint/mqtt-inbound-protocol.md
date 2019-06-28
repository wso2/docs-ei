# MQTT Inbound Protocol

MQ Telemetry Transport (MQTT) is a lightweight broker-based
publish/subscribe messaging protocol, designed to be open, simple,
lightweight and easy to implement. These characteristics make it ideal
for use in constrained environments.

For example,

-   When the network is expensive, has low bandwidth or is unreliable.

-   When running on an embedded device with limited processor or memory
    resources.

To configure the MQTT inbound endpoint, you need to specify XML
fragments that represents various parameters.

Following is a sample MQTT inbound endpoint configuration that
can subscribe to messages using the specified topic:

``` html/xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="Test" sequence="TestIn" onError="fault" protocol="mqtt" suspend="false">
       <parameters>
          <parameter name="sequential">true</parameter>
          <parameter name="mqtt.connection.factory">mqttFactory</parameter>
          <parameter name="mqtt.server.host.name">localhost</parameter>
          <parameter name="mqtt.server.port">1883</parameter>
          <parameter name="mqtt.topic.name">ei.test2</parameter>
          <parameter name="mqtt.subscription.qos">2</parameter>
          <parameter name="content.type">application/xml</parameter>
          <parameter name="mqtt.session.clean">false</parameter>
          <parameter name="mqtt.ssl.enable">false</parameter>
          <parameter name="mqtt.subscription.username">client</parameter>
          <parameter name="mqtt.subscription.password">e13</parameter>
          <parameter name="mqtt.temporary.store.directory">m1y</parameter>
          <parameter name="mqtt.reconnection.interval">5</parameter>
       </parameters>
    </inboundEndpoint>
```

### MQTT inbound endpoint parameters

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Parameter</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>sequential</code></pre></td>
<td>The behaviour when executing the given sequence.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><pre><code>coordination</code></pre></td>
<td>In a cluster setup, if set to true, the inbound endpoint will run only in a single worker node. If set to false,  it will run on all worker nodes.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><pre><code>suspend</code></pre></td>
<td>If set to true, the inbound listener will not accept incoming request messages from clients and will not inject messages to the synapse message mediation. If set to false, incoming messages will be accepted.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><pre><code>mqtt.connection.factory</code></pre></td>
<td>Name of the connection factory.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><pre><code>mqtt.server.host.name</code></pre></td>
<td><p>Address of the message broker (eg., localhost).</p></td>
<td>Yes</td>
</tr>
<tr class="even">
<td><pre><code>mqtt.server.port</code></pre></td>
<td>Port of the message broker (e.g., 1883).</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><pre><code>mqtt.topic.name</code></pre></td>
<td>MQTT topic to which the message should be published.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><pre><code>mqtt.subscription.qos</code></pre></td>
<td>The quality of service level that need to be maintained with the subscription. The quality of service level can be either 0,1 or 2.<br />
0 -  Specifying this level ensures that the message delivery is efficient. However, specifying this level does not guarantee that the message will be delivered to its recipient.<br />
1 -  Specifying this level ensures that the message is delivered at least once, but this can lead to messages being duplicated.<br />
2 - This is the highest level of quality of service. Specifying this guarantees that the message is delivered and that it is delivered only once.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>mqtt.client.id</code></pre></td>
<td>The id of the client.</td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>content.type</code></pre></td>
<td>The content type of the message. (i.e., XML or JSON)</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><pre><code>mqtt.session.clean</code></pre></td>
<td><p>Whether the client and server should remember the state across restarts and reconnects.</p></td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>mqtt.ssl.enable</code></pre></td>
<td>Whether to use tcp connection or ssl connection.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>mqtt.subscription.username</code></pre></td>
<td>The username for the subscription.</td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>mqtt.subscription.password</code></pre></td>
<td>The password for the subscription.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>mqtt.temporary.store.directory</code></pre></td>
<td><p>Path of the directory to be used as the persistent data store for quality of service purposes.</p></td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>mqtt.reconnection.interval</code></pre></td>
<td><p>The retry interval to reconnect to the MQTT server.</p></td>
<td>No</td>
</tr>
</tbody>
</table>

### Samples

For a sample that demonstrates how the MQTT connector publishes a
message on a particular topic and how a MQTT client that is subscribed
to that topic receives it, see [Sample 906: MQTT Inbound Endpoint
Sample](https://docs.wso2.com/display/EI600/Sample+906%3A+Inbound+Endpoint+MQTT+Protocol+Sample)
.
