# Message Tracing

Message tracing helps you to track issues after an integration process finishes and thereby, allows you to identify and fix issues by identifying the root cause. It is used to trace, track and visualize a body of a message in each intermediate stage of its transmission. This is useful for a number of reasons, including auditing and debugging.

[Endpoints](message-exit-points.md) have a trace attribute, which turns on detailed trace information for messages being sent to the endpoint. These are available in the trace.log file, which is configured in the `MI_HOME/repository/conf/log4j.properties` file. Setting the trace log level to TRACE logs detailed trace information including message payloads.