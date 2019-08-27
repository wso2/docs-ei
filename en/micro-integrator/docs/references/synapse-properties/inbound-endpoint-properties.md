# Inbound Endpoint Properties

See the topics given below for details.

## Common Inbound Endpoint Properties

The following parameters are common to all inbound endpoints:

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
         <td>sequential</td>
         <td>The behavior when executing the given sequence.<br />
            When set as <code>true</code> , mediation will happen within the same thread. When set as <code>false</code> , the mediation engine will use the inbound thread pool. The default thread pool values can be found in the <code>MI_HOME/conf/synapse.properties</code> file. The default setting is <code>true</code>.
         </td>
      </tr>
      <tr>
         <td>suspend</td>
         <td>When set to <code>true</code>, this makes the inbound endpoint inactive. The default setting is <code>false</code>.</td>
      </tr>
   </tbody>
</table>

## Properties: Listening Inbound 

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

### HTTP Inbound Endpoint Properties (Required)

<table>
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
</table>

### HTTPS Inbount Endpoint (Required Properties)

<table>
  <tr>
    <th>Property Name</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>
            inbound.http.port
    </td>
    <td>
            <p>The port on which the e ndpoint listener should be started .</p>
    </td>
    </tr>
   <tr>
         <td>
            keystore
         </td>
         <td>The KeyStore location where keys are stored.</td>
  </tr>
</table>

### HTTPS Inbount Endpoint (Optional Properties)
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

### HTTP/HTTPS Worker Pool Configuration Properties

Following is a sample configuration for an inbound endpoint with a separate worker pool:

```
<inboundEndpoint name="HttpListenerEP" protocol="http" suspend="false" sequence="TestIn" onError="fault" >
        <p:parameters xmlns:p="http://ws.apache.org/ns/synapse">
            <p:parameter  name="inbound.http.port">8081</p:parameter>
            <p:parameter  name="api.dispatching.enabled">false</p:parameter> 
            <p:parameter name="inbound.thread.group.id">Pass_Through Inbound worker Pool</p:parameter>
            <p:parameter name="inbound.worker.pool.size.max">500</p:parameter>
            <p:parameter name="inbound.thread.id">PassThroughInboundWorkerPool</p:parameter>
            <p:parameter name="inbound.worker.pool.queue.length">-1</p:parameter>
            <p:parameter name="inbound.worker.pool.size.core">400</p:parameter>
            <p:parameter name="inbound.worker.thread.keep.alive.sec">60</p:parameter>
        </p:parameters>
</inboundEndpoint>
```

By default inbound endpoints share the PassThrough transport worker pool to handle incoming requests. If you need a separate worker pool for the inbound endpoint, you need to configure the following parameters:

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
    <td>dispatch.filter.pattern</td>
    <td>
      The regular expression that defines the proxy services and API's to expose via the inbound endpoint. Provide the .* expression to expose all proxy services and API's or provide an expression similar to <code>^(/foo|/bar|/services/MyProxy)$</code> to define a set of services to expose via the inbound endpoint. If you do not provide an expression only the defined sequence of the inbound endpoint will be accessible.
    </td>
  </tr>
</table>

**Inbound endpoint properties for proxy services**

Following is a sample proxy configuration:

```
<proxy xmlns="http://ws.apache.org/ns/synapse" name="InboundProxy" transports="https,http" statistics="disable" trace="disable" startOnLoad="true">
        <target>
            <outSequence>
                <send/>
            </outSequence>
            <endpoint>
                <address uri="http://localhost:9773/services/HelloService/"/>
            </endpoint>
        </target>
        <parameter name="inbound.only">true</parameter>
</proxy>
```

If a proxy service is to be exposed only via inbound endpoints, the following service parameter has to be set in the proxy configuration.

<table>
   <thead>
      <tr>
         <th>Service Parameter</th>
         <th>Description</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>inbound.only</td>
         <td>
            <p>Whether the proxy service needs to be exposed only via inbound endpoints.</p>
            <p>If set to <code>true</code> all requests that the proxy service receives via normal transport will be rejected. The proxy service will process only the requests that are received via inbound endpoints.</br></br> The default setting is <code>false</code>.</p>
         </td>
      </tr>
   </tbody>
</table>

### CXF WS-RM Inbound Endpoint Properties

The CXF WS-RM Inbound endpoint can be configured by specifying the following parameters:

<table>
  <tr>
    <th>Property Name</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Sequence</td>
    <td>
      The sequence to which the message will be injected.
    </td>
  </tr>
  <tr>
    <td>Error sequence</td>
    <td>
      The sequence to be called if a fault occurs.
    </td>
  </tr>
  <tr>
    <td>Suspend</td>
    <td>
      If the inbound listener should pause when accepting incoming requests, set this to <code>true</code>. If the inbound listener should not pause when accepting incoming requests, set this to <code>false</code>.
    </td>
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
</table>

