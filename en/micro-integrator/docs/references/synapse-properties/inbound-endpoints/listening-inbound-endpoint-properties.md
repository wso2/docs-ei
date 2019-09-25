# Listening Inbound Endpoint Properties

See the topics given below for the list of properties that can be configured for a Listening Inbound Endpoint artifact.

``` java tab='HTTP Listener'
<inboundEndpoint name="HttpListenerEP" protocol="http" suspend="false" sequence="TestIn" onError="fault" >
    <p:parameters xmlns:p="http://ws.apache.org/ns/synapse">
        <p:parameter  name="inbound.http.port">8081</p:parameter>
    </p:parameters>
<inboundEndpoint>
```

``` java tab='HTTPS Listener'
<inboundEndpoint name="HttpListenerEP" protocol="https" suspend="false" sequence="TestIn" onError="fault" >
        <p:parameters xmlns:p="http://ws.apache.org/ns/synapse">
            <p:parameter  name="inbound.http.port">8081</p:parameter>
            <p:parameter name="keystore">
                <KeyStore>
                    <Location>repository/resources/security/wso2carbon.jks</Location>
                    <Type>JKS</Type>
                    <Password>wso2carbon</Password>
                    <KeyPassword>wso2carbon</KeyPassword>
                </KeyStore>
            </p:parameter>
            <p:parameter name="truststore">
                <TrustStore>
                    <Location>repository/resources/security/client-truststore.jks</Location>
                    <Type>JKS</Type>
                    <Password>wso2carbon</Password>
                </TrustStore>
            </p:parameter>
            <p:parameter name="SSLVerifyClient">require</p:parameter>
            <p:parameter name="HttpsProtocols">TLSv1,TLSv1.1,TLSv1.2</p:parameter>
            <p:parameter name="SSLProtocol">SSLV3</p:parameter>
            <p:parameter name="CertificateRevocationVerifier">
                <CertificateRevocationVerifier enable="true">
                   <CacheSize>10</CacheSize>
                   <CacheDelay>2</CacheDelay>
                </CertificateRevocationVerifier>
             </p:parameter>
         </p:parameters>
</inboundEndpoint>
```

``` java tab='CXF'
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

``` java tab='HL7'
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                    name="InboundHL7"
                    sequence="main"
                    onError="fault"
                    protocol="hl7"
                    suspend="false">
    <parameters>
         <parameter name="inbound.hl7.Port">20000</parameter>
         <parameter name="inbound.hl7.AutoAck">true</parameter>
         <parameter name="inbound.hl7.ValidateMessage">true</parameter>
         <parameter name="inbound.hl7.TimeOut">10000</parameter>
         <parameter name="inbound.hl7.CharSet">UTF-8</parameter>
         <parameter name="inbound.hl7.BuildInvalidMessages">false</parameter>
         <parameter name="inbound.hl7.PassThroughInvalidMessages">false</parameter>  
    </parameters>
</inboundEndpoint>
```

``` java tab='Websocket'
    <inboundEndpoint name="WebSocketListenerEP" onError="fault" protocol="ws" sequence="TestIn" suspend="false" xmlns="http://ws.apache.org/ns/synapse">
      <parameters>
         <parameter name="inbound.ws.port">9091</parameter>
         <parameter name="ws.outflow.dispatch.sequence">TestOut</parameter> 
         <parameter name="ws.client.side.broadcast.level">0</parameter>
         <parameter name="ws.outflow.dispatch.fault.sequence">fault</parameter>
      </parameters> 
    </inboundEndpoint>
```

``` java tab='Secure Websocket'
    <inboundEndpoint name="SecureWebSocketEP" onError="fault" protocol="wss" sequence="TestIn" suspend="false" xmlns="http://ws.apache.org/ns/synapse">
      <parameters>
         <parameter name="inbound.ws.port">9091</parameter>
         <parameter name="ws.client.side.broadcast.level">0</parameter>
         <parameter name="ws.outflow.dispatch.sequence">TestOut</parameter>
         <parameter name="ws.outflow.dispatch.fault.sequence">fault</parameter>
         <parameter name="wss.ssl.key.store.file">repository/resources/security/wso2carbon.jks</parameter>
         <parameter name="wss.ssl.key.store.pass">wso2carbon</parameter>
         <parameter name="wss.ssl.trust.store.file">repository/resources/security/client-truststore.jks</parameter>
         <parameter name="wss.ssl.trust.store.pass">wso2carbon</parameter>
         <parameter name="wss.ssl.cert.pass">wso2</parameter>
       </parameters>
    </inboundEndpoint>
```

## HTTP/HTTPS Inbound Endpoint

Listed below are the properties used for [creating an HTTP/HTTPS inbound endpiont](../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

### Required Properties

Listed below are the required properties when [creating an HTTP/HTTPS inbound endpiont](../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

<table>
      <tr>
         <th>
            Property
         </th>
         <th>
            Description
         </th>
      </tr>
   <tbody>
      <tr>
        <th>Property</th>
        <th>Description</th>
      </tr>
      <tr>
         <td>inbound.http.port</td>
         <td>
          The port on which the endpoint listener should be started.
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

### Optional Properties

Listed below are the optional properties you can configure when [creating an HTTP/HTTPS inbound endpiont](../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

<table>
   <thead>
      <tr>
         <th>
            <p>Property Name</p>
         </th>
         <th>
            <p>Description</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>
            keystore
         </td>
         <td>The KeyStore location where keys are stored.</td>
      </tr>
      <tr>
         <td>
            truststore
         </td>
         <td>The TrustStore location where keys are stored.</td>
      </tr>
      <tr>
         <td>
          SSLVerifyClient
         </td>
         <td>
            <p>Used when enabling mutual verification.</p>
         </td>
      </tr>
      <tr>
         <td>
          HttpsProtocols
         </td>
         <td>The supporting protocols.</td>
      </tr>
      <tr>
         <td>
            SSLProtocol
         </td>
         <td>The supporting SSL protocol.</td>
      </tr>
      <tr>
         <td>
            CertificateRevocationVerifier
         </td>
         <td>
            When the <code>enable</code> attribute is set to <code>true</code>, this validates and verifies the revocation status of the host certificates using OCSP/CRL when making HTTPS connections.<br />
            If the <code>enable</code> attribute of this parameter is set to <code>true</code>, you also need to specify the following:
            <ul>
              <li><b>CacheSize</b>: The maximum size of the cache.</li>
              <li><b>CacheDelay</b>: The time duration between two consecutive scheduled cache managing tasks that perform housekeeping work for the cache.</li>
            </ul>
         </td>
      </tr>
   </tbody>
</table>

### Worker Pool Configuration Properties

By default inbound endpoints share the PassThrough transport worker pool to handle incoming requests. If you need a separate worker pool for the inbound endpoint, you need to configure the following properties when [creating an HTTP/HTTPS inbound endpiont](../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

<table>
  <tr>
    <th>Property Name</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>inbound.worker.pool.size.core</td>
    <td>
      The initial number of threads in the worker thread pool. This value can be changed accordingly based on the number of messages to be processed. The maximum value that can be specified here is the value of the inbound.worker.pool.size.max parameter.</br></br> The default value is 400.
    </td>
  </tr>
  <tr>
    <td>inbound.worker.pool.size.max</td>
    <td>
      The maximum number of threads in the worker thread pool. Specify a maximum limit in order to avoid performance degradation that can occur due to context switching.</br></br> The default value is 500.
    </td>
  </tr>
  <tr>
    <td>inbound.worker.thread.keep.alive.sec</td>
    <td>
      The keep-alive time for extra threads in the worker pool. This value should be less than the socket timeout. When this time is elapsed for an extra thread, it will be destroyed. The purpose of this parameter is to optimize the usage of resources by avoiding wastage that results from having extra threads that are not utilized.</br></br> The default value is 60.
    </td>
  </tr>
  <tr>
    <td>inbound.worker.pool.queue.length</td>
    <td>
      The length of the queue that is used to hold runnable tasks that are to be executed by the worker pool. The thread pool starts queuing jobs when all existing threads are busy and the pool has reached the maximum number of threads. The value for this parameter should be -1 to use an unbounded queue. If a bound queue is used and the queue gets filled to its capacity, any further attempt to submit jobs will fail causing synapse to drop some messages. </br></br> The default value is -1.
    </td>
  </tr>
  <tr>
    <td>inbound.thread.group.id</td>
    <td>
      Unique Identifier of the thread group. The default value is the PassThrough inbound worker thread group.
    </td>
  </tr>
  <tr>
    <td>inbound.thread.id</td>
    <td>
      Unique Identifier of the thread group. The default value is the PassThrough inbound worker thread.
    </td>
  </tr>
  <tr>
    <td>dispatch.filter.pattern</td>
    <td>
      The regular expression that defines the proxy services and API's to expose via the inbound endpoint. Provide the .* expression to expose all proxy services and API's or provide an expression similar to <code>^(/foo|/bar|/services/MyProxy)$</code> to define a set of services to expose via the inbound endpoint. If you do not provide an expression only the defined sequence of the inbound endpoint will be accessible.
    </td>
  </tr>
</table>

## CXF WS-RM Inbound Properties

The following properties can be configured when [creating a CXF WS-RM inbound endpiont](../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

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

Listed below are the properties used for [creating a HL7 inbound endpiont](../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

### Optional Properties

The following properties can be configured when [creating a HL7 inbound endpiont](../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

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

Listed below are the properties used for [creating a Websocket inbound endpiont](../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

### Required Properties

The following properties are required when [creating a Websocket inbound endpiont](../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

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

In addition to the [common websocket inbound properties](#common-websocket-inbound-required-properties) listed above, the following properties are required when [creating a **secured** Websocket inbound endpiont](../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

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

The following optional properties can be configured when [creating a Websocket inbound endpiont](../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

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

The following optional properties can be configured when [creating a **Secured** Websocket inbound endpiont](../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

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