# SOAP Headers

\[ [To](#SOAPHeaders-To) \] \[ [From](#SOAPHeaders-From) \] \[
[Action](#SOAPHeaders-Action) \] \[ [ReplyTo](#SOAPHeaders-ReplyTo) \]
\[ [MessageID](#SOAPHeaders-MessageID) \] \[
[RelatesTo](#SOAPHeaders-RelatesTo) \] \[
[FaultTo](#SOAPHeaders-FaultTo) \]

SOAP headers provide information about the message, such as the To and
From values. You can use the `         get-property()        ` function
in the [Property mediator](_Property_Mediator_) to retrieve these
headers. You can also add Custom SOAP headers using either the
[PayloadFactory
mediator](https://docs.wso2.com/display/EI6xx/PayloadFactory+Mediator#PayloadFactoryMediator-Example8:AddingacustomSOAPheader)
or the [Script
mediator](https://docs.wso2.com/display/EI6xx/Script+Mediator#ScriptMediator-Example4-AddingacustomSOAPheader)
.

#### To

|                     |                               |
|---------------------|-------------------------------|
| **Header Name**     | To                            |
| **Possible Values** | Any URI                       |
| **Description**     | The To header of the message. |
| **Example**         | get-property("To")            |

#### From

|                     |                                 |
|---------------------|---------------------------------|
| **Header Name**     | From                            |
| **Possible Values** | Any URI                         |
| **Description**     | The From header of the message. |
| **Example**         | get-property("From")            |

#### Action

|                     |                                       |
|---------------------|---------------------------------------|
| **Header Name**     | Action                                |
| **Possible Values** | Any URI                               |
| **Description**     | The SOAPAction header of the message. |
| **Example**         | get-property("Action")                |

#### ReplyTo

|                     |                                            |
|---------------------|--------------------------------------------|
| **Header Name**     | ReplyTo                                    |
| **Possible Values** | Any URI                                    |
| **Description**     | The ReplyTo header of the message.         |
| **Example**         | \<header name="ReplyTo" action="remove"/\> |

#### MessageID

|                     |                                                                                                                |
|---------------------|----------------------------------------------------------------------------------------------------------------|
| **Header Name**     | MessageID                                                                                                      |
| **Possible Values** | UUID                                                                                                           |
| **Description**     | The unique message ID of the message. It is not recommended to make alterations to this property of a message. |
| **Example**         | get-property("MessageID")                                                                                      |

#### RelatesTo

|                     |                                                                                                              |
|---------------------|--------------------------------------------------------------------------------------------------------------|
| **Header Name**     | RelatesTo                                                                                                    |
| **Possible Values** | UUID                                                                                                         |
| **Description**     | The unique ID of the request to which the current message is related. It is not recommended to make changes. |
| **Example**         | get-property("RelatesTo")                                                                                    |

#### FaultTo

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Header Name</strong></p></td>
<td><p>FaultTo</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>Any URI</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>The FaultTo header of the message.</p></td>
</tr>
<tr class="even">
<td><p><strong>Example</strong></p></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;header name=<span class="st">&quot;FaultTo&quot;</span> value=<span class="st">&quot;http://localhost:9000&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>
