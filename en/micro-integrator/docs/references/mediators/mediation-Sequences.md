# Mediation Sequences

A **mediation sequence** , commonly called a **sequence** , is a tree of
[mediators](_ESB_Mediators_) that you can use in your mediation
workflow. When a message is delivered to a sequence, the sequence sends
it through all its mediators.

![](attachments/30540641/30705246.png)

When you want to work with mediation sequences, you can use the EI
tooling plug-in to create a new sequence as well as to import an
existing sequence, or you can add, edit, and delete sequences via the
Management Console.

### Configuring a mediation sequence

You can define mediation sequences using the Management Console as
described in [Adding a Mediation
Sequence](_Working_with_Sequences_via_Tooling_) . The underlying Synapse
configuration uses the following syntax:

``` html/xml
    <sequence name="string" [onError="string"] [key="string"] [trace="enable"] [statistics="enable"]>
        mediator*
    </sequence>
```

You can list the mediators right in the sequence definition (referred to
as an **in-line sequence** ) and refer to other sequences by name. For
example:

``` html/xml
    <sequence name="foo">
        <log/>
        <property name="test" value="test value"/>
        <sequence key="other_sequence"/>
        <send/>
    </sequence>
```

This sequence specifies three mediators in-line: the [log
mediator](_Log_Mediator_) , [property mediator](_Property_Mediator_) ,
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
    Analytics](https://docs.wso2.com/display/TESB/Monitoring+WSO2+ESB+Using+WSO2+Analytics)
    .
-   Activate trace collection by setting the trace attribute to enable.
    If tracing is enabled on a sequence, all messages being processed
    through the sequence will write tracing information through each
    mediation step.
-   Use the onError attribute to define a custom error handler sequence.
    If an error occurs while executing the sequence, this error handler
    will be called. If you do not specify an error handler, the fault
    sequence will be used, as described in the next section.

### About the main and fault sequences

A mediation configuration holds two special sequences named **main** and
**fault** . All messages that are not destined for [proxy
services](https://docs.wso2.com/display/EI650/Working+with+Proxy+Services)
are sent through the main sequence. By default, the main sequence simply
sends a message without mediation, so to add message mediation, you add
mediators and/or named sequences in the main sequence.

By default, the fault sequence will log the message, the payload, and
any error/exception encountered, and the [drop
mediator](_Drop_Mediator_) stops further processing. You should
configure the fault sequence with the correct error handling instead of
simply dropping messages. For more information, see [Error
Handling](https://docs.wso2.com/display/EI650/Error+Handling) .
