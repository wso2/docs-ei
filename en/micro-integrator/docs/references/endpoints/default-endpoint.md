# Default Endpoint

The **Default Endpoint** is an [endpoint](_Working_with_Endpoints_)
defined for adding QoS and other configurations to the endpoint which is
resolved from the `         To        ` address of the message context.
All the configurations such as message format for the endpoint, the
method to optimize attachments, and security policies for the endpoint
can be specified as in the [Address Endpoint](_Address_Endpoint_) . This
endpoint differs from the address endpoint only in the
`         URI        ` attribute which will not be present in this
endpoint.

------------------------------------------------------------------------

[XML Configuration](#DefaultEndpoint-XMLConfiguration) \|
[Parameters](#DefaultEndpoint-Parameters)

------------------------------------------------------------------------

### XML Configuration

``` html/xml
    <default [format="soap11|soap12|pox|get"] [optimize="mtom|swa"]
         [encoding="charset encoding"]
         [statistics="enable|disable"] [trace="enable|disable"]>
    
        <enableRM [policy="key"]/>?
        <enableSec [policy="key"]/>?
        <enableAddressing [version="final|submission"] [separateListener="true|false"]/>?
    
        <timeout>
            <duration>timeout duration in milliseconds</duration>
            <action>discard|fault</action>
        </timeout>?
    
        <markForSuspension>
            [<errorCodes>xxx,yyy</errorCodes>]
            <retriesBeforeSuspension>m</retriesBeforeSuspension>
            <retryDelay>d</retryDelay>
        </markForSuspension>
    
        <suspendOnFailure>
            [<errorCodes>xxx,yyy</errorCodes>]
            <initialDuration>n</initialDuration>
            <progressionFactor>r</progressionFactor>
            <maximumDuration>l</maximumDuration>
        </suspendOnFailure>
    </default>
```

------------------------------------------------------------------------

### Parameters

Parameters available to configure the default endpoint are as follows.

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
<td>This parameter is used to define a unique name for the endpoint.</td>
</tr>
<tr class="even">
<td><strong>Show Advanced Options</strong></td>
<td><div class="content-wrapper">
<p>Click this link if you want to add advanced options for the endpoint. Advanced options specific to the default endpoint are as follows.</p>
<ul>
<li><strong>Format</strong> - The message format for the endpoint. The available values are as follows.
<ul>
<li><strong>Leave As-Is</strong> : If this is selected, no transformation is done to the outgoing message.</li>
</ul></li>
</ul>
<ul>
<li><ul>
<li><strong>SOAP 1.1</strong> : If this is selected the message is transformed to SOAP 1.1.</li>
<li><strong>SOAP 1.2</strong> : If this is selected the message is transformed to SOAP 1.2.</li>
<li><strong>Plain Old XML (POX)</strong> : If this is selected the message is transformed to plain old XML format.</li>
<li><strong>Representational State Transfer (REST)</strong> - If this is selected, the message is transformed to REST.</li>
<li><strong>GET</strong> : If this is selected, the message is transformed to a HTTP Get Request.</li>
</ul></li>
<li><strong>Optimize</strong> - Optimization for the message, which transfers binary data. The available values are as follows.<br />

<ul>
<li><strong>Leave As-Is</strong> - If this is selected, there will be no special optimization. The original message will be kept.</li>
<li><strong>SwA</strong> - If this is selected, the message is optimized as a SwA (SOAP with Attachment) message.</li>
<li><strong>MTOM</strong> - If this is selected, the message is optimized using a MTOM (message transmission optimization mechanism).</li>
</ul></li>
</ul>
!!! info
<p>Note</p>
<p>The rest of the advanced options are common for Address, WSDL, Default endpoints. See the description of common options in <a href="_Working_with_Endpoints_via_Tooling_">Managing Endpoints</a> .</p>

</div></td>
</tr>
<tr class="odd">
<td><strong>Add Property</strong></td>
<td>Click this link if you want to add properties to the endpoint. See <a href="https://docs.wso2.com/display/EI650/Properties+Reference">Properties Reference</a> for further information on the available properties.</td>
</tr>
</tbody>
</table>
