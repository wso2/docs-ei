# Properties Reference

**Properties** provide the means of accessing various types of
information regarding a message that passes through the ESB profile. You
can also use properties to control the behavior of the ESB profile on a
given message. Following are the types of properties you can use:

-   [Generic Properties](_Generic_Properties_) : Allow you to configure
    messages as they're processed by the ESB profile, such as marking a
    message as out-only (no response message will be expected), adding a
    custom error message or code to the message, and disabling
    WS-Addressing headers.
-   [HTTP Transport Properties](_HTTP_Transport_Properties_) : Allow you
    to configure how the HTTP transport processes messages, such as
    forcing a 202 HTTP response to the client so that it stops waiting
    for a response, setting the HTTP status code, and appending a
    context to the target URL in RESTful invocations.
-   [SOAP Headers](_SOAP_Headers_) : Provide information about the
    message, such as the To and From values.
-   [Axis2 Properties](_Axis2_Properties_) : Allow you to configure the
    web services engine in the ESB profile, such as specifying how to
    cache JMS objects, setting the minimum and maximum threads for
    consuming messages, and forcing outgoing HTTP/S messages to use HTTP
    1.0.
-   [Synapse Message Context
    Properties](_Synapse_Message_Context_Properties_) : Allow you to get
    information about the message, such as the date/time it was sent,
    the message format, and the message operation.

For many properties, you can use the [Property
mediator](_Property_Mediator_) to retrieve and set the properties.
Additionally, for information on the XPath extension functions and
Synapse XPath variables that you can use, see [Accessing Properties with
XPath](_Accessing_Properties_with_XPath_) .
