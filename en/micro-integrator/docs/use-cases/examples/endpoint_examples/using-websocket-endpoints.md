# WebSocket Endpoint

WebSocket is a protocol that provides full-duplex communication channels over a single TCP connection, and can be used by any client or server application. The ESB Profile of WSO2 Enterprise Integrator (WSO2 EI) provides WebSocket support via the [WebSocket Transport](https://docs.wso2.com/display/EI650/WebSocket+Transport), [WebSocket Inbound Protocol](https://docs.wso2.com/display/EI650/WebSocket+Inbound+Protocol), and [Secure WebSocket Inbound Protocol](https://docs.wso2.com/display/EI650/Secure+WebSocket+Inbound+Protocol).

## Example 1: Sending a Message from a WebSocket Client to a WebSocket Endpoint

If you need to send a message from a WebSocket client to a WebSocket
endpoint via the ESB Profile of WSO2 EI, you need to establish a
persistent WebSocket connection from the WebSocket client to the ESB
Profile of WSO2 EI as well as from the ESB Profile of WSO2 EI to the
WebSocket back-end.

To demonstrate this scenario, you need to create two dispatching
sequences. One for the client to back-end mediation, and another for the
back-end to client mediation. Finally you need to configure the
WebSocket inbound endpoint of the ESB Profile of WSO2 EI to use the
created sequences and listen on port 9091.

### Synapse configurations

Create the sequence for client to back-end mediation, the sequence for back-end to client mediation, the WebSocket inbound endpoint in WSO2 Micro Integrator to use the created sequences and listen on port 9091.

``` java tab='Sequence (Backend Mediation)'
<sequence name="dispatchSeq">
    <property name="OUT_ONLY" value="true"/>
        <send>
            <endpoint>
                <address uri="ws://localhost:8082/websocket"/>
            </endpoint>
        </send>
</sequence>
```

``` java tab='Sequence (Backend to Client Mediation)'
<sequence name="outDispatchSeq" xmlns="http://ws.apache.org/ns/synapse">
    <log level="full"/>
    <respond/>
</sequence>
```

``` java tab='Inbound Endpoint'
<inboundEndpoint name="test" onError="fault" protocol="ws" sequence="dispatchSeq" suspend="false" xmlns="http://ws.apache.org/ns/synapse">
    <parameters>
        <parameter name="inbound.ws.port">9091</parameter>
        <parameter name="ws.outflow.dispatch.sequence">outDispatchSeq</parameter>
        <parameter name="ws.client.side.broadcast.level">0</parameter>
        <parameter name="ws.outflow.dispatch.fault.sequence">fault</parameter>
    </parameters>
</inboundEndpoint>
```

### Run the Example

1.  Start the ESB Profile of WSO2 EI. For information on how to start
    the ESB Profile of WSO2 EI, see [Running the
    Product](https://docs.wso2.com/display/EI650/Running+the+Product) .
2.  Download the [sample netty
    artifacts](https://github.com/wso2-docs/ESB/blob/master/ESB-Artifacts/Netty_artifacts_for_WebSocket_samples/netty-example-4.0.30.Final.jar).
3.  Execute the following command to start the WebSocket client:

    ```
    java -DclientPort=9091 -cp netty-example-4.0.30.Final.jar:lib/*:. io.netty.example.http.websocketx.client.WebSocketClient
    ```
If you analyze the log, you will see that a connection from the
WebSocket client to the ESB Profile of WSO2 EI is established, and the
sequences are executed by the WebSocket inbound endpoint. You will also
see that the message sent to the WebSocket server is not transformed,
and that the response injected to the out sequence is also not
transformed.

## Example 2: Sending a Message from a HTTP Client to a WebSocket Endpoint

If you need to send a message from a HTTP client to a WebSocket endpoint
via the ESB Profile of WSO2 Enterprise Integrator, you need to establish
a persistent Websocket connection from the ESB Profile of WSO2 EI to the
WebSocket back-end.

To demonstrate this scenario, you need to create two dispatching
sequences. One for the client to back-end mediation, and another for the
back-end to client mediation. Then you need to create a proxy service to
call the created sequences.

### Synapse configuration

Create the sequence for client to back-end mediation, sequence for the back-end to client mediation, and a proxy service as follows to call the sequences.

``` java tab='Sequence (Backend Mediation)'
<sequence name="dispatchSeq" xmlns="http://ws.apache.org/ns/synapse">
    <in>
        <property name="OUT_ONLY" value="true"/>
        <property name="FORCE_SC_ACCEPTED" scope="axis2" type="STRING" value="true"/>
        <property name="websocket.accept.contenType" scope="axis2" value="application/json"/>
             <send>
                <endpoint>
                     <address uri="ws://localhost:8082/websocket"/>
                </endpoint>
            </send>
     </in>
</sequence>
```

``` java tab='Sequence (Backend to Client Mediation)'
<sequence name="outDispatchSeq" xmlns="http://ws.apache.org/ns/synapse">
   <log level="full"/>
</sequence>
```

``` java tab='Sequence (Proxy Service)'
<proxy xmlns="http://ws.apache.org/ns/synapse"
                   name="websocketProxy1"
                   transports="http,https"
                   statistics="disable"
                   trace="disable"
                   startOnLoad="true">
    <target inSequence="dispatchSeq" faultSequence="outDispatchSeq"/>
    <description/>
</proxy>
```

### Run the Example

1.  Start the ESB Profile of WSO2 EI. For information on how to start
    the ESB Profile of WSO2 EI, see [Running the
    Product](https://docs.wso2.com/display/EI650/Running+the+Product) .
2.  Download the [sample netty
    artifacts](https://github.com/wso2-docs/ESB/blob/master/ESB-Artifacts/Netty_artifacts_for_WebSocket_samples/netty-example-4.0.30.Final.jar).
3.  Execute the following command to start the WebSocket server on port
    8082:

    ``` java
    java -cp netty-example-4.0.30.Final.jar:lib/*:. io.netty.example.http.websocketx.server.WebSocketServer
    ```
4.  Execute the following command to invoke the proxy service:

    ``` java
    curl -v --request POST -d "<?xml version=\"1.0\" encoding=\"UTF-8\"?><soapenv:Envelope xmlns:soapenv=\"http://schemas.xmlsoap.org/soap/envelope/\"><soapenv:Body><test>Value</test></soapenv:Body></soapenv:Envelope>" -H Content-Type:"text/xml" http://localhost:8280/services/websocketProxy1
    ```
If you analyze the log, you will see that a HTTP request is sent to the
WebSocket server, and that the WebSocket server injects the response to
the out sequence.
