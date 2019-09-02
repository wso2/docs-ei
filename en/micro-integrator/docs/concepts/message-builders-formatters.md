# Message builders and formatters

When a message comes into the Enterprise Integrator, the receiving
transport selects a **message builder** based on the message's content
type. It uses that builder to process the message's raw payload data and
convert it into common XML, which the Enterprise Integrator mediation
engine can then read and understand. WSO2 Enterprise Integrator includes
message builders for text-based and binary content.

Conversely, before a transport sends a message out from the Enterprise
Integrator, a **message formatter** is used to build the outgoing stream
from the message back into its original format. As with message
builders, the message formatter is selected based on the message's
content type.

You can implement new message builders and formatters using the Axis2
framework.