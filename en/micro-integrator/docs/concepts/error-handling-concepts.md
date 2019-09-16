# Error Handling

The main role of WSO2 Micro Itegrator is to act as the backbone of an organization’s service-oriented architecture. It is the spine through which all the systems and applications within the enterprise (and external applications that integrate with the enterprise) communicate with each other. For example, the Micro Integrator often has to deal with many wire-level protocols, messaging standards, and remote APIs. But applications and networks can be full of errors. Applications crash. Network routers and links get into states where they cannot pass messages through with the expected efficiency. These error conditions are very likely to cause a fault or trigger a runtime exception in the Micro Integrator server.

When you define a mediation sequence, [Fault Sequences](../../concepts/message-processing-units/#fault-sequences) are used to handle messages that are affected by mediation errors. Errors that can occur at [endpoints](../../concepts/message-exit-points.md) can be specifically configured using the [endpoint error handling properties](../../references/synapse-properties/endpoint-properties/#endpoint-error-handling-properties).

See [mediation debugging](debugging-concepts.md) for information on debugging errors when they occur.
	