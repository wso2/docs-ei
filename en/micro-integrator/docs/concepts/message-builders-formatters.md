# Message Builders and Formatters

When a message comes in to WSO2 Micro Integrator, the receiving transport selects a **message builder** based on the message's content type. It uses that builder to process the message's raw payload data and converts it to common XML, which the mediation engine of WSO2 Micro Integrator can then read and understand. WSO2 Micro Integrator includes
message builders for text-based and binary content.

Conversely, before a transport sends a message out from WSO2 Micro Integrator, a **message formatter** is used to build the outgoing stream from the message back into its original format. As with message builders, the message formatter is selected based on the message's content type. You can implement new message builders and formatters for custom requirements.