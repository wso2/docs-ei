# POJOCommand Mediator

!!! info

Note

Please note that this feature is deprecated.

!!! info

Tip

This mediator implements the popular [command
pattern.](http://en.wikipedia.org/wiki/Command_pattern)


The **POJOCommand** (or Command) **Mediator** creates an instance of the
specified command class, which may implement the
`         org.apache.synapse.Command        ` interface or should have a
public void method `         public void execute()        ` . If any
properties are specified, the corresponding *setter* methods are invoked
on the class before each message is executed. It should be noted that a
new instance of the POJOCommand class is created to process each
message. After execution of the POJOCommand Mediator, the new value
returned by a call to the corresponding *getter* method is stored back
in the message or in the context depending on the
`         action        ` attribute of the property.

The `         action        ` attribute may specify whether this
behavior is expected or not via the `         Read        ` ,
`         Update        ` and `         ReadAndUpdate        `
properties.

------------------------------------------------------------------------

[Syntax](#POJOCommandMediator-Syntax) \|
[Configuration](#POJOCommandMediator-Configuration)

------------------------------------------------------------------------

### Syntax

``` java
    <pojoCommand name="class-name">
       (
       <property name="string" value="string"/> |
       <property name="string" context-name="literal" [action=(ReadContext | UpdateContext | ReadAndUpdateContext)]>
        (either literal or XML child)
       </property> |
       <property name="string" expression="xpath" [action=(ReadMessage | UpdateMessage | ReadAndUpdateMessage)]/>
       ) *
     </pojoCommand>
```

------------------------------------------------------------------------

### Configuration

To load the POJOCommand class, enter the class name in the **Class
Name** parameter and click **Load Class** .

Parameters available to configure properties for the POJOCommand
mediator are as follows.

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Property Name</strong></td>
<td>The name of the property. This will be automatically loaded from the class.</td>
</tr>
<tr class="even">
<td><strong>Read Info</strong></td>
<td><p>The value to set for the property. You can select one of the following sources in the <strong>From</strong> field.</p>
<ul>
<li><strong>Value</strong> : Select this if you want the property value to be a static value. This static value should be entered in the <strong>Value</strong> field.</li>
<li><strong>Message</strong> : Select this if you want to read the property value from an incoming message. The XPath expression to execute on the relevant message should be entered in the <strong>Value</strong> <strong></strong> field.</li>
<li><strong>Context</strong> : Select this if you want to read a value from message context properties. The relevant property key should be entered in the <strong>Value</strong> field.</li>
</ul></td>
</tr>
<tr class="odd">
<td><strong>Update Info</strong></td>
<td><p>This parameter specifies the action to be executed on the property value. You can select one for the following actions in the <strong>To</strong> field.</p>
<ul>
<li><strong>None</strong> : Select this if no activity should be performed on the property value.</li>
<li><strong>Message</strong> : Select this if you want to update the message. The XPath expression of the element you want to update should be entered in the <strong>Value</strong> field.</li>
<li><strong>Context</strong> : Select this if you want to update properties (message context). The relevant property key should be entered in the <strong>Value</strong> field.</li>
</ul></td>
</tr>
<tr class="even">
<td><strong>Action</strong></td>
<td>Click <strong>Delete</strong> to delete a property.</td>
</tr>
</tbody>
</table>
