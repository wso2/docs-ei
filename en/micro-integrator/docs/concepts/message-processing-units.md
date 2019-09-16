# Message processing units

## Mediators

Mediators are individual processing units that perform a specific function on messages that pass through the Micro Integrator. The mediator takes the message received by the proxy service or REST API, carries out some predefined actions on it (such as transforming, enriching, filtering), and outputs the modified message. 

For example, the [Clone](../references/mediators/clone-Mediator.md) mediator splits a message into several clones, the [Send](../references/mediators/send-Mediator.md) mediator sends the messages, and the [Aggregate](../references/mediators/aggregate-Mediator.md) mediator collects and merges the responses before sending them back to the client. 

Mediators also include functionality to match incompatible protocols, data formats, and interaction patterns across different resources. [XQuery](../references/mediators/xQuery-Mediator.md) and [XSLT](../references/mediators/xSLT-Mediator.md) mediators allow rich transformations on the messages. The [Rule](../references/mediators/rule-Mediator.md) mediator allows users to cope with the uncertainty of business logic through rule-based message mediation. Content-based routing using XPath filtering is supported in different flavors, allowing users to get the most convenient configuration experience. Built-in capability to handle transactions allow message mediation to be done transactionally inside the Micro Integrator.

Mediators are always defined within a [mediation sequence](#mediation-sequences).

### Classification of Mediators

Mediators are classified as follows based on whether or not they access the message's content: 

<table>
	<col width="140">
	<tr>
		<th>Classification</th>
		<th>Description</th>
	</tr>
	<tr>
		<td><b>Content-Aware</b> mediators</td>
		<td>
			These mediators always access the message content when mediating messages (e.g., Enrich mediator).
		</td>
	</tr>
	<tr>
		<td><b>Content-Unaware</b> mediators</td>
		<td>
			These mediators never access the message content when mediating messages (e.g., Send mediator).
		</td>
	</tr>
	<tr>
		<td><b>Conditionally Content-Aware</b> mediators</td>
		<td>
			These mediators could be either content-aware or content-unaware depending on their exact instance configuration. For an example a simple Log Mediator instance (i.e. configured as <log/>) is content-unaware. However a log mediator configured as <log level=”full”/> would be content-aware since it is expected to log the message payload.
		</td>
	</tr>
</table>

### List of Mediators

WSO2 Micro Integrator includes a comprehensive library of mediators that provide functionality for implementing widely used **Enterprise Integration Patterns** (EIPs). You can also easily write a custom mediator to provide additional functionality using various technologies such as Java, scripting, and Spring.

**Core Mediators**

[Call](../references/mediators/call-Mediator.md) | [Send](../references/mediators/send-Mediator.md) | [Loopback](../references/mediators/loopback-Mediator.md) | [Sequence](../references/mediators/sequence-Mediator.md) | [Respond](../references/mediators/respond-Mediator.md) | [Drop](../references/mediators/drop-Mediator.md) | [Call Template](../references/mediators/call-Template-Mediator.md) | [Enrich](../references/mediators/enrich-Mediator.md) | [Property](../references/mediators/property-Mediator.md) | [Property Group](../references/mediators/property-Group-Mediator.md) | [Log](../references/mediators/log-Mediator.md) | 

**Filter Mediators**

[Filter](../references/mediators/filter-Mediator.md) | [Validate](../references/mediators/validate-Mediator.md) | [Switch](../references/mediators/switch-Mediator.md) | 

**Transform Mediators**

[XSLT](../references/mediators/xSLT-Mediator.md) | [FastXSLT](../references/mediators/fastXSLT-Mediator.md) | [URLRewrite](../references/mediators/uRLRewrite-Mediator.md) | [XQuery](../references/mediators/xQuery-Mediator.md) | [Header](../references/mediators/header-Mediator.md) | [Fault](../references/mediators/fault-Mediator.md) | [PayloadFactory](../references/mediators/payloadFactory-Mediator.md) | 

**Advanced Mediators**

[Cache](../references/mediators/cache-Mediator.md) | [ForEach](../references/mediators/forEach-Mediator.md) | [Clone](../references/mediators/clone-Mediator.md) | [Store](../references/mediators/store-Mediator.md) | [Iterate](../references/mediators/iterate-Mediator.md) | [Aggregate](../references/mediators/aggregate-Mediator.md) | [Callout](../references/mediators/callout-Mediator.md) | [Transaction](../references/mediators/transaction-Mediator.md) | [Throttle](../references/mediators/throttle-Mediator.md) | [DBReport](../references/mediators/db-Report-Mediator.md) | [DBLookup](../references/mediators/dbLookup-Mediator.md) | [EJB](../references/mediators/ejb-Mediator.md) | [Rule](../references/mediators/rule-Mediator.md) | [Binder](../references/mediators/call-Mediator.md) | [Entitlement](../references/mediators/call-Mediator.md) | [OAuth](../references/mediators/call-Mediator.md) | [Smooks](../references/mediators/binder-Mediator.md) | [Data Mapper](../references/mediators/data-Mapper-Mediator.md) | 

**Extension Mediators**

[Class](../references/mediators/class-Mediator.md) | [Script](../references/mediators/script-Mediator.md) |

**Agent Mediators**

[Publish Event](../references/mediators/publish-Event-Mediator.md)

## Mediation Sequences

A mediation sequence is a set of [mediators](#mediators) organized into a logical flow, allowing you to implement pipes and filter patterns. The mediators in the sequence will perform the necessary message processing and route the message to the required destination. 

Typically, the required mediation sequences are defined within the proxy service or the REST API. This includes an [In](#inout-sequences) sequence, an [Out](#inout-sequences) sequence, and a [default fault](#fault-sequences) sequence. 

All messages that are not destined for proxy services are sent through the [main sequence](#main-sequence). Some other sequences ([named sequences](#named-sequences)) are defined independant of the proxy service, or REST API, and then reused in one or several proxy services, REST APIs for efficiency.

![mediation sequence](../../assets/img/concepts/sequence.png)

### IN/OUT Sequences

Once a request message is received by the proxy service, REST API, inbound endpoint, or even the [main sequence](#main-sequence), the message is injected to the **IN** sequence. The **OUT** sequence defines the mediation logic that processes the response message before sending the response back to the client.

### Fault Sequences

Fault sequences are used for [error handling](error-handling-concepts.md) in message mediation. A fault sequence is a collection of [mediators](#mediators) just like any other [sequence](#mediation-sequences), and it can be associated with another sequence, a proxy service, or REST API. When an error occurs in the mediation logic, the message that triggered the error is delegated to the specified fault sequence. The mediators in the fault sequence can then log the erroneous message, forward it to a special error-tracking service, and send a SOAP fault back to the client indicating the error or even send an email to the system admin.

![fault sequence](../../assets/img/concepts/fault-sequence.png)

If a fault sequence is not specified explicitly, the default fault sequence of the proxy service or REST API will be used to handle errors. The default fault sequence will log the message, the payload, and any error/exception encountered before the [Drop](../references/mediators/drop-Mediator.md) mediator stops further processing. You should configure the fault sequence with the correct **error handling** parameters instead of simply dropping messages.

### Named Sequences

A named sequence is a custom, reusable sequence artifact that holds a specific mediation logic. You can create named sequences and reuse them within your project. A [fault sequence](#fault-sequences) is an example of a named sequence that can be used to replace the default fault sequence of a proxy service, or REST API. While the [main sequence](#main-sequence), proxy services, and REST APIs contain [IN and OUT](#inout-sequences) sequences to define a specific in flow and out flow of mediators, a named sequence is simply a combination of mediators that can be reused within an [IN](#inout-sequences) sequence or [Out](#inout-sequences) sequence.

To reuse a named sequence in multiple integration projects, you need to save it as a [dynamic sequence](#dynamic-sequence).

### Dynamic Sequences

When you create a [named sequence](#named-sequence), you can save it as a dynamic sequence in the product's **registry** (as a **registry resource**). This sequence can then be reused in the mediation flow of any of your integration projects.

### Main Sequence

All messages that are not destined for a proxy service, REST API, or inbound endpoint are sent through the main sequence. A sequence functions as the main sequence when it is named '<b>main</b>'. By default, the main sequence simply sends a message without mediation. Therefore, to add message mediation, you can add mediators and/or [named sequences](#named-sequences) to the main sequence.

## Message Stores and Processors

A **Message Store** is used to temporarily store messages before they are delivered to their destination. This approach is useful for serving traffic to back-end services that can only accept messages at a given rate, whereas incoming traffic arrives at different rates. 

The **Store Mediator** in a mediation sequence is used to store incoming messages in the message store. The **Message Processor** retrieves the messages from the message store and delivers them to the back-end service at a given rate. Message stores and message processors allows you to implement different messaging and integration patterns.

Multiple message processors can use the same message store. For example, in a clustered environment, each of the nodes would have an instance of the same message processor, each of which would connect to the same message store and evenly consume messages. The message store acts as a manager of these consumers and their connections and ensures that messages are processed by only one message processor, preventing message duplication. You can further control which nodes a message processor runs on by specifying pinned servers.

!!! Info
    You can increase performance of message processors either by **increasing the member count** or by having multiple message processors. If you increase the member count, it will create multiple child processors of the message processor.

### List of Message Stores

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
			Used for storing and retrieving messages more efficiently in comparison with other message stores. The JDBC message store implementation is a variation of the already existing synapse message store implementation and is designed in a manner similar to the message store. The JDBC message store uses a JDBC connector to connect to external relational databases.</br></br>
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
			Persists messages in a JMS queue inside a JMS Broker. Since messages are persisted in an orderly manner, JMS message stores implement the Store-and-Forward integration pattern. This message store can be configured by specifying the class as  <code>org.apache.synapse.message.store.impl.jms.JmsStore</code>.
		</td>
	</tr>
	<tr>
		<td>
			RabbitMQ Message Store
		</td>
		<td>
			Persists messages in a RabbitMQ queue inside a RabbitMQ broker. The RabbitMQ message store can be configured by specifying the class as <code>org.apache.synapse.message.store.impl.rabbitmq.RabbitmqStore</code>.
		</td>
	</tr>
	<tr>
		<td>
			Resequence Message Store
		</td>
		<td>
			Used for storing a stream of related, but out-of-sequence, messages in order to put them back into the correct order. It collects and re-orders the stored messages based on a defined sequence number derived from some part of the message. The messages are then published to the output channel in a specific order. This helps when the order of message delivery is important. For example, it avoids some messages arriving earlier than others.</br>
			The resequencing store is an extension of the existing JDBC-based message store. Hence, it inherits most of its properties from the <b>JDBC message store</b>.
		</td>
	</tr>
	<tr>
		<td>
			WSO2 MB Message Store
		</td>
		<td>
			WSO2 Message Broker is used as the <b>message store</b> for the Micro Integrator.
		</td>
	</tr>
	<tr>
		<td>
			In-Memory Message Store
		</td>
		<td>
			This is a basic <b>message store</b> that stores messages in an in-memory queue. This means that all the stored messages will be lost when the server restarts. The in memory message store is a lot faster than a persistent message store. Therefore, it can be used to temporarily store messages for high-speed store and forward integrations where message persistence is not a requirement.</br></br>
			<b>Note</b>: In memory message stores are not recommended for use in production as well as in scenarios where large scale message storing is required. You can use an external message store (e.g., <b>JMS message store</b>) for such scenarios.
		</td>
	</tr>
	<tr>
		<td>Custom Message Store</td>
		<td>
			Users can create a message store with their own message store implementation. Custom message stores are configured by giving the fully qualified class name of the message store implementation as the class value. Any configuration parameter that is needed by the message store implementation class can be passed.
		</td>
	</tr>
</table>

### List of Message Processors

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
			The message sampling processor consumes messages in a <b>message store</b> and sends them to a configured <b>sequence</b>. This process happens at a preconfigured interval. This message processor does not ensure reliable messaging.
		</td>
	</tr>
	<tr>
		<td>Scheduled Failover Message Forwarding Processor</td>
		<td>
			The scheduled failover message forwarding processor ensures reliable message delivery. This helps ensure guaranteed message delivery even when there is a failure in the message store.</br></br>
			The only difference between the scheduled failover message forwarding processor and the scheduled message forwarding processor is that the scheduled message forwarding processor forwards messages to a defined endpoint, whereas the scheduled failover message forwarding processor forwards messages to a target message store.
		</td>
	</tr>
	<tr>
		<td>Scheduled Message Forwarding Processor</td>
		<td>
			The scheduled message forwarding processor consumes messages in a message store and sends them to an <b>endpoint</b>. If a message is successfully delivered to the endpoint, the processor deletes the message from the message store. In case of a failure, it will retry after a specified interval.
		</td>
	</tr>
	<tr>
		<td>Custom Message Processor</td>
		<td>
			Existing message processor implementations are created using the <a href="http://quartz-scheduler.org/">Quartz</a> enterprise job scheduler. If needed, you can create your own implementation of message processors by creating a Java class that implements the <code>MessageProcessor</code> interface. You can then add your custom message processor by specifying your implementation class.</br></br>
			<b>Note</b> that message processors go through several life-cycle stages. Therefore, it is recommended to use existing implementations, which are tested and proven under high loads whenever possible.
		</td>
	</tr>
</table>

## Templates

A template is a collection of mediation artifacts. It is a way to prototype mediation message flows, which can be reused in multiple projects without having to create duplicates. For example, you can template the artifacts ([proxy services](../../concepts/message-entry-points/#proxy-services), [sequences](#mediation-sequences), [endpoints](message-exit-points.md), etc.) that implement common **enterprise integration patterns**. Therefore, templates help you develop integrations faster, while making your configurations manageable, and readable.

<table>
	<tr>
		<th>Template Type</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>Endpoint Template</td>
		<td>
			Used for parameterizing a list endpoint configurations. This allows a mediation flow to use the template parameters to select specific endpoint configurations (defined in the template) and apply them to the mediation flow.
		</td>
	</tr>
	<tr>
		<td>Sequence Template</td>
		<td>
			This is a parametrized <b>sequence</b> providing an abstract or generic form of a sequence defined in the Micro Integrator. Parameters of a template are defined in the form of XPath statement/s. Callers can invoke the template by populating the parameters with static values/XPath expressions using the <b>Call Template</b> mediator, which makes a sequence template into a concrete sequence.
		</td>
	</tr>
</table>

## Connectors

Connectors allow your mediation flows to connect and interact with external services such as Twitter and Salesforce. Typically, connectors are used to wrap the API of an external service. It is also a collection of mediation **templates** that define specific operations that should be performed on the service. Each connector provides operations that perform different actions in that service. For example, the Twitter connector has operations for creating a tweet, getting a user's followers, and more.

To download a required connector, go to the [WSO2 Connector Store](https://store.wso2.com/store).