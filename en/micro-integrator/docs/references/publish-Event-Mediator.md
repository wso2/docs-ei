# Publish Event Mediator

The **Publish Event mediator** constructs events and publishes them to
different systems such as BAM and CEP. This is done via event sinks.

!!! info

The Publish Event mediator is a
[content-aware](ESB-Mediators_119131045.html#ESBMediators-Content-awareness)
mediator.


------------------------------------------------------------------------

[Syntax](#PublishEventMediator-Syntax) \|
[Configuration](#PublishEventMediator-Configuration) \|
[Example](#PublishEventMediator-Example)

------------------------------------------------------------------------

### Syntax

``` xml
    <publishEvent>
        <eventSink>String</eventSink>
        <streamName>String</streamName>
        <streamVersion>String</streamVersion>
        <attributes>
            <meta>
                <attribute name="string" type="dataType" default="" (value="literal" | expression="[XPath") />
            </meta>
            <correlation>
                 <attribute name="string" type="dataType" default="" (value="literal" | expression="[XPath") />
            </correlation>
            <payload>
                 <attribute name="string" type="dataType" default="" (value="literal" | expression="[XPath") />
            </payload>
        <arbitrary>
                 <attribute name="string" type="dataType" default="" value="literal" />
            </arbitrary>
        </attributes>
    </publishEvent>
```

  

------------------------------------------------------------------------

### Configuration

Parameters that can be configured for the Publish Event mediator are as
follows.

| Parameter Name     | Description                                                         |
|--------------------|---------------------------------------------------------------------|
| **Stream Name**    | The name of the stream to be used for sending data.                 |
| **Stream Version** | The version of the stream to be used for sending data.              |
| **EventSink**      | The name of the event sink to which the events should be published. |

You can add the following four types of attributes to a Publish Event
mediator configuration.

**Meta Attributes** : The list of attributes which are included in the
Meta section of the event.  
**Correlated Attributes** : The list of attributes that are included in
the Correlated section of the event.  
**Payload Attributes** : The list of attributes that are included in the
Payload section of the event.  
**Arbitrary Attributes** : The list of attributes that are included in
the Arbitrary section of the event.

The parameters that are available to configure an individual attribute
are as follows.

<table>
<thead>
<tr class="header">
<th>Parameter Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Attribute Name</strong></td>
<td>The name of the attribute.</td>
</tr>
<tr class="even">
<td><strong>Attribute Value</strong></td>
<td><p>This parameter specifies whether the value of the attribute should be a static value or an expression.</p>
<ul>
<li><strong>Value</strong> : Select this if the attribute value should be a static value, and enter the relevant value in the <strong>Value/Expression</strong> parameter.</li>
<li><strong>Expression</strong> : Select this if the attribute value should be evaluated via an XPath expression, and enter the relevant expression in the <strong>Value/Expression</strong> parameter.</li>
</ul></td>
</tr>
<tr class="odd">
<td><strong>Value/Expression</strong></td>
<td>The value of the attribute. You can enter a static value, or an expression to evaluate the value depending on the selection made in the <strong>Attribute Value</strong> parameter.</td>
</tr>
<tr class="even">
<td><strong>Type</strong></td>
<td>The data type of the attribute.</td>
</tr>
<tr class="odd">
<td><strong>Action</strong></td>
<td>Click <strong>Delete</strong> in the relevant row to delete an attribute.</td>
</tr>
</tbody>
</table>

  

------------------------------------------------------------------------

### Example

In this configuration, the Publish Event mediator uses four attributes
to extract information from the ESB profile . This information is
published in an event sink in the ESB profile named
`         sample_event_sink        ` .

``` xml
    <publishEvent>
      <eventSink>sample_event_sink</eventSink>
      <streamName>stream_88</streamName>
      <streamVersion>1.1.2</streamVersion>
      <attributes>
         <meta>
            <attribute name="http_method"
                       type="STRING"
                       defaultValue=""
                       expression="get-property('axis2', 'HTTP_METHOD')"/>
            <attribute name="destination"
                       type="STRING"
                       defaultValue=""
                       expression="get-property('To')"/>
         </meta>
         <correlation>
            <attribute name="date"
                       type="STRING"
                       defaultValue=""
                       expression="get-property('SYSTEM_DATE')"/>
         </correlation>
         <payload>
            <attribute xmlns:m0="http://services.samples"
                       name="symbol"
                       type="STRING"
                       defaultValue=""
                       expression="$body/m0:getQuote/m0:request/m0:symbol"/>
         </payload>
      </attributes>
    </publishEvent>
```
