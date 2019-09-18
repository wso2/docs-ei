# Error Handling

The main role of WSO2 Micro Itegrator is to act as the backbone of an organization’s service-oriented architecture. It is the spine through which all the systems and applications within the enterprise (and external applications that integrate with the enterprise) communicate with each other. For example, the Micro Integrator often has to deal with many wire-level protocols, messaging standards, and remote APIs. But applications and networks can be full of errors. Applications crash. Network routers and links get into states where they cannot pass messages through with the expected efficiency. These error conditions are very likely to cause a fault or trigger a runtime exception in the Micro Integrator server.

When you define a mediation sequence, [Fault Sequences](../../concepts/message-processing-units/#fault-sequences) are used to handle messages that are affected by mediation errors. Errors that can occur at [endpoints](../../concepts/message-exit-points.md) can be specifically configured using the [endpoint error handling properties](../../references/synapse-properties/endpoint-properties/#endpoint-error-handling-properties).

The last step of a message processing inside WSO2 Enterprise Service Bus is to send the message to a service provider (see also Working with Mediators) by sending the message to a listening service endpoint. During this process, transport errors can occur. For example, the connection might time out, or it might be closed by the actual service. Therefore, endpoint error handling is a key part of any successful Enterprise Integrator deployment.

Messages can fail or be lost due to various reasons in a real TCP network. When an error occurs, if the Enterprise Integrator is not configured to accept the error, it will mark the endpoint as failed, which leads to a message failure. By default, the endpoint is marked as failed for quite a long time, and due to this error, subsequent messages can get lost.

To avoid lost messages, you configure error handling at the endpoint level. You should also run a few long-running load tests to discover errors and fine-tune the endpoint configurations for errors that can occur intermittently due to various reasons.

See [mediation debugging](debugging-concepts.md) for information on debugging errors when they occur.
	