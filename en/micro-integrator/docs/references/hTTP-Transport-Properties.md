# HTTP Transport Properties

\[ [POST\_TO\_URI](#HTTPTransportProperties-POST_TO_URI) \] \[
[FORCE\_SC\_ACCEPTED](#HTTPTransportProperties-FORCE_SC_ACCEPTED) \] \[
[DISABLE\_CHUNKING](#HTTPTransportProperties-DISABLE_CHUNKINGDISABLE_CHUNKING)
\] \[ [NO\_ENTITY\_BODY](#HTTPTransportProperties-NO_ENTITY_BODY) \] \[
[FORCE\_HTTP\_1.0](#HTTPTransportProperties-FORCE_HTTP_1.0) \] \[
[HTTP\_SC](#HTTPTransportProperties-HTTP_SC) \] \[
[HTTP\_SC\_DESC](#HTTPTransportProperties-HTTP_SC_DESC) \] \[
[FAULTS\_AS\_HTTP\_200](#HTTPTransportProperties-FAULTS_AS_HTTP_200) \]
\[ [NO\_KEEPALIVE](#HTTPTransportProperties-NO_KEEPALIVE) \] \[
[REST\_URL\_POSTFIX](#HTTPTransportProperties-REST_URL_POSTFIXREST_URL_POSTFIX)
\] \[
[REQUEST\_HOST\_HEADER](#HTTPTransportProperties-REQUEST_HOST_HEADER) \]
\[
[FORCE\_POST\_PUT\_NOBODY](#HTTPTransportProperties-FORCE_POST_PUT_NOBODY)
\] \[
[FORCE\_HTTP\_CONTENT\_LENGTH](#HTTPTransportProperties-force-httpFORCE_HTTP_CONTENT_LENGTH)
\] \[
[COPY\_CONTENT\_LENGTH\_FROM\_INCOMING](#HTTPTransportProperties-CopyContentLengthCOPY_CONTENT_LENGTH_FROM_INCOMING)
\]

HTTP transport properties allow you to configure how the HTTP transport
processes messages, such as forcing a 202 HTTP response to the client so
that it stops waiting for a response, setting the HTTP status code, and
appending a context to the target URL in RESTful invocations.

#### POST\_TO\_URI

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p><strong>POST_TO_URI</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>"true", "false"</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>false</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>This property makes the request URL that is sent from the ESB profile a complete URL. When set to <code>              false             </code> only the context path will be included in the request URL that is sent. It is important that this property is set to <code>              true             </code> when the ESB profile needs to communicate with the back-end service through a proxy server.</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;property name=<span class="st">&quot;POST_TO_URI&quot;</span> scope=<span class="st">&quot;axis2&quot;</span> value=<span class="st">&quot;true&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### FORCE\_SC\_ACCEPTED

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>FORCE_SC_ACCEPTED</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>"true", "false"</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>false</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>When set to true, this property forces a 202 HTTP response to the client immediately after the ESB profile receives the message so that the client stops waiting for a response.</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;property name=<span class="st">&quot;FORCE_SC_ACCEPTED&quot;</span> value=<span class="st">&quot;true&quot;</span> scope=<span class="st">&quot;axis2&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### DISABLE\_CHUNKING

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>DISABLE_CHUNKING</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>"true", "false"</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>false</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><div class="content-wrapper">
<p>If you set this to true, it disables HTTP chunking for outgoing messages. Instead, the ESB profile builds the message to calculate the content length and then sends the particular message to the backend with the content length (e.g., <code>               Content-Length: 25              </code> ).</p>
<p>You can use this parameter if the client sends the request with HTTP chunking (i.e., with <code>               Transfer Encoding:chunked              </code> ) although you need to send the message without HTTP chunking to the backend, or if you need to modify the message payload, which the client receives before sending it to the backend.</p>
!!! warning
<p>This property might decrease performance since the messages get built per each invocation. Also, this property does not affect Callout mediators, whose chunking must be <a href="https://docs.wso2.com/display/EI620/Callout+Mediator#CalloutMediator-disabling">disabled separately</a> .</p>

</div></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;property name=<span class="st">&quot;DISABLE_CHUNKING&quot;</span> value=<span class="st">&quot;true&quot;</span> scope=<span class="st">&quot;axis2&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### NO\_ENTITY\_BODY

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>NO_ENTITY_BODY</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>"true", "false"</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>In case of GET requests this property is set to true.</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>Axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><div class="content-wrapper">
<p>Set this property if you want to do the following:</p>
<ul>
<li>check if an incoming request to the ESB mediation flow has an entity body or not</li>
<li>check if an outgoing request/response generated from the ESB mediation flow has an entity body or not</li>
</ul>
!!! info
<p>If using the <a href="_PayloadFactory_Mediator_">PayloadFactory mediator</a> , this property does not need to be manually set since it is done automatically by the mediator.</p>

</div></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;property name=<span class="st">&quot;NO_ENTITY_BODY&quot;</span> value=<span class="st">&quot;true&quot;</span> scope=<span class="st">&quot;axis2&quot;</span> type=<span class="st">&quot;BOOLEAN&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### FORCE\_HTTP\_1.0

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>FORCE_HTTP_1.0</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>"true", "false"</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>false</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>Force HTTP 1.0 for outgoing HTTP messages.</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;property name=<span class="st">&quot;FORCE_HTTP_1.0&quot;</span> value=<span class="st">&quot;true&quot;</span> scope=<span class="st">&quot;axis2&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### HTTP\_SC

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>HTTP_SC</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>HTTP status code number</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>none</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>Set the HTTP status code.</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;property name=<span class="st">&quot;HTTP_SC&quot;</span> value=<span class="st">&quot;500&quot;</span> scope=<span class="st">&quot;axis2&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### HTTP\_SC\_DESC

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>HTTP_SC_DESC</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><div>
HTTP response's Reason- Phrase that is sent by the backend. For example, if the backend sends the response's status as HTTP/1.1 200 OK, then the value of HTTP_SC_DESC is OK.
</div></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>none</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>Set the HTTP status message ( Reason-Phrase ).</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;property name=<span class="st">&quot;HTTP_SC_DESC&quot;</span> value=<span class="st">&quot;Your description here&quot;</span> scope=<span class="st">&quot;axis2&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### FAULTS\_AS\_HTTP\_200

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>FAULTS_AS_HTTP_200</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>"true", "false"</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>false</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>When WSO2 EI receives a soap fault as a HTTP 500 message, the ESB profile will forward this fault to client with status code 200.</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;property name=<span class="st">&quot;FAULTS_AS_HTTP_200&quot;</span> value=<span class="st">&quot;true&quot;</span> scope=<span class="st">&quot;axis2&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### NO\_KEEPALIVE

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>NO_KEEPALIVE</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>"true", "false"</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>false</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>Disables HTTP keep alive for outgoing requests.</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;property name=<span class="st">&quot;NO_KEEPALIVE&quot;</span> value=<span class="st">&quot;true&quot;</span> scope=<span class="st">&quot;axis2&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### REST\_URL\_POSTFIX

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>REST_URL_POSTFIX</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>A URL fragment starting with "/"</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>In the case of GET requests through an address endpoint, this contains the query string.</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>The value of this property will be appended to the target URL when sending messages out in a RESTful manner through an address endpoint. This is useful when you need to append a context to the target URL in case of RESTful invocations. If you are using an HTTP endpoint instead of an address endpoint, specify variables in the format of "uri.var.*" instead of using this property.</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;property name=<span class="st">&quot;REST_URL_POSTFIX&quot;</span> value=<span class="st">&quot;/context&quot;</span> scope=<span class="st">&quot;axis2&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### REQUEST\_HOST\_HEADER

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>REQUEST_HOST_HEADER</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>string</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>The ESB profile will set hostname of target endpoint and port as the HTTP host header</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>The value of this property will be set as the HTTP host header of outgoing request</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;property name=<span class="st">&quot;REQUEST_HOST_HEADER&quot;</span> value=<span class="st">&quot;www.wso2.org&quot;</span> scope=<span class="st">&quot;axis2&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

  

#### FORCE\_POST\_PUT\_NOBODY

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>FORCE_POST_PUT_NOBODY</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>"true", "false"</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>false</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>This property allows to send a request without a body for POST and PUT HTTP methods.</p>
<p>Applicable only for HTTP Passthrough transport.</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;property name=<span class="st">&quot;FORCE_POST_PUT_NOBODY&quot;</span> value=<span class="st">&quot;true&quot;</span> scope=<span class="st">&quot;axis2&quot;</span> type=<span class="st">&quot;BOOLEAN&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

  

#### FORCE\_HTTP\_CONTENT\_LENGTH

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>FORCE_HTTP_CONTENT_LENGTH</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>"true", "false"</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>false</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><div class="content-wrapper">
<p>If the request sent by the client contains the ‘Content-Length’ header, this property allows the ESB profile to send the request with the content length (without HTTP chunking) to the back end server.</p>
<p>You should set this to true in scenarios where the backend server is not able to accept chunked content. For example, in a scenario where a pass-through proxy is defined and the backend does not accept chunked content, this property should be used together with the <a href="#HTTPTransportProperties-CopyContentLength">COPY_CONTENT_LENGTH_FROM_INCOMING</a> property, to simply add the content length without chunking.</p>
<p>When HTTP 1.1 is used, this property disables chunking and sends the content length. When HTTP 1.0 is used, the property only sends the content length.</p>
!!! warning
<p>This property can cause performance degradation, and thereby, you should only use it with message relay. If you set this to true, the ESB profile forwards the content length coming from the client request to the backend without building the message and calculating the content length. Since the message doesn’t get build, using these properties will perform better than using <a href="#HTTPTransportProperties-DISABLE_CHUNKING">DISABLE_CHUNKING</a> . However, if you change the receiving payload before sending it to the backend, then having this property will result in an error due to a content length mismatch.</p>

</div></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">&lt;property</span><span class="ot"> name=</span><span class="st">&quot;FORCE_HTTP_CONTENT_LENGTH&quot;</span><span class="ot"> scope=</span><span class="st">&quot;axis2&quot;</span><span class="ot"> value=</span><span class="st">&quot;true&quot;</span><span class="kw">&gt;&lt;/property&gt;</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

  

#### COPY\_CONTENT\_LENGTH\_FROM\_INCOMING

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>COPY_CONTENT_LENGTH_FROM_INCOMING</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>"true", "false"</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>false</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>This property allows the HTTP content length to be copied from an incoming message. It is only valid when the <code>                             FORCE_HTTP_CONTENT_LENGTH                           </code> property is used. The <code>              COPY_CONTENT_LENGTH_FROM_INCOMING             </code> avoids buffering the message in memory for calculating the content length, thus reducing the risk of performance degradation.</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">&lt;property</span><span class="ot"> name=</span><span class="st">&quot;COPY_CONTENT_LENGTH_FROM_INCOMING&quot;</span><span class="ot"> value=</span><span class="st">&quot;true&quot;</span><span class="ot"> scope=</span><span class="st">&quot;axis2&quot;</span><span class="kw">/&gt;</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>
