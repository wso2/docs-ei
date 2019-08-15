# Using an HTTP Endpoint

See the examples given below.

## Example 1: Populating an HTTP endpoint during mediation

```
<endpoint xmlns="http://ws.apache.org/ns/synapse" name="HTTPEndpoint">
    <http uri-template="http://localhost:8080/{uri.var.servicepath}/restapi/{uri.var.servicename}/menu?category={uri.var.category}&amp;type={uri.var.pizzaType}" method="GET">
    </http>
</endpoint>
```

The URI template variables in this example HTTP endpoint can be populated during mediation as follows:

```
<inSequence>           
    <property name="uri.var.servicepath" value="PizzaShopServlet"/>
    <property name="uri.var.servicename" value="PizzaWS"/>
    <property name="uri.var.category" value="pizza"/>
    <property name="uri.var.pizzaType" value="pan"/>
     <send>
        <endpoint key="HTTPEndpoint"/>
    </send>
</inSequence>
```

This configuration will cause the RESTful URL to evaluate to: `http://localhost:8080/PizzaShopServlet/restapi/PizzaWS/menu?category=pizza&type=pan`


## Example 2; Sending a Message from a WebSocket Client to an HTTP Endpoint

The following sections walk you through a sample scenario that
demonstrates how to send a message from a WebSocket client to an HTTP
endpoint via the ESB Profile of WSO2 Enterprise Integrator (WSO2 EI):

If you need to send a message from a WebSocket client to an HTTP
endpoint via the ESB Profile of WSO2 EI, you need to establish a
persistent WebSocket connection from the WebSocket client to the ESB
Profile of WSO2 EI.  
To demonstrate this scenario, you need to create two dispatching
sequences. One for the client to back-end mediation, and another for the
back-end to client mediation. Finally you need to configure the
WebSocket inbound endpoint of the ESB Profile of WSO2 EI to use the
created sequences and listen on port 9091. 

### Synapse configuration

Create the sequence for client to back-end mediation, a sequence for back-end to client mediation, and configure the WebSocket inbound endpoint in the ESB Profile of WSO2 EI as follows to use the created sequences and listen on port 9091.

``` java tab='Sequence (Backend Mediation)'
<sequence name="dispatchSeq" xmlns="http://ws.apache.org/ns/synapse">
              <switch source="get-property('websocket.source.handshake.present')">
                <case regex="true">
                  <drop/>
                </case>
                <default>
                  <call>
                    <endpoint>
                     <address uri="http://www.mocky.io/v2/56f84ee5240000d1127866c8"/>
                    </endpoint>
                  </call>
                 <respond/>
                 </default>
               </switch>
</sequence>
```

``` java tab='Sequence (Backend to Client Mediation)'
<sequence name="outDispatchSeq" xmlns="http://ws.apache.org/ns/synapse">
    <log/>
    <respond/>
</sequence>
```

``` java tab='Inbound Endpoint (Websocket)'
<inboundEndpoint name="test" onError="falut" protocol="ws"sequence="dispatchSeq" suspend="false" xmlns="http://ws.apache.org/ns/synapse">
    <parameters>
        <parameter name="inbound.ws.port">9091</parameter>
        <parameter name="ws.outflow.dispatch.sequence">outDispatchSeq</parameter>
        <parameter name="ws.client.side.broadcast.level">0</parameter>
         <parameter name="ws.outflow.dispatch.fault.sequence">fault</parameter>
    </parameters>
</inboundEndpoint>
```

### Run the Example

1. Start the ESB Profile of WSO2 EI. For information on how to start the ESB Profile, see [Running the Product](https://docs.wso2.com/display/EI650/Running+the+Product) .
2.  Download the [sample netty artifacts](https://github.com/wso2-docs/ESB/blob/master/ESB-Artifacts/Netty_artifacts_for_WebSocket_samples/netty-example-4.0.30.Final.jar)
3.  Execute the following command to start the WebSocket client:
    ``` java
    java -DclientPort=9091 -cp netty-example-4.0.30.Final.jar:lib/*:. io.netty.example.http.websocketx.client.WebSocketClient
    ```
4.  Analyzing the output: If you analyze the log, you will see that a connection from the WebSocket client to the ESB Profile of WSO2 EI is established. You will also see that the sequences are executed by the WebSocket inbound endpoint.

## Example 3

You can specify one parameter as the HTTP endpoint by
using multiple other parameters, and then pass that to define the HTTP
endpoint as follows:

```
<property name="uri.var.httpendpointurl" expression="fn:concat($ctx:prefixuri, $ctx:host, $ctx:port, $ctx:urlparam1, $ctx:urlparam2)" />
    <send>
    <endpoint>
        <http uri-template="{uri.var.httpendpointurl}"/>
    </endpoint>
</send>
```
