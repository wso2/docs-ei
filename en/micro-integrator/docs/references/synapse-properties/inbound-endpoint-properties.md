# Inbound Endpoint Properties

Right-click the inbound endpoint and click **Show Properties View** to go to the **Property** tab. You can update the parameters relevant to the type of inbound endpoint.

## Common inbound endpoint parameters

The following parameters are common to all inbound endpoints:

<table>
   <colgroup>
      <col style="width: 20%" />
      <col style="width: 20%" />
      <col style="width: 20%" />
      <col style="width: 20%" />
      <col style="width: 20%" />
   </colgroup>
   <thead>
      <tr class="header">
         <th>
            <p>Parameter Name</p>
         </th>
         <th>
            <p>Description</p>
         </th>
         <th>
            <p>Required</p>
         </th>
         <th>
            <p>Possible Values</p>
         </th>
         <th>
            <p>Default Value</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>sequential</td>
         <td>The behavior when executing the given sequence.<br />
            When set as <code>             true            </code> , mediation will happen within the same thread. When set as <code>             false            </code> , the mediation engine will use the inbound thread pool. (The default thread pool values can be found in the <code>             &lt;EI_HOME&gt;/conf/synapse.properties            </code> file).
         </td>
         <td>Yes</td>
         <td>true or false</td>
         <td>true</td>
      </tr>
      <tr class="even">
         <td>suspend</td>
         <td>When set to <code>             true            </code> , this makes the inbound endpoint inactive.</td>
         <td>Yes</td>
         <td>true or false</td>
         <td>false</td>
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

