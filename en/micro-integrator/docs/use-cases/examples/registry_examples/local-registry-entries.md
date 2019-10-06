# Sequences and Endpoints as Local Registry Entries

## Example use case

This sample demonstrates the functionality of local registry entry definitions, reusable endpoints, and sequences. This example uses a sequence named *main* that specifies the main mediation rules to be executed. This is equivalent to directly
specifying the mediators of the main sequence within the `\<definitions\>` tag. This sample scenario is a
recommended approach for non-trivial configurations.

## Synapse configurations

Following are the integration artifacts that we can used to implement this scenario. See the instructions on how to [build and run](#build-and-run) this example.

```xml tab='Proxy Service'
<proxy name="MainProxy" startOnLoad="true" transports="http https" xmlns="http://ws.apache.org/ns/synapse">
    <target>
        <inSequence>
            <property name="direction" scope="default" type="STRING" value="incoming"/>
            <sequence key="stockquote"/>
        </inSequence>
        <outSequence>
            <send/>
        </outSequence>
        <faultSequence/>
    </target>
</proxy>
```

```xml tab='Sequence'
<sequence name="stockquote" trace="disable" xmlns="http://ws.apache.org/ns/synapse">
    <!-- log the message using the custom log level. illustrates custom properties for log -->
    <log level="custom">
        <property name="Text" value="Sending quote request"/>
        <property expression="get-property('version')" name="version"/>
        <property expression="get-property('direction')" name="direction"/>
    </log>
    <!-- send message to real endpoint referenced by key "simple" endpoint definition -->
    <send>
        <endpoint key="simple"/>
    </send>
</sequence>
```

```xml tab='Endpoint'
<endpoint name="simple" xmlns="http://ws.apache.org/ns/synapse">
    <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
</endpoint>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. Create the [proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service), [sequence](../../../../develop/creating-artifacts/creating-reusable-sequences), and [endpoint](../../../../develop/creating-an-endpoint) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Setup a backend server.

Analyze the mediation log on the Micro Integrator start-up console.

You will see that a sequence named *main* is executed. Then, for the
incoming message flow, the In Mediator executes and it calls the
sequence named *stockquote*.

```bash
DEBUG SequenceMediator - Sequence mediator <main> :: mediate()
DEBUG InMediator - In mediator mediate()
DEBUG SequenceMediator - Sequence mediator <stockquote> :: mediate()
```

You will also see that the *stockquote* sequence is executed. and that
the l og mediator dumps a simple *text/string* property, which is the
result of an XPath evaluation that picks up the *version* key
and a second result of an XPath evaluation that picks up a local message
property set previously by the property mediator. The
`         get-property()        ` XPath extension function is able to
read message properties local to the current message, local or remote
registry entries, Axis2 message context properties as well as transport
headers. The local entry definition for *version* defines a simple
*text/string* registry entry, which is visible to all messages that pass
through the Micro Integrator.

```bash
[HttpServerWorker-1] INFO LogMediator - Text = Sending quote request, version = 0.1, direction = incoming
[HttpServerWorker-1] DEBUG SendMediator - Send mediator :: mediate()
[HttpServerWorker-1] DEBUG AddressEndpoint - Sending To: http://localhost:9000/services/SimpleStockQuoteService
```