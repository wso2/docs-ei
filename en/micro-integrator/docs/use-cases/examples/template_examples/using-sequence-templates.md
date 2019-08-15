# Using Sequence Templates

See the examples given below.

## Example 1
Let's illustrate the sequence template with a simple example. Suppose we have a sequence that logs the text "hello world" in four different languages.

```
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

Instead of printing our "hello world" message for each and every language inside the sequence, we can create a generalized template of these actions, which will accept any greeting message (from a particular language) and log it on screen. For example, let's create the following template named "HelloWorld_Logger". Thus, due to the availability of the Call Template mediator, you are not required to have the message entered in all four languages included in the sequence template configuration itself.

```
<template name="HelloWorld_Logger">
  <parameter name="message"/>
  <sequence>
    <log level="custom">
      <property name="GREETING_MESSAGE" expression="$func:message" />
    </log>
  </sequence>
</template>
```

The following four Call Template mediator configurations populate a sequence template named HelloWorld_Logger with the "hello world" text in four different languages.

``` 
<call-template target="HelloWorld_Logger">
  <with-param name="message" value="HELLO WORLD!!!!!!" />
</call-template>
```

```
<call-template target="HelloWorld_Logger">
   <with-param name="message" value="Bonjour tout le monde!!!!!!" />
</call-template>
```

```
<call-template target="HelloWorld_Logger">
  <with-param name="message" value="Ciao a tutti!!!!!!!" />
</call-template>
```

```
<call-template target="HelloWorld_Logger">
  <with-param name="message" value="???????!!!!!!!" />
</call-template>
```

With our "HelloWorld_Logger" in place, the Call Template mediator can
populate this template with actual hello world messages and execute the
sequence of actions defined within the template like with any other
sequence. This is illustrated below.

```
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

The Call Template mediator points to the same template "HelloWorld_Logger" and passes different arguments to it. In this way, sequence templates make it easy to stereotype different workflows inside the Micro Integrator.

!!! Note
    The `target` attribute is used to specify the sequence template you want to use. The `<with-param>` element is used to parse parameter values to the target sequence template. The parameter names should be the same as the names specified in target template. The parameter value can contain a string, an XPath expression (passed in with curly braces { }), or a dynamic XPath expression (passed in with double curly braces) of which the values are compiled dynamically.

## Example 2

The following Call Template mediator configuration populates a sequence template named `Testtemp` with a dynamic XPath expression.

```
<call-template target="Testtemp">
  <with-param name="message_store" value="<MESSAGE_STORE_NAME>" />
</call-template>
```

The following `Testtemp` template includes a dynamic XPath expression to save messages in a Message Store, which is dynamically set via the message context.

```
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