``` java tab='HTTPS'
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

### HTTP inbound endpoint parameters

| Parameter                                    | Description                                                | Required |
|----------------------------------------------|------------------------------------------------------------|----------|
| ` inbound.http.port            ` | The port on which the endpoint listener should be started. | Yes      |


**Worker pool configuration parameters**

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

By default inbound endpoints share the PassThrough transport worker pool to handle incoming requests. If you need a separate worker pool for the
inbound endpoint, you need to configure the following parameters:

| Parameter                                                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Default Value                           |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
| `              inbound.worker.pool.size.core             `        | The initial number of threads in the worker thread pool. This value can be changed accordingly based on the number of messages to be processed. The maximum value that can be specified here is the value of the `             i             nbound.worker.pool.size.max            ` parameter.                                                                                                                                                                          | 400                                     |
| `              inbound.worker.pool.size.max             `         | The maximum number of threads in the worker thread pool. Specify a maximum limit in order to avoid performance degradation that can occur due to context switching.                                                                                                                                                                                                                                                                                                       | 500                                     |
| `              inbound.worker.thread.keep.alive.sec             ` | The keep-alive time for extra threads in the worker pool. This value should be less than the socket timeout. When this time is elapsed for an extra thread, it will be destroyed. The purpose of this parameter is to optimize the usage of resources by avoiding wastage that results from having extra threads that are not utilized.                                                                                                                                   | `             60            `           |
| `              inbound.worker.pool.queue.length             `     | The length of the queue that is used to hold runnable tasks to be executed by the worker pool. The thread pool starts queuing jobs when all existing threads are busy and the pool has reached the maximum number of threads. The value for this parameter should be -1 to use an unbounded queue. If a bound queue is used and the queue gets filled to its capacity, and any further attempt to submit jobs will fail causing synapse to drop some messages.            | -1                                      |
| `              inbound.thread.group.id             `              | Unique Identifier of the thread group.                                                                                                                                                                                                                                                                                                                                                                                                                                    | PassThrough inbound worker thread group |
| `              inbound.thread.id             `                    | Unique Identifier of the thread.                                                                                                                                                                                                                                                                                                                                                                                                                                          | PassThroughInboundWorkerThread          |
| `             dispatch.filter.pattern            `                | The regular expression that defines the proxy services and API's to expose via the inbound endpoint. Provide the `             .*            ` expression to expose all proxy services and API's or provide an expression similar to `             ^(/foo|/bar|/services/MyProxy)$            ` to define a set of services to expose via the inbound endpoint. If you do not provide an expression only the defined sequence of the inbound endpoint will be accessible. | blank                                   |

**Inbound endpoint parameter for proxy services**

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
   <colgroup>
      <col style="width: 33%" />
      <col style="width: 33%" />
      <col style="width: 33%" />
   </colgroup>
   <thead>
      <tr class="header">
         <th>Service Parameter</th>
         <th>Description</th>
         <th>Default Value</th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td><code>             inbound.only            </code></td>
         <td>
            <p>Whether the proxy service needs to be exposed only via inbound endpoints.</p>
            <p>If set to <code>              true             </code> all requests that the proxy service receives via normal transport will be rejected. The proxy service will process only the requests that are received via inbound endpoints.</p>
         </td>
         <td>false</td>
      </tr>
   </tbody>
</table>

### HTTPS Inbount Protocol

<table>
   <colgroup>
      <col style="width: 33%" />
      <col style="width: 33%" />
      <col style="width: 33%" />
   </colgroup>
   <thead>
      <tr class="header">
         <th>
            <p>Parameter</p>
         </th>
         <th>
            <p>Description</p>
         </th>
         <th>
            <p>Required</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>
            <pre><code>inbound.http.port</code></pre>
         </td>
         <td>
            <p>The port on which the e ndpoint listener should be started .</p>
         </td>
         <td>Yes</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>keystore</code></pre>
         </td>
         <td>The KeyStore location where keys are stored.</td>
         <td>Yes</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>truststore</code></pre>
         </td>
         <td>The TrustStore location where keys are stored.</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>SSLVerifyClient</code></pre>
         </td>
         <td>
            <p>Used when enabling mutual verification.</p>
         </td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>HttpsProtocols</code></pre>
         </td>
         <td>The supporting protocols.</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>SSLProtocol</code></pre>
         </td>
         <td>The supporting SSL protocol.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>CertificateRevocationVerifier</code></pre>
         </td>
         <td>
            <p>When the <code>              enable             </code> attribute is set to <code>              true             </code> , this validates and verifies the revocation status of the host certificates using OCSP/CRL, when making HTTPS connections.<br />
               If the <code>              enable             </code> attribute of this parameter is set to <code>              true             </code> , you also need to specify the <code>              CacheSize             </code> and <code>              CacheDelay             </code> .<br />
               <code>              CacheSize -             </code> The maximum size of the cache.<br />
               <code>              CacheDelay -             </code> The time duration between two consecutive scheduled cache managing tasks that perform housekeeping work for the cache.
            </p>
         </td>
         <td>No</td>
      </tr>
   </tbody>
</table>

**Worker pool configuration parameters**

By default inbound endpoints share the PassThrough transport worker pool to handle incoming requests. If you need a separate worker pool for the
inbound endpoint, you need to configure the following parameters:

| Parameter                                                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Default Value                           |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
| `              inbound.worker.pool.size.core             `        | The initial number of threads in the worker thread pool. This value can be changed accordingly based on the number of messages to be processed. The maximum value that can be specified here is the value of the `             i             nbound.worker.pool.size.max            ` parameter.                                                                                                                                                                          | 400                                     |
| `              inbound.worker.pool.size.max             `         | The maximum number of threads in the worker thread pool. Specify a maximum limit in order to avoid performance degradation that can occur due to context switching.                                                                                                                                                                                                                                                                                                       | 500                                     |
| `              inbound.worker.thread.keep.alive.sec             ` | The keep-alive time for extra threads in the worker pool. This value should be less than the socket timeout. When this time is elapsed for an extra thread, it will be destroyed. The purpose of this parameter is to optimize the usage of resources by avoiding wastage that results from having extra threads that are not utilized.                                                                                                                                   | `             60            `           |
| `              inbound.worker.pool.queue.length             `     | The length of the queue that is used to hold runnable tasks to be executed by the worker pool. The thread pool starts queuing jobs when all existing threads are busy and the pool has reached the maximum number of threads. The value for this parameter should be -1 to use an unbounded queue. If a bound queue is used and the queue gets filled to its capacity, and any further attempt to submit jobs will fail causing synapse to drop some messages.            | -1                                      |
| `              inbound.thread.group.id             `              | Unique Identifier of the thread group.                                                                                                                                                                                                                                                                                                                                                                                                                                    | PassThrough inbound worker thread group |
| `              inbound.thread.id             `                    | Unique Identifier of the thread.                                                                                                                                                                                                                                                                                                                                                                                                                                          | PassThroughInboundWorkerThread          |
| `             dispatch.filter.pattern            `                | The regular expression that defines the proxy services and API's to expose via the inbound endpoint. Provide the `             .*            ` expression to expose all proxy services and API's or provide an expression similar to `             ^(/foo|/bar|/services/MyProxy)$            ` to define a set of services to expose via the inbound endpoint. If you do not provide an expression only the defined sequence of the inbound endpoint will be accessible. | blank                                   |

### CXF WS-RM Inbound Protocol

The CXF WS-RM Inbound endpoint can be configured by specifying the following parameters:

-   `           Sequence          ` - The sequence that the message will
    be injected to.

-   `           Error sequence          ` - The sequence to be called if
    a fault occurs.

-   `          Suspend         ` - If the inbound listener should pause
    when accepting incoming requests, set this to
    `          true         ` . If the inbound listener should not pause
    when accepting incoming requests, set this to
    `          false         ` .
-   `           inbound.cxf.rm.host          ` - The host name.

-   `           inbound.cxf.rm.port          ` - The port to listen to.

        !!! info
    
        Note
    
        When configuring a SSL enabled cxf\_ws\_rm inbound endpoint, the
        `           inbound.cxf.rm.port          ` parameter should be set
        to the same value as the engine port number specified in the CXF
        spring configuration file saved in the
        `           <EI_HOME>/conf/cxf          ` directory.
    
        For example, in the above CXF spring configuration, the engine port
        is 8081. This same port number should be specified as the
        `           inbound.cxf.rm.port          ` in the cxf\_ws\_rm
        inbound endpoint configuration.
    

-   `           inbound.cxf.rm.config-file          ` - The path to the
    CXF Spring configuration file.

-   `           enableSSL          ` - Set to
    `           true          ` if SSL is enabled in the CXF Spring
    configuration file.

### HL7 Inbound Endpoint

**HL7 inbound endpoint parameters**: The following table provides information on the HL7 inbound endpoint parameters you can set:

<table>
   <thead>
      <tr class="header">
         <th><strong>Parameter</strong></th>
         <th><strong>Description</strong></th>
         <th><strong>Default Value</strong></th>
         <th><strong>Possible Values</strong></th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>inbound.hl7.Port</td>
         <td>The port on which you need to run the MLLP listener.</td>
         <td>N/A</td>
         <td>You need to specify this.</td>
      </tr>
      <tr class="even">
         <td>inbound.hl7.AutoAck</td>
         <td>Whether or not an auto acknowledgement should be sent on message receipt. If set to false, you can define the type of HL7 acknowledgement to be sent . For more information, see <a href="#HL7InboundProtocol-MediationProperties">HL7 inbound endpoint mediation level properties</a> .</td>
         <td>true</td>
         <td>true | false</td>
      </tr>
      <tr class="odd">
         <td>inbound.hl7.ValidateMessage</td>
         <td>This enables HL7 message validation.</td>
         <td>true</td>
         <td>true | false</td>
      </tr>
      <tr class="even">
         <td>inbound.hl7.TimeOut</td>
         <td>The timeout interval in milliseconds to trigger a NACK message.</td>
         <td>10000</td>
         <td>[0..9]*</td>
      </tr>
      <tr class="odd">
         <td>inbound.hl7.CharSet</td>
         <td>The character set used for encoding and decoding messages. Some multi-byte character encodings (e.g. UTF-16, UTF-32) may result in byte values equal to the MLLP framing characters or byte values lower than 0x1F, resulting in errors.</td>
         <td>UTF-8</td>
         <td>ISO-8859-1<br />
            UTF-8<br />
            US-ASCII
         </td>
      </tr>
      <tr class="even">
         <td>inbound.hl7.BuildInvalidMessages</td>
         <td>If the <code>             inbound.hl7.ValidateMessage            </code> parameter is set to <code>             false            </code> and the incoming message is invalid, this parameter specifies whether the raw message received through the MLLP transport should be passed onto the mediation layer.</td>
         <td>false</td>
         <td>true | false</td>
      </tr>
      <tr class="odd">
         <td>inbound.hl7.PassthroughInvalidMessages</td>
         <td>If the <code>             inbound.hl7.BuildInvalidMessages            </code> parameter is set to <code>             true            </code> , this parameter notifies the Axis2 HL7 transport sender whether to use the raw message.</td>
         <td>false</td>
         <td>true | false</td>
      </tr>
      <tr class="even">
         <td>inbound.hl7.MessagePreProcessor</td>
         <td>An implementation of the <code>             org.wso2.carbon.inbound.endpoint.protocol.hl7.HL7MessagePreprocessor            </code> interface can be defined here. It provides an extension point to intercept incoming messages before any type of message parsing occurs.</td>
         <td>N/A</td>
         <td>Fully qualified class name.</td>
      </tr>
   </tbody>
</table>

**HL7 inbound endpoint mediation level properties**

Following are the mediation level properties that you can set in the HL7 inbound endpoint:

!!! Note
    The scope of these properties is the `         default        ` scope.


| **Property**                                                                                            | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|---------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `             <property name="HL7_RESULT_MODE" value="ACK|NACK" scope="default"/>            `          | This is use to define the type of HL7 acknowledgement to be sent. If the `             inbound.hl7.AutoAck            ` parameter is set to `             true            ` this property has no effect.                                                                                                                                                                                                                                                                                                             |
| `             <property name="HL7_NACK_MESSAGE" value="<ERROR MESSAGE>" scope="default" />            ` | This is used to define a custom error message to be sent if you have set the property `             HL7_RESULT_MODE            ` as `             NACK            ` .                                                                                                                                                                                                                                                                                                                                                |
| `             <property name="HL7_APPLICATION_ACK" value="true" scope="default"/>            `          | If the `             inbound.hl7.AutoAck            ` parameter is set to `             false            ` and no immediate auto generated ACK is sent back to the client, this property defines whether we should automatically generate the ACK for the request once the mediation flow is complete. If both the `             inbound.hl7.AutoAck            ` parameter and this property are set to `             false            ` , you need to generate an ACK message in the correct format as a response. |

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
            2 - All the clients connected with the same subscriber path, except the one who publishes the frame to the inbound, receives the WebSocket frame.
         </td>
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
         <td>
            <div class="content-wrapper">
               <p>Specifies the content type of the Web Socket frames that are received from the inbound endpoint.</p>
            </div>
         </td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td><code>             ws.shutdown.status.code            </code></td>
         <td>Specifies the status code of the closed web socket frame sent when the inbound endpoint is closed.</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td><code>             ws.shutdown.status.message            </code></td>
         <td>Specifies the status message of the closed web socket frame when the inbound endpoint is closed.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td><code>             ws.pipeline.handler.class            </code></td>
         <td>
            <p>The fully qualified class name of a pipeline handler class that you implemented.</p>
         </td>
         <td>No</td>
      </tr>
   </tbody>
</table>

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
            2 - All the clients connected with the same subscriber path, except the one who publishes the frame to the inbound, receives the WebSocket frame.
         </td>
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
         <td><code>             wss.ssl.key.store.file            </code></td>
         <td>The keystore location where keys are stored.</td>
         <td>Yes</td>
      </tr>
      <tr class="even">
         <td><code>             wss.ssl.key.store.pass            </code></td>
         <td>The password to access the keystore file.</td>
         <td>Yes</td>
      </tr>
      <tr class="odd">
         <td><code>             wss.ssl.trust.store.file            </code></td>
         <td>The truststore location where keys are stored.</td>
         <td>Yes</td>
      </tr>
      <tr class="even">
         <td><code>             wss.ssl.trust.store.pass            </code></td>
         <td>The password to access the truststore file.</td>
         <td>Yes</td>
      </tr>
      <tr class="odd">
         <td><code>             wss.ssl.cert.pass            </code></td>
         <td>The SSL certificate password.</td>
         <td>Yes</td>
      </tr>
      <tr class="even">
         <td><code>             ws.boss.thread.pool.size            </code></td>
         <td>The size of the netty boss pool.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td><code>             ws.worker.thread.pool.size            </code></td>
         <td>The size of the worker thread pool.</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td><code>             ws.subprotocol.handler.class            </code></td>
         <td>Specify one or more custom subprotocol handler classes that are required. Separate each custom subprotocol handler class using a semicolon.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td><code>             ws.default.content.type            </code></td>
         <td>
            <div class="content-wrapper">
               <p>Specifies the content type of the Web Socket frames that are received from the inbound endpoint.</p>
            </div>
         </td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td><code>             ws.shutdown.status.code            </code></td>
         <td>Specifies the s tatus code of the closed web socket frame sent when the inbound endpoint is closed.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td><code>             ws.shutdown.status.message            </code></td>
         <td>Specifies the s tatus message of the closed web socket frame when the inbound endpoint is closed.</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td><code>             ws.pipeline.handler.class            </code></td>
         <td>The fully qualified class name of a pipeline handler class that you implemented.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td><code>             wss.ssl.protocols            </code></td>
         <td>Enables the SSL protocol for the particular WebSocket inbound endpoint. Default value is "TLS". You can change it to a TLS version(s), which is/are enabled with the SSL protocol (i.e., TLSv1,TLSv1.1,TLSv1.2) . E.g., <code>             &lt;parameter name="wss.ssl.protocols"&gt;TLSv1.1,TLSv1.2&lt;/parameter&gt;            </code></td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td><code>             wss.ssl.cipher.suites            </code></td>
         <td>
            <div class="content-wrapper">
               <p>Enables the specified Cipher Suites for the particular WebSocket inbound endpoint. For example,</p>
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
         <td>No</td>
      </tr>
   </tbody>
</table>

## Polling inbound endpoint parameters

The following parameters are common to all polling inbound endpoints:

| Parameter Name | Description                                                                                                                                                                                                                                                                                                                                                       | Required | Possible Values    | Default Value |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|--------------------|---------------|
| interval       | The polling interval for the inbound endpoint to execute each cycle. This value is set in milliseconds.                                                                                                                                                                                                                                                           | No       | A positive integer | \-            |
| coordination   | This parameter only applicable in a cluster environment. In a cluster environment an inbound endpoint will only be executed in worker nodes. If set to `             true            ` in a cluster setup, this will run the inbound only in a single worker node. Once the running worker is down the inbound starts on another available worker in the cluster. | No       | true or false      | true          |

### JMS inbound endpoint parameters

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

The parameters `         interval        ` and
`         coordination        ` are common to all polling inbound
endpoints. For descriptions of these parameters, see [Common polling
inbound endpoint
parameters](Polling-Inbound-Endpoints_119130486.html#PollingInboundEndpoints-CommonPolling)
.

<table>
   <thead>
      <tr class="header">
         <th>
            <p>Parameter Name</p>
         </th>
         <th>
            <p>Description</p>
         </th>
         <th>
            <p>Required</p>
         </th>
         <th>
            <p>Possible Values</p>
         </th>
         <th>
            <p>Default Value</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>
            <p><code>              java.naming.factory.initial             </code></p>
         </td>
         <td>
            <p>The JNDI initial context factory class. The class must implement the <code>              java.naming.spi.InitialContextFactory             </code> interface.</p>
         </td>
         <td>
            <p>Yes</p>
         </td>
         <td>
            <p>A valid class name</p>
         </td>
         <td>
            <p>-</p>
         </td>
      </tr>
      <tr class="even">
         <td>
            <p><code>              java.naming.provider.url             </code></p>
         </td>
         <td>
            <p>The URL of the JNDI provider.</p>
         </td>
         <td>
            <p>Yes</p>
         </td>
         <td>
            <p>A valid URL</p>
         </td>
         <td>
            <p>-</p>
         </td>
      </tr>
      <tr class="odd">
         <td>
            <p><code>              transport.jms.ConnectionFactoryJNDIName             </code></p>
         </td>
         <td>
            <p>The JNDI name of the connection factory.</p>
         </td>
         <td>
            <p>Yes</p>
         </td>
         <td>
            <p>-</p>
         </td>
         <td>
            <p>-</p>
         </td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>sequential</code></pre>
         </td>
         <td>Whether the messages need to be polled and injected sequentially or not.</td>
         <td>Yes</td>
         <td>
            <pre><code>true, false</code></pre>
         </td>
         <td>
            <pre><code>true</code></pre>
         </td>
      </tr>
      <tr class="odd">
         <td>
            <p><code>              transport.jms.ConnectionFactoryType             </code></p>
         </td>
         <td>
            <p>The type of the connection factory.</p>
         </td>
         <td>
            <p>No</p>
         </td>
         <td>
            <p><code>              queue             </code> , <em></em> <code>              topic             </code></p>
         </td>
         <td>
            <p><code>              queue             </code></p>
         </td>
      </tr>
      <tr class="even">
         <td>
            <p><code>              transport.jms.Destination             </code></p>
         </td>
         <td>
            <p>The JNDI name of the destination.</p>
         </td>
         <td>
            <p>No</p>
         </td>
         <td>
            <p>-</p>
         </td>
         <td>
            <p>The defaults value is the service name.</p>
         </td>
      </tr>
      <tr class="odd">
         <td>
            <p><code>              transport.jms.SessionAcknowledgement             </code></p>
         </td>
         <td>
            <p>The JMS session acknowledgment mode.</p>
         </td>
         <td>
            <p>No</p>
         </td>
         <td>
            <p><code>              AUTO_ACKNOWLEDGE             </code> , <code>              CLIENT_ACKNOWLEDGE             </code> , <code>              DUPS_OK_ACKNOWLEDGE             </code> , <code>              SESSION_TRANSACTED             </code></p>
         </td>
         <td>
            <p><code>              AUTO_ACKNOWLEDGE             </code></p>
         </td>
      </tr>
      <tr class="even">
         <td>
            <p><code>              transport.jms.CacheLevel             </code></p>
         </td>
         <td>
            <p>The JMS resource cache level.</p>
         </td>
         <td>
            <p>No</p>
         </td>
         <td>
            <div class="content-wrapper">
               <p><code>               0-               none               , 1-               connection               , 2-               session, 3-consumer              </code></p>
               !!! info
               <p>To subscribe to topics, set the value of <code>               transport.jms.CacheLevel              </code> to <code>               3              </code> .</p>
            </div>
         </td>
         <td>
            <p>0</p>
         </td>
      </tr>
      <tr class="odd">
         <td>
            <p><code>              transport.jms.UserName             </code></p>
         </td>
         <td>
            <p>The JMS connection username.</p>
         </td>
         <td>
            <p>No</p>
         </td>
         <td>
            <p>-</p>
         </td>
         <td>
            <p>-</p>
         </td>
      </tr>
      <tr class="even">
         <td>
            <p><code>              transport.jms.Password             </code></p>
         </td>
         <td>
            <p>The JMS connection password.</p>
         </td>
         <td>
            <p>No</p>
         </td>
         <td>
            <p>-</p>
         </td>
         <td>
            <p>-</p>
         </td>
      </tr>
      <tr class="odd">
         <td>
            <p><code>              transport.jms.JMSSpecVersion             </code></p>
         </td>
         <td>
            <p>The JMS API version.</p>
         </td>
         <td>
            <p>No</p>
         </td>
         <td>
            <p><code>              1.0.2b             </code> , <code>              1.1             </code> , <code>              2.0             </code></p>
         </td>
         <td>
            <p><code>              1.1             </code></p>
         </td>
      </tr>
      <tr class="even">
         <td><code>             transport.jms.SubscriptionDurable            </code></td>
         <td>Whether the connection factory is subscription durable or not.</td>
         <td>No</td>
         <td><code>             true            </code> , <code>             false            </code></td>
         <td><code>             false            </code></td>
      </tr>
      <tr class="odd">
         <td><code>             transport.jms.DurableSubscriberClientID            </code></td>
         <td>The <code>             ClientId            </code> parameter when using durable subscriptions.</td>
         <td>Required if the value specified as <code>             transport.jms.SubscriptionDurable            </code> is <code>             true            </code> .</td>
         <td>-</td>
         <td>-</td>
      </tr>
      <tr class="even">
         <td>
            <p><code>              transport.jms.DurableSubscriberName             </code></p>
         </td>
         <td>
            <p>The name of the durable subscriber.</p>
         </td>
         <td>
            <p>Required if the value specified as <code>              transport.jms.SubscriptionDurable             </code> is <code>              true             </code> .</p>
         </td>
         <td>
            <p>-</p>
         </td>
         <td>
            <p>-</p>
         </td>
      </tr>
      <tr class="odd">
         <td>
            <p><code>              transport.jms.MessageSelector             </code></p>
         </td>
         <td>
            <p>Message selector implementation.</p>
         </td>
         <td>
            <p>No</p>
         </td>
         <td>
            <p>-</p>
         </td>
         <td>
            <p><br /></p>
         </td>
      </tr>
      <tr class="even">
         <td>
            <p><code>              transport.jms.ReceiveTimeout             </code></p>
         </td>
         <td>The time to wait for a JMS message during polling.<br />
            Set this parameter value to a negative integer to wait indefinitely. Set it to zero to prevent waiting.
         </td>
         <td>No</td>
         <td>The number of milliseconds to wait.</td>
         <td><code>             1            </code></td>
      </tr>
      <tr class="odd">
         <td><code>             transport.jms.ContentType            </code></td>
         <td>How the inbound listener should determine the content type of received messages. Priority is always given to the JMS message type.</td>
         <td>No</td>
         <td>A simple string value, in which case the transport listener assumes that the received messages always have the specified content type, or a set of rules. For more information, see <a href="http://axis.apache.org/axis2/java/transports/jms.html#Service_configuration">http://axis.apache.org/axis2/java/transports/jms.html#Service_configuration</a> .</td>
         <td>-</td>
      </tr>
      <tr class="even">
         <td><code>             transport.jms.ContentTypeProperty            </code></td>
         <td>Get the content type from message property.</td>
         <td>No</td>
         <td><code>             contentType            </code></td>
         <td>-</td>
      </tr>
      <tr class="odd">
         <td><code>             transport.jms.ReplyDestination            </code></td>
         <td>The destination that the response generated by the back-end service is stored.</td>
         <td>No</td>
         <td>-</td>
         <td>ReplyTo from the message</td>
      </tr>
      <tr class="even">
         <td>
            <p><code>              transport.jms.PubSubNoLocal             </code></p>
         </td>
         <td>
            <p>Whether messages should be published via the same connection that they were received.</p>
         </td>
         <td>
            <p>No</p>
         </td>
         <td>
            <p><code>              true, false             </code></p>
         </td>
         <td>
            <p>false</p>
         </td>
      </tr>
      <tr class="odd">
         <td>
            <p><code>              transport.jms.SharedSubscription             </code></p>
         </td>
         <td>
            <p>If set to true, messages will be forwarded to only one of the consumers and consumers will share the messages that are published to the topic.</p>
         </td>
         <td>
            <p>No</p>
         </td>
         <td>
            <p><code>              true, false                           </code></p>
         </td>
         <td>
            <p>false</p>
         </td>
      </tr>
      <tr class="even">
         <td>
            <p><code>              pinnedServers             </code></p>
         </td>
         <td>
            <p>List of synapse server names separated by commas or spaces where this inbound endpoint should be deployed. If there is no pinned server list, the inbound endpoint configuration will be deployed in all server instances.</p>
         </td>
         <td>
            <p>No</p>
         </td>
         <td>
            <p>List of valid synapse server names</p>
         </td>
         <td>
            <p>-</p>
         </td>
      </tr>
      <tr class="odd">
         <td><code>             transport.jms.ConcurrentConsumers            </code></td>
         <td>Number of concurrent threads to be started to consume messages when polling.</td>
         <td>No</td>
         <td>Any positive integer.<br />
            For topics this must always be 1.
         </td>
         <td>1</td>
      </tr>
      <tr class="even">
         <td>
            <p><code>              transport.jms.retry.duration             </code></p>
         </td>
         <td>
            <p>The retry interval to reconnect to the JMS server.</p>
         </td>
         <td>
            <p>No</p>
         </td>
         <td>Retry interval in miliseconds.</td>
         <td>
            <p>-</p>
         </td>
      </tr>
      <tr class="odd">
         <td><code>             transport.jms.RetriesBeforeSuspension            </code></td>
         <td>The number of consecutive mediation failures after which polling should be suspended.</td>
         <td>No</td>
         <td>Specify any positive numerical value based on your requirement. Polling will be suspended when the mediation failure count reaches the specified value.</td>
         <td>-</td>
      </tr>
      <tr class="even">
         <td><code>             transport.jms.PollingSuspensionPeriod            </code></td>
         <td>The period of time that polling is to be suspended when the <code>             transport.jms.RetriesBeforeSuspension            </code> parameter is set.</td>
         <td>No</td>
         <td>Specify a required time period in milliseconds.</td>
         <td>60000</td>
      </tr>
   </tbody>
</table>

### Kafka inbound endpoint parameters

Following are two high level Kafka configurations that can be used to consume messages in two ways: Using **specific topics** or using a **topic filter**.

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

Kafka inbound endpoint parameters for a high level configuration

<table>
   <thead>
      <tr class="header">
         <th>
            <p>Parameter</p>
            <p><br /></p>
         </th>
         <th>
            <p>Description</p>
            <p><br /></p>
         </th>
         <th>
            <p>Required</p>
            <p><br /></p>
         </th>
         <th>Possible Values</th>
         <th>Default Value</th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>
            <pre><code>zookeeper.connect</code></pre>
         </td>
         <td>The host and port of a ZooKeeper server ( <code>             hostname:port            </code> ).</td>
         <td>Yes</td>
         <td>localhost:2181</td>
         <td><br /></td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>consumer.type</code></pre>
         </td>
         <td>The consumer configuration type. This can either be <code>             simple            </code> or <code>             highlevel            </code> .</td>
         <td>Yes</td>
         <td>highlevel, simple</td>
         <td><br /></td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>interval</code></pre>
         </td>
         <td>The polling interval for the inbound endpoint to poll the messages.</td>
         <td>Yes</td>
         <td><br /></td>
         <td><br /></td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>coordination</code></pre>
         </td>
         <td>If set to <code>             true            </code> in a cluster setup, this will run the inbound only in a single worker node.</td>
         <td>Yes</td>
         <td>true, false</td>
         <td>true</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>sequential</code></pre>
         </td>
         <td>The behaviour when executing the given sequence.</td>
         <td>Yes</td>
         <td>true, false</td>
         <td>true</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>topics</code></pre>
         </td>
         <td>The category to feed the messages. A high level kafka configuration can have more than one topic. You can specify multiple topic names as comma separated values.</td>
         <td>Yes</td>
         <td><br /></td>
         <td><br /></td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>content.type</code></pre>
         </td>
         <td>The content of the message.</td>
         <td>Yes</td>
         <td>appllication/xml, application/json</td>
         <td><br /></td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>group.id</code></pre>
         </td>
         <td>
            <p>If all the consumer instances have the same consumer group, this works as a traditional queue balancing the load over the consumers.</p>
            <p>If all the consumer instances have different consumer groups, this works as publish-subscribe and all messages are broadcast to all consumers.</p>
         </td>
         <td>Yes</td>
         <td><br /></td>
         <td><br /></td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>thread.count</code></pre>
         </td>
         <td>The number of threads.</td>
         <td>No</td>
         <td><br /></td>
         <td>1</td>
      </tr>
      <tr class="even">
         <td><code>             consumer.id            </code></td>
         <td>The id of the consumer.</td>
         <td>No</td>
         <td><br /></td>
         <td>null</td>
      </tr>
      <tr class="odd">
         <td><code>             socket.timeout.ms            </code></td>
         <td>The socket timeout for network requests.</td>
         <td>No</td>
         <td><br /></td>
         <td>30 * 1000</td>
      </tr>
      <tr class="even">
         <td><code>             socket.receive.buffer.bytes            </code></td>
         <td>The socket receive buffer for network requests.</td>
         <td>No</td>
         <td><br /></td>
         <td>64 * 1024</td>
      </tr>
      <tr class="odd">
         <td><code>             fetch.message.max.bytes            </code></td>
         <td>The number of byes of messages to attempt to fetch for each topic-partition in each fetch request.</td>
         <td>No</td>
         <td><br /></td>
         <td>1024 * 1024</td>
      </tr>
      <tr class="even">
         <td><code>             num.consumer.fetchers            </code></td>
         <td>The number fetcher threads used to fetch data.</td>
         <td>No</td>
         <td><br /></td>
         <td>1</td>
      </tr>
      <tr class="odd">
         <td><code>             auto.commit.enable            </code></td>
         <td>The committed offset to be used as the position from which the new consumer will begin when the process fails.</td>
         <td>No</td>
         <td>true, false</td>
         <td>true</td>
      </tr>
      <tr class="even">
         <td><code>             auto.commit.interval.ms            </code></td>
         <td>The frequency in ms that the consumer offsets are committed to zookeeper.</td>
         <td>No</td>
         <td><br /></td>
         <td>60 * 1000</td>
      </tr>
      <tr class="odd">
         <td><code>             queued.max.message.chunks            </code></td>
         <td>The maximum number of message chunks buffered for consumption. Each chunk can go up to the value specified in <code>             fetch.message.max.bytes            </code> .</td>
         <td>No</td>
         <td><br /></td>
         <td>2</td>
      </tr>
      <tr class="even">
         <td><code>             rebalance.max.retries            </code></td>
         <td>The maximum number of retry attempts.</td>
         <td>No</td>
         <td><br /></td>
         <td>4</td>
      </tr>
      <tr class="odd">
         <td><code>             fetch.min.bytes            </code></td>
         <td>The minimum amount of data the server should return for a fetch request.</td>
         <td>No</td>
         <td><br /></td>
         <td>1</td>
      </tr>
      <tr class="even">
         <td><code>             fetch.wait.max.ms            </code></td>
         <td>The maximum amount of time the server will block before responding to the fetch request when sufficient data is not available to immediately serve <code>             fetch.min.bytes            </code> .</td>
         <td>No</td>
         <td><br /></td>
         <td>100</td>
      </tr>
      <tr class="odd">
         <td><code>             rebalance.backoff.ms            </code></td>
         <td>The backoff time between retries during rebalance.</td>
         <td>No</td>
         <td><br /></td>
         <td>2000</td>
      </tr>
      <tr class="even">
         <td><code>             refresh.leader.backoff.ms            </code></td>
         <td>The backoff time to wait before trying to determine the leader of a partition that has just lost its leader.</td>
         <td>No</td>
         <td><br /></td>
         <td>200</td>
      </tr>
      <tr class="odd">
         <td><code>             auto.offset.reset            </code></td>
         <td>
            <p>Set this to one of the following values based on what you need to do when there is no initial offset in ZooKeeper or if an offset is out of range.</p>
            <p>smallest - Automatically reset the offset to the smallest offset.<br />
               largest - Automatically reset the offset to the largest offset.<br />
               anything else - Throw an exception to the consumer.
            </p>
         </td>
         <td>No</td>
         <td>smallest, largest, anything else</td>
         <td>largest</td>
      </tr>
      <tr class="even">
         <td><code>             consumer.timeout.ms            </code></td>
         <td>The timeout interval after which a timeout exception is to be thrown to the consumer if no message is available for consumption. It is a good practice to set this value lower than the interval of the Kafka inbound endpoint.</td>
         <td>No</td>
         <td><br /></td>
         <td>3000</td>
      </tr>
      <tr class="odd">
         <td><code>             exclude.internal.topics            </code></td>
         <td>Set to <code>             true            </code> if messages from internal topics such as offsets should be exposed to the consumer.</td>
         <td>No</td>
         <td>true, false</td>
         <td>true</td>
      </tr>
      <tr class="even">
         <td><code>             partition.assignment.strategy            </code></td>
         <td>The partitions assignment strategy to be used when assigning partitions to consumer streams.</td>
         <td>No</td>
         <td>range, roundrobin</td>
         <td>range</td>
      </tr>
      <tr class="odd">
         <td><code>             client.id            </code></td>
         <td>The user specified string sent in each request to help trace calls.</td>
         <td>No</td>
         <td><br /></td>
         <td>value of group  id</td>
      </tr>
      <tr class="even">
         <td>
            <p><code>              zookeeper.session.timeout.ms             </code></p>
         </td>
         <td>The ZooKeeper session timeout value in milliseconds.</td>
         <td>No</td>
         <td><br /></td>
         <td>6000</td>
      </tr>
      <tr class="odd">
         <td><code>             zookeeper.connection.timeout.ms            </code></td>
         <td>The maximum time in milliseconds that the client should wait while establishing a connection to ZooKeeper .</td>
         <td>No</td>
         <td><br /></td>
         <td>6000</td>
      </tr>
      <tr class="even">
         <td>
            <p><code>              zookeeper.sync.time.ms             </code></p>
         </td>
         <td>The time difference in milliseconds that a ZooKeeper follower can be behind a ZooKeeper leader.</td>
         <td>No</td>
         <td><br /></td>
         <td>2000</td>
      </tr>
      <tr class="odd">
         <td><code>             offsets.storage            </code></td>
         <td>The offsets storage location.</td>
         <td>No</td>
         <td>zookeeper, kafka</td>
         <td>zookeeper</td>
      </tr>
      <tr class="even">
         <td><code>             offsets.channel.backoff.ms            </code></td>
         <td>The backoff period in milliseconds when reconnecting the offsets channel or retrying failed offset fetch/commit requests.</td>
         <td>No</td>
         <td><br /></td>
         <td>1000</td>
      </tr>
      <tr class="odd">
         <td><code>             offsets.channel.socket.timeout.ms            </code></td>
         <td>The socket timeout in milliseconds when reading responses for offset fetch/commit requests.</td>
         <td>No</td>
         <td><br /></td>
         <td>10000</td>
      </tr>
      <tr class="even">
         <td><code>             offsets.commit.max.retries            </code></td>
         <td>The maximum retry attempts allowed. If a consumer metadata request fails for any reason, retry takes place but does not have an impact on this limit.</td>
         <td>No</td>
         <td><br /></td>
         <td>5</td>
      </tr>
      <tr class="odd">
         <td><code>             dual.commit.enabled            </code></td>
         <td>If <code>             offsets.storage            </code> is set to <code>             kafka            </code> , the commit offsets can be dual to ZooKeeper. Set this to true if you need to perform migration from zookeeper-based offset storage to kafka-based offset storage.</td>
         <td>No</td>
         <td>true, false</td>
         <td>true</td>
      </tr>
   </tbody>
</table>

Following are descriptions of the parameters set to consume topic filters in a kafka configuration:

!!! Info
    In high level kafka configurations, the follwing parameters are used instead of the `         topics        ` paramater.
    <parameter name="topic.filter">test</parameter>
    <parameter name="filter.from.whitelist">true</parameter>

<table>
   <thead>
      <tr class="header">
         <th>
            <p>Parameter</p>
            <p><br /></p>
         </th>
         <th>
            <p>Description</p>
            <p><br /></p>
         </th>
         <th>
            <p>Required</p>
            <p><br /></p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>
            <pre><code>topic.filter</code></pre>
         </td>
         <td>The name of the topic filter.</td>
         <td>Yes</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>filter.from.whitelist</code></pre>
         </td>
         <td>If this is set to <code>             true            </code> , messages are consumed from the whitelist(include).<br />
            If this is set to <code>             false            </code> , messages are consumed from the blacklist(exclude).
         </td>
         <td>Yes</td>
      </tr>
   </tbody>
</table>

Kafka inbound endpoint parameters for a low level configuration:

Following is a sample low level kafka configuration that can be used to
consume messages from a specific server in a specific partition, so that
the messages are limited:

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
      <tr class="header">
         <th>
            <p>Parameter</p>
            <p><br /></p>
         </th>
         <th>
            <p>Description</p>
            <p><br /></p>
         </th>
         <th>
            <p>Required</p>
            <p><br /></p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>
            <pre><code>simple.topic</code></pre>
         </td>
         <td>The category to feed the messages.</td>
         <td>Yes</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>simple.brokers</code></pre>
         </td>
         <td>The specific Kafka broker name.</td>
         <td>Yes</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>simple.port</code></pre>
         </td>
         <td>The specific Kafka server port number.</td>
         <td>Yes</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>simple.partition</code></pre>
         </td>
         <td>The partition of the topic.</td>
         <td>Yes</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>simple.max.messages.to.read</code></pre>
         </td>
         <td>
            <p>The maximum number of messages to retrieve.</p>
         </td>
         <td>Yes</td>
      </tr>
   </tbody>
</table>

## Event-based endpoint parameters

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

The following parameter is common to all event-based inbound endpoints:

<table>
   <thead>
      <tr class="header">
         <th>
            <p>Parameter Name</p>
         </th>
         <th>
            <p>Description</p>
         </th>
         <th>
            <p>Required</p>
         </th>
         <th>
            <p>Possible Values</p>
         </th>
         <th>
            <p>Default Value</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>coordination</td>
         <td>This parameter is only applicable in a cluster environment.<br />
            In a cluster environment an inbound endpoint will only be executed in worker nodes. If this parameter is set to <code>             true            </code> in a cluster environment, the inbound will only be executed in a single worker node. Once the running worker node is down the inbound will start on another available worker node in the cluster.
         </td>
         <td>No</td>
         <td>true or false</td>
         <td>true</td>
      </tr>
   </tbody>
</table>

### MQTT Inbound Protocol parameters

<table>
<colgroup>
   <col style="width: 33%" />
   <col style="width: 33%" />
   <col style="width: 33%" />
</colgroup>
<thead>
   <tr class="header">
      <th>
         <p>Parameter</p>
      </th>
      <th>
         <p>Description</p>
      </th>
      <th>
         <p>Required</p>
      </th>
   </tr>
</thead>
<tbody>
   <tr class="odd">
      <td>
         <pre><code>sequential</code></pre>
      </td>
      <td>The behaviour when executing the given sequence.</td>
      <td>Yes</td>
   </tr>
   <tr class="even">
      <td>
         <pre><code>coordination</code></pre>
      </td>
      <td>In a cluster setup, if set to true, the inbound endpoint will run only in a single worker node. If set to false,  it will run on all worker nodes.</td>
      <td>Yes</td>
   </tr>
   <tr class="odd">
      <td>
         <pre><code>suspend</code></pre>
      </td>
      <td>If set to true, the inbound listener will not accept incoming request messages from clients and will not inject messages to the synapse message mediation. If set to false, incoming messages will be accepted.</td>
      <td>Yes</td>
   </tr>
   <tr class="even">
      <td>
         <pre><code>mqtt.connection.factory</code></pre>
      </td>
      <td>Name of the connection factory.</td>
      <td>Yes</td>
   </tr>
   <tr class="odd">
      <td>
         <pre><code>mqtt.server.host.name</code></pre>
      </td>
      <td>
         <p>Address of the message broker (eg., localhost).</p>
      </td>
      <td>Yes</td>
   </tr>
   <tr class="even">
      <td>
         <pre><code>mqtt.server.port</code></pre>
      </td>
      <td>Port of the message broker (e.g., 1883).</td>
      <td>Yes</td>
   </tr>
   <tr class="odd">
      <td>
         <pre><code>mqtt.topic.name</code></pre>
      </td>
      <td>MQTT topic to which the message should be published.</td>
      <td>Yes</td>
   </tr>
   <tr class="even">
      <td>
         <pre><code>mqtt.subscription.qos</code></pre>
      </td>
      <td>The quality of service level that need to be maintained with the subscription. The quality of service level can be either 0,1 or 2.<br />
         0 -  Specifying this level ensures that the message delivery is efficient. However, specifying this level does not guarantee that the message will be delivered to its recipient.<br />
         1 -  Specifying this level ensures that the message is delivered at least once, but this can lead to messages being duplicated.<br />
         2 - This is the highest level of quality of service. Specifying this guarantees that the message is delivered and that it is delivered only once.
      </td>
      <td>No</td>
   </tr>
   <tr class="odd">
      <td>
         <pre><code>mqtt.client.id</code></pre>
      </td>
      <td>The id of the client.</td>
      <td>No</td>
   </tr>
   <tr class="even">
      <td>
         <pre><code>content.type</code></pre>
      </td>
      <td>The content type of the message. (i.e., XML or JSON)</td>
      <td>Yes</td>
   </tr>
   <tr class="odd">
      <td>
         <pre><code>mqtt.session.clean</code></pre>
      </td>
      <td>
         <p>Whether the client and server should remember the state across restarts and reconnects.</p>
      </td>
      <td>No</td>
   </tr>
   <tr class="even">
      <td>
         <pre><code>mqtt.ssl.enable</code></pre>
      </td>
      <td>Whether to use tcp connection or ssl connection.</td>
      <td>No</td>
   </tr>
   <tr class="odd">
      <td>
         <pre><code>mqtt.subscription.username</code></pre>
      </td>
      <td>The username for the subscription.</td>
      <td>No</td>
   </tr>
   <tr class="even">
      <td>
         <pre><code>mqtt.subscription.password</code></pre>
      </td>
      <td>The password for the subscription.</td>
      <td>No</td>
   </tr>
   <tr class="odd">
      <td>
         <pre><code>mqtt.temporary.store.directory</code></pre>
      </td>
      <td>
         <p>Path of the directory to be used as the persistent data store for quality of service purposes.</p>
      </td>
      <td>No</td>
   </tr>
   <tr class="even">
      <td>
         <pre><code>mqtt.reconnection.interval</code></pre>
      </td>
      <td>
         <p>The retry interval to reconnect to the MQTT server.</p>
      </td>
      <td>No</td>
   </tr>
</tbody>
</table>

### RabbitMQ Inbound Protocol parameters

<table>
   <thead>
      <tr class="header">
         <th>
            <p>Parameter Name</p>
            <p><br /></p>
         </th>
         <th>
            <p>Description</p>
            <p><br /></p>
         </th>
         <th>
            <p>Required</p>
            <p><br /></p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>
            <pre><code>sequential</code></pre>
         </td>
         <td>The behavior when executing the given sequence.</td>
         <td>Yes</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.connection.factory</code></pre>
         </td>
         <td>Name of the connection factory.</td>
         <td>Yes</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>rabbitmq.server.host.name</code></pre>
         </td>
         <td>
            <p>Host name of the server.</p>
         </td>
         <td>Yes</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.server.port</code></pre>
         </td>
         <td>The port number of the server.</td>
         <td>Yes</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>rabbitmq.server.user.name</code></pre>
         </td>
         <td>The user name to connect to the server.</td>
         <td>Yes</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.server.password</code></pre>
         </td>
         <td>The password to connect to the server.</td>
         <td>Yes</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>rabbitmq.queue.name</code></pre>
         </td>
         <td>The queue name to send or consume messages.</td>
         <td>Yes</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.exchange.name</code></pre>
         </td>
         <td>The name of the RabbitMQ exchange to which the queue is bound.</td>
         <td>Yes</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>rabbitmq.server.virtual.host</code></pre>
         </td>
         <td>The virtual host name of the server, if available.</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.connection.ssl.enabled</code></pre>
         </td>
         <td>Whether to use TCP connection or SSL connection.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>rabbitmq.connection.ssl.keystore.location</code></pre>
         </td>
         <td>The keystore location.</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.connection.ssl.keystore.type</code></pre>
         </td>
         <td>
            <p>The keystore type.</p>
         </td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>rabbitmq.connection.ssl.keystore.password</code></pre>
         </td>
         <td>The keystore password.</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.connection.ssl.truststore.location</code></pre>
         </td>
         <td>The truststore location.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>rabbitmq.connection.ssl.truststore.type</code></pre>
         </td>
         <td>The truststore type.</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.connection.ssl.truststore.password</code></pre>
         </td>
         <td>The truststore password.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>rabbitmq.connection.ssl.version</code></pre>
         </td>
         <td>The SSL version.</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.queue.durable</code></pre>
         </td>
         <td>Whether the queue should remain declared even if the broker restarts.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>rabbitmq.queue.exclusive</code></pre>
         </td>
         <td>Whether the queue should be exclusive or should be consumable by other connections.</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.queue.auto.delete</code></pre>
         </td>
         <td>Whether to keep the queue even if it is not being consumed anymore.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>rabbitmq.queue.auto.ack</code></pre>
         </td>
         <td>Whether to send back an acknowledgment.</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.queue.routing.key</code></pre>
         </td>
         <td>The routing key of the queue.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>rabbitmq.queue.delivery.mode</code></pre>
         </td>
         <td>The delivery mode of the queue (ie., Whether it is persistent or not).</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.exchange.type</code></pre>
         </td>
         <td>The type of the exchange.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>rabbitmq.exchange.durable</code></pre>
         </td>
         <td>Whether the exchange should remain declared even if the broker restarts.</td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.exchange.auto.delete</code></pre>
         </td>
         <td>Whether to keep the queue even if it is not used anymore.</td>
         <td>No</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>rabbitmq.factory.heartbeat</code></pre>
         </td>
         <td>
            <p>The period of time after which the connection should be considered dead.</p>
         </td>
         <td>No</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>rabbitmq.message.content.type</code></pre>
         </td>
         <td>The content type of the message.</td>
         <td>No</td>
      </tr>
   </tbody>
</table>

**Connection recovery parameters**

In case of a network failure or broker shutdown, the ESB can try to
recreate the connection.

If you want to enable connection recovery, you should configure the
following parameters in the inbound endpoint:

``` xml
    <parameter name="rabbitmq.connection.retry.interval" locked="false">10000</parameter>
    <parameter name="rabbitmq.connection.retry.count" locked="false">5</parameter>   