### HL7 Inbound Endpoint

#### HL7 Endpoint Properties

The following table provides information on the HL7 inbound endpoint parameters you can set:

<table>
   <thead>
      <tr>
         <th><strong>Property</strong></th>
         <th><strong>Description</strong></th>
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
   </tbody>
</table>

#### HL7 Mediation-Level Properties

Following are the mediation level properties that you can set in the HL7 inbound endpoint:

!!! Note
    The scope of these properties is the `default` scope.

| **Property**                                                                                            | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|---------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `                         `          | This is use to define the type of HL7 acknowledgement to be sent. If the `             inbound.hl7.AutoAck            ` parameter is set to `             true            ` this property has no effect.                                                                                                                                                                                                                                                                                                             |
| `             <property name="HL7_NACK_MESSAGE" value="<ERROR MESSAGE>" scope="default" />            ` | This is used to define a custom error message to be sent if you have set the property `             HL7_RESULT_MODE            ` as `             NACK            ` .                                                                                                                                                                                                                                                                                                                                                |
| `             <property name="HL7_APPLICATION_ACK" value="true" scope="default"/>            `          | If the `             inbound.hl7.AutoAck            ` parameter is set to `             false            ` and no immediate auto generated ACK is sent back to the client, this property defines whether we should automatically generate the ACK for the request once the mediation flow is complete. If both the `             inbound.hl7.AutoAck            ` parameter and this property are set to `             false            `, you need to generate an ACK message in the correct format as a response. |

### WebSocket Inbound Endpoint Properties

#### Required Properties

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
</table>

#### Optional Properties

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
         <td>
           The fully qualified class name of a pipeline handler class that you implemented.
         </td>
      </tr>
   </tbody>
</table>

### Secured WebSocket Inbound Endpoint Properties

#### Required Properties

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

#### Optional Properties

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
   </tbody>
</table>

## Polling Inbound Endpoint Properties

The following parameters are common to all polling inbound endpoints:

### Common Polling Inbound Endpoint Properties

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>interval</td>
    <td>
      The polling interval for the inbound endpoint to execute each cycle. This value is set in milliseconds.
    </td>
  </tr>
  <tr>
    <td>coordination</td>
    <td>
      This optional property is only applicable in a cluster environment. In a clustered environment, an inbound endpoint will only be executed in worker nodes. If set to <code>true</code> in a cluster setup, this will run the inbound only in a single worker node. Once the running worker is down, the inbound starts on another available worker in the cluster. By default, coordniation is enabled.
    </td>
  </tr>
</table>

### JMS Inbound Endpoint Properties

Following is a sample JMS inbound endpoint configuration:

```
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" 
                     name="jms" sequence="request" 
                     onError="fault" 
                     protocol="jms" 
                     suspend="false">
          <parameters>
             <parameter name="interval">1000</parameter>
             <parameter name="coordination">true</parameter> 
             <parameter name="transport.jms.Destination">ordersQueue</parameter>
             <parameter name="transport.jms.CacheLevel">3</parameter>
             <parameter name="transport.jms.ConnectionFactoryJNDIName">QueueConnectionFactory</parameter>
             <parameter name="sequential">true</parameter>
             <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
             <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>
             <parameter name="transport.jms.SessionAcknowledgement">AUTO_ACKNOWLEDGE</parameter>
             <parameter name="transport.jms.ConnectionFactoryType">queue</parameter>
          </parameters>
</inboundEndpoint>
```
#### Required Properties

Listed below are the required properties when defining a JMS Inbound Endpint:

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>
      <p>java.naming.factory.initial</p>
    </td>
    <td>
      <p>The JNDI initial context factory class. The class must implement the <code>java.naming.spi.InitialContextFactory</code> interface.</p>
    </td>
  </tr>
  <tr>
    <td>
       <p>java.naming.provider.url</p>
     </td>
     <td>
       <p>The URL of the JNDI provider.</p>
     </td>
  </tr>
  <tr>
    <td>
      <p>transport.jms.ConnectionFactoryJNDIName</p>
    </td>
    <td>
      <p>The JNDI name of the connection factory.</p>
    </td>
  </tr>
  <tr>
    <td>
       sequential
     </td>
     <td>Whether the messages need to be polled and injected sequentially or not.</td>
  </tr>
</table>

#### Optional Properties

