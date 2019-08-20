# Tracing and handling errors

Endpoints have a `         trace        ` attribute, which turns on
detailed trace information for messages being sent to the endpoint.
These are available in the `         trace.log        ` file, which is
configured in the
`         <PRODUCT_HOME>/repository/conf/log4j.properties        ` file.
Setting the trace log level to `         TRACE        ` logs detailed
trace information including message payloads. For more information on
endpoint states and handling errors, see [Endpoint Error
Handling](_Endpoint_Error_Handling_) .