# Bean Mediator

!!! info

Note

Please note that this feature is deprecated.


The **Bean Mediator** is used to manipulate a JavaBean that is bound to
the Synapse message context as a property. Classes of objects
manipulated by this mediator need to follow the JavaBeans specification.

!!! info

The Bean mediator is a
[content-aware](ESB-Mediators_119131045.html#ESBMediators-Content-awareness)
mediator.


------------------------------------------------------------------------

[Syntax](#BeanMediator-Syntax) \|
[Configuration](#BeanMediator-Configuration) \|
[Example](#BeanMediator-Example)

------------------------------------------------------------------------

### Syntax

``` xml
    <bean action="CREATE | REMOVE | SET_PROPERTY | GET_PROPERTY" var="string" 
      [class="string"] [property="string"] 
      [value="string | {xpath}"] />
```

  

------------------------------------------------------------------------

### Configuration

The parameters available to configure the Bean mediator are as follows.

<table>
<thead>
<tr class="header">
<th>Field Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Class</strong></td>
<td>The class on which the action selected for the <strong>Action</strong> parameter is performed by the Beanstalks manager.</td>
</tr>
<tr class="even">
<td><strong>Action</strong></td>
<td>The action to be carried out by the Bean mediator. The possible values are:
<ul>
<li><strong>CREATE</strong> : This action creates a new JavaBean.</li>
<li><strong>REMOVE</strong> : This action removes an existing JavaBean.</li>
<li><strong>SET_PROPERTY</strong> : This action sets a property of an existing JavaBean.</li>
<li><strong>GET_PROPERTY</strong> :This action retrieves a property of an existing JavaBean.</li>
</ul></td>
</tr>
<tr class="odd">
<td><strong>Var</strong></td>
<td>The variable which is created, removed, set or retrieved for the JavaBean based on the value selected for the <strong>Action</strong> parameter.</td>
</tr>
<tr class="even">
<td><strong>Property</strong></td>
<td>The name of the property used to bind the JavaBean to the Synapse client.</td>
</tr>
<tr class="odd">
<td><strong>Value</strong></td>
<td><div class="content-wrapper">
<p>The value of the property used to bind the JavaBean to the Synapse client. The property value can be entered using one of the following methods.</p>
<ul>
<li><strong>Value</strong> : If this is selected, the property value can be entered as a static value.</li>
<li><strong>Expression:</strong> If this is selected, the property value can be entered as a dynamic value. You can enter the XPath expression to evaluate the relevant property value.</li>
</ul>
!!! tip
<p>You can click <strong>NameSpaces</strong> to add namespaces if you are providing an expression. Then the <strong>Namespace Editor</strong> panel would appear where you can provide any number of namespace prefixes and URLs used in the XPath expression.</p>

</div></td>
</tr>
<tr class="even">
<td><strong>Target</strong></td>
<td><p>The element of the message which should be affected by execution of the Bean mediator configuration. The target element can be specified using one of the following methods.</p>
<ul>
<li><strong>Value</strong> : If this is selected, the target element can be entered as a static value. The static value should be entered in the text field.</li>
<li><strong>Expression</strong> : If this is selected, the target element can be evaluated via an XPath expression. The XPath expression should be entered in the text field.</li>
</ul></td>
</tr>
</tbody>
</table>

  

------------------------------------------------------------------------

### Example

In this example, the Bean mediator first creates a JavaBean with the
`         loc        ` as the variable. The next Bean mediator creates
property named `         latitude        ` within the
`         loc        ` variable using the
`         SET_PROPERTY        ` action. The third Bean mediator creates
another property named `         longitude        ` in the same variable
using the `         SET_PROPERTY        ` action.

``` xml
    ...     
    <bean action="CREATE" class="org.ejb.wso2.test.bean.Location" var="loc"></bean>
    <bean action="SET_PROPERTY" property="latitude" value="{//latitude}" var="loc" xmlns:ns3="http://org.apache.synapse/xsd"           xmlns:ns="http://org.apache.synapse/xsd"></bean>
    <bean action="SET_PROPERTY" property="longitude" value="{//longitude}" var="loc" xmlns:ns3="http://org.apache.synapse/xsd" xmlns:ns="http://org.apache.synapse/xsd"></bean>
    ...
```