Listed below are the optional properties when defining a JMS Inbound Endpint:

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
            transport.jms.ConnectionFactoryType
         </td>
         <td>
            The type of the connection factory.</br></br>  Set to <b>queue</b> by default.
         </td>
      </tr>
      <tr>
         <td>
            <p>transport.jms.Destination/p>
         </td>
         <td>
            <p>The JNDI name of the destination.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.SessionAcknowledgement
         </td>
         <td>
            <p>The JMS session acknowledgment mode. You can use one of the following: <code>AUTO_ACKNOWLEDGE</code> , <code>CLIENT_ACKNOWLEDGE</code> , <code>DUPS_OK_ACKNOWLEDGE</code> , <code>SESSION_TRANSACTED</code>.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.CacheLevel
         </td>
         <td>
            <p>The JMS resource cache level. Possible values are as follows: <code>0</code>(none), <code>1</code>(connection), <code>2</code>(session), <code>3</code>(consumer).</br></br> The default value is <code>0-none</code>.</br></br>
            <b>Note</b>:To subscribe to topics, set the value of <code>transport.jms.CacheLevel</code> to <code>3</code>.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.UserName
         </td>
         <td>
            <p>The JMS connection username.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.Password
         </td>
         <td>
            <p>The JMS connection password.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.JMSSpecVersion
         </td>
         <td>
            <p>The JMS API version. The possible values are as follows: <code>1.0.2b</code>, <code>1.1</code>, <code>2.0</code>.</br></br> The default value is <code>1.1.</code>.</p>
         </td>
      </tr>
      <tr>
         <td>transport.jms.SubscriptionDurable</td>
         <td>Whether the connection factory is subscription durable or not.</td>
      </tr>
      <tr>
         <td>transport.jms.DurableSubscriberClientID</td>
         <td>The <code>ClientId</code> parameter when using durable subscriptions. This property is required if the value specified as <code>transport.jms.SubscriptionDurable</code> is <code>true</code>.</td>
      </tr>
      <tr>
         <td>
            transport.jms.DurableSubscriberName
         </td>
         <td>
            <p>The name of the durable subscriber. This property is required if the value specified as <code>transport.jms.SubscriptionDurable</code> is <code>true</code>.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.MessageSelector
         </td>
         <td>
            Message selector implementation.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.ReceiveTimeout
         </td>
         <td>The time to wait for a JMS message during polling.<br />
            Set this parameter value to a negative integer to wait indefinitely. Set it to zero to prevent waiting.</br></br> The default value is 1.
         </td>
      </tr>
      <tr>
         <td>transport.jms.ContentType</td>
         <td>How the inbound listener should determine the content type of received messages. Priority is always given to the JMS message type. Possible values are any simple string value. In which case the transport listener assumes that the received messages always have the specified content type, or a set of rules. For more information, see <a href="http://axis.apache.org/axis2/java/transports/jms.html#Service_configuration">http://axis.apache.org/axis2/java/transports/jms.html#Service_configuration</a>.</td>
      </tr>
      <tr>
         <td>transport.jms.ContentTypeProperty</td>
         <td>Gets the content type from the message property.</td>
      </tr>
      <tr>
         <td>transport.jms.ReplyDestination</td>
         <td>The destination where the response generated by the back-end service is stored.</td>
      </tr>
      <tr>
         <td>
            <p>transport.jms.PubSubNoLocal</p>
         </td>
         <td>
            <p>Whether messages should be published via the same connection that they were received.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.SharedSubscription
         </td>
         <td>
            <p>If set to <code>true</code>, messages will be forwarded to only one of the consumers and consumers will share the messages that are published to the topic.</p>
         </td>
      </tr>
      <tr>
         <td>
            <p>pinnedServers</p>
         </td>
         <td>
            <p>List of synapse server names separated by commas or spaces where this inbound endpoint should be deployed. If there is no pinned server list, the inbound endpoint configuration will be deployed in all server instances.</p>
         </td>
      </tr>
      <tr>
         <td>transport.jms.ConcurrentConsumers</td>
         <td>Number of concurrent threads to be started to consume messages when polling.</br></br> The default value is 1. You can change this to any positive integer. However, for topics the value must always be 1.</td>
      </tr>
      <tr>
         <td>
            transport.jms.retry.duration
         </td>
         <td>
            <p>The retry interval (in miliseconds) to reconnect to the JMS server.</p>
         </td>
      </tr>
      <tr>
         <td>transport.jms.RetriesBeforeSuspension</td>
         <td>The number of consecutive mediation failures after which polling should be suspended. Specify any positive numerical value based on your requirement. Polling will be suspended when the mediation failure count reaches the specified value.</td>
      </tr>
      <tr>
         <td>transport.jms.PollingSuspensionPeriod</td>
         <td>The period of time that polling is to be suspended when the <code>             transport.jms.RetriesBeforeSuspension</code> parameter is set.</br></br> Default value is 60000.</td>
      </tr>
   </tbody>
