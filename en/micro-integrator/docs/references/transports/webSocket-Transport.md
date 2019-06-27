# WebSocket Transport

The WebSocket transport implementation of WSO2 Enterprise
Integrator(WSO2 EI) is based on the [WebSocket
protocol](http://tools.ietf.org/html/rfc6455) , and consists of an Axis2
sender implementation for WebSockets and secure WebSockets. WebSocket is
a protocol that provides full-duplex communication channels over a
single TCP connection, and can be used by any client or server
application. This transport supports bi-directional message mediation.

The ESB Profile of WSO2 Enterprise Integrator (WSO2 EI)
provides WebSocket support also via the [WebSocket
Transport](_WebSocket_Transport_) , [WebSocket Inbound
Protocol](https://docs.wso2.com/display/EI650/WebSocket+Inbound+Protocol)
and [Secure WebSocket Inbound
Protocol](https://docs.wso2.com/display/EI650/Secure+WebSocket+Inbound+Protocol)
.

### Enabling the transport

The following transport sender class should be included in the WSO2 EI
configuration to enable the WebSocket transport.

-   `          org.wso2.carbon.websocket.transport.WebsocketTransportSender         `

**To enable the WebSocket transport sender**

-   Edit the `           <EI_HOME>/conf/axis2/axis2.xml          ` file
    and uncomment the following WebSocket sender configuration:

    ``` xml
        <transportSender name="ws" class="org.wso2.carbon.websocket.transport.WebsocketTransportSender">
              <parameter name="ws.outflow.dispatch.sequence" locked="false">outflowDispatchSeq</parameter>
              <parameter name="ws.outflow.dispatch.fault.sequence" locked="false">outflowFaultSeq</parameter>       
        </transportSender>
    ```

**To enable the secure WebSocket transport sender**

-   Edit the `           <EI_HOME>/conf/axis2/axis2.xml          ` file
    and uncomment the following secure WebSocket sender configuration:

    ``` xml
            <transportSender name="wss" class="org.wso2.carbon.websocket.transport.WebsocketTransportSender">
                  <parameter name="ws.outflow.dispatch.sequence" locked="false">outflowDispatchSeq</parameter>
                  <parameter name="ws.outflow.dispatch.fault.sequence" locked="false">outflowFaultSeq</parameter>
                  <parameter name="ws.trust.store" locked="false">
                        <ws.trust.store.location>repository/resources/security/client-truststore.jks</ws.trust.store.location>
                        <ws.trust.store.Password>wso2carbon</ws.trust.store.Password>
                  </parameter>
            </transportSender>
    ```
