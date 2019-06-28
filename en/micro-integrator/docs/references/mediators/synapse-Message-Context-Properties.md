# Synapse Message Context Properties

\[ [SYSTEM\_DATE](#SynapseMessageContextProperties-SYSTEM_DATE) \] \[
[SYSTEM\_TIME](#SynapseMessageContextProperties-SYSTEM_TIME) \] \[ [To,
From, Action, FaultTo, ReplyTo,
MessageID](#SynapseMessageContextProperties-To,From,Action,FaultTo,ReplyTo,MessageID)
\] \[ [MESSAGE\_FORMAT](#SynapseMessageContextProperties-MESSAGE_FORMAT)
\] \[ [OperationName](#SynapseMessageContextProperties-OperationName) \]

The Synapse message context properties allow you to get information
about the message, such as the date/time it was sent, the message
format, and the message operation. You can use the
`         get-property()        ` function in the [Property
mediator](_Property_Mediator_) with the scope set to
`         Synapse        ` to retrieve these properties.

#### SYSTEM\_DATE

<table>
<tbody>
<tr class="odd">
<td><p><strong>Name</strong></p></td>
<td><p>SYSTEM_DATE</p></td>
</tr>
<tr class="even">
<td><p><strong>Possible Values</strong></p></td>
<td><p>-</p></td>
</tr>
<tr class="odd">
<td><p><strong>Default Behavior</strong></p></td>
<td><p>-</p></td>
</tr>
<tr class="even">
<td><p><strong>Scope</strong></p></td>
<td><p>-</p></td>
</tr>
<tr class="odd">
<td><p><strong>Description</strong></p></td>
<td><p>Returns the current date as a String.</p>
<p>Optionally, a date format as per the standard date format may be supplied as follows:</p>
<div class="table-wrap">
<table>
<tbody>
<tr class="odd">
<td>Management Console</td>
<td><code>                  synapse:'get-property("SYSTEM_DATE", "yyyy-MM-dd&amp;apos;T&amp;apos;HH:mm:ss.SSSXXX")'                 </code></td>
</tr>
<tr class="even">
<td>EI Tooling</td>
<td>Enter the following in the Namespaced Property Editor: <code>                  get-property("SYSTEM_DATE", "yyyy-MM-dd'T'HH:mm:ss.SSSXXX")                 </code></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
</tbody>
</table>

#### SYSTEM\_TIME

|                      |                                                                                                                                                      |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**             | SYSTEM\_TIME                                                                                                                                         |
| **Possible Values**  | \-                                                                                                                                                   |
| **Default Behavior** | \-                                                                                                                                                   |
| **Scope**            | \-                                                                                                                                                   |
| **Description**      | Returns the current time in milliseconds, i.e. the difference, measured in milliseconds, between the current time and midnight, January 1, 1970 UTC. |

#### To, From, Action, FaultTo, ReplyTo, MessageID

|                      |                                                      |
|----------------------|------------------------------------------------------|
| **Names**            | To, From, Action, FaultTo, ReplyTo, MessageID        |
| **Possible Values**  | \-                                                   |
| **Default Behavior** | \-                                                   |
| **Scope**            | \-                                                   |
| **Description**      | The message To, Action and WS-Addressing properties. |

#### MESSAGE\_FORMAT

|                      |                                                                      |
|----------------------|----------------------------------------------------------------------|
| **Names**            | MESSAGE\_FORMAT                                                      |
| **Possible Values**  | \-                                                                   |
| **Default Behavior** | \-                                                                   |
| **Scope**            | \-                                                                   |
| **Description**      | Returns the message format, i.e. returns pox, get, soap11 or soap12. |

#### OperationName

|                      |                                             |
|----------------------|---------------------------------------------|
| **Names**            | OperationName                               |
| **Possible Values**  | \-                                          |
| **Default Behavior** | \-                                          |
| **Scope**            | \-                                          |
| **Description**      | Returns the operation name for the message. |
