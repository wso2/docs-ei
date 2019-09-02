# Message processing units

### Mediators

Mediators are individual processing units that perform a specific
function, such as sending, transforming, or filtering messages. WSO2
Enterprise Integrator includes a comprehensive mediator library that
provides functionality for implementing widely used [Enterprise
Integration Patterns
(EIPs)](https://docs.wso2.com/display/EI650/Enterprise+Integration+Patterns)
. You can also easily write a custom mediator to provide additional
functionality using various technologies such as Java, scripting, and
Spring.

### Sequences

A sequence is a set of mediators organized into a logical flow, allowing
you to implement pipes and filter patterns. You can add sequences to
proxy services and REST APIs.

A **mediation sequence** , commonly called a **sequence** , is a tree of
[mediators](_ESB_Mediators_) that you can use in your mediation
workflow. When a message is delivered to a sequence, the sequence sends
it through all its mediators.

![](attachments/30540641/30705246.png)

When you want to work with mediation sequences, you can use the EI
tooling plug-in to create a new sequence as well as to import an
existing sequence, or you can add, edit, and delete sequences via the
Management Console.

#### Main and Fault Sequences

A mediation configuration holds two special sequences named **main** and
**fault** . All messages that are not destined for [proxy
services](https://docs.wso2.com/display/EI650/Working+with+Proxy+Services)
are sent through the main sequence. By default, the main sequence simply
sends a message without mediation, so to add message mediation, you add
mediators and/or named sequences in the main sequence.

By default, the fault sequence will log the message, the payload, and
any error/exception encountered, and the [drop
mediator](_Drop_Mediator_) stops further processing. You should
configure the fault sequence with the correct error handling instead of
simply dropping messages. For more information, see [Error
Handling](https://docs.wso2.com/display/EI650/Error+Handling) .


You can create a sequence in your ESB Config project or in the registry
and then add it right to that project's mediation workflow, or you can
refer to it from a sequence mediator in the same ESB Config project or
another project in this Eclipse workspace.

This section describes how to [create a new
sequence](#WorkingwithSequencesviaTooling-create) or [import an existing
sequence](#WorkingwithSequencesviaTooling-import) from an XML file (such
as a Synapse Configuration file), and how to [use the
sequence](#WorkingwithSequencesviaTooling-use) in your mediation flow.

#### Dynamic sequences

WSO2 Integration Studio allows you to create a Registry Resource
project, which can be used to store Resources and Collections you want
to deploy to the registry of a Carbon Server through a Composite
Application (C-App) project. When you create a sequence, you can save it
as a dynamic sequence in the Registry Resource project and refer to that
sequence from the mediation flow. At runtime, when you deploy the CAR
file with both the Registry Resource project and mediation flow, the ESB
profile looks up and uses the sequence from the registry.

### Message stores and processors

A **message store** is used to temporarily store messages before they
are delivered to their destination by a **message
processor**. This approach is useful for serving
traffic to back-end services that can only accept messages at a given
rate, whereas incoming traffic to the Micro Integrator arrives at different
rates. You must have added a message store before you can add a message processor.

To store incoming traffic in a message store, use the Store mediator, and then use a message processor to deliver messages to the back-end service at a
given rate. Using message processors and message stores allows you to
implement different messaging and integration patterns.

Multiple message processors can use the same message store. For example,
in a clustered environment, each of the nodes would have an instance of
the same message processor, each of which would connect to the same
message store and evenly consume messages. The message store
acts as a manager of these consumers and their connections and ensures
that messages are processed by only one message processor, preventing
message duplication. You can further control which nodes a message
processor runs on by specifying pinned servers.

!!! Info
    You can increase performance of message processors either by **increasing the member count** or by having multiple message processors. If you increase the member count, it will create multiple child processors of the message processor.

**Message Stores**

<table>
	<tr>
		<th>Message Store Type</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>
			JDBC Message Store
		</td>
		<td>
			The JDBC message store can be used to store and retrieve messages more efficiently in comparison with other message stores. The JDBC message store implementation is a variation of the already existing synapse message store implementation and is designed in a manner similar to the message store of the ESB profile. The JDBC message store uses a JDBC connector to connect to external relational databases.</br></br>
			The advantages of using a JDBC message store instead of any other message store are as follows:
			<ul>
				<li>
					<b>Easy to connect</b>: You only need to have a JDBC connector to connect to an external relational database.
				</li>
				<li>
					<b>Quick transactions</b>: JDBC message stores are capable of handling a large number of transactions per second.
				</li>
				<li>
					<b>Ability to work with a high capacity for a long period of time</b>: Since JDBC stores use databases as the medium to store data, it can store a large volume of data and is capable of handling data for a longer period of time.
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td>
			JMS Message Store
		</td>
		<td>
			The JMS message store persists messages in a JMS queue inside a JMS Broker. The JMS message store can be configured by specifying the class as  <code>org.apache.synapse.message.store.impl.jms.JmsStore</code>. Since the JMS message stores persist messages in a JMS queue in an ordered manner, JMS message stores can be used to implement the store-and-forward pattern.
		</td>
	</tr>
	<tr>
		<td>
			RabbitMQ Message Store
		</td>
		<td>
			RabbitMQ message store persists messages in a RabbitMQ queue inside a RabbitMQ broker. The RabbitMQ message store can be configured by specifying the class as <code>org.apache.synapse.message.store.impl.rabbitmq.RabbitmqStore</code> and then setting all other required parameters to connect to a RabbitMQ broker.
		</td>
	</tr>
	<tr>
		<td>
			Resequence Message Store
		</td>
		<td>
			The Resequence Message Store is used to store a stream of related but out-of-sequence messages to put them back into the correct order. It collects and re-orders the stored messages based on a defined sequence number derived from some part of the message, so that they can be published to the output channel in a specific order. This is advantageous specially when the order of message delivery is important to avoid some messages arriving earlier than others.</br>
			The resequencing store is an extension of the existing JDBC-based message store. Hence, it inherits most of its properties from the **JDBC Message Store**.
		</td>
	</tr>
	<tr>
		<td>
			WSO2 MB Message Store
		</td>
		<td>
			WSO2 Enterprise Integrator (WSO2 EI) is shipped with a separate message broker profile (WSO2 MB). You can easily set up this WSO2 MB profile as the **message store** for the ESB. Explained below are the parameters you need to configure for a WSO2 MB Message Store. Also, find details of other configurations that are relevant to the WSO2 MB Message Store.  
		</td>
	</tr>
	<tr>
		<td>
			In-Memory Message Store
		</td>
		<td>
			In memory message store is a basic **Message Store** that stores messages in an in-memory queue. Since the messages are stored in an in-memory queue in case of a ESB profile restart, all the messages stored will be lost.</br>
			The in memory message store is lot faster than a persistent message store implementation, so it can be used to temporarily store messages for use cases such as the implementation of a high-speed store and forwarded pattern where message persistence is not a requirement.</br></br>
			<b>Note</b>: In memory message stores are not recommended for use in production as well as in scenarios where large scale message storing is required. You can use an external message store (e.g., **JMS message store**) for such scenarios.
		</td>
	</tr>
	<tr>
		<td>Custom Message Store</td>
		<td>
			**Custom Message Store** allows users to create a message store with their own message store implementation. It can be configured using configuration by giving the fully qualified class name of the message store implementation as the class value.
			Messages will be stored as specified in the underlying message store implementation. Parameter configuration can be used to pass any configuration parameters that is needed by the message store implementation class.
		</td>
	</tr>
</table>

**Message Processors**

<table>
	<tr>
		<th>Message Processor</th>
		<th>
			Description
		</th>
	</tr>
	<tr>
		<td>
			Message Sampling Processor
		</td>
		<td>
			The message sampling processor consumes messages in a **message store** and sends them to a configured **sequence**. This process happens in a preconfigured interval. This message processor does not ensure reliable messaging.
		</td>
	</tr>
	<tr>
		<td>Scheduled Failover Message Forwarding Processor</td>
		<td>
			The scheduled failover message forwarding processor is a **message processor** that ensures reliable message delivery. This message processor is useful when it comes to scenarios where a message store failure takes place and it is necessary to ensure guaranteed message delivery.</br></br>
			The only difference of the scheduled failover message forwarding processor from the scheduled message forwarding processor is that the scheduled message forwarding processor forwards messages to a defined endpoint, whereas the scheduled failover message forwarding processor forwards messages to a target message store.
		</td>
	</tr>
	<tr>
		<td>Scheduled Message Forwarding Processor'</td>
		<td>
			The scheduled message forwarding processor is a **message processor** that consumes messages in a message store and sends them to an **endpoint**. If a message is successfully delivered to the endpoint, the processor deletes the message from the message store. In case of a failure, it will retry after a specified interval.
		</td>
	</tr>
	<tr>
		<td>Custom Message Processor</td>
		<td>
			Existing message processor implementations are created using the [Quartz](http://quartz-scheduler.org/) enterprise job scheduler. If needed, you can create your own implementations of message processors by creating a Java class that implements the `MessageProcessor` interface. You then select the **Add Custom Message Processor** option when adding a message processor and specify your implementation class.
			Note that message processors go through several life-cycle stages, so you must take great care when creating your own implementation. Because existing implementations are tested and proven under high loads, the best practice is to use the existing implementations whenever possible.
		</td>
	</tr>
</table>

### Templates

The synapse configuration language is a very powerful and robust way of driving enterprise data/messages through the Micro Integrator mediation engine. However, a large number of configuration files in the form of **sequences**, **endpoints**, **proxy services**, and transformations can be required to satisfy all the mediation requirements of your system. To keep your configurations manageable, it's important to avoid scattering configuration files across different locations and to avoid duplicating redundant configurations.

**Templates** help minimize this redundancy by creating prototypes that users can use and reuse when needed. This is very much analogous to classes and instances of classes: a template is a class that can be used to wield instance objects such as templates and endpoints. Thus, templates are an ideal way to improve reusability and readability of synapse configurations. Additionally, users can use predefined templates that reflect common **enterprise integration patterns** for rapid development of message/mediation flows in the Micro Integrator.

**Endpoint Template**: Defines a templated form of an endpoint. An endpoint template can parameterize an endpoint defined inline. An endpoint template would be useless without a template endpoint referring to it.

**Sequence Template**: A **Sequence Template** is a parametrized **sequence** providing an abstract or generic form of a sequence defined in the Micro Integrator. Parameters of a template are defined in the form of XPath statement/s. Callers can invoke the template by populating the parameters with static values/XPath expressions using the **Call Template** Mediator, which makes a sequence template into a concrete sequence.

!!! Info
    The **Call Template** mediator allows you to construct a sequence by passing values into a **sequence template**. This is currently only supported for special types of mediators such as the **Iterator** and **Aggregate Mediators**, where actual XPath operations are performed on a different SOAP message, and not on the message coming into the mediator.

Sequence template parameters can be referenced using an XPath expression defined inside the in-line sequence. For example, the parameter named "foo" can be referenced by the Property mediator (defined inside the in-line sequence of the template) in the following ways:

`<property name=”fooValue” expression=”$func:foo” />`

or

`<property name=”fooValue” expression=”get-property('foo','func')” />`

Using function scope or "?func?" in the XPath expression allows us to refer to a particular parameter value passed externally by an invoker
such as the Call Template mediator.

### Connectors

Connectos allow your message flows to connect to and interact with services such as Twitter and Salesforce. A connector is a collection of **templates** that define specific operations. Typically, connectors are used to wrap the API of an external service such as Twitter or a Google Spreadsheet. Each connector provides operations that perform different actions in that service. For example, the Twitter connector has operations for creating a tweet, getting a user's followers, and more.

To download a required connector, go to the [WSO2 Connector Store](https://store.wso2.com/store).