</table>

### Kafka Inbound Endpoint Properties

Following are two high-level Kafka configurations that can be used to consume messages in two ways: Using **specific topics** or using a **topic filter**.

``` java tab='Using Specific Topics'
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                     name="KakfaListenerEP"
                     sequence="requestHandlerSeq"
                     onError="inFaulte"
                     protocol="kafka"
                     suspend="false">
       <parameters>
          <parameter name="interval">100</parameter> 
          <parameter name="coordination">true</parameter>
          <parameter name="sequential">true</parameter>
          <parameter name="zookeeper.connect">localhost:2181</parameter>
          <parameter name="consumer.type">highlevel</parameter>
          <parameter name="content.type">application/xml</parameter>
          <parameter name="topics">test,sampletest</parameter>
          <parameter name="group.id">test-group</parameter>
       </parameters>
</inboundEndpoint>
```

``` java tab='Using a Topic Filter'
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                     name="KakfaListenerEP"
                     sequence="requestHandlerSeq"
                     onError="inFaulte"
                     protocol="kafka"
                     suspend="false">
       <parameters>
          <parameter name="interval">100</parameter> 
          <parameter name="coordination">true</parameter>
          <parameter name="sequential">true</parameter>
          <parameter name="zookeeper.connect">localhost:2181</parameter>
          <parameter name="consumer.type">highlevel</parameter>
          <parameter name="content.type">application/xml</parameter>
          <parameter name="topic.filter">test</parameter>
          <parameter name="filter.from.whitelist">true</parameter>
          <parameter name="group.id">test-group</parameter>      
       </parameters>
</inboundEndpoint>
```

Listed below are the Kafka inbound endpoint properties for a high-level configuration.

#### High-level Kafka Configuration (Required Properties)

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
      <td>
        zookeeper.connect
      </td>
      <td>The host and port of a ZooKeeper server (<code>hostname:port</code>).</td>
      </tr>
      <tr>
         <td>
            consumer.type
         </td>
         <td>The consumer configuration type. This can either be <code>simple</code> or <code>highlevel</code>.</td>
      </tr>
      <tr>
         <td>
          interval
         </td>
         <td>The polling interval for the inbound endpoint to poll the messages.</td>
      </tr>
      <tr>
         <td>
          coordination
         </td>
         <td>If set to <code>true</code> in a clustered setup, this will run the inbound only in a single worker node.</br></br> The property is set to <code>true</code> by default.</td>
      </tr>
      <tr>
         <td>
           sequential
         </td>
         <td>The behaviour when executing the given sequence.</br></br> The property is set to <code>true</code> by default.</td>
      </tr>
      <tr>
         <td>
            topics
         </td>
         <td>The category to feed the messages. A high-level kafka configuration can have more than one topic. You can specify multiple topic names as comma separated values.</td>
      </tr>
      <tr>
         <td>
           content.type
         </td>
         <td>The content of the message. The possible values are as follows: <code>appllication/xml</code> or <code>application/json</code>.</td>
      </tr>
      <tr>
         <td>
          group.id
         </td>
         <td>
            <p>If all the consumer instances have the same consumer group, this works as a traditional queue balancing the load over the consumers.</p>
         </td>
      </tr>
</table>

#### High-level Kafka Configuration (Optional Properties)

