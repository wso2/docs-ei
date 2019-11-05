# Mediation Sequences

A mediation sequence is a set of [mediators](../../../references/mediators/about-mediators) organized into a logical flow, allowing you to implement pipes and filter patterns. The mediators in the sequence will perform the necessary message processing and route the message to the required destination. 

Typically, the required mediation sequences are defined within the proxy service or the REST API. This includes an [In](#inout-sequences) sequence, an [Out](#inout-sequences) sequence, and a [default fault](#fault-sequences) sequence. 

All messages that are not destined for a proxy service or REST API are sent through the [main sequence](#main-sequence). Some other sequences ([named sequences](#named-sequences)) are defined independant of the proxy service, or REST API, and then reused in one or several proxy services, REST APIs for efficiency.

![mediation sequence](../../assets/img/concepts/sequence.png)

## IN/OUT Sequences

Once a request message is received by the [proxy service](../../../references/synapse-properties/proxy-service-properties), [REST API](../../../references/synapse-properties/rest-api-properties), [inbound endpoint](../../../references/synapse-properties/inbound-endpoints/about-inbound-endpoints), or even the [main sequence](#main-sequence), the message is injected to the **IN** sequence. The **OUT** sequence defines the mediation logic that processes the response message before sending the response back to the client.

## Fault Sequences

Fault sequences are used for [error handling](../../references/synapse-properties/endpoint-properties/#endpoint-error-handling-properties) in message mediation. A fault sequence is a collection of [mediators](#mediators) just like any other [sequence](#mediation-sequences), and it can be associated with another [sequence](#mediation-sequences), [proxy service](../../../references/synapse-properties/proxy-service-properties), or [REST API](../../../references/synapse-properties/rest-api-properties). When an error occurs in the mediation logic, the message that triggered the error is delegated to the specified fault sequence. The mediators in the fault sequence can then log the erroneous message, forward it to a special error-tracking service, and send a SOAP fault back to the client indicating the error or even send an email to the system admin.

![fault sequence](../../assets/img/concepts/fault-sequence.png)

If a fault sequence is not specified explicitly, the **default** fault sequence of the proxy service or REST API will be used to handle errors. The default fault sequence will log the message, the payload, and any error/exception encountered before the [Drop](../../../references/mediators/drop-Mediator) mediator stops further processing. You should configure the fault sequence for error handling instead of simply dropping messages.

## Named Sequences

A named sequence is a custom, reusable sequence that holds a specific mediation logic. You can create named sequences and reuse them within your project. A [fault sequence](#fault-sequences) is an example of a named sequence that can be used to replace the default fault sequence of a proxy service, or REST API. While the [main sequence](#main-sequence), [proxy service](../../../references/synapse-properties/proxy-service-properties), and [REST API](../../../references/synapse-properties/rest-api-properties) contain [IN and OUT](#inout-sequences) sequences to define a specific in flow and out flow of messages, a named sequence is simply a combination of mediators that can be reused within an [IN](#inout-sequences) sequence or [Out](#inout-sequences) sequence.

To reuse a named sequence in multiple integration projects, you need to save it as a [dynamic sequence](#dynamic-sequences).

## Dynamic Sequences

When you create a [named sequence](#named-sequences), you can save it as a **registry resource** in the product's [registry](../../../overview/key-concepts/#registry). This sequence can then be dynamically reused in the mediation flow of any of your integration projects by specifying the registry path.

## Main Sequence

All messages that are not destined for a [proxy service](../../../references/synapse-properties/proxy-service-properties), [REST API](../../../references/synapse-properties/rest-api-properties), or [inbound endpoint](../../../references/synapse-properties/inbound-endpoints/about-inbound-endpoints) are sent through the main sequence. A sequence functions as the main sequence when it is named '<b>main</b>'. By default, the main sequence simply sends a message without mediation. Therefore, to add message mediation, you can add mediators and/or [named sequences](#named-sequences) to the main sequence.

<!--

```
<sequence name="string" [onError="string"] [key="string"] [trace="enable"] [statistics="enable"]>
        mediator*
</sequence>
```
You can list the mediators right in the sequence definition (referred to
as an **in-line sequence** ) and refer to other sequences by name. For
example:

```
<sequence name="foo">
    <log/>
    <property name="test" value="test value"/>
    <sequence key="other_sequence"/>
    <send/>
</sequence>
```

This sequence specifies three mediators in-line: the [log mediator](_Log_Mediator_) , [property mediator](_Property_Mediator_) ,
and the [send mediator](_Send_Mediator_) . It also references the named
sequence "other\_sequence" and therefore uses all the mediators defined
in that sequence.

In addition to mediators and other sequences, you can configure the
following:

-   Create a dynamic sequence by referring to an entry in the registry,
    in which case the sequence will change as the registry entry
    changes.
-   Activate statistics collection by setting the statistics attribute
    to enable. In this mode the sequence will keep track of the number
    of messages processed and their processing times. For more
    information, see [Monitoring WSO2 EI Using WSO2
    Analytics](https://docs.wso2.com/pages/viewpage.action?pageId=51479177)
    .
-   Activate trace collection by setting the trace attribute to enable.
    If tracing is enabled on a sequence, all messages being processed
    through the sequence will write tracing information through each
    mediation step.
-   Use the onError attribute to define a custom error handler sequence.
    If an error occurs while executing the sequence, this error handler
    will be called. If you do not specify an error handler, the fault
    sequence will be used, as described in the next section.
-->