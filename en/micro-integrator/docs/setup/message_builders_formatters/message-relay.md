# Message Relay

**Message Relay** enables the ESB profile of WSO2 Enterprise Integrator
(WSO2 EI) to pass messages along without building or processing them
unless specifically requested to do so. When Message Relay is enabled,
an incoming message is wrapped inside a default SOAP envelope as binary
content and sent through the ESB. This is useful for scenarios where the
ESB does not need to work on the full message but can work on [message
properties](https://docs.wso2.com/display/EI650/Properties+Reference)
like request URLs or transport headers instead. With Message Relay, the
ESB can achieve a very high throughput.

See also [PassThrough
Transport](https://docs.wso2.com/display/EI650/PassThrough+Transport) .

## Configuring Message Relay

In the `         axis2.xml        ` file, there are two configuration
sections: `         messageBuilders        ` and
`         messageFormatters        ` . The user can replace the expected
content types with the Message Relay builder and formatter to pass these
messages through the ESB profile of WSO2 Enterprise Integrator (WSO2 EI)
without building them.

> Warning Content cannot be altered once the b inary relay is enabled.
Therefore, if you are enabling the binary relay, c ontent-aware

##### Message Relay Builder and Formatter Class Names

|           |                                                                              |
|-----------|------------------------------------------------------------------------------|
| Builder   | `              org.wso2.carbon.relay.BinaryRelayBuilder             `        |
| Formatter | `              org.wso2.carbon.relay.ExpandingMessageFormatter             ` |

##### Sample Configuration for Message Builder

``` java
    <messageBuilders>
            <messageBuilder contentType="application/xml"
                            class="org.wso2.carbon.relay.BinaryRelayBuilder"/>
            <messageBuilder contentType="application/x-www-form-urlencoded"
                            class="org.wso2.carbon.relay.BinaryRelayBuilder"/>
            <messageBuilder contentType="multipart/form-data"
                            class="org.wso2.carbon.relay.BinaryRelayBuilder"/>
            <messageBuilder contentType="text/plain"
                             class="org.wso2.carbon.relay.BinaryRelayBuilder"/>
            <messageBuilder contentType="text/xml"
                             class="org.wso2.carbon.relay.BinaryRelayBuilder"/>
        </messageBuilders>
```

##### Sample Configuration for Message Formatter

``` java
    <messageFormatters>
            <messageFormatter contentType="application/x-www-form-urlencoded"
                              class="org.wso2.carbon.relay.ExpandingMessageFormatter"/>
            <messageFormatter contentType="multipart/form-data"
                              class="org.wso2.carbon.relay.ExpandingMessageFormatter"/>
            <messageFormatter contentType="application/xml"
                              class="org.wso2.carbon.relay.ExpandingMessageFormatter"/>
            <messageFormatter contentType="text/xml"
                             class="org.wso2.carbon.relay.ExpandingMessageFormatter"/>
            <!--<messageFormatter contentType="text/plain"
                             class="org.apache.axis2.format.PlainTextBuilder"/>-->
            <messageFormatter contentType="application/soap+xml"
                             class="org.wso2.carbon.relay.ExpandingMessageFormatter"/>
        </messageFormatters>
```

### Example

If you want the ESB profile of WSO2 EI to receive messages of the
`         image/png        ` content type, add the following
configurations to the `         <EI_HOME>/conf/axis2/axis2.xml        `
file.

In the `         Message Builders        ` section:

``` xml
    <messageBuilder contentType="image/png"
                            class="org.wso2.carbon.relay.BinaryRelayBuilder"/>
```

In the `         Message Formatters        ` section:

``` xml
    <messageFormatter contentType="image/png"
                            class="org.wso2.carbon.relay.ExpandingMessageFormatter"/>
```

## Message Relay Module Policy

Syntax of Relay Module Policy.
```
<wsp:Policy wsu:Id="MessageRelayPolicy" xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy"
                    xmlns:wsmr="http://www.wso2.org/ns/2010/01/carbon/message-relay"
                    xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">
            <wsmr:RelayAssertion>
                <wsp:Policy>
                    <wsp:All>
                        <wsmr:includeHiddenServices>false | false</wsmr:includeHiddenServices>
                        <wsmr:services>
                            <wsmr:service>Name of the service</wsmr:service>*
                        </wsmr:services>
                        <wsmr:builders>
                            <wsmr:messageBuilder contentType="content type of the message" class="message builder implementation class" class="message formatter implementation class"/>
                        </wsmr:builders>
                    </wsp:All>
                </wsp:Policy>
            </wsmr:RelayAssertion>
</wsp:Policy>
```

These are the assertions:

-   **includeHiddenServices** - If this is true message going to the
    services with `          hiddenService         ` parameter will be
    built.
-   **wsmr:services** - Messages going to these services will be built.
-   **wsmr:service** - Name of the service.
-   **wsmr:builders** - Message builders to be used for building the
    message.
-   **wsmr:builder** - A message builder to be used for a content type.

> After changing the policy, the user has to restart the ESB profile of
WSO2 EI for the changes to take effect.

> If the Message Relay is enabled for particular content type, there
cannot be any services with security enabled for that content type.

## Message Relay Module

Message Relay has an `         axis2        ` module as well. This is an
optional feature. This module can be used to build the actual SOAP
message from the messages that went through the Message Relay. See
[Working with Modules](_Working_with_Modules_) .

To enable this module, the user has to enable the relay module globally
in the `         axis2.xml.        `

```
<module ref="relay"/>
```

Also, the user has to put the following phase into the `InFlow` of `axis2`.

```
<phase name="BuildingPhase"/>
```

This module is designed to be used by Admin Services that runs inside
the ESB profile. All the admin services are running with content type:
`application/soap+xml`. So if a user wants to use the
admin console of the ESB profile for receiving messages with content
type `application/soap+xml`, this module should be
used.

Users can configure the module by going to the modules section in
the admin console and then going to the relay module. The module
configuration is specified as a module [policy](_Message_Relay_Module_Policy_).

> After changing the policy, the user has to restart the ESB for changes
to take effect.
