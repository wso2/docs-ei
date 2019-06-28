# Address Endpoint

The **Address endpoint** is an [endpoint](_Working_with_Endpoints_)
defined by specifying the EPR (Endpoint Reference) and other attributes
of the configuration.

------------------------------------------------------------------------

[XML configuration](#AddressEndpoint-XMLconfigurationXMLConfiguration)
\| [Parameters](#AddressEndpoint-Parameters)

------------------------------------------------------------------------

### XML configuration

``` html/xml
    <address uri="endpoint address" [format="soap11|soap12|pox|rest|get"] [optimize="mtom|swa"]
        [encoding="charset encoding"]
        [statistics="enable|disable"] [trace="enable|disable"]>
    
        <enableSec [policy="key"]/>?
        <enableAddressing [version="final|submission"] [separateListener="true|false"]/>?
    
        <timeout>
            <duration>timeout duration in milliseconds</duration>
            <responseAction>discard|fault</responseAction>
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
    </address>
```

#### Address endpoint attributes

| Attribute  | Description                                   |
|------------|-----------------------------------------------|
| uri        | EPR of the target endpoint.                   |
| format     | Message format for the endpoint.              |
| optimize   | Method to optimize the attachments.           |
| encoding   | The charset encoding to use for the endpoint. |
| statistics | This enables statistics for the endpoint.     |
| trace      | This enables trace for the endpoint.          |

#### Other elements

##### QoS for the endpoint

QoS (Quality of Service) aspects such as WS-Security and WS-Addressing
may be enabled on messages sent to an endpoint using
`         enableSec        ` and `         enableAddressing        `
elements. Optionally, the WS-Security policies could be specified using
the `         policy        ` attribute.

###### QoS configuration

|                                                                                          |                                                                                                                                                                      |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| enableSec \[policy=" *key* "\]                                                           | This enables WS-Security for the message which is sent to the endpoint. The optional `              policy             ` attribute specifies the WS-Security policy. |
| enableAddressing \[version="final \| submission"\] \[seperateListener=" true \| false"\] | This enables WS-Addressing for the message which is sent to the endpoint. User can specify to have separate listener with version final or submission.               |

##### Endpoint timeout

The parameters available to configure an endpoint time out are as
follows.

<table>
<tbody>
<tr class="odd">
<td><p>duration</p></td>
<td><p>Timeout duration that should elapse before the end point is timed out.</p></td>
</tr>
<tr class="even">
<td><p>responseAction</p></td>
<td><div class="content-wrapper">
<p>This parameter is used to specify the action to perform once an endpoint has timed out. The available options are as follows.</p>
<ul>
<li><code>                discard               </code> : If this is selected, the responses which arrive after the endpoint has timed out will be discarded.</li>
<li><code>                fault               </code> : If this is selected, a fault is triggered when the endpoint is timed out.</li>
</ul>
!!! tip
<p>You can specify a value that is 1 millisecond less than the time duration you specify for the endpoint time out for the <code>               synapse.timeout_handler_interval              </code> property in the <code>               &lt;EI_Home&gt;/conf/synapse.properties              </code> file. This would minimise the number of late responses from the backend.</p>

</div></td>
</tr>
</tbody>
</table>

##### Marking an endpoint for suspension

The `         markForSuspension        ` element contains the following
parameters which affect the suspension of a  endpoint which would be
timed out after a specified time duration.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>errorCodes</p></td>
<td><p>This parameter is used to specify one or more error codes which can cause the endpoint to be marked for suspension when they are returned by the endpoint. Multiple error codes can be specified separated by comas. See <a href="https://svn.apache.org/repos/asf/synapse/trunk/java/modules/core/src/main/java/org/apache/synapse/SynapseConstants.java">SynpaseConstant</a> class for a list of available error codes.</p></td>
</tr>
<tr class="even">
<td><p>retriesBeforeSuspension</p></td>
<td><p>Number of retries before go into suspended state.</p>
<p>The number of times the endpoint should be allowed to retry sending the response before it is marked for suspension.</p></td>
</tr>
<tr class="odd">
<td><p>retryDelay</p></td>
<td><p>The delay between each try.</p></td>
</tr>
</tbody>
</table>

###### Suspending the endpoint on failure

Leaf endpoints(Address and WSDL) can be suspended if they are detected
as failed endpoints. When an endpoint is in in suspended state for a
specified time duration following a failure, it cannot process any new
messages. The following formula determines the wait time before the next
attempt.

`         next suspension time period = Max (Initial Suspension duration * (progression factor*        `
`                              try count                           `
`         *), Maximum Duration)        `

All the variables in the above formula are configuration values used to
calculate the try count. Try count is the number of tries carried out
after the endpoint is suspended. The increase in the try count causes an
increase in the next suspension time period. This time period is bound
to a maximum duration.

The parameters available to configure a suspension of an endpoint due to
failure are as follows.

<table>
<thead>
<tr class="header">
<th>Parameter Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>errorCode</td>
<td><p>A comma separated error code list which can be returned by the endpoint.</p>
<p>This parameter is used to specify one or more error codes which can cause the endpoint to be suspended when they are returned from the endpoint. Multiple error codes can be specified, separated by commas.</p></td>
</tr>
<tr class="even">
<td>initialDuration</td>
<td>The number of milliseconds after which the endpoint should be suspended when it is being suspended for the first time.</td>
</tr>
<tr class="odd">
<td>progressionFactor</td>
<td>The progression factor for the geometric series. See the above formula for a more detailed description.</td>
</tr>
<tr class="even">
<td>maximumDuration</td>
<td>The maximum duration (in milliseconds) to suspend the endpoint.</td>
</tr>
</tbody>
</table>

###### Following are the sample address URI definition.

<table>
<thead>
<tr class="header">
<th><p>Transport</p></th>
<th><p>Sample Address</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>HTTP</p></td>
<td><p>http://localhost:9000/services/SimpleStockQuoteService</p></td>
</tr>
<tr class="even">
<td><p>JMS</p></td>
<td><p>jms:/SimpleStockQuoteService?<br />
transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;<br />
java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;<br />
java.naming.provider.url=tcp://localhost:61616&amp;<br />
transport.jms.DestinationType=topic</p></td>
</tr>
<tr class="odd">
<td><p>Mail</p></td>
<td><p>guest@host</p></td>
</tr>
<tr class="even">
<td><p>VFS</p></td>
<td><p>vfs:file:///home/user/directory\ vfs:"&gt;file:///home/user/file\ vfs:</p></td>
</tr>
<tr class="odd">
<td><p>FIX</p></td>
<td><p>fix://host:port?BeginString=FIX4.4&amp;SenderCompID=WSO2&amp;TargetCompID=APACHE</p></td>
</tr>
</tbody>
</table>

### Parameters

The parameters available to configure the endpoint are as follows.

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
<td>The unique name of the endpoint.</td>
</tr>
<tr class="even">
<td><strong>Address</strong></td>
<td><div class="content-wrapper">
<p>The URL of the endpoint. You can test the availability of the given URL by clicking <strong>Test</strong> .</p>
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
<td><strong>Show Advanced Options</strong></td>
<td><div class="content-wrapper">
<p>This section is used to enter advanced settings for the endpoint. The advanced options specific for the Address endpoint are as follows.</p>
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
<tr class="even">
<td><strong>Add Property</strong></td>
<td>This section is used to add properties to an endpoint.</td>
</tr>
</tbody>
</table>
