# Working with Mediators

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
[Transactions](https://docs.wso2.com/display/EI650/Working+with+Transactions)
allows message mediation to be done transactionally inside the ESB
profile . With the
[eventing](https://docs.wso2.com/display/EI650/Working+with+Topics+and+Events)
capabilities of the ESB profile , EDA based components can be easily
interconnected, allowing the ESB profile to be used in the front-end of
an organisation's SOA infrastructure.

**Mediators**

A **mediator** is a full-powered processing unit in the ESB profile . In
run-time it has access to all parts of the ESB profile along with the
current message. Usually, a mediator is configured using XML. Different
mediators have their own XML configurations.

At the run-time, a message is injected in to the mediator with the ESB
profile information. Then this mediator can do virtually anything with
the message. A user can write a mediator and put it into the ESB profile
. This custom mediator and any other built-in mediator will be exactly
the same as the API and the privileges (Refer to more information in
[Creating Custom Mediators](_Creating_Custom_Mediators_) ).

Refer to [ESB Mediators](_ESB_Mediators_) .

**Mediation Sequence**

A mediation sequence, commonly called a "sequence", is a list of
mediators. That means, it can hold other mediators and execute them. It
is part of the ESB profile 's core and message mediation cannot live
without this mediator. When a message is delivered to a sequence, it
sends the message through all its child mediators.

Read more in [Mediation Sequences](_Mediation_Sequences_) .

### The Process of Message Mediation

![](attachments/30540586/30705074.png)

In case an error occurs in the main sequence while processing, the
message goes to the fault sequence.

![](attachments/30540586/30705076.png)

For detailed information about mediators, refer to the following pages:

-   [Working with Mediators via
    Tooling](_Working_with_Mediators_via_Tooling_)
-   [ESB Mediators](_ESB_Mediators_)
-   [Creating Custom Mediators](_Creating_Custom_Mediators_)
-   [Mediation Sequences](_Mediation_Sequences_)
-   [Prioritizing Messages](_Prioritizing_Messages_)
-   [Debugging Mediation](_Debugging_Mediation_)

  
