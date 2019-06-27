# WebSocket Inbound Protocol

The ESB profile of WSO2 Enterprise Integrator (WSO2 EI) WebSocket
protocol implementation is based on the [WebSocket
protocol](http://tools.ietf.org/html/rfc6455) , and allows full-duplex
message mediation.

Following is a sample WebSocket inbound endpoint configuration:

``` xml
    <inboundEndpoint name="WebSocketListenerEP" onError="fault" protocol="ws" sequence="TestIn" suspend="false" xmlns="http://ws.apache.org/ns/synapse">
      <parameters>
         <parameter name="inbound.ws.port">9091</parameter>
         <parameter name="ws.outflow.dispatch.sequence">TestOut</parameter> 
         <parameter name="ws.client.side.broadcast.level">0</parameter>
         <parameter name="ws.outflow.dispatch.fault.sequence">fault</parameter>
      </parameters> 
    </inboundEndpoint>
```

### WebSocket inbound endpoint parameters

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Required</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             inbound.ws.port            </code></td>
<td>The netty listener port on which the WebSocket inbound listens.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><code>             ws.client.side.broadcast.level            </code></td>
<td>The client broadcast level that defines how WebSocket frames are broadcasted from the WebSocket inbound endpoint to the client. Broadcast happens based on the subscriber path client connected to the WebSocket inbound endpoint. The three possible levels are as follows:<br />
0 - Only a unique client can receive the frame from a WebSocket inbound endpoint.<br />
1 - All the clients connected with the same subscriber path receives the WebSocket frame.<br />
2 - All the clients connected with the same subscriber path, except the one who publishes the frame to the inbound, receives the WebSocket frame.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><code>             ws.outflow.dispatch.sequence            </code></td>
<td>The sequence for the back-end to client mediation.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><code>             ws.outflow.dispatch.fault.sequence            </code></td>
<td>The fault sequence for the back-end to client mediation path .</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><code>             ws.boss.thread.pool.size            </code></td>
<td>The size of the netty boss pool.</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             ws.worker.thread.pool.size            </code></td>
<td>The size of the worker thread pool.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             ws.subprotocol.handler.class            </code></td>
<td>Specify one or more custom subprotocol handler classes that are required. Separate each custom subprotocol handler class using a semicolon.</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             ws.default.content.type            </code></td>
<td><div class="content-wrapper">
<p>Specifies the content type of the Web Socket frames that are received from the inbound endpoint.</p>
</div></td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             ws.shutdown.status.code            </code></td>
<td>Specifies the status code of the closed web socket frame sent when the inbound endpoint is closed.</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             ws.shutdown.status.message            </code></td>
<td>Specifies the status message of the closed web socket frame when the inbound endpoint is closed.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             ws.pipeline.handler.class            </code></td>
<td><p>The fully qualified class name of a pipeline handler class that you implemented.</p></td>
<td>No</td>
</tr>
</tbody>
</table>