<table>
   <thead>
      <tr>
         <th>
           Property Name
         </th>
         <th>
            <p>Description</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>
          thread.count
         </td>
         <td>The number of threads. The default value is 1.</td>
      </tr>
      <tr>
         <td>consumer.id</td>
         <td>The id of the consumer. The default value is <code>null</code>.</td>
      </tr>
      <tr>
         <td>socket.timeout.ms</td>
         <td>The socket timeout for network requests. The default value is <code>30 * 1000</code>.</td>
      </tr>
      <tr>
         <td>socket.receive.buffer.bytes</td>
         <td>The socket receive buffer for network requests. The default value is <code>64 * 1024</code>.</td>
      </tr>
      <tr>
         <td>fetch.message.max.bytes</td>
         <td>The number of bytes of messages that the system should attempt to fetch for each topic-partition in each fetch request. The default values is <code>1024 * 1024</code>.</td>
      </tr>
      <tr>
         <td>num.consumer.fetchers</td>
         <td>The number fetcher threads used to fetch data. The default value is 1.</td>
      </tr>
      <tr>
         <td>auto.commit.enable</td>
         <td>The committed offset to be used as the position from which the new consumer will begin when the process fails.</br></br> The default value is <code>true</code>.</td>
      </tr>
      <tr>
         <td>auto.commit.interval.ms</td>
         <td>The frequency (in miliseconds) at which the consumer offsets are committed to zookeeper.</br></br> The default value is <code>60 * 1000</code>.</td>
      </tr>
      <tr>
         <td>queued.max.message.chunks</td>
         <td>The maximum number of message chunks buffered for consumption. Each chunk can go up to the value specified in <code>fetch.message.max.bytes</code>.</br></br> The default value is 2.</td>
      </tr>
      <tr>
         <td>rebalance.max.retries</td>
         <td>The maximum number of retry attempts. The default value is 4.</td>
      </tr>
      <tr>
         <td>fetch.min.bytes</td>
         <td>The minimum amount of data the server should return for a fetch request. The default value is 1.</td>
      </tr>
      <tr>
         <td>fetch.wait.max.ms</td>
         <td>The maximum amount of time the server will stay blocked before responding to the fetch request when sufficient data is not available to immediately serve <code>fetch.min.bytes</code>.</br></br> The default value is <code>100</code></td>
      </tr>
      <tr>
         <td>rebalance.backoff.ms</td>
         <td>The backoff time between retries during rebalance. The default value is 2000.</td>
      </tr>
      <tr>
         <td>refresh.leader.backoff.ms</td>
         <td>The backoff time to wait before trying to determine the leader of a partition that has just lost its leader.</br></br> The default value is 200.</td>
      </tr>
      <tr>
         <td>auto.offset.reset</td>
         <td>
            <p>Set this to one of the following values based on what you need to do when there is no initial offset in ZooKeeper or if an offset is out of range.</p>
            <ul>
              <li><b>smallest</b>: Automatically reset the offset to the smallest offset.</li>
              <li><b>largest</b>: Automatically reset the offset to the largest offset.</li>
              <li><b>anything else</b>: Throw an exception to the consumer.</li>
            </ul>
            The default values is <b>largest</b>.
         </td>
      </tr>
      <tr>
         <td>consumer.timeout.ms</td>
         <td>The timeout interval after which a timeout exception is to be thrown to the consumer if no message is available for consumption. It is a good practice to set this value lower than the interval of the Kafka inbound endpoint. The default value is 2000.</td>
      </tr>
      <tr>
         <td>exclude.internal.topics</td>
         <td>Set to <code>true</code> if messages from internal topics such as offsets should be exposed to the consumer. The default value is <code>true</code>.</td>
      </tr>
      <tr>
         <td>partition.assignment.strategy</td>
         <td>The partitions assignment strategy to be used when assigning partitions to consumer streams. Possible values are <code>range</code> and <code>roundrobin</code>.</br></br> Default setting is <code>range</code>.</td>
      </tr>
      <tr>
         <td>client.id</td>
         <td>The user specified string sent in each request to help trace calls.</td>
      </tr>
      <tr>
         <td>
            <p>zookeeper.session.timeout.ms</p>
         </td>
         <td>The ZooKeeper session timeout value in milliseconds. The default value is 6000.</td>
      </tr>
      <tr>
         <td>zookeeper.connection.timeout.ms</td>
         <td>The maximum time in milliseconds that the client should wait while establishing a connection to ZooKeeper.</br></br> The default value is 6000.</td>
      </tr>
      <tr>
         <td>
            <p>zookeeper.sync.time.ms</p>
         </td>
         <td>The time difference in milliseconds that a ZooKeeper follower can be behind a ZooKeeper leader. The default value is 2000.</td>
      </tr>
      <tr>
         <td>offsets.storage</td>
         <td>The offsets storage location. Possible values are <code>zookeeper<code> and <code>kafka</code>. Default setting is <code>zookeeper</code>.</td>
      </tr>
      <tr>
         <td>offsets.channel.backoff.ms</td>
         <td>The backoff period in milliseconds when reconnecting the offsets channel or retrying failed offset fetch/commit requests.</br></br> Default value is 1000.</td>
      </tr>
      <tr>
         <td>offsets.channel.socket.timeout.ms</td>
         <td>The socket timeout in milliseconds when reading responses for offset fetch/commit requests.</br></br> The default value is 10000.</td>
      </tr>
      <tr>
         <td>offsets.commit.max.retries</td>
         <td>The maximum retry attempts allowed. If a consumer metadata request fails for any reason, retry takes place but does not have an impact on this limit.</br></br> Default value is 5.</td>
      </tr>
      <tr>
         <td>dual.commit.enabled</td>
         <td>If <code>offsets.storage</code> is set to <code>kafka</code>, the commit offsets can be dual to ZooKeeper. Set this to <code>true</code> if you need to perform migration from zookeeper-based offset storage to kafka-based offset storage. The default value is <code>true</code>.</td>
      </tr>
   </tbody>
