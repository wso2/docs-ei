# Event Mediator

!!! info

Note

Please note that this feature is deprecated.


  

The **Event Mediator** redirects incoming events to the specified event
topic. For more information, see [Working with Topics and
Events](https://docs.wso2.com/display/EI650/Working+with+Topics+and+Events)
.

!!! info

The Event mediator is a
[content-aware](ESB-Mediators_119131045.html#ESBMediators-Content-awareness)
mediator.


------------------------------------------------------------------------

[Syntax](#EventMediator-Syntax) \|
[Configuration](#EventMediator-Configuration) \|
[Examples](#EventMediator-Examples)

------------------------------------------------------------------------

### Syntax

``` java
    <event xmlns="http://ws.apache.org/ns/synapse" topic="" [expression=""] />
```

------------------------------------------------------------------------

### Configuration

The parameters available for configuring the Event mediator are as
follows:

<table>
<thead>
<tr class="header">
<th>Parameter Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Topic Type</strong></td>
<td><p>The type of topic. The available options are as follows.</p>
<ul>
<li><strong>Static</strong> : If this is selected, the topic to which the events are published is a static value.</li>
<li><strong>Dynamic</strong> : If this is selected, the topic to which the events are published is a dynamic value that should be derived via an XPath expression.</li>
</ul></td>
</tr>
<tr class="even">
<td><strong>Topic</strong></td>
<td><div class="content-wrapper">
<p>The topic to which the events should be published. You can enter a static value or an XPath expression based on the option you selected for the <strong>Topic Type</strong> parameter.</p>
!!! info
<p>Tip</p>
<p>You can click <strong>NameSpaces</strong> to add namespaces when you are providing an expression. Then the <strong>Namespace Editor</strong> panel would appear where you can provide any number of namespace prefixes and URLs used in the XPath expression.</p>

</div></td>
</tr>
<tr class="odd">
<td><strong>Expression</strong></td>
<td><div class="content-wrapper">
<p>The XPath expression used to build the message to be published to the specified topic.</p>
!!! info
<p>Tip</p>
<p>You can click <strong>NameSpaces</strong> to add namespaces when you are providing an expression. Then the <strong>Namespace Editor</strong> panel would appear where you can provide any number of namespace prefixes and URLs used in the XPath expression.</p>

</div></td>
</tr>
</tbody>
</table>

  

------------------------------------------------------------------------

### Examples

In this example, when an event notification comes to the
`         EventingProxy        ` proxy service, they are processed by
the `         PublicEventSource        ` sequence, which logs the
messages and publishes them to the topic
`         SampleEventSource        ` . Services that subscribe to the
topic `         SampleEventSource        ` will then receive these
messages.

``` java
    <!-- Simple Eventing configuration -->
     <definitions xmlns="http://ws.apache.org/ns/synapse">
    
         <sequence name="PublicEventSource" >
                <log level="full"/>
                <event topic="SampleEventSource"/>
         </sequence>
    
         <proxy name="EventingProxy">
             <target inSequence="PublicEventSource" />
         </proxy>
     </definitions>
```

#### Sample

See the following sample for another example.

[Sample 460: Introduction to Eventing and Event
Mediator](https://docs.wso2.com/display/EI6xx/Sample+460%3A+Introduction+to+Eventing+and+Event+Mediator)
.
