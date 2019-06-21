# Conditional Router Mediator

!!! info

Note

Please note that the Conditional Router Mediator is deprecated and will
be removed from the next release.


The **Conditional Router Mediator** specifies how a message should be
routed based on given conditions. The specified target
[sequence](_Mediation_Sequences_) is applied if the condition of the
mediator evaluates to `         true        ` .

!!! info

The Conditional Router mediator is a
[content-aware](ESB-Mediators_119131045.html#ESBMediators-Content-awareness)
mediator.


------------------------------------------------------------------------

[Syntax](#ConditionalRouterMediator-Syntax) \|
[Configuration](#ConditionalRouterMediator-Configuration) \|
[Example](#ConditionalRouterMediator-Example)

------------------------------------------------------------------------

### Syntax

``` java
    <conditionalRouter continueAfter="(true|false)">
        <route breakRoute="(true|false)">
          <condition ../>
          <target ../>
        </route>+
    </conditionalRouter>
```

------------------------------------------------------------------------

### Configuration

The parameters available to configure the Conditional Router mediator
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
<td><strong>Continue after Routing</strong></td>
<td><p>This parameter specifies whether the mediation flow should/should not continue after executing the conditional router mediator. Possible values are as follows.</p>
<ul>
<li><strong>Yes/True</strong> : If this is selected, mediation continues to execute (any other mediators specified) after the conditiional router mediator.</li>
<li><strong>No/False</strong> : If this is selected, mediation discontinues after executing the conditiional router mediator . This is the default value.</li>
</ul></td>
</tr>
<tr class="even">
<td><strong>Add Route</strong></td>
<td><div class="content-wrapper">
The conditional route will be added as a child to the Conditional Router mediator in the mediator tree as shown below.<br />
You can add multiple conditional routes to a Conditional Router mediator by clicking on this link.
</div></td>
</tr>
</tbody>
</table>

Click on the conditional route in the mediator tree to configure it. The
parameters available to configure a conditional route are as follows.

<table>
<thead>
<tr class="header">
<th>Parameter Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Break after route</strong></td>
<td><p>You can specify this for each conditional route of the conditional route mediator. It specifies whether the router should/should not continue after executing the specified conditional route.</p>
<ul>
<li><strong>Yes/True</strong> : If this is selected, a matching route would break the router, so that it does not continue to execute the next conditional route.</li>
<li><strong>No/False</strong> : If this is selected, the router continues to execute the next conditional route defined in the conditional router mediator.</li>
</ul></td>
</tr>
<tr class="even">
<td><strong>Evaluator Expression</strong></td>
<td>The expression to evaluate the condition based on which the target <a href="_Mediation_Sequences_">mediation sequence</a> should be applied.</td>
</tr>
<tr class="odd">
<td><strong>Target Sequence</strong></td>
<td>The <a href="_Mediation_Sequences_">mediation sequence</a> to be applied if the expression entered in the <strong>Evaluator Expression</strong> parameter evaluates to <code>             true            </code> .</td>
</tr>
</tbody>
</table>

### Example

See [Sample 157: Conditional Router for Routing Messages based on HTTP
URL](https://docs.wso2.com/display/EI6xx/Sample+157%3A+Conditional+Router+for+Routing+Messages+based+on+HTTP+URL%2C+HTTP+Headers+and+Query+Parameters)
for an example of the Conditional Router mediator.
