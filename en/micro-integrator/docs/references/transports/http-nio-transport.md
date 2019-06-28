# HTTP-NIO Transport

**HTTP-NIO transport** is a module of the Apache Synapse project. The
two classes that implement the receiver and sender APIs are
`         org.apache.synapse.transport.nhttp.HttpCoreNIOListener        `
and
`         org.apache.synapse.transport.nhttp.HttpCoreNIOSender        `
respectively. These classes are available in the JAR file named
`         synapse-nhttp-transport.jar        ` . The transport
implementation is based on Apache HTTP Core - NIO and uses a
configurable pool of non-blocking worker threads to grab incoming HTTP
messages off the wire.

------------------------------------------------------------------------

### Transport receiver parameters

!!! info

In transport parameter tables, literals displayed in italic mode under
the "Possible Values" column should be considered as fixed literal
constant values. Those values can be directly put in transport
configurations.


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
<td><p>port</p></td>
<td><p>The port on which this transport receiver should listen for incoming messages.</p></td>
<td><p>No</p></td>
<td><p>A positive integer less than 65535</p></td>
<td><p>8280</p></td>
</tr>
<tr class="even">
<td><p>non-blocking</p></td>
<td><p>Setting this parameter to true is vital for a number of scenarios to work properly.</p></td>
<td><p>Yes</p></td>
<td><p><em>true/false</em></p></td>
<td><p><em>true</em></p></td>
</tr>
<tr class="odd">
<td><p>bind-address</p></td>
<td><p>The address of the interface to which the transport listener should bind.</p></td>
<td><p>No</p></td>
<td><p>A host name or an IP address</p></td>
<td><p>127.0.0.1</p></td>
</tr>
<tr class="even">
<td><p>hostname</p></td>
<td><p>The host name of the server to be displayed in service EPRs, WSDLs etc. This parameter takes effect only when the WSDLEPRPrefix parameter is not set.</p></td>
<td><p>No</p></td>
<td><p>A host name or an IP address</p></td>
<td><p>localhost</p></td>
</tr>
<tr class="odd">
<td><p>WSDLEPRPrefix</p></td>
<td><p>A URL prefix which will be added to all service EPRs and EPRs in WSDLs etc.</p></td>
<td><p>No</p></td>
<td><p>A URL of the form &lt;protocol&gt;://&lt;hostname&gt;:&lt;port&gt;/</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>

### Transport sender parameters

<table>
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
<td><p>http.proxyHost</p></td>
<td><p>If the outgoing messages should be sent through an HTTP proxy server, use this parameter to specify the target proxy.</p></td>
<td><p>No</p></td>
<td><p>A host name or an IP address</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p>http.proxyPort</p></td>
<td><p>The port through which the target proxy accepts HTTP traffic.</p></td>
<td><p>No</p></td>
<td><p>A positive integer less than 65535</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><p>http.nonProxyHosts</p></td>
<td><div class="content-wrapper">
<p>The list of hosts to which the HTTP traffic should be sent directly without going through the proxy.</p>
!!! note
<p>When trying to add multiple hostnames along with an asterisk in order to define a set of sub-domains for non-proxy hosts, you need to add a period before the asterisk when configuring proxy server.</p>
<p><code>               ".*.                               abc.my.com                              |localhost"              </code></p>
<div>
<code>                             </code>
</div>

</div></td>
<td><p>No</p></td>
<td><p>A list of host names or IP addresses separated by '|'</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p>non-blocking</p></td>
<td><p>Setting this parameter to true is vital for a number of scenarios to work properly.</p></td>
<td><p>Yes</p></td>
<td><p><em>true/false</em></p></td>
<td><p><em>true</em></p></td>
</tr>
</tbody>
</table>

For more information, see [Carrying
Messages](https://docs.wso2.com/display/EI650/Carrying+Messages) .

### Connection throttling

With the HTTP PassThrough and HTTP NIO transports, you can enable
connection throttling to restrict the number of simultaneous open
connections. To enable connection throttling, edit the
`         <EI_HOME>/conf/nhttp.properties        ` (for the HTTP NIO
transport) or `         <EI_HOME>/conf/passthru-http.properties        `
(for the PassThrough transport) and add the following line:

`         max_open_connections = 2        `

This will restrict simultaneous open incoming connections to 2. To
disable throttling, delete the `         max_open_connections        `
setting or set it to -1.

!!! info

Connection throttling is never exact. For example, setting this property
to 2 will result in roughly two simultaneous open connections at any
given time.

