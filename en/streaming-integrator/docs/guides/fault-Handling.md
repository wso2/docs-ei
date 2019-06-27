# Fault Handling

Siddhi allows you to manage any faults that may occur when handling
streaming data in a graceful manner. This section explains the different
ways in which the faults can be handled gracefully.

## Handling runtime errors

To specify how errors that occur at runtime, you need to add an
`         @OnError        ` annotation to a stream definition as shown 
below.

`                       @OnError(action='on_error_action')                     `

`           define stream <stream name> (<attribute name> <attribute type>, <attribute name> <attribute type>, ... );          `

The on\_error\_action parameter specifies the action to be executed
during failure scenarios. The possible action types are as follows:

-   `          LOG         ` : This logs the event with an error, and
    then drops the event. If you do not specify the fault handling
    actionvia the `          @OnError         ` annotation,
    `          LOG         ` is considered the default action.
-   `          STREAM         ` : This automatically creates a fault
    stream for the base stream. The definition of the fault stream
    includes all the attributes of the base stream as well as an
    additional attribute named `          _error         ` . The events
    are inserted into the fault stream during a failure. The error
    identified is captured as the value for the
    `          _error         ` attribute.

e.g., The following is a Siddhi application that includes the
`         @OnError        ` annotation to handle failures during
runtime.

`                       @OnError(name='STREAM')                     `

`           define stream StreamA (symbol string, volume long);          `

  

`           from StreamA[custom:fault() > volume]          `

`           insert into StreamB;          `

`           from !StreamA#log("Error Occured")          `

`           select symbol, volume long, _error          `

`           insert into tempStream;          `

Here, if an error occurs for the base stream named
`         StreamA        ` , a stream named `         !StreamA        `
is automatically created. The base stream has two attributes named
symbol and volume. Therefore, `         !StreamA        ` has the same
two attributes, and in addition, another attribute named
`         _error.        `

The Siddhi query uses the `         custom:fault()        ` extension
generates the error detected bsed on the specified condition (i.e., if
the volume is less than a specified amount). If no error is detected,
the output is inserted into `         StreamB        ` stream. However,
if an error is detected, it is logged with the
`         Error Occured        ` text. The output is inserted into a
stream named `         tempStream        ` , and the error details are
presented via the `         _error        ` stream attribute (which is
automatically included in the `         !StreamA        ` i fault stream
and then inserted into the TempStream which is the inferred output
stream)..

  

## Handling errors that occur when publishing the output

To specify the error handling methods for errors that occur at the time
the output is published, you can include the on.error parameter in the
sink configuration as shown below.

`           @sink(type='sink_type',                       on.error='on.error.action'                      )          `

`           define stream <stream name> (<attribute name> <attribute type>, <attribute name> <attribute type>, ... );          `

The action types that can be specified via the
`         on.error        ` parameter when configuring a sink are as
follows. If this parameter is not included in the sink configuration,
`         LOG        ` is the action type by default.

-   `          LOG         ` : Logs the event with the error, and then
    drops the event.
-   `          WAIT         ` : The thread waits in the
    `          back-off and re-trying         ` state, and reconnects
    once the connection is re-established.
-   `          STREAM         ` : The corresponding fault stream is
    populated with the failed event and the error that occured while
    publishing.