</table>

Following are descriptions of the parameters set to consume topic filters in a kafka configuration:

!!! Info
    In high-level kafka configurations, the follwing parameters are used instead of the `topics` paramater.
    <parameter name="topic.filter">test</parameter>
    <parameter name="filter.from.whitelist">true</parameter>

<table>
   <thead>
      <tr>
         <th>
            <p>Parameter</p>
         </th>
         <th>
            <p>Description</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>
            topic.filter
         </td>
         <td>The name of the topic filter.</td>
      </tr>
      <tr>
         <td>
            filter.from.whitelist
         </td>
         <td>If this is set to <code>true</code>, messages are consumed from the whitelist(include).<br />
            If this is set to <code>false</code>, messages are consumed from the blacklist(exclude).
         </td>
      </tr>
   </tbody>
</table>

#### Low-Level Kafka Inbound Endpoint Properties

Kafka inbound endpoint parameters for a low-level configuration:

Following is a sample low-level Kafka configuration that can be used to consume messages from a specific server in a specific partition, so that the messages are limited:

```
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                     name="KakfaListenerEP"
                     sequence="requestHandlerSeq"
                     onError="inFaulte"
                     protocol="kafka"
                     interval="1000"
                     suspend="false">
       <parameters>     
          <parameter name="zookeeper.connect">localhost:2181</parameter>
          <parameter name="group.id">test-group</parameter>  
          <parameter name="content.type">application/xml</parameter>
          <parameter name="consumer.type">simple</parameter>
          <parameter name="simple.max.messages.to.read">5</parameter>
          <parameter name="simple.topic">test</parameter>
          <parameter name="simple.brokers">localhost</parameter>
          <parameter name="simple.port">9092</parameter>
          <parameter name="simple.partition">1</parameter>
          <parameter name="interval">1000</parameter>
       </parameters>
</inboundEndpoint>
```

<table>
   <thead>
      <tr>
         <th>
            <p>Property</p>
         </th>
         <th>
            <p>Description</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>
            simple.topic
         </td>
         <td>The category to feed the messages.</td>
      </tr>
      <tr>
         <td>
            simple.brokers
         </td>
         <td>The specific Kafka broker name.</td>
      </tr>
      <tr>
         <td>
            simple.port
         </td>
         <td>The specific Kafka server port number.</td>
      </tr>
      <tr>
         <td>
            simple.partition
         </td>
         <td>The partition of the topic.</td>
      </tr>
      <tr>
         <td>
            simple.max.messages.to.read
         </td>
         <td>
            <p>The maximum number of messages to retrieve.</p>
         </td>
      </tr>
   </tbody>
</table>

## Event-based Inbound Endpoint Properties

``` java tab='MQTT Inbound Protocol'
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

``` java tab='RabbitMQ Inbound Protocol'
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="RabbitMQConsumer" sequence="amqpSeq" onError="amqpErrorSeq" protocol="rabbitmq" suspend="false">
       <parameters>
          <parameter name="sequential">true</parameter>
          <parameter name="coordination">true</parameter>
          <parameter name="rabbitmq.connection.factory">AMQPConnectionFactory</parameter>
          <parameter name="rabbitmq.server.host.name">localhost</parameter>
          <parameter name="rabbitmq.server.port">5672</parameter>
          <parameter name="rabbitmq.server.user.name">guest</parameter>
          <parameter name="rabbitmq.server.password">guest</parameter>
          <parameter name="rabbitmq.queue.name">queue</parameter>
          <parameter name="rabbitmq.exchange.name">exchange</parameter>
          <parameter name="rabbitmq.connection.ssl.enabled">false</parameter>
       </parameters>
    </inboundEndpoint>
