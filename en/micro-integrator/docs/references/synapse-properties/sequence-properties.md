# Sequence Properties

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