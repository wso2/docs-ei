# Custom Inbound Endpoint

The ESB profile of WSO2 Enterprise Integrator (WSO2 EI) supports several
inbound endpoints, but there can be scenarios that require functionality
not provided by the existing inbound endpoints. For example, you might
need an inbound endpoint to connect to a certain back-end server or
vendor specific protocol.

To support such scenarios, you can write your own custom inbound
endpoint by extending the behavior for
[listening](_Listening_Inbound_Endpoints_) ,
[polling](_Polling_Inbound_Endpoints_) , and
[event-based](_Event-Based_Inbound_Endpoints_) inbound endpoints.

### Custom listening inbound endpoint

Following is a sample custom listening inbound endpoint configuration:

``` xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="custom_listener" sequence="request" onError="fault"
                         class="org.wso2.carbon.inbound.custom.listening.SampleListeningEP" suspend="false">
       <parameters>
          <parameter name="sequential">true</parameter>
          <parameter name="inbound.behavior">listening</parameter>
          <parameter name="coordination">true</parameter>
       </parameters>
    </inboundEndpoint>
```

In addition to the parameters provided in the above sample
configuration, you can provide other required parameters based on the
listening behaviour you need to implement.

### Custom listening inbound endpoint parameters

<table>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
<th><p>Possible Values</p></th>
<th><p>Default Value</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>class</code></pre></td>
<td><p>Name of the custom class implementation</p></td>
<td><p>Yes</p></td>
<td><p>A valid class name</p></td>
<td><p>n/a</p></td>
</tr>
<tr class="even">
<td><pre><code>sequence</code></pre>
<p><br />
</p></td>
<td>Name of the sequence message that should be injected</td>
<td>Yes</td>
<td>A valid sequence name</td>
<td>n/a</td>
</tr>
<tr class="odd">
<td><pre><code>onError</code></pre>
<p><br />
</p></td>
<td>Name of the fault sequence that should be invoked in case of failure</td>
<td>Yes</td>
<td><p>A valid fault sequence name</p></td>
<td>n/a</td>
</tr>
<tr class="even">
<td><pre><code>inbound.behavior</code></pre></td>
<td><p>The behaviour of the inbound endpoint</p></td>
<td>Yes</td>
<td><code>             listening            </code></td>
<td>n/a</td>
</tr>
</tbody>
</table>

You can download the maven artifact used in the sample custom listening
inbound endpoint configuration above from [Custom listening inbound
endpoint
sample](https://github.com/wso2-docs/ESB/tree/master/ESB-Artifacts/inbound/custom_inbound_listening)
.

You need to copy the built jar file to the
`         <EI_HOME>/lib        ` directory and restart WSO2 EI to load
the class.

### Custom polling inbound endpoint

Following is a sample custom polling inbound endpoint configuration:

``` html/xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="class" sequence="request" onError="fault"
                                class="org.wso2.carbon.inbound.custom.poll.SamplePollingClient" suspend="false">
       <parameters>
          <parameter name="sequential">true</parameter>
          <parameter name="interval">2000</parameter>
          <parameter name="coordination">true</parameter>
       </parameters>
    </inboundEndpoint>
```

### Custom polling inbound endpoint parameters

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
<th><p>Required</p></th>
<th><p>Possible Values</p></th>
<th><p>Default Value</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>class</code></pre></td>
<td><p>Name of the custom class implementation</p></td>
<td><p>Yes</p></td>
<td><p>A valid class name</p></td>
<td><p>n/a</p></td>
</tr>
</tbody>
</table>

You can download the maven artifact used in the sample custom polling
inbound endpoint configuration above from [Custom polling inbound
endpoint
sample](https://github.com/wso2-docs/ESB/tree/master/ESB-Artifacts/inbound/custom_inbound)
.

You need to copy the built jar file to the
`         <EI_HOME>/lib        ` directory and restart WSO2 EI to load
the class.

### Custom event-based inbound endpoint

Following is a sample custom event-based inbound endpoint configuration:

``` xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="custom_waiting" sequence="request" onError="fault"
                         class="org.wso2.carbon.inbound.custom.wait.SampleWaitingClient" suspend="false">
       <parameters>
          <parameter name="sequential">true</parameter>
          <parameter name="inbound.behavior">eventBased</parameter>
          <parameter name="coordination">true</parameter>
       </parameters>
    </inboundEndpoint>
```

In addition to the parameters provided in the above sample
configuration, you can provide other required parameters based on the
event-based behaviour you need to implement.

### Custom event-based inbound endpoint parameters

<table>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
<th><p>Possible Values</p></th>
<th><p>Default Value</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>class</code></pre></td>
<td><p>Name of the custom class implementation</p></td>
<td><p>Yes</p></td>
<td><p>A valid class name</p></td>
<td><p>n/a</p></td>
</tr>
<tr class="even">
<td><pre><code>sequence</code></pre>
<p><br />
</p></td>
<td>Name of the sequence message that should be injected</td>
<td>Yes</td>
<td>A valid sequence name</td>
<td>n/a</td>
</tr>
<tr class="odd">
<td><pre><code>onError</code></pre>
<p><br />
</p></td>
<td>Name of the fault sequence that should be invoked in case of failure</td>
<td>Yes</td>
<td><p>A valid fault sequence name</p></td>
<td>n/a</td>
</tr>
<tr class="even">
<td><pre><code>inbound.behavior</code></pre></td>
<td><p>The behaviour of the inbound endpoint</p></td>
<td>Yes</td>
<td><code>             event-based            </code></td>
<td>n/a</td>
</tr>
</tbody>
</table>

You can download the maven artifact used in the sample custom
event-based inbound endpoint configuration above from [Custom
event-based inbound endpoint
sample](https://github.com/wso2-docs/ESB/tree/master/ESB-Artifacts/inbound/custom_inbound_waiting)
.

You need to copy the built jar file to the
`         <EI_HOME>/lib        ` directory and restart the the ESB
profile to load the class.

  
