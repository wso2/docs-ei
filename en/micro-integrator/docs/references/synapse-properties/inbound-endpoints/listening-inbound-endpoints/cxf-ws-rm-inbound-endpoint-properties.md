# CXF WS-RM Inbound Endpoint
## Introduction

WS­ReliableMessaging allows SOAP messages to be reliably delivered between distributed applications regardless of software or hardware failures. The CXF WS­-RM inbound endpoint allows a client (RM Source) to communicate with the Micro Integrator with a guarantee that a sent message will be delivered.

## Syntax

```xml
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                     name="RM_INBOUND"
                     sequence="RMIn"
                     onError="fault"
                     class="org.wso2.carbon.inbound.endpoint.ext.wsrm.InboundRMHttpListener"
                     suspend="false">
       <parameters>
          <parameter name="inbound.cxf.rm.port">20940</parameter>
          <parameter name="inbound.cxf.rm.config-file">conf/cxf/server.xml</parameter>
          <parameter name="coordination">true</parameter>
          <parameter name="inbound.cxf.rm.host">127.0.0.1</parameter>
          <parameter name="inbound.behavior">listening</parameter>
          <parameter name="sequential">true</parameter>
       </parameters>
</inboundEndpoint>
``` 

## Properties

The following properties can be configured when [creating a CXF WS-RM inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

<table>
  <tr>
    <th>Property Name</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>inbound.cxf.rm.host</td>
    <td>
      The host name.
    </td>
  </tr>
  <tr>
    <td>inbound.cxf.rm.port</td>
    <td>
      The port to listen to.</br></br>
      <b>Note</b>: When configuring an SSL-enabled cxf\_ws\_rm inbound endpoint, the <code>inbound.cxf.rm.port</code> parameter should be set to the same value as the engine port number specified in the CXF spring configuration file saved in the MI_HOME/conf/cxf directory. For example, in the above CXF spring configuration, the engine port is 8081. This same port number should be specified as the <code>inbound.cxf.rm.port</code> in the cxf\_ws\_rm inbound endpoint configuration.
    </td>
  </tr>
  <tr>
    <td>inbound.cxf.rm.config-file</td>
    <td>
      The path to the CXF Spring configuration file.
    </td>
  </tr>
  <tr>
    <td>enableSSL</td>
    <td>
      Set to <code>true</code> if SSL is enabled in the CXF Spring configuration file.
    </td>
  </tr>
  <tr>
         <td>sequential</td>
         <td>The behavior when executing the given sequence.<br />
            When set as <code>true</code> , mediation will happen within the same thread. When set as <code>false</code> , the mediation engine will use the inbound thread pool. The default thread pool values can be found in the <code>MI_HOME/conf/synapse.properties</code> file. The default setting is <code>true</code>.
         </td>
      </tr>
      <tr>
        <td>Suspend</td>
        <td>
          If the inbound listener should pause when accepting incoming requests, set this to <code>true</code>. If the inbound listener should not pause when accepting incoming requests, set this to <code>false</code>.
        </td>
  </tr>
</table>

## HL7 Inbound Properties

The HL7 inbound protocol is an alternative to the HL7 transport. The HL7 inbound endpoint implementation is fully asynchronous and is based on the <b>Minimal Lower Layer Protocol(MLLP)</b> implemented on top of event driven I/O.

Listed below are the properties used for [creating a HL7 inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

### Optional Properties

The following properties can be configured when [creating a HL7 inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

<table>
   <thead>
      <tr>
         <th>Property</th>
         <th>Description</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>inbound.hl7.Port</td>
         <td>The port on which you need to run the MLLP listener.</td>
      </tr>
      <tr>
         <td>inbound.hl7.AutoAck</td>
         <td>Whether or not an auto acknowledgement should be sent on message receipt. If set to false, you can define the type of HL7 acknowledgement to be sent. For more information, see <a href="#hl7-mediation-level-properties">HL7 inbound endpoint mediation level properties</a>.</br></br> The default setting is <code>true</code>.</td>
      </tr>
      <tr>
         <td>inbound.hl7.ValidateMessage</td>
         <td>This enables HL7 message validation.</br></br> The default setting is <code>true</code>.</td>
      </tr>
      <tr>
         <td>inbound.hl7.TimeOut</td>
         <td>The timeout interval in milliseconds to trigger a NACK message.</br></br> The default values is 10000.</td>
      </tr>
      <tr>
         <td>inbound.hl7.CharSet</td>
         <td>
          The character set used for encoding and decoding messages. Some multi-byte character encodings (e.g. UTF-16, UTF-32) may result in byte values equal to the MLLP framing characters or byte values lower than 0x1F, which results in errors. Possible values are <code>UTF-8</code>, <code>UTF-8</code>, and <code>US-ASCII</code>.</br></br> Default value is <code>US-ASCII</code>.
        </td>
      </tr>
      <tr>
         <td>inbound.hl7.BuildInvalidMessages</td>
         <td>If the <code>inbound.hl7.ValidateMessage</code> parameter is set to <code>false</code> and the incoming message is invalid, this parameter specifies whether the raw message received through the MLLP transport should be passed onto the mediation layer.</br></br> The default setting is <code>false</code>.</td>
      </tr>
      <tr>
         <td>inbound.hl7.PassthroughInvalidMessages</td>
         <td>If the <code>inbound.hl7.BuildInvalidMessages</code> parameter is set to <code>true</code>, this parameter notifies the Axis2 HL7 transport sender whether to use the raw message. The default setting is <code>false</code>.</td>
      </tr>
      <tr>
         <td>inbound.hl7.MessagePreProcessor</td>
         <td>
          An implementation of the <code>org.wso2.carbon.inbound.endpoint.protocol.hl7.HL7MessagePreprocessor</code> interface can be defined here. It provides an extension point to intercept incoming messages before any type of message parsing occurs. You can use any fully qualified class name.
        </td>
      </tr>
      <tr>
         <td>sequential</td>
         <td>The behavior when executing the given sequence.<br />
            When set as <code>true</code> , mediation will happen within the same thread. When set as <code>false</code> , the mediation engine will use the inbound thread pool. The default thread pool values can be found in the <code>MI_HOME/conf/synapse.properties</code> file. The default setting is <code>true</code>.
         </td>
      </tr>
      <tr>
        <td>Suspend</td>
        <td>
          If the inbound listener should pause when accepting incoming requests, set this to <code>true</code>. If the inbound listener should not pause when accepting incoming requests, set this to <code>false</code>.
        </td>
      </tr>
   </tbody>
</table>

### Mediation-Level Properties

Following are the mediation level properties that you can set in the HL7 inbound endpoint:

!!! Note
    The scope of these properties is the `default` scope.

| **Property**                                                                                            | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|---------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<property name="HL7_RESULT_MODE" value="ACK|NACK" scope="default"/>`          | This is use to define the type of HL7 acknowledgement to be sent. If the `             inbound.hl7.AutoAck            ` parameter is set to `             true            ` this property has no effect.                                                                                                                                                                                                                                                                                                             |
| `             <property name="HL7_NACK_MESSAGE" value="<ERROR MESSAGE>" scope="default" />            ` | This is used to define a custom error message to be sent if you have set the property `             HL7_RESULT_MODE            ` as `             NACK            ` .                                                                                                                                                                                                                                                                                                                                                |
| `             <property name="HL7_APPLICATION_ACK" value="true" scope="default"/>            `          | If the `             inbound.hl7.AutoAck            ` parameter is set to `             false            ` and no immediate auto generated ACK is sent back to the client, this property defines whether we should automatically generate the ACK for the request once the mediation flow is complete. If both the `             inbound.hl7.AutoAck            ` parameter and this property are set to `             false            `, you need to generate an ACK message in the correct format as a response. |

## Websocket Inbound Endpoint Properties

The WebSocket Inbound protocol is based on the <a href="http://tools.ietf.org/html/rfc6455">Websocket protocol</a> and allows full-duplex message mediation.

Listed below are the properties used for [creating a Websocket inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

### Required Properties

The following properties are required when [creating a Websocket inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>inbound.ws.port</td>
    <td>The netty listener port on which the WebSocket inbound listens.</td>
  </tr>
  <tr>
    <td>ws.client.side.broadcast.level</td>
    <td>The client broadcast level that defines how WebSocket frames are broadcast from the WebSocket inbound endpoint to the client. Broadcast happens based on the subscriber path client connected to the WebSocket inbound endpoint. The three possible levels are as follows:<br />
      0 - Only a unique client can receive the frame from a WebSocket inbound endpoint.<br />
      1 - All the clients connected with the same subscriber path receives the WebSocket frame.<br />
      2 - All the clients connected with the same subscriber path, except the one who publishes the frame to the inbound, receives the WebSocket frame.
    </td>
  </tr>
  <tr>
    <td>ws.outflow.dispatch.sequence</td>
    <td>The sequence for the back-end to client mediation.</td>
  </tr>
  <tr>
    <td>ws.outflow.dispatch.fault.sequence</td>
    <td>The fault sequence for the back-end to client mediation path.</td>
  </tr>
  <tr>
         <td>sequential</td>
         <td>The behavior when executing the given sequence.<br />
            When set as <code>true</code> , mediation will happen within the same thread. When set as <code>false</code> , the mediation engine will use the inbound thread pool. The default thread pool values can be found in the <code>MI_HOME/conf/synapse.properties</code> file. The default setting is <code>true</code>.
         </td>
      </tr>
      <tr>
        <td>Suspend</td>
        <td>
          If the inbound listener should pause when accepting incoming requests, set this to <code>true</code>. If the inbound listener should not pause when accepting incoming requests, set this to <code>false</code>.
        </td>
      </tr>
</table>

### Required Properties (for Secured Websocket)

In addition to the [common websocket inbound properties](#common-websocket-inbound-required-properties) listed above, the following properties are required when [creating a **secured** Websocket inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>inbound.ws.port</td>
    <td>The netty listener port on which the WebSocket inbound listens.</td>
  </tr>
  <tr>
    <td>ws.client.side.broadcast.level</td>
    <td>The client broadcast level that defines how WebSocket frames are broadcast from the WebSocket inbound endpoint to the client. Broadcast happens based on the subscriber path client connected to the WebSocket inbound endpoint. The three possible levels are as follows:<br />
      0 - Only a unique client can receive the frame from a WebSocket inbound endpoint.<br />
      1 - All the clients connected with the same subscriber path receives the WebSocket frame.<br />
      2 - All the clients connected with the same subscriber path, except the one who publishes the frame to the inbound, receives the WebSocket frame.
    </td>
  </tr>
  <tr>
    <td>ws.outflow.dispatch.sequence</td>
    <td>The sequence for the back-end to client mediation.</td>
  </tr>
  <tr>
    <td>ws.outflow.dispatch.fault.sequence</td>
    <td>The fault sequence for the back-end to client mediation path.</td>
  </tr>
  <tr>
    <td>wss.ssl.key.store.file</td>
    <td>The keystore location where keys are stored.</td>
  </tr>
  <tr>
    <td>wss.ssl.key.store.pass</td>
    <td>The password to access the keystore file.</td>
  </tr>
  <tr>
    <td>wss.ssl.trust.store.file</td>
    <td>The truststore location where keys are stored.</td>
  </tr>
  <tr>
    <td>wss.ssl.trust.store.pass</td>
    <td>The password to access the truststore file.</td>
  </tr>
  <tr>
    <td>wss.ssl.cert.pass</td>
    <td>The SSL certificate password.</td>
  </tr>
</table>

### Optional Properties

The following optional properties can be configured when [creating a Websocket inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

<table>
   <thead>
      <tr>
         <th>Property</th>
         <th>Description</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>ws.boss.thread.pool.size</td>
         <td>The size of the netty boss pool.</td>
      </tr>
      <tr>
         <td>ws.worker.thread.pool.size</td>
         <td>The size of the worker thread pool.</td>
      </tr>
      <tr>
         <td>ws.subprotocol.handler.class</td>
         <td>Specify one or more custom subprotocol handler classes that are required. Separate each custom subprotocol handler class using a semicolon.</td>
      </tr>
      <tr>
         <td>ws.default.content.type</td>
         <td>
            Specifies the content type of the Web Socket frames that are received from the inbound endpoint.
         </td>
      </tr>
      <tr>
         <td>ws.shutdown.status.code</td>
         <td>Specifies the status code of the closed web socket frame sent when the inbound endpoint is closed.</td>
      </tr>
      <tr>
         <td>ws.shutdown.status.message</td>
         <td>Specifies the status message of the closed web socket frame when the inbound endpoint is closed.</td>
      </tr>
      <tr>
         <td>ws.pipeline.handler.class</td>
         <td>The fully qualified class name of a pipeline handler class that you implemented.</td>
      </tr>
      <tr>
        <td>Ws Use Port Offset</td>
        <td></td>
      </tr>
   </tbody>
</table>

### Optional Properties (for Secured Websocket)

The following optional properties can be configured when [creating a **Secured** Websocket inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
         <td>wss.ssl.protocols</td>
         <td>Enables the SSL protocol for the particular WebSocket inbound endpoint. Default value is "TLS". You can change it to a TLS version(s), which is/are enabled with the SSL protocol (i.e., TLSv1,TLSv1.1,TLSv1.2). For example, <code>parameter name="wss.ssl.protocols"TLSv1.1,TLSv1.2/parameter</code></td>
      </tr>
      <tr>
         <td>wss.ssl.cipher.suites</td>
         <td>
            <div class="content-wrapper">
               <p>Enables the specified Cipher Suites for the particular WebSocket inbound endpoint. For example:</p>
               <div class="code panel pdl" style="border-width: 1px;">
                  <div class="codeContent panelContent pdl">
                     <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence">
                        <pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;parameter name=<span class="st">&quot;wss.ssl.cipher.suites&quot;</span>&gt;</span>
<span id="cb1-2"><a href="#cb1-2"></a>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256,</span>
<span id="cb1-3"><a href="#cb1-3"></a>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,</span>
<span id="cb1-4"><a href="#cb1-4"></a>TLS_DHE_RSA_WITH_AES_128_CBC_SHA256,</span>
<span id="cb1-5"><a href="#cb1-5"></a>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA,</span>
<span id="cb1-6"><a href="#cb1-6"></a>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,</span>
<span id="cb1-7"><a href="#cb1-7"></a>TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span>
<span id="cb1-8"><a href="#cb1-8"></a>&lt;/parameter&gt;</span></code></pre>
                     </div>
                  </div>
               </div>
            </div>
         </td>
      </tr>
</table>