```

### Common Event-Based Endpoint Properties

The following parameter is common to all event-based inbound endpoints:

<table>
   <thead>
      <tr>
         <th>
            Property
         </th>
         <th>
            Description
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>coordination</td>
         <td>This parameter is only applicable in a clustered environment.<br />
            In a cluster environment an inbound endpoint will only be executed in worker nodes. If this parameter is set to <code>true</code> in a clustered environment, the inbound will only be executed in a single worker node. Once the running worker node is down, the inbound will start on another available worker node in the cluster. By default, this setting is <code>true</code>.
         </td>
      </tr>
   </tbody>
</table>

### MQTT Inbound Protocol Properties

#### Required Properties

Listed below are the required properties when you define an MQTT Inbound Endpoint:

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
      <td>
         sequential
      </td>
      <td>The behaviour when executing the given sequence.</td>
   </tr>
   <tr>
      <td>
         coordination
      </td>
      <td>In a clustered setup, if set to <code>true</code>, the inbound endpoint will run only in a single worker node. If set to false, it will run on all worker nodes.</td>
   </tr>
   <tr>
      <td>
         suspend
      </td>
      <td>If set to <code>true</code>, the inbound listener will not accept incoming requests from clients and will not inject messages to the synapse message mediation. If set to <code>false</code>, incoming messages will be accepted.</td>
   </tr>
   <tr>
      <td>
         mqtt.connection.factory
      </td>
      <td>Name of the connection factory.</td>
   </tr>
   <tr>
      <td>
         mqtt.server.host.name
      </td>
      <td>
        Address of the message broker (eg., localhost).
      </td>
   </tr>
   <tr>
      <td>
         mqtt.server.port
      </td>
      <td>Port of the message broker (e.g., 1883).</td>
   </tr>
   <tr>
      <td>
         mqtt.topic.name
      </td>
      <td>MQTT topic to which the message should be published.</td>
   </tr>
   <tr>
      <td>
         content.type
      </td>
      <td>The content type of the message, i.e., XML or JSON)</td>
   </tr>
</table>

#### Optional Properties

Listed below are the optional properties when you define an MQTT Inbound Endpoint:

<table>
<thead>
   <tr>
      <th>
        Property
      </th>
      <th>
        Description
      </th>
   </tr>
</thead>
<tbody>
   <tr>
      <td>
         mqtt.subscription.qos
      </td>
      <td>The quality of service level that need to be maintained with the subscription. The quality of service level can be either 0,1 or 2.<br />
         0 -  Specifying this level ensures that the message delivery is efficient. However, specifying this level does not guarantee that the message will be delivered to its recipient.<br />
         1 -  Specifying this level ensures that the message is delivered at least once, but this can lead to messages being duplicated.<br />
         2 - This is the highest level of quality of service. Specifying this guarantees that the message is delivered and that it is delivered only once.
      </td>
   </tr>
   <tr>
      <td>
         mqtt.client.id
      </td>
      <td>The id of the client.</td>
   </tr>
   <tr>
      <td>
         mqtt.session.clean
      </td>
      <td>
        Whether the client and server should remember the state across restarts and reconnects.
      </td>
   </tr>
   <tr>
      <td>
         mqtt.ssl.enable
      </td>
      <td>
        Whether to use TCP connection or SSL connection.
      </td>
   </tr>
   <tr>
      <td>
         mqtt.subscription.username
      </td>
      <td>The username for the subscription.</td>
   </tr>
   <tr>
      <td>
         mqtt.subscription.password
      </td>
      <td>The password for the subscription.</td>
   </tr>
   <tr>
      <td>
         mqtt.temporary.store.directory
      </td>
      <td>
        Path of the directory to be used as the persistent data store for quality of service purposes.
      </td>
   </tr>
   <tr>
      <td>
         mqtt.reconnection.interval
      </td>
      <td>
        The retry interval to reconnect to the MQTT server.
      </td>
   </tr>
</tbody>
</table>

### RabbitMQ Inbound Protocol Properties

#### Required Properties

Listed below are the required properties when you define a RabbitMQ Inbound Endpoint:

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
         <td>
            sequential
         </td>
         <td>The behavior when executing the given sequence.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.factory
         </td>
         <td>Name of the connection factory.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.server.host.name
         </td>
         <td>
            Host name of the server.
         </td>
      </tr>
      <tr>
         <td>
            rabbitmq.server.port
         </td>
         <td>The port number of the server.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.server.user.name
         </td>
         <td>The user name to connect to the server.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.server.password
         </td>
         <td>The password to connect to the server.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.name
         </td>
         <td>The queue name to send or consume messages.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.exchange.name
         </td>
         <td>The name of the RabbitMQ exchange to which the queue is bound.</td>
      </tr>
</table>

#### Optional Properties

Listed below are the optional properties when you define a RabbitMQ Inbound Endpoint:

<table>
   <thead>
      <tr>
         <th>
          Property Name
         </th>
         <th>
          Description
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>
            rabbitmq.server.virtual.host
         </td>
         <td>The virtual host name of the server (if available).</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.enabled
         </td>
         <td>Whether to use TCP connection or SSL connection.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.keystore.location
         </td>
         <td>The keystore location.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.keystore.type
         </td>
         <td>
          The keystore type.
         </td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.keystore.password
         </td>
         <td>The keystore password.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.truststore.location
         </td>
         <td>The truststore location.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.truststore.type
         </td>
         <td>The truststore type.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.truststore.password
         </td>
         <td>The truststore password.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.connection.ssl.version
         </td>
         <td>The SSL version.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.durable
         </td>
         <td>Whether the queue should remain declared even if the broker restarts.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.exclusive
         </td>
         <td>Whether the queue should be exclusive or should be consumable by other connections.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.auto.delete
         </td>
         <td>Whether to keep the queue even if it is not being consumed anymore.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.auto.ack
         </td>
         <td>Whether to send back an acknowledgment.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.routing.key
         </td>
         <td>The routing key of the queue.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.queue.delivery.mode
         </td>
         <td>The delivery mode of the queue (whether or not the queue is persistent).</td>
      </tr>
      <tr>
         <td>
            rabbitmq.exchange.type
         </td>
         <td>The type of the exchange.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.exchange.durable
         </td>
         <td>Whether the exchange should remain declared even if the broker restarts.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.exchange.auto.delete
         </td>
         <td>Whether to keep the queue even if it is not used anymore.</td>
      </tr>
      <tr>
         <td>
            rabbitmq.factory.heartbeat
         <td>
            <p>The period of time after which the connection should be considered dead.</p>
         </td>
      </tr>
      <tr>
         <td>
            rabbitmq.message.content.type
         </td>
         <td>The content type of the message.</td>
      </tr>
   </tbody>
</table>

**Connection recovery properties**

In case of a network failure or broker shutdown, the Micro Integrator can try to
recreate the connection.

If you want to enable connection recovery, you should configure the
following parameters in the inbound endpoint:

```
<parameter name="rabbitmq.connection.retry.interval" locked="false">10000</parameter>
<parameter name="rabbitmq.connection.retry.count" locked="false">5</parameter>   
```

If the parameters are configured with sample values as given above, the
server makes 5 retry attempts with a time interval of 10000 miliseconds between each
retry attempt to reconnect when the connection is lost. If reconnecting
fails after 5 retry attempts, the Micro Integrator terminates the connection.

In addition to the above parameters, if you want to set the interval
between retry attempts with which the RabbitMQ client should try
reconnecting to the Micro Integrator, you can configure the following parameter:

```
<parameter name="rabbitmq.server.retry.interval" locked="false">10000</parameter>
```

Setting the value of `rabbitmq.server.retry.interval` to be less than the value of `rabbitmq.connection.retry.interval` helps synchronize the reconnection of the Micro Integrator and the RabbitMQ client.

## Custom Inbound Endpoint Properties

Sample customer inbound endpoint implementation.

``` java tab='Custom Listening Inbound'
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="custom_listener" sequence="request" onError="fault"
                         class="org.wso2.carbon.inbound.custom.listening.SampleListeningEP" suspend="false">
       <parameters>
          <parameter name="sequential">true</parameter>
          <parameter name="inbound.behavior">listening</parameter>
          <parameter name="coordination">true</parameter>
       </parameters>
