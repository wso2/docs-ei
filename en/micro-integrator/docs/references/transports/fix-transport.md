# FIX Transport

**FIX (Financial Information eXchang) transport** implementation is a
module developed under the Apache Synapse project. This transport is
mainly used in conjunction with proxy services. This transport supports JMX.

FIX transport does not support any global parameters. All the FIX
configuration parameters should be specified at service level.

QuickFix 4J configuration parameters can be found
[here](http://www.quickfixengine.org/quickfix/doc/html/configuration.html)

## Configuring the FIX transport

Update the following configurations in the ei.toml file:

```toml
[transport.fix]
listener.enabled=false
sender.enabled=false
```

The transport implementation is based on Quickfix/J open source FIX engine and hence the following additional dependencies are required to enable the FIX transport.

-   `          mina-core.jar         `
-   `          quickfixj-core.jar         `
-   `          quickfixj-msg-fix40.jar         `
-   `          quickfixj-msg-fix41.jar         `
-   `          quickfixj-msg-fix42.jar         `
-   `          quickfixj-msg-fix43.jar         `
-   `          quickfixj-msg-fix44.jar         `
-   `          slf4j-api.jar         `
-   `          slf4j-log4j12.jar         `

Download [Quickfix/J](https://www.quickfixj.org/) and in the
distribution archive you will find all the dependencies listed above.
Also please refer to Quickfix/J documentation on configuring FIX
acceptors and initiators.

### Service Level FIX Parameters

!!! Info
	In transport parameter tables, literals displayed in italic mode under the "Possible Values" column should be considered as fixed literal constant values. Those values can be directly put in transport configurations.

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Requried</p></th>
<th><p>Possible Values</p></th>
<th><p>Default Value</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>transport.fix.<br />
AcceptorConfigURL</p></td>
<td><p>URL to the Quickfix/J<br />
acceptor configuration<br />
file (see notes below).</p></td>
<td><p>Required for receiving<br />
messages over FIX</p></td>
<td><p>A valid URL</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p>transport.fix.<br />
InitiatorConfigURL</p></td>
<td><p>URL to the Quickfix/J<br />
initiator configuration<br />
file (see notes below).</p></td>
<td><p>Required for sending<br />
messages over FIX</p></td>
<td><p>A valid URL</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><p>transport.fix.<br />
AcceptorLogFactory</p></td>
<td><p>Log factory implementation<br />
to be used for the<br />
FIX acceptor (Determines<br />
how logging is done at the<br />
acceptor level).</p></td>
<td><p>No</p></td>
<td><p><em>console, file, jdbc</em></p></td>
<td><p>Logging disabled</p></td>
</tr>
<tr class="even">
<td><p>transport.fix.<br />
InitiatorLogFactory</p></td>
<td><p>Log factory implementation<br />
to be used for the<br />
FIX acceptor (Determines<br />
how logging is done at the<br />
acceptor level).</p></td>
<td><p>No</p></td>
<td><p><em>console, file, jdbc</em></p></td>
<td><p>Logging disabled</p></td>
</tr>
<tr class="odd">
<td><p>transport.fix.<br />
AcceptorMessage<br />
Store</p></td>
<td><p>Message store<br />
mechanism to be<br />
used with the<br />
acceptor (Determines<br />
how the FIX message<br />
store is maintained).</p></td>
<td><p>No</p></td>
<td><p><em>memory, file,</em><br />
<em>sleepycat, jdbc</em></p></td>
<td><p>memory</p></td>
</tr>
<tr class="even">
<td><p>transport.fix.<br />
InitiatorMessage<br />
Store</p></td>
<td><p>Message store<br />
mechanism to be<br />
used with the initiator<br />
(Determines how the<br />
FIX message store is<br />
maintained).</p></td>
<td><p>No</p></td>
<td><p><em>memory, file,</em><br />
<em>sleepycat, jdbc</em></p></td>
<td><p>memory</p></td>
</tr>
<tr class="odd">
<td><p>transport.fix.<br />
ResponseDeliverTo<br />
CompID</p></td>
<td><p>If the response FIX<br />
messages should<br />
be delivered to a<br />
location different<br />
from the location<br />
the request was<br />
originated use this<br />
property to set the<br />
DeliverToCompID<br />
field of the FIX<br />
messages.</p></td>
<td><p>No</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p>transport.fix.<br />
ResponseDeliverTo<br />
SubID</p></td>
<td><p>If the response FIX<br />
messages should<br />
be delivered to a<br />
location different<br />
from the location<br />
the request was<br />
originated use this<br />
property to set<br />
the DeliverToSubID<br />
field of the FIX<br />
messages.</p></td>
<td><p>No</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><p>transport.fix.<br />
ResponseDeliverTo<br />
LocationID</p></td>
<td><p>If the response FIX<br />
messages should<br />
be delivered to a<br />
location different<br />
from the location<br />
the request was<br />
originated use this<br />
property to set the<br />
DeliverToLocationID<br />
field of the FIX<br />
messages.</p></td>
<td><p>No</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p>transport.fix.<br />
SendAllToInSequence</p></td>
<td><p>By default, all received<br />
FIX messages (including<br />
responses) will be<br />
directed to the in<br />
sequence of the proxy<br />
service.<br />
<br />
Use this property to<br />
override that behavior.</p></td>
<td><p>No</p></td>
<td><p><em>true, false</em></p></td>
<td><p>true</p></td>
</tr>
<tr class="odd">
<td><p>transport.fix.<br />
BeginStringValidation</p></td>
<td><p>Whether the transport<br />
should validate<br />
BeginString values<br />
when forwrding FIX<br />
messages across<br />
sessions.</p></td>
<td><p>No</p></td>
<td><p><em>true, false</em></p></td>
<td><p>true</p></td>
</tr>
<tr class="even">
<td><p>transport.fix.<br />
DropExtraResponses</p></td>
<td><p>In situation where the<br />
FIX recipient sends<br />
multiple responses<br />
per request use this<br />
parameter to drop<br />
excessive responses<br />
and use only the first<br />
one.</p></td>
<td><p>No</p></td>
<td><p><em>true, false</em></p></td>
<td><p>false</p></td>
</tr>
</tbody>
</table>