```

If the parameters are configured with sample values as given above, the
ESB makes 5 retry attempts with a time interval of 10000 ms between each
retry attempt to reconnect when the connection is lost. If reconnecting
fails after 5 retry attempts, the ESB terminates the connection.

In addition to the above parameters, if you want to set the interval
between retry attempts with which the RabbitMQ client should try
reconnecting to the ESB, you can configure the following parameter:

``` xml
    <parameter name="rabbitmq.server.retry.interval" locked="false">10000</parameter>
```

Setting the value of `         rabbitmq.server.retry.interval        `
to be less than the value of
`         rabbitmq.connection.retry.interval        ` helps synchronize
the reconnection of the ESB and the RabbitMQ client.

## Custom Inbound Endpoint parameters

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

### Custom listening inbound endpoint parameters

<table>
   <thead>
      <tr class="header">
         <th>
            <p>Parameter Name</p>
         </th>
         <th>
            <p>Description</p>
         </th>
         <th>
            <p>Required</p>
         </th>
         <th>
            <p>Possible Values</p>
         </th>
         <th>
            <p>Default Value</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>
            <pre><code>class</code></pre>
         </td>
         <td>
            <p>Name of the custom class implementation</p>
         </td>
         <td>
            <p>Yes</p>
         </td>
         <td>
            <p>A valid class name</p>
         </td>
         <td>
            <p>n/a</p>
         </td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>sequence</code></pre>
            <p><br /></p>
         </td>
         <td>Name of the sequence message that should be injected</td>
         <td>Yes</td>
         <td>A valid sequence name</td>
         <td>n/a</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>onError</code></pre>
            <p><br /></p>
         </td>
         <td>Name of the fault sequence that should be invoked in case of failure</td>
         <td>Yes</td>
         <td>
            <p>A valid fault sequence name</p>
         </td>
         <td>n/a</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>inbound.behavior</code></pre>
         </td>
         <td>
            <p>The behaviour of the inbound endpoint</p>
         </td>
         <td>Yes</td>
         <td><code>             listening            </code></td>
         <td>n/a</td>
      </tr>
   </tbody>
</table>

### Custom polling inbound endpoint parameters

<table>
   <colgroup>
      <col style="width: 20%" />
      <col style="width: 20%" />
      <col style="width: 20%" />
      <col style="width: 20%" />
      <col style="width: 20%" />
   </colgroup>
   <thead>
      <tr class="header">
         <th>
            <p>Parameter Name</p>
         </th>
         <th>
            <p>Description</p>
         </th>
         <th>
            <p>Required</p>
         </th>
         <th>
            <p>Possible Values</p>
         </th>
         <th>
            <p>Default Value</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>
            <pre><code>class</code></pre>
         </td>
         <td>
            <p>Name of the custom class implementation</p>
         </td>
         <td>
            <p>Yes</p>
         </td>
         <td>
            <p>A valid class name</p>
         </td>
         <td>
            <p>n/a</p>
         </td>
      </tr>
   </tbody>
</table>

### Custom event-based inbound endpoint parameters

<table>
   <thead>
      <tr class="header">
         <th>
            <p>Parameter Name</p>
         </th>
         <th>
            <p>Description</p>
         </th>
         <th>
            <p>Required</p>
         </th>
         <th>
            <p>Possible Values</p>
         </th>
         <th>
            <p>Default Value</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>
            <pre><code>class</code></pre>
         </td>
         <td>
            <p>Name of the custom class implementation</p>
         </td>
         <td>
            <p>Yes</p>
         </td>
         <td>
            <p>A valid class name</p>
         </td>
         <td>
            <p>n/a</p>
         </td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>sequence</code></pre>
            <p><br /></p>
         </td>
         <td>Name of the sequence message that should be injected</td>
         <td>Yes</td>
         <td>A valid sequence name</td>
         <td>n/a</td>
      </tr>
      <tr class="odd">
         <td>
            <pre><code>onError</code></pre>
            <p><br /></p>
         </td>
         <td>Name of the fault sequence that should be invoked in case of failure</td>
         <td>Yes</td>
         <td>
            <p>A valid fault sequence name</p>
         </td>
         <td>n/a</td>
      </tr>
      <tr class="even">
         <td>
            <pre><code>inbound.behavior</code></pre>
         </td>
         <td>
            <p>The behaviour of the inbound endpoint</p>
         </td>
         <td>Yes</td>
         <td><code>             event-based            </code></td>
         <td>n/a</td>
      </tr>
   </tbody>
</table>