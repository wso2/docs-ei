# Working with Templates

The ESB profile configuration language is a very powerful and robust way
of driving enterprise data/messages through the ESB profile mediation
engine. However, a large number of configuration files in the form of
[sequences](https://docs.wso2.com/display/EI650/Mediation+Sequences) ,
[endpoints](https://docs.wso2.com/display/EI650/Working+with+Endpoints)
, [proxy
services](https://docs.wso2.com/display/EI650/Working+with+Proxy+Services)
, and transformations can be required to satisfy all the mediation
requirements of your system. To keep your configurations manageable,
it's important to avoid scattering configuration files across different
locations and to avoid duplicating redundant configurations.

**Templates** help minimize this redundancy by creating prototypes that
users can use and reuse when needed. This is very much analogous to
classes and instances of classes: a template is a class that can be used
to wield instance objects such as templates and endpoints. Thus,
templates are an ideal way to improve reusability and readability of
configurations/XMLs. Additionally, users can use predefined templates
that reflect common [enterprise integration
patterns](https://docs.wso2.com/display/EIP/Enterprise+Integration+Patterns+with+WSO2+Enterprise+Integrator)
for rapid development of message/mediation flows in the ESB profile.

Templates comes in two different forms.

-   [Endpoint Template](_Endpoint_Template_) - Defines a templated form
    of an endpoint. An endpoint template can parameterize an endpoint
    defined within it. You invoke an endpoint template as follows:

    ``` java
        <endpoint template=”name” ...>
        <parameter name=”name” value=”value”/>
        .....
        </endpoint>
    ```

-   Sequence Template - Defines a templated form of a sequence in
    the ESB profile. A sequence template can parameterize XPath
    expressions used within a sequence that is defined inside a
    template. You invoke a sequence template with the [Call Template
    mediator](https://docs.wso2.com/display/EI650/Call+Template+Mediator)
    by passing in the parameter values as follows:

    ``` java
            <call-template target=”template” >
            <parameter name=”name” value=”value”/>
            .....
            </call-template>
    ```

For more information on creating and using these templates, see
[Endpoint Template](_Endpoint_Template_) and Sequence Template .

## Endpoint Template

**Endpoint template** is a generalized form of endpoint configuration
used in the ESB profile. Unlike [sequence
templates](_Sequence_Template_) , endpoint templates are always
parametrized using `         $        ` prefixed values (not XPath
expressions).

Template endpoint is the artifact that makes a template of endpoint type
into a concrete endpoint. In other words, an endpoint template would be
useless without a template endpoint referring to it. This is
semantically similar to the relationship between a sequence template and
the Call Template Mediator.

------------------------------------------------------------------------

[Configuration](#EndpointTemplate-Configuration) \|
[Syntax](#EndpointTemplate-Syntax) \| [Template Endpoint
Sample](#EndpointTemplate-TemplateEndpointSample)

------------------------------------------------------------------------

### Configuration

For example, let's say we have two default endpoints with following
hypothetical configurations:

``` java
    <endpoint  name="ep1">
                <default>
                   <suspendOnFailure>
                         <errorCodes>10001,10002</errorCodes>
                         <progressionFactor>1.0</progressionFactor>
                    </suspendOnFailure>
                    <markForSuspension>
                         <retriesBeforeSuspension>5</retriesBeforeSuspension>
                         <retryDelay>0</retryDelay>
                    </markForSuspension>
                </default>
    </endpoint>
```

``` java
    <endpoint  name="ep2">
                <default>
                   <suspendOnFailure>
                         <errorCodes>10001,10003</errorCodes>
                         <progressionFactor>2.0</progressionFactor>
                    </suspendOnFailure>
                    <markForSuspension>
                         <retriesBeforeSuspension>3</retriesBeforeSuspension>
                         <retryDelay>0</retryDelay>
                    </markForSuspension>
                </default>
    </endpoint>
```

We can see that these two endpoints have different set of error codes
and different progression factors for suspension. Furthermore, the
number of retries is different between them. By defining a endpoint
template, these two endpoints can be converged to a generalized form.
This is illustrated in the following:

``` java
    <template name="ep_template">
         <parameter name="codes"/>
         <parameter name="factor"/>
         <parameter name="retries"/>
         <endpoint name="$name">
                <default>
                    <suspendOnFailure>
                         <errorCodes>$codes</errorCodes>
                         <progressionFactor>$factor</progressionFactor>
                    </suspendOnFailure>
                    <markForSuspension>
                         <retriesBeforeSuspension>$retries</retriesBeforeSuspension>
                         <retryDelay>0</retryDelay>
                    </markForSuspension>
                </default>
         </endpoint>
    </template>
```

!!! info

Note

`         $        ` is used to parametrize configuration and
`         $name        ` is an implicit/default parameter.


Since we have a template defined, we can use template endpoints to
create two concrete endpoint instances with different parameter values
for this scenario. This is shown below.

``` java
    <endpoint name="ep1" template="ep_template">
           <parameter name="codes" value="10001,10002" />
           <parameter name="factor" value="1.0" />
           <parameter name="retries" value="5" />
    </endpoint>
```

``` java
    <endpoint name="ep2" template="ep_template">
           <parameter name="codes" value="10001,10003" />
           <parameter name="factor" value="2.0" />
           <parameter name="retries" value="3" />
    </endpoint>
```

As with the Call Template Mediator, the above template endpoint will
stereotype endpoints with customized configuration parameters. This
makes it very easy to understand and maintain certain ESB
profile configurations and improves re-usability in a certain way.

### Syntax

``` java
    <template name="string">
       <!-- parameters this endpoint template will be supporting -->
       (
       <parameter name="string"/>
       ) *
       <!--this is the in-line endpoint of the template    -->
       <endpoint [name="string"] >
               address-endpoint | default-endpoint | wsdl-endpoint | load-balanced-
    endpoint | fail-over-endpoint
         </endpoint>
    </template>
```

Similar to sequence templates, endpoint templates are defined as top
level element (with the name specified by the name attribute) of the ESB
profile configuration. Parameters (for example,
`         <parameter>        ` ) are the inputs supported by this
endpoint template. These endpoint template parameters can be referred by
`         $        ` prefixed parameter name. For example, parameter
named `         foo        ` can be referred by `         $foo        `
. Most of the parameters of the endpoint definition can be parametrized
with `         $        ` prefixed values. This is shown in the
following extract:

``` java
    <template name="sample_ep_template">
        <endpoint>
                <parameter name="foo"/>
                <parameter name="bar"/>
                <default>
                   <suspendOnFailure>
                         <errorCodes>$foo</errorCodes>
                         <progressionFactor>$bar</progressionFactor>
                    </suspendOnFailure>
                    <markForSuspension>
                         <retriesBeforeSuspension>0</retriesBeforeSuspension>
                         <retryDelay>0</retryDelay>
                    </markForSuspension>
                </default>
         </endpoint>
    </template>
```

!!! info

Note

`         $name        ` and `         $uri        ` are default
parameters that a template can use anywhere within the endpoint template
(usually used as parameters for endpoint name and address attributes).


### Template Endpoint Sample

``` java
    <endpoint [name="string"] [key="string"] template="string">
     <!-- parameter values will be passed on to a endpoint template -->
    (
    <parameter name="string" value="string" />
    ) *
    </endpoint>
```

Template endpoint defines parameter values that can parametrize
endpoint. The `         template        ` attribute points to a target
endpoint template.

!!! info

Note

Parameter names has to be exact match to the names specified in target
endpoint template.


The ESB profile allows add, delete, and edit endpoint templates.

For more information about templates, see [Working with
Templates](_Working_with_Templates_via_Tooling_) .

## Sequence Template

A **Sequence Template** is a parametrized
[sequence](https://docs.wso2.com/display/EI650/Mediation+Sequences) ,
providing an abstract or generic form of a sequence defined in the ESB
profile. Parameters of a template are defined in the form of XPath
statement/s. Callers can invoke the template by populating the
parameters with static values/XPath expressions using the Call Template
Mediator, which makes a sequence template into a concrete sequence.

------------------------------------------------------------------------

[Configuration](#SequenceTemplate-Configuration) \|
[Syntax](#SequenceTemplate-Syntax) \| [Call Template
Mediator](#SequenceTemplate-CallTemplateMediator) \|
[Syntax](#SequenceTemplate-Syntax.1) \|
[Configuration](#SequenceTemplate-Configuration.1) \|
[Examples](#SequenceTemplate-Examples)

------------------------------------------------------------------------

### Configuration

Let's illustrate the sequence template with a simple example. Suppose we
have a sequence that logs the text "hello world" in four different
languages.

``` java
    <sequence>
       <switch source="//m0:greeting/m0:lang" xmlns:m0="http://services.samples">
            <case regex="EN">
                <log level="custom">
                  <property name="GREETING_MESSAGE" value="HELLO WORLD!!!!!!" />
                </log>
            </case>
            <case regex="FR">
                <log level="custom">
                  <property name="GREETING_MESSAGE" value="Bonjour tout le monde!!!!!!" />
                </log>
            </case>
            <case regex="IT">
                <log level="custom">
                  <property name="GREETING_MESSAGE" value="Ciao a tutti!!!!!!!" />
                </log>
            </case>
            <case regex="JPN">
                <log level="custom">
                  <property name="GREETING_MESSAGE" value="???????!!!!!!!" />
                </log>
            </case>
        </switch>
    </sequence>
```

Instead of printing our "hello world" message for each and every
language inside the sequence, we can create a generalized template of
these actions, which will accept any greeting message (from a particular
language) and log it on screen. For example, let's create the following
template named "HelloWorld\_Logger".

``` java
    <template name="HelloWorld_Logger">
       <parameter name="message"/>
       <sequence>
            <log level="custom">
                  <property name="GREETING_MESSAGE" expression="$func:message" />
        </log>
       </sequence>
    </template>
```

With our "HelloWorld\_Logger" in place, the Call Template mediator can
populate this template with actual hello world messages and execute the
sequence of actions defined within the template like with any other
sequence. This is illustrated below.

``` java
    <sequence>
       <switch source="//m0:greeting/m0:lang" xmlns:m0="http://services.samples">
            <case regex="EN">
                <call-template target="HelloWorld_Logger">
            <with-param name="message" value="HELLO WORLD!!!!!!" />
            </call-template>
            </case>
            <case regex="FR">
            <call-template target="HelloWorld_Logger">
            <with-param name="message" value="Bonjour tout le monde!!!!!!" />
            </call-template>
            </case>
        <case regex="IT">
            <call-template target="HelloWorld_Logger">
            <with-param name="message" value="Ciao a tutti!!!!!!!" />
            </call-template>
            </case>
        <case regex="JPN">
            <call-template target="HelloWorld_Logger">
            <with-param name="message" value="???????!!!!!!!" />
            </call-template>
            </case>
        </switch>
    
    </sequence>
```

The Call Template mediator points to the same template
"HelloWorld\_Logger" and passes different arguments to it. In this way,
sequence templates make it easy to stereotype different workflows inside
the ESB profile.

  

------------------------------------------------------------------------

### Syntax

``` java
    <template name="string">
       <!-- parameters this sequence template will be supporting -->
       (
       <parameter name="string"/>
       ) *
       <!--this is the in-line sequence of the template     -->
       <sequence>
            mediator+
       </sequence>
     </template>
```

The sequence template is a top-level element defined by the name
attribute in the ESB profile configuration. Both endpoint and sequence
template starts with a template element.

The parameters available to configure the Sequence Template are as
follows.

| Parameter Name      | Description                                                       |
|---------------------|-------------------------------------------------------------------|
| Name                | The name of the Sequence Template                                 |
| onError             | Select the error sequence that needs to be invoked.               |
| Trace Enabled       | Whether or not trace is to be enabled for the sequence.           |
| Statistics Enabled  | Whether or not statistics is to be enabled for the sequence.      |
| Template Parameters | The input parameter that are supported by this Sequence Template. |

Sequence template parameters can be referenced using an XPath expression
defined inside the in-line sequence. For example, the parameter named
"foo" can be referenced by the Property mediator (defined inside the
in-line sequence of the template) in the following ways:

`         <property name=”fooValue” expression=”$func:foo” />        `

or

`         <property name=”fooValue” expression=”get-property('foo','func')” />        `

Using function scope or "?func?" in the XPath expression allows us to
refer to a particular parameter value passed externally by an invoker
such as the Call Template mediator.

  

------------------------------------------------------------------------

### Call Template Mediator

The Call Template mediator allows you to construct a sequence by passing
values into a [sequence
template](https://docs.wso2.com/display/EI650/Sequence+Template) . !!!
info

This is currently only supported for special types of mediators such as
the [Iterator](https://docs.wso2.com/display/EI650/Iterate+Mediator) and
[Aggregate
Mediators](https://docs.wso2.com/display/EI650/Aggregate+Mediator) ,
where actual XPath operations are performed on a different SOAP message,
and not on the message coming into the mediator.


### Syntax

``` java
    <call-template target="string">
       <!-- parameter values will be passed on to a sequence template -->
       (
        <!--passing plain static values -->
       <with-param name="string" value="string" /> |
        <!--passing xpath expressions -->
       <with-param name="string" value="{string}" /> |
        <!--passing dynamic xpath expressions where values will be compiled
    dynamically-->
       <with-param name="string" value="{{string}}" /> |
       ) *
       <!--this is the in-line sequence of the template    -->
     </call-template>
```

You use the `          target         ` attribute to specify the
sequence template you want to use. The `          <with-param>         `
element is used to parse parameter values to the target sequence
template. The parameter names should be the same as the names specified
in target template. The parameter value can contain a string, an XPath
expression (passed in with curly braces { }), or a dynamic XPath
expression (passed in with double curly braces) of which the values are
compiled dynamically.

### Configuration

The parameters available to configure the Call-Template mediator are as
follows.

| Parameter Name      | Description                                                                                                             |
|---------------------|-------------------------------------------------------------------------------------------------------------------------|
| **Target Template** | The sequence template to which values should be passed. You can select a template from the **Available Templates** list |

When a target template is selected, the parameter section will be
displayed as shown below if the sequence template selected has any
parameters. This enables parameter values to be parsed into the sequence
template selected.

<table>
<thead>
<tr class="header">
<th>Parameter Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Parameter Name</strong></td>
<td>The name of the parameter.</td>
</tr>
<tr class="even">
<td><strong>Parameter Type</strong></td>
<td><p>The type of the parameter. Possible values are as follows.</p>
<ul>
<li><strong>Value</strong> : Select this to define the parameter value as a static value. This value should be entered in the <strong>Value / Expression</strong> parameter.</li>
<li><strong>Expression</strong> : Select this to define the parameter value as a dynamic value. The XPath expression to calculate the parameter value should be entered in the <strong>Value / Expression</strong> parameter.</li>
</ul></td>
</tr>
<tr class="odd">
<td><strong>Value / Expression</strong></td>
<td>The parameter value. This can be a static value, or an XPath expression to calculate a dynamic value depending on the value you selected for the <strong>Parameter Type</strong> parameter.</td>
</tr>
<tr class="even">
<td><strong>Action</strong></td>
<td>Click <strong>Delete</strong> <strong></strong> to delete a parameter.</td>
</tr>
</tbody>
</table>

### Examples

Following examples demonstrate different usecases of the Call Template
mediator.

#### Example one

The following four Call Template mediator configurations populate a
sequence template named HelloWorld\_Logger with the "hello world" text
in four different languages.

``` xml
    <call-template target="HelloWorld_Logger">
        <with-param name="message" value="HELLO WORLD!!!!!!" />
    </call-template>
```

``` xml
    <call-template target="HelloWorld_Logger">
        <with-param name="message" value="Bonjour tout le monde!!!!!!" />
    </call-template>
```

``` xml
    <call-template target="HelloWorld_Logger">
        <with-param name="message" value="Ciao a tutti!!!!!!!" />
    </call-template>
```

``` xml
    <call-template target="HelloWorld_Logger">
        <with-param name="message" value="???????!!!!!!!" />
    </call-template>
```

The sequence template can be configured as follows to log any greetings
message passed to it by the Call Template mediator. Thus, due to the
availability of the Call Template mediator, you are not required to have
the message entered in all four languages included in the sequence
template configuration itself.

``` java
    <template name="HelloWorld_Logger">
       <parameter name="message"/>
       <sequence>
            <log level="custom">
                  <property expression="$func:message" name="GREETING_MESSAGE"/>
        </log>
       </sequence>
    </template>
```

See [Sequence
Template](https://docs.wso2.com/display/EI650/Sequence+Template) for a
more information about this scenario.

#### Example two

The following Call Template mediator configuration populates a sequence
template named `          Testtemp         ` with a dynamic XPath
expression.

``` xml
    <call-template target="Testtemp">
        <with-param name="message_store" value="<MESSAGE_STORE_NAME>" />
    </call-template>
```

The following `          Testtemp         ` template includes a dynamic
XPath expression to save messages in a Message Store, which is
dynamically set via the message context.

``` java
    <template name="Testtemp">
            <parameter name="message_store"/>
            <sequence>
                <log level="custom">
                    <property expression="$func:message_store"
                        name="STORENAME"
                        xmlns:ns="http://org.apache.synapse/xsd"
                        xmlns:ns2="http://org.apache.synapse/xsd" xmlns:soapenv="http://www.w3.org/2003/05/soap-envelope"/>
                </log>
                <store messageStore="{$func:message_store}"
                    xmlns:ns="http://org.apache.synapse/xsd"
                    xmlns:ns2="http://org.apache.synapse/xsd" xmlns:soapenv="http://www.w3.org/2003/05/soap-envelope"/>
            </sequence>
        </template>
```

For more information, see [Adding a New Sequence
Template](_Working_with_Templates_via_Tooling_) .