</inboundEndpoint>
```

``` java tab='Custom Polling Inbound'
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="class" sequence="request" onError="fault"
                                class="org.wso2.carbon.inbound.custom.poll.SamplePollingClient" suspend="false">
       <parameters>
          <parameter name="sequential">true</parameter>
          <parameter name="interval">2000</parameter>
          <parameter name="coordination">true</parameter>
       </parameters>
</inboundEndpoint>
```

``` java tab='Custom Event-Based Inbound'
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="custom_waiting" sequence="request" onError="fault"
                         class="org.wso2.carbon.inbound.custom.wait.SampleWaitingClient" suspend="false">
       <parameters>
          <parameter name="sequential">true</parameter>
          <parameter name="inbound.behavior">eventBased</parameter>
          <parameter name="coordination">true</parameter>
       </parameters>
</inboundEndpoint>
```

Given below are the properties that should be configured when you define a custom inbound endpoint.

<table>
   <thead>
      <tr>
         <th>
            <p>Parameter Name</p>
         </th>
         <th>
            <p>Description</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>
          class
         </td>
         <td>
          Name of the custom class implementation. Specify a valid class name.
         </td>
      </tr>
      <tr>
         <td>
          sequence
         </td>
         <td>Name of the sequence message that should be injected. Specify a valid sequence name.</td>
      </tr>
      <tr>
         <td>
            onError
         </td>
         <td>Name of the fault sequence that should be invoked in case of failure. Specify a valid sequence name.</td>
      </tr>
      <tr>
         <td>
          inbound.behavior
         </td>
         <td>
          The behaviour of the inbound endpoint. Specify <code>listening</code>.
         </td>
      </tr>
   </tbody>
</table>