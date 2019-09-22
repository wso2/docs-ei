# Message Relay

**Message Relay** enables WSO2 Micro Integrator to pass messages along without building or processing them
unless specifically requested to do so. When Message Relay is enabled,
an incoming message is wrapped inside a default SOAP envelope as binary
content and sent through the Micro Integrator. This is useful for scenarios where the
Micro Integrator does not need to work on the full message but can work on [message properties](../../references/mediators/property-Mediator.md)
like request URLs or transport headers instead. With Message Relay, the
Micro Integrator can achieve a very high throughput.

See also [PassThrough Transport](../../concepts/messaging-transports.md).

## Configuring Message Relay

The user can replace the expected content types with the Message Relay builder and formatter to pass these
messages through WSO2 Micro Integrator without building them.

!!! Warning 
    Content cannot be altered once the binary relay is enabled. Therefore, if you are enabling the binary relay, content-aware

### Message Relay Builder and Formatter Class Names

-   Builder: `org.wso2.carbon.relay.BinaryRelayBuilder `
-   Formatter: `org.wso2.carbon.relay.ExpandingMessageFormatter `

### Sample Configuration for Message Builder

```toml
[[custom_message_builders]]
class = "org.wso2.carbon.relay.BinaryRelayBuilder"
content_type = "application/json/badgerfish"
```

Content types:
-   application/xml
-   application/x-www-form-urlencoded
-   multipart/form-data
-   text/plain
-   text/xml

### Sample Configuration for Message Formatter

```toml
[[custom_message_formatters]]
class = "org.apache.axis2.json.JSONBadgerfishMessageFormatter"
content_type = "application/json/badgerfish"
```

Content types:
-   application/soap+xml
-   application/xml
-   application/x-www-form-urlencoded
-   multipart/form-data
-   text/plain
-   text/xml

## Example

If you want the Micro Integrator to receive messages of the `image/png` content type, add the following to the deployment.toml file:

```toml tab='Message Builder'
[[custom_message_builders]]
class = "org.wso2.carbon.relay.BinaryRelayBuilder"
content_type = "image/png"
```

```toml tab='Message Formatter'
[[custom_message_formatters]]
class = "org.apache.axis2.json.JSONBadgerfishMessageFormatter"
content_type = "image/png"
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

After changing the policy, the user has to restart the Micro Integrator for the changes to take effect.

If the Message Relay is enabled for particular content type, there
cannot be any services with security enabled for that content type.

## Message Relay Module

Message Relay has an `         axis2        ` module as well. This is an optional feature. This module can be used to build the actual SOAP message from the messages that went through the Message Relay. See [Working with Modules](_Working_with_Modules_) .

To enable this module, the user has to enable the relay module globally in the `         axis2.xml.        `

```
<module ref="relay"/>
```

Also, the user has to put the following phase into the `InFlow` of `axis2`.

```
<phase name="BuildingPhase"/>
```

This module is designed to be used by Admin Services that runs inside the Micro Integrator. All the admin services are running with content type: `application/soap+xml`. So if a user wants to use the admin console of the Micro Integrator for receiving messages with content type `application/soap+xml`, this module should be used.

Users can configure the module by going to the modules section in the admin console and then going to the relay module. The module
configuration is specified as a module [policy](#message-relay-module-policy).

After changing the policy, the user has to restart the Micro Integrator for changes
to take effect.
