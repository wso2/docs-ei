# WebSocket Endpoint

WebSocket is a protocol that provides full-duplex communication channels over a single TCP connection, and can be used by any client or server application. The ESB Profile of WSO2 Enterprise Integrator (WSO2 EI) provides WebSocket support via the [WebSocket Transport](https://docs.wso2.com/display/EI650/WebSocket+Transport), [WebSocket Inbound Protocol](https://docs.wso2.com/display/EI650/WebSocket+Inbound+Protocol), and [Secure WebSocket Inbound Protocol](https://docs.wso2.com/display/EI650/Secure+WebSocket+Inbound+Protocol).

## Example 1: Sending a Message from a WebSocket Client to a WebSocket Endpoint

If you need to send a message from a WebSocket client to a WebSocket
endpoint via WSO2 MI, you need to establish a
persistent WebSocket connection from the WebSocket client to WSO2 MI as well as from WSO2 MI to the
WebSocket back-end.

To demonstrate this scenario, you need to create two dispatching
sequences. One for the client to back-end mediation, and another for the
back-end to client mediation. Finally you need to configure the
WebSocket inbound endpoint of WSO2 MI to use the
created sequences and listen on port 9092.

### Synapse configurations

For sample synapse configs, see [Websocket Inbound](../../inbound_endpoint_examples/inbound-endpoint-secured-websocket)

If you analyze the log, you will see that a connection from the
WebSocket client to WSO2 MI is established, and the
sequences are executed by the WebSocket inbound endpoint. You will also
see that the message sent to the WebSocket server is not transformed,
and that the response injected to the out sequence is also not
transformed.

## Example 2: Sending a Message from a HTTP Client to a WebSocket Endpoint

If you need to send a message from a HTTP client to a WebSocket endpoint
via WSO2 Micro Integrator, you need to establish
a persistent Websocket connection from WSO2 MI to the
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

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).

    !!! Note
        The Websocket sender functionality of the Micro Integrator is disabled by default. To enable the transport, open the `deployment.toml` file from the `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/conf/` directory and add the following: 

        ```toml
        [transport.ws]
        sender.enable = true
        ```
        
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. Create the [mediation sequences](../../../../develop/creating-artifacts/creating-reusable-sequences) and [inbound endpoint](../../../../develop/creating-an-inbound-endpoint) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Starting the Websocket client:

-  Download the netty artifacts zip file from [here](https://github.com/wso2-docs/ESB) and extract it. The extracted folder will be shown as `ESB-master`
-  Open a terminal, navigate to `ESB-master/ESB-Artifacts/Netty_artifacts_for_WebSocket_samples` and execute the following command to start the WebSocket server on port 8082:
    ```bash
    java -cp netty-example-4.0.30.Final.jar:lib/*:. io.netty.example.http.websocketx.server.WebSocketServer
    ```

If you analyze the log, you will see that a HTTP request is sent to the
WebSocket server, and that the WebSocket server injects the response to
the out sequence.
