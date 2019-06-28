# ESB Mediators

A mediator is the basic message processing unit and a fundamental part
the ESB profile . A mediator can take a message, carry out some
predefined actions on it, and output the modified message. For example,
the Clone mediator splits a message into several clones, the Send
mediator sends the messages, and the Aggregate mediator collects and
merges the responses before sending them back to the client.

The ESB profile ships with a range of mediators capable of carrying out
various tasks on input messages, including functionality to match
incompatible protocols, data formats and interaction patterns across
different resources. Data can be split, cloned, aggregated, and
enriched, allowing the ESB profile to match the different capabilities
of services. XQuery and XSLT allow rich transformations on the messages.
Rule-based message mediation allows users to cope with the uncertainty
of business logic. Content-based routing using XPath filtering is
supported in different flavors, allowing users to get the most
convenient configuration experience. Built-in capability to handle
[Transactions](https://docs.wso2.com/display/EI6xx/Working+with+Transactions)
allows message mediation to be done transactionally inside the ESB
profile . With the
[eventing](https://docs.wso2.com/display/EI6xx/Working+with+Topics+and+Events)
capabilities of the ESB profile , EDA based components can be easily
interconnected, allowing the ESB profile to be used in the front-end of
an organisation's SOA infrastructure.

A **mediator** is a full-powered processing unit in the ESB profile of
WSO2 EI. At run-time, a mediator has access to all the parts of the ESB
profile along with the current message and can do virtually anything
with the message. At the run-time, a message is injected in to the
mediator with the ESB profile information. Then this mediator can do
virtually anything with the message. A user can write a mediator and put
it into the ESB profile . This custom mediator and any other built-in
mediator will be exactly the same as the API and the privileges (Refer
to more information in [Creating Custom
Mediators](https://docs.wso2.com/display/EI6xx/Creating+Custom+Mediators)
).

A **mediation sequence** , commonly called a "sequence", is a list of
mediators. That means, it can hold other mediators and execute them. It
is part of the ESB profile 's core and message mediation cannot live
without this mediator. When a message is delivered to a sequence, it
sends the message through all its child mediators. For more information,
see [Mediation
Sequences](https://docs.wso2.com/display/EI6xx/Mediation+Sequences) .

The Process of message mediation is depicted in the diagram below.

![](attachments/119131045/119131047.png){width="600"}  

In case an error occurs in the main sequence while processing, the
message goes to the fault sequence.

![](attachments/119131045/119131046.png){width="600"}  

When [adding a mediator to a
sequence](_Working_with_Mediators_via_Tooling_) , you can configure the
mediator in design view or in source view. Usually, a mediator is
configured using XML. Source view provides the XML representation of the
configurations done in the UI. When you edit the source XML all changes
immediately reflect in the design view as well.

Each mediator has its own XML configuration. You can change the
following mediators using their source view:

-   Aggregate Mediator
-   Cache Mediator
-   Filter Mediator
-   In Mediator
-   Iterate Mediator
-   Out Mediator
-   Sequence Mediator
-   Synapse Mediator
-   Template Mediator
-   Validate Mediator  

!!! tip

It is possible to comment out lines of code in the Synapse definition as
well as in the source view of the complete EI configuration.


Mediators in a sequence can be one of the following types:

-   **Node mediators** - Contains child mediators.
-   **Leaf mediators** - Does not hold any other child mediators.

Mediators are classified as follows based on whether or not they access
the message's content:

-   **Content-aware mediators** : These mediators always access the
    message content when mediating messages (e.g., [Enrich
    mediator](_Enrich_Mediator_) ).
-   **Content-unaware mediators** : These mediators never access the
    message content when mediating messages (e.g., [Send
    mediator](_Send_Mediator_) ).
-   **Conditionally content-aware mediators** : These mediators could be
    either content-aware or content-unaware depending on their exact
    instance configuration. For an example a simple [Log
    Mediator](_Log_Mediator_) instance (i.e. configured as
    `          <log/>         ` ) is content-unaware. However a log
    mediator configured as `          <log level=”full”/>         `
    would be content-aware since it is expected to log the message
    payload.

Mediators are considered to be one of the main mechanisms for extending
an EI. You can [create custom mediators](_Creating_Custom_Mediators_)
and add them to the ESB profile . This custom mediator and any other
built-in mediator will be exactly the same as the API and the
privileges.

The standard mediators in the ESB profile are listed in the table below.
Click a link for details on that mediator. There are also many
[samples](https://docs.wso2.com/display/EI650/Samples) that demonstrate
how to use mediators.

### The WSO2 EI mediator catalog

Category

Name

Description

Core  

  

  

  

  
  

  

  

  

  

  

[Call](_Call_Mediator_)

Invoke a service in non blocking synchronous manner

[Enqueue](_Enqueue_Mediator_) (deprecated)

Uses a [priority executor](_Prioritizing_Messages_) to ensure
high-priority messages are not dropped

[Send](_Send_Mediator_)

Sends a message

[Loopback](_Loopback_Mediator_)

Moves the message from the In flow to the Out flow, skipping all
remaining configuration in the In flow

[Sequence](_Sequence_Mediator_)

Inserts a reference to a sequence

[Respond](_Respond_Mediator_)

Stops processing on the message and sends it back to the client

[Event](_Event_Mediator_)

Sends event notifications to an event source, publishes messages to
predefined topics

[Drop](_Drop_Mediator_)

Drops a message

[Call Template](_Call_Template_Mediator_)

Constructs a sequence by passing values into a [sequence
template](https://docs.wso2.com/display/EI650/Sequence+Template)

[Enrich](_Enrich_Mediator_)

Enriches a message

[Property](_Property_Mediator_)

Sets or remove properties associated with the message

[Log](_Log_Mediator_)

Logs a message

Filter

  

  

  

  

  

  

[Filter](_Filter_Mediator_)

Filters a message using XPath, if-else kind of logic

[Out](_In_and_Out_Mediators_) (deprecated)

Applies to messages that are in the Out path of the ESB profile

[In](_In_and_Out_Mediators_) (deprecated)

Applies to messages that are in the In path of the ESB profile

[Validate](_Validate_Mediator_)

Validates XML messages against a specified schema.

[Switch](_Switch_Mediator_)

Filters messages using XPath, switch logic

[Conditional Router](_Conditional_Router_Mediator_) (deprecated)

Implements complex routing rules (Header based routing, content based
routing and other rules)

Transform

  

  

  

  

  

[XSLT](_XSLT_Mediator_)

Performs XSLT transformations on the XML payload

[FastXSLT](_FastXSLT_Mediator_)

Performs XSLT transformations on the message stream

[URLRewrite](_URLRewrite_Mediator_)

Modifies and rewrites URLs or URL fragments

[XQuery](_XQuery_Mediator_)

Performs XQuery transformation

[Header](_Header_Mediator_)

Sets or removes SOAP headers

[Fault](_Fault_Mediator_) (also called Makefault)

Create SOAP Faults

[PayloadFactory](_PayloadFactory_Mediator_)

Transforms or replaces message content in between the client and the
backend server

Advanced

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  
  

[Cache](_Cache_Mediator_)

Evaluates messages based on whether the same message came to the ESB
profile

[ForEach](_ForEach_Mediator_)

Splits a message into a number of different messages by finding matching
elements in an XPath expression of the original message.

[Clone](_Clone_Mediator_)

Clones a message

[Store](_Store_Mediator_)

Stores messages in a predefined message store

[Iterate](_Iterate_Mediator_)

Splits a message

[Aggregate](_Aggregate_Mediator_)

Combines a message

[Callout](_Callout_Mediator_)

Blocks web services calls

[Transaction](_Transaction_Mediator_)

Executes a set of mediators transactionally

[Throttle](_Throttle_Mediator_)

Limits the number of messages

[DBReport](_DB_Report_Mediator_)

Writes data to database

[DBLookup](_DBLookup_Mediator_)

Retrieves information from database

[EJB](_EJB_Mediator_)

Calls an external Enterprise JavaBean(EJB) and stores the result in the
message payload or in a message context property.

[Rule](_Rule_Mediator_)

Executes rules

[Builder](_Builder_Mediator_)

Builds the actual SOAP message, from a message, which is coming into the
ESB profile through the Binary Relay.

[Entitlement](_Entitlement_Mediator_)

Evaluates user actions against a XACML policy

[OAuth](_OAuth_Mediator_)

2-legged OAuth support

[Smooks](_Smooks_Mediator_)

Used to apply lightweight transformations on messages in an efficient
manner.

[Data Mapper](_Data_Mapper_Mediator_)

Converts and transforms one data format to another, or changes the
structure of the data in a message.

Extension

  

  

  

  

[Bean](_Bean_Mediator_) (deprecated)

Manipulates JavaBeans

[Class](_Class_Mediator_)

Creates and executes a custom mediator

[POJOCommand](_POJOCommand_Mediator_) (deprecated)

Executes a custom command

[Script](_Script_Mediator_)

Executes a mediator written in Scripting language

[Spring](_Spring_Mediator_) (deprecated)

Creates a mediator managed by Spring

Agent

[Publish Event](_Publish_Event_Mediator_)

Constructs events and publishes them to different systems such as WSO2
BAM/DAS/CEP/SP via event sinks.
