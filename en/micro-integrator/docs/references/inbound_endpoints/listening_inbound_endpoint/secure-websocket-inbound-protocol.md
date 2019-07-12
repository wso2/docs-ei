# Secure WebSocket Inbound Protocol

The secure WebSocket inbound protocol implementation is based on the
[WebSocket protocol](http://tools.ietf.org/html/rfc6455) , and allows
full-duplex, secure message mediation.

Following is a sample secure WebSocket inbound endpoint configuration:

``` xml
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

## WebSocket inbound endpoint parameters

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
<td><div class="content-wrapper">
<p>Specifies the content type of the Web Socket frames that are received from the inbound endpoint.</p>
</div></td>
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
<td><div class="content-wrapper">
<p>Enables the specified Cipher Suites for the particular WebSocket inbound endpoint. For example,</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;parameter name=<span class="st">&quot;wss.ssl.cipher.suites&quot;</span>&gt;</span>
<span id="cb1-2"><a href="#cb1-2"></a>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256,</span>
<span id="cb1-3"><a href="#cb1-3"></a>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,</span>
<span id="cb1-4"><a href="#cb1-4"></a>TLS_DHE_RSA_WITH_AES_128_CBC_SHA256,</span>
<span id="cb1-5"><a href="#cb1-5"></a>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA,</span>
<span id="cb1-6"><a href="#cb1-6"></a>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,</span>
<span id="cb1-7"><a href="#cb1-7"></a>TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span>
<span id="cb1-8"><a href="#cb1-8"></a>&lt;/parameter&gt;</span></code></pre></div>
</div>
</div>
</div></td>
<td>No</td>
</tr>
</tbody>
</table>

## WebSocket to WebSocket Integration using Subprotocols

If you need to read and transform the content of WebSocket frames, the
information in incoming WebSocket frames is not sufficient because the
WebSocket protocol does not specify any information about the
content-type of frames that flow through WebSocket channels. Hence, the
ESB Profile of WSO2 Enterprise Integrator (WSO2 EI) supports a WebSocket
subprotocol extension to determine the content type of WebSocket frames.

The [WebSocket inbound endpoint](_WebSocket_Inbound_Protocol_) of the
ESB Profile supports the following Synapse subprotocols by default:

-   `          synapse(contentType='application/json')         `
-   `          synapse(contentType='application/xml')         `
-   `          synapse(contentType='text/xml')         `

Now let's look at a sample scenario that demonstrates WebSocket to
WebSocket integration using subprotocols to support content handling.

This scenario includes the following sections:

-   [Introduction](#WebSockettoWebSocketIntegrationusingSubprotocols-Introduction)
-   [Prerequisites](#WebSockettoWebSocketIntegrationusingSubprotocols-Prerequisites)
-   [Configuring the sample
    scenario](#WebSockettoWebSocketIntegrationusingSubprotocols-Configuringthesamplescenario)
-   [Executing the sample
    scenario](#WebSockettoWebSocketIntegrationusingSubprotocols-Executingthesamplescenario)
-   [Analyzing the
    output](#WebSockettoWebSocketIntegrationusingSubprotocols-Analyzingtheoutput)

### Introduction

Let's say you need to send messages between two WebSocket based systems
using the ESB Profile of WSO2 EI as a WebSocket gateway that facilitates
the messaging. Let's also assume that you need to read and transform the
content of WebSocket frames that are sent and received.

The following should take place in this scenario:

-   The WebSocket Client sends WebSocket frames to the ESB Profile of
    WSO2 EI.
-   When the initial handshake happens between the WebSocket client and
    the WebSocket inbound endpoint of the ESB Profile, the WebSocket
    client sends a `          Sec-WebSockets-Protocol         ` header
    that specifies the content type of the WebSocket frame. In this
    sample it is
    `          synapse(contentType='application/json')         ` .
-   The WebSocket inbound endpoint of the ESB Profile determines the
    content-type of the incoming WebSocket frame using the subprotocol.
-   Once the handshake is complete, the WebSocket inbound endpoint
    builds all the subsequent WebSocket frames based on the content-type
    specified during the initial handshake.
-   The ESB Profile of WSO2 EI sends the transformed message in the form
    of WebSocket frames.

!!! tip

If necessary, you can use the [data
mapper](https://docs.wso2.com/display/ESB500/Data+Mapper+Mediator) to
perform data transformation inside the ESB message flow. For example,
you can perform JSON to JSON transformation. To do this, you have to
explicitly apply the required data mapping logic for all WebSocket
frames.


### Prerequisites

-   Start the ESB Profile of WSO2 EI. For information on how to start
    the ESB Profile of WSO2 Enterprise Integrator, see [Running the
    Product](https://docs.wso2.com/display/ESB500/Running+the+Product) .
-   Download the [sample netty
    artifacts](https://github.com/wso2-docs/ESB/tree/master/ESB-Artifacts/Netty_artifacts_for_WebSocket_samples)
    .
-   Open a terminal, navigate to the location where you saved the netty
    artifacts, and execute the following command to start the WebSocket
    server on port 8082:

    ``` java
        java -cp netty-example-4.0.30.Final.jar:lib/*:. io.netty.example.http.websocketx.server.WebSocketServer
    ```

### Configuring the sample scenario

-   Create the sequence for client to back-end mediation as follows:

    ``` xml
            <sequence name="dispatchSeq" xmlns="http://ws.apache.org/ns/synapse">
                <property name="OUT_ONLY" value="true"/>
                <property name="websocket.accept.contenType" scope="axis2" value="application/json"/>
                <log level="full">
                    <property name="LOGGED_MESSAGE" value="LOGGED"/>
                </log>
                <send>
                    <endpoint>
                        <address uri="ws://localhost:8082/websocket"/>
                    </endpoint>
                </send>
            </sequence>
    ```

        !!! tip
    
        Specify the `           websocket.accept.contenType          `
        property to inform the WebSocket sender to build the frames with the
        specified content type, and to include the same subprotocol header
        that was used to determine the content of the WebSocket frames. In
        this case it is JSON.
    

<!-- -->

-   Create the sequence for back-end to client mediation as follows:

    ``` xml
        <sequence name="outDispatchSeq" trace="enable" xmlns="http://ws.apache.org/ns/synapse">
            <log level="full"/>
            <respond/>
        </sequence>
    ```

-   Configure the WebSocket inbound endpoint in the ESB Profile of WSO2
    EI as follows to use the created sequences and listen on port 9091:

    ``` xml
            <inboundEndpoint name="test" onError="fault" protocol="ws"
                sequence="dispatchSeq" suspend="false">
                <parameters>
                    <parameter name="inbound.ws.port">9091</parameter>
                    <parameter name="ws.outflow.dispatch.sequence">outDispatchSeq</parameter>
                    <parameter name="ws.client.side.broadcast.level">0</parameter>
                    <parameter name="ws.outflow.dispatch.fault.sequence">fault</parameter>
                </parameters>
            </inboundEndpoint>
    ```

### Executing the sample scenario

-   Open a terminal, navigate to the location where you saved the netty
    artifacts, and execute the following command to start the WebSocket
    client:

    ``` java
            java -DsubProtocol="synapse(contentType='application/json') -DclientPort=9091 -cp netty-example-4.0.30.Final.jar:lib/*:.io.netty.example.http.websocketx.client.WebSocketClient
    ```

    You will see the following message on the client terminal:

    ``` java
            WebSocket Client connected!
    ```

-   Send the following sample JSON payload from the client terminal:

    ``` java
            ("sample message":"test"}
    ```

### Analyzing the output

When you send a sample JSON payload from the client, you will see that a
connection from the WebSocket client to the ESB Profile of WSO2 EI is
established, and that the ESB Profile of WSO2 EI receives the message.
Following is the log that you will see:

![](attachments/119130481/119130482.png){width="957" height="162"}

This shows that the sequences are executed by the WebSocket inbound
endpoint.

You will also see that the message sent to the WebSocket server is
transformed, and that the response injected to the out sequence is also
transformed.

Following is the server log that you will see:

![](attachments/119130481/119130483.png){width="569" height="235"}

Following is the client log that you will see:

![](attachments/119130481/119130485.png){width="568" height="122"}
