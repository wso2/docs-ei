# HTTP Endpoint

The **HTTP endpoint** allows you to define REST
[endpoints](_Working_with_Endpoints_) using [URI
templates](https://docs.wso2.com/enterprise-service-bus/Working+with+APIs#WorkingwithAPIs-URItemplates)
similar to the REST API. The URI templates allow a RESTful URI to
contain variables that can be populated during mediation runtime using
property values whose names have the " `         uri.var.        ` "
prefix. An HTTP endpoint can also define the particular HTTP method to
use in the RESTful invocation.

------------------------------------------------------------------------

[XML Configuration](#HTTPEndpoint-XMLConfigurationXMLConfiguration) \|
[Parameters](#HTTPEndpoint-Parameters) \|
[Examples](#HTTPEndpoint-Examples)

------------------------------------------------------------------------

### XML Configuration

The syntax is as follows.

``` xml
    <http uri-template="URI Template" method="GET|POST|PATCH|PUT|DELETE|OPTIONS|HEAD" />
```

#### HTTP Endpoint Attributes

<table>
<thead>
<tr class="header">
<th><p>Attribute</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>uri-template</p></td>
<td><div class="content-wrapper">
<p>The <a href="https://docs.wso2.com/enterprise-service-bus/Working+with+APIs#WorkingwithAPIs-URItemplates">URI template</a> that constructs the RESTful endpoint URL at runtime.</p>
!!! info
<p>Using the URI postfix property:</p>
<p>Let's take a look at it using an example. If you add a <code>               /              </code> between <code>               {uri.var.context2}{uri.var.postfix}              </code> , it is evaluated as an invalid invocation because <code>               {uri.var.postfix}              </code> contains a <code>               /              </code> at the start.</p>
<p>Example:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;http uri-template=<span class="st">&quot;http://{uri.var.host}:{uri.var.port}/{uri.var.context1}/services/{uri.var.context2}{uri.var.postfix}&quot;</span> method=<span class="st">&quot;get&quot;</span>&gt;</span></code></pre></div>
</div>
</div>

</div></td>
</tr>
<tr class="even">
<td><p>method</p></td>
<td><p>The HTTP method to use during the invocation.</p></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Parameters

The parameters to configure an HTTP endpoint are as follows.

<table>
<thead>
<tr class="header">
<th>Parameter Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Name</strong></td>
<td>This parameter is used to enter a unique name for the endpoint.</td>
</tr>
<tr class="even">
<td><strong>URI Template</strong></td>
<td><div class="content-wrapper">
<p>The URI template of the endpoint. Insert <code>               uri.var.              </code> before each variable. Click <strong>Test</strong> to test the URI.</p>
!!! info
<p>If the endpoint URL is an encoded URL, then you need to add <code>               legacy-encoding:              </code> when defining the uri-template.</p>
<p>e.g., <code>               uri-template="legacy-encoding:{uri.var.APIurl}"              </code></p>
!!! tip
<p>If you want to define the URL with environment properties, you can define it as shown below.</p>
<div class="panel" style="border-width: 1px;">
<div class="panelContent">
<p><code>                 &lt;?xml version="1.0" encoding="UTF-8"?&gt;                </code><br />
<code>                 &lt;endpoint xmlns="                                   http://ws.apache.org/ns/synapse                                  " name="JSON_EP"&gt;                </code><br />
<code>                                   &lt;address uri="$SYSTEM:VAR"/&gt;                                 </code><br />
<code>                 &lt;/endpoint&gt;                </code></p>
<div>
<code>                                 </code>
</div>
</div>
</div>
<p>Here <code>               VAR              </code> is the url you need to have set as environment property.</p>
<p>This is useful when you need to need to deploy the endpoint in a container.</p>

</div></td>
</tr>
<tr class="odd">
<td><strong>HTTP Method</strong></td>
<td><p>The HTTP method to use during the invocation of the endpoint. Supported methods are as follows.</p>
<ul>
<li>Get</li>
<li>Post</li>
<li>Patch</li>
<li>Put</li>
<li>Delete</li>
<li>Options</li>
<li>Head</li>
<li>Leave-as-is</li>
</ul></td>
</tr>
<tr class="even">
<td><strong>Show Advanced Options</strong></td>
<td>Click this link if you want to add advanced options to the endpoint. See Advanced Options for details of common advanced options you can add.</td>
</tr>
</tbody>
</table>

You can create HTTP endpoints by specifying values for the parameters
given above.

Alternatively, you can specify one parameter as the HTTP endpoint by
using multiple other parameters, and then pass that to define the HTTP
endpoint as follows:

``` xml
    <property name="uri.var.httpendpointurl" expression="fn:concat($ctx:prefixuri, $ctx:host, $ctx:port, $ctx:urlparam1, $ctx:urlparam2)" />
    <send>
    <endpoint>
    <http uri-template="
    {uri.var.httpendpointurl}
    "/>
    </endpoint>
    </send>
```

------------------------------------------------------------------------

### Examples

#### Example 1 - populating an HTTP endpoint during mediation

``` html/xml
    <endpoint xmlns="http://ws.apache.org/ns/synapse" name="HTTPEndpoint">
        <http uri-template="http://localhost:8080/{uri.var.servicepath}/restapi/{uri.var.servicename}/menu?category={uri.var.category}&amp;type={uri.var.pizzaType}" method="GET">
    </http>
    </endpoint>
```

The URI template variables in this example HTTP endpoint can be
populated during mediation as follows:

``` html/xml
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

This configuration will cause the RESTful URL to evaluate to:

    http://localhost:8080/PizzaShopServlet/restapi/PizzaWS/menu?category=pizza&type=pan

#### Example 2 - Sending a Message from a WebSocket Client to an HTTP Endpoint

The following sections walk you through a sample scenario that
demonstrates how to send a message from a WebSocket client to an HTTP
endpoint via the ESB Profile of WSO2 Enterprise Integrator (WSO2 EI):

-   [Introduction](#HTTPEndpoint-Introduction)
-   [Prerequisites](#HTTPEndpoint-Prerequisites)
-   [Configuring the sample
    scenario](#HTTPEndpoint-Configuringthesamplescenario)
-   [Executing the sample
    scenario](#HTTPEndpoint-Executingthesamplescenario)
-   [Analyzing the output](#HTTPEndpoint-Analyzingtheoutput)

##### Introduction

If you need to send a message from a WebSocket client to an HTTP
endpoint via the ESB Profile of WSO2 EI, you need to establish a
persistent WebSocket connection from the WebSocket client to the ESB
Profile of WSO2 EI.  
To demonstrate this scenario, you need to create two dispatching
sequences. One for the client to back-end mediation, and another for the
back-end to client mediation. Finally you need to configure the
WebSocket inbound endpoint of the ESB Profile of WSO2 EI to use the
created sequences and listen on port 9091.

##### Prerequisites

-   Start the ESB Profile of WSO2 EI. For information on how to start
    the ESB Profile, see [Running the
    Product](https://docs.wso2.com/display/EI650/Running+the+Product) .
-   Download the [sample netty
    artifacts](https://github.com/wso2-docs/ESB/blob/master/ESB-Artifacts/Netty_artifacts_for_WebSocket_samples/netty-example-4.0.30.Final.jar)
    .

##### Configuring the sample scenario

-   Create the sequence for client to back-end mediation as follows:

    ``` xml
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

    This sequence calls an HTTP endpoint.

-   Create the sequence for back-end to client mediation as follows:

    ``` xml
            <sequence name="outDispatchSeq" xmlns="http://ws.apache.org/ns/synapse">
              <log/>
              <respond/>
            </sequence>
    ```

-   Configure the WebSocket inbound endpoint in the ESB Profile of WSO2
    EI as follows to use the created sequences and listen on port 9091:

    ``` xml
            <inboundEndpoint name="test" onError="falut" protocol="ws"
                sequence="dispatchSeq" suspend="false" xmlns="http://ws.apache.org/ns/synapse">
                <parameters>
                    <parameter name="inbound.ws.port">9091</parameter>
                    <parameter name="ws.outflow.dispatch.sequence">outDispatchSeq</parameter>
                    <parameter name="ws.client.side.broadcast.level">0</parameter>
                    <parameter name="ws.outflow.dispatch.fault.sequence">fault</parameter>
                </parameters>
            </inboundEndpoint>
    ```

##### Executing the sample scenario

-   Execute the following command to start the WebSocket client:

    ``` java
            java -DclientPort=9091 -cp netty-example-4.0.30.Final.jar:lib/*:. io.netty.example.http.websocketx.client.WebSocketClient
    ```

##### Analyzing the output

If you analyze the log, you will see that a connection from the
WebSocket client to the ESB Profile of WSO2 EI is established. You will
also see that the sequences are executed by the WebSocket inbound
endpoint.
