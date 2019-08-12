# About Integration Use Cases

#### Message routing

When there is an incoming message into WSO2 Enterprise Integrator, it is
able to determine and route the message to the recipient. Routing can
also be done based on some component of the message. This is known as
content-based routing and is done using the [Switch
mediator](https://docs.wso2.com/display/EI650/Switch+Mediator) .

![message routing
](attachments/119129502/119129520.png "message routing ")

For information in implementing content based routing, see [Routing
Requests Based on Message
Content](https://docs.wso2.com/display/EI650/Routing+Requests+Based+on+Message+Content)
[.](https://docs.wso2.com/display/EI611/Routing+Requests+Based+on+Message+Content)

#### Message filtering

The WSO2 Enterprise Integrator is able to filter out messages based on
the message content using the [Filter
mediator](https://docs.wso2.com/display/EI650/Filter+Mediator) . This
feature allows you to perform complex logic, where you are able to
filter out messages and send them in different mediation flows.

![message
filtering](attachments/119129502/119129519.png "message filtering")

#### Message transformation

When sender and receiver messages do not have the same data format, the
WSO2 Enterprise Integrator can be used to translate the messages between
the sender and recipient. For information on how the Enterprise
Integrator can be used for this, see the [Message
Translator](https://docs.wso2.com/display/IntegrationPatterns/Message+Translator)
pattern in the EIP Guide. The [PayloadFactory
mediator](https://docs.wso2.com/display/EI650/PayloadFactory+Mediator)
and [Data Mapper
mediator](https://docs.wso2.com/display/EI650/Data+Mapper+Mediator) can
be used to implement this. You can manipulate messages by adding and
removing content from them, converting them to a completely different
message format and even validating messages based on the available
validation mechanisms of the message format.

![message
transformation](attachments/119129502/119129518.png "message transformation")  

For an example of how you can implement message transformation using the
Data Mapper mediator, see [Transforming Message
Content](https://docs.wso2.com/display/EI650/Transforming+Message+Content)
.

#### Content enriching

You can use the [Enrich
Mediator](https://docs.wso2.com/display/EI650/Enrich+Mediator) to
process a message based on a given source configuration and then perform
a specified action on the message by using the target configuration. It
gets an OMElement using the configuration specified in the source and
then modifies the message by putting it on the current message using the
configuration in the target.

![content
enriching](attachments/119129502/119129517.png "content enriching")

#### Protocol switching

The WSO2 Enterprise Integrator has the capability to take messages that
come in one protocol and then send the message out in a completely
different protocol (e.g. HTTP to JMS). The protocol bridging technology
in Enterprise Integrator takes the business content of a message that
comes in from one protocol and sends this content out in a completely
different format and protocol.

![protocol switching
new](attachments/119129502/119129516.png "protocol switching new")  

#### Service chaining

Service chaining (orchestration) is a popular use case in the Enterprise
Integrator, where several services are exposed as a single service,
aggregated service. Enterprise Integrator is used for the integration
and sequential calling of these services so that the expected response
can be provided to the client.

![service
chaining](attachments/119129502/119129515.png "service chaining")

For information on implementing a simple service chaining scenario, see
[Exposing Several Services as a Single
Service](https://docs.wso2.com/display/EI650/Exposing+Several+Services+as+a+Single+Service)
.

#### Message storing and forwarding

Store and forward messaging pattern is used in asynchronous messaging.
This can be used when integrating with systems that accept message
traffic at a given rate and handling failover scenarios. In this
pattern, messages are sent to a [Message
Store](https://docs.wso2.com/display/EI650/Message+Stores) where they
are temporarily stored before they are delivered to their destination by
a [Message
Processor](https://docs.wso2.com/display/EI650/Message+Processors) . The
Enterprise Integrator is shipped with a few message store
implementations and also allows you to implement a custom message store
implementation.

![message store and
forward](attachments/119129502/119129514.png "message store and forward")

For information on implementing a store and forward pattern using the
in-memory store of Enterprise Integrator, see the [Storing and
Forwarding
Messages](https://docs.wso2.com/display/EI620/Storing+and+Forwarding+Messages)
.

For information on an example on implementing guaranteed delivery in
Enterprise Integrator, see [Guaranteed Delivery with Failover Message
Store and Scheduled Failover Message Forwarding
Processor](https://docs.wso2.com/display/EI620/Guaranteed+Delivery+with+Failover+Message+Store+and+Scheduled+Failover+Message+Forwarding+Processor)
.
