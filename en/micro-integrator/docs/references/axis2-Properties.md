# Axis2 Properties

\[ [CacheLevel](#Axis2Properties-CacheLevel) \] \[
[ConcurrentConsumers](#Axis2Properties-ConcurrentConcurrentConsumers) \]
\[ [HTTP\_ETAG](#Axis2Properties-HTTP_ETAGHTTP_ETAG) \] \[
[JMS\_COORELATION\_ID](#Axis2Properties-JMS_Coorelation_IDJMS_COORELATION_ID)
\] \[ [MaxConcurrentConsumers](#Axis2Properties-MaxConcurrentConsumers)
\] \[ [MercurySequenceKey](#Axis2Properties-MercurySequenceKey) \] \[
[MercuryLastMessage](#Axis2Properties-MercuryLastMessage) \] \[
[FORCE\_HTTP\_1.0](#Axis2Properties-FORCE_HTTP_1.0) \] \[
[setCharacterEncoding](#Axis2Properties-setCharacterEncoding) \] \[
[CHARACTER\_SET\_ENCODING](#Axis2Properties-CHARACTER_SET_ENCODING) \]

Axis2 properties allow you to configure the web services engine in
the ESB profile, such as specifying how to cache JMS objects, setting
the minimum and maximum threads for consuming messages, and forcing
outgoing HTTP/S messages to use HTTP 1.0. You can access some of these
properties through the [Property mediator](_Property_Mediator_) with the
scope set to `         axis2        ` or `         axis2-client        `
as shown below.

#### CacheLevel

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>CacheLevel</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td>none, connection, session, consumer, producer, auto</td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>This property determines which JMS objects should be cached. JMS objects are cached so that they can be reused in the subsequent invocations. Each caching level can be described as follows:</p>
<p><code>              none             </code> : No JMS object will be cached.<br />
<code>              connection             </code> <code>              :             </code> JMS connection objects will be cached.<br />
<code>              session             </code> : JMS connection and session objects will be cached.<br />
<code>              consumer             </code> : JMS connection, session and consumer objects will be cached.<br />
<code>              producer             </code> : JMS connection, session and producer objects will be cached.</p>
<code>             auto            </code> : An appropriate caching level will be used depending on the transaction strategy.</td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">&lt;parameter</span><span class="ot"> name=</span><span class="st">&quot;transport.jms.CacheLevel&quot;</span><span class="kw">&gt;</span>consumer<span class="kw">&lt;/parameter&gt;</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### ConcurrentConsumers

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>ConcurrentConsumers</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td>integer
<p><br />
</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>The minimum number of threads for message consuming. The value specified for this property is the initial number of threads started. As the number of messages to be consumed increases, number of threads are also increased to match the load until the total number of threads equals the value specified for the <code>              transport.jms.MaxConcurrentConsumers             </code> property.</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">&lt;parameter</span><span class="ot"> name=</span><span class="st">&quot;transport.jms.ConcurrentConsumers&quot;</span><span class="er">locked=&quot;false&quot;</span><span class="kw">&gt;</span>50<span class="kw">&lt;/parameter&gt;</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### HTTP\_ETAG

<table>
<tbody>
<tr class="odd">
<td>Name</td>
<td>HTTP_ETAG</td>
</tr>
<tr class="even">
<td>Possible Values</td>
<td>true/false</td>
</tr>
<tr class="odd">
<td>Scope</td>
<td>axis2</td>
</tr>
<tr class="even">
<td>Description</td>
<td><div class="content-wrapper">
<p>This property determines whether the HTTP Etag should be enabled for the request or not.</p>
!!! info
<p><a href="https://en.wikipedia.org/wiki/HTTP_ETag">HTTP Etag</a> is a mechanism provided by HTTP for Web cache validation.</p>

</div></td>
</tr>
<tr class="odd">
<td>Example</td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">&lt;property</span><span class="ot"> name=</span><span class="st">&quot;HTTP_ETAG&quot;</span><span class="ot"> scope=</span><span class="st">&quot;axis2&quot;</span><span class="ot"> type=</span><span class="st">&quot;BOOLEAN&quot;</span><span class="ot"> value=</span><span class="st">&quot;true&quot;</span><span class="kw">/&gt;</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### JMS\_COORELATION\_ID

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>JMS_COORELATION_ID</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>String</p></td>
</tr>
<tr class="odd">
<td><p><strong>Scope</strong></p></td>
<td><p>axis2</p></td>
</tr>
<tr class="even">
<td><p><strong>Description</strong></p></td>
<td><p>The JMS coorelation ID is used to match responses with specific requests. This property can be used to set the JMS coorrelation ID as a dynamic or a hard coded value in a request. As a result, responses with the matching JMS correlation IDs will be matched with the request.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">&lt;property</span><span class="ot"> name=</span><span class="st">&quot;JMS_COORELATION_ID&quot;</span><span class="ot"> action=</span><span class="st">&quot;set&quot;</span><span class="ot"> scope=</span><span class="st">&quot;axis2&quot;</span><span class="ot"> expression=</span><span class="st">&quot;$header/wsa:MessageID&quot;</span><span class="ot"> xmlns:sam=</span><span class="st">&quot;http://sample.esb.org/&gt;</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### MaxConcurrentConsumers

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>MaxConcurrentConsumers</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td>integer
<p><br />
</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>The maximum number of threads that can be added for message consuming. See <a href="#Axis2Properties-Concurrent">ConcurrentConsumers</a> .</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">&lt;parameter</span><span class="ot"> name=</span><span class="st">&quot;transport.jms.MaxConcurrentConsumers&quot;</span><span class="er">locked=&quot;false&quot;</span><span class="kw">&gt;</span>50<span class="kw">&lt;/parameter&gt;</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### MercurySequenceKey

|                     |                                                                  |
|---------------------|------------------------------------------------------------------|
| **Name**            | MercurySequenceKey                                               |
| **Possible Values** | integer                                                          |
| **Description**     | Can be an identifier specifying a Mercury internal sequence key. |

#### MercuryLastMessage

|                     |                                                                                    |
|---------------------|------------------------------------------------------------------------------------|
| **Name**            | MercuryLastMessage                                                                 |
| **Possible Values** | true/false                                                                         |
| **Description**     | When set to "true", it will make this the last message and terminate the sequence. |

#### FORCE\_HTTP\_1.0

|                     |                                                                               |
|---------------------|-------------------------------------------------------------------------------|
| **Name**            | FORCE\_HTTP\_1.0                                                              |
| **Possible Values** | true/false                                                                    |
| **Scope**           | axis2-client                                                                  |
| **Description**     | Forces outgoing http/s messages to use HTTP 1.0 (instead of the default 1.1). |

#### setCharacterEncoding

|                      |                                                                                                                                                             |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**             | setCharacterEncoding                                                                                                                                        |
| **Possible Values**  | false                                                                                                                                                       |
| **Default Behavior** | By default character encoding is enabled in the ESB profile.                                                                                                |
| **Scope**            | axis2                                                                                                                                                       |
| **Description**      | This property can be used to remove character encode. Note that if this property is set to 'false', the 'CHARACTER\_SET\_ENCODING' property cannot be used. |
| **Example**          | `             <property name="             setCharacterEncoding             " value="false" scope="axis2" type="STRING"/>            `                      |

#### CHARACTER\_SET\_ENCODING

|                      |                                                                                                                                                                                            |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**             | CHARACTER\_SET\_ENCODING                                                                                                                                                                   |
| **Possible Values**  | Any valid encoding standard (E.g., UTF-8, UTF-16 etc.)                                                                                                                                     |
| **Default Behavior** | N/A                                                                                                                                                                                        |
| **Scope**            | axis2                                                                                                                                                                                      |
| **Description**      | Specifies the encoding type used for the content of the files processed by the transport. Note that this property cannot be used if the 'setCharacterEncoding' property is set to 'false'. |
| **Example**          | `             <property name="CHARACTER_SET_ENCODING" value="UTF-8" scope="axis2" type="STRING"/>            `                                                                             |
