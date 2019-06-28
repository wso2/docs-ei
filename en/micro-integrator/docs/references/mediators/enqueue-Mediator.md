# Enqueue Mediator

!!! info

Note

Please note that this feature is deprecated.


  

The **Enqueue mediator** uses a [priority
executor](_Prioritizing_Messages_) to handle messages. Priority
executors are used in high-load scenarios when you want to execute
different sequences for messages with different priorities. This
approach allows you to control the resources allocated to executing
sequences and to prevent high-priority messages from getting delayed and
dropped. For example, if there are two priorities with value 10 and 1,
messages with priority 10 will get 10 times more resources than messages
with priority 1.

!!! info

The Enqueue mediator is a
[content-unaware](ESB-Mediators_119131045.html#ESBMediators-Content-awareness)
mediator.


------------------------------------------------------------------------

[Syntax](#EnqueueMediator-Syntax) \|
[Configuration](#EnqueueMediator-Configuration) \|
[Example](#EnqueueMediator-Example)

------------------------------------------------------------------------

### Syntax

``` java
    <enqueue xmlns="http://ws.apache.org/ns/synapse" 
        executor="[Priority Executor Name]" priority="[Priority]" sequence="[Sequence Name]">
    </enqueue>
```

  

------------------------------------------------------------------------

### Configuration

The parameters available to configure the Enqueue mediator are as
follows.

| Parameter Name | Description                                                                                                                                                                                                                                                                                                                                                               |
|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Executor**   | The [priority executor](_Prioritizing_Messages_) that should be used to process the messages.                                                                                                                                                                                                                                                                             |
| **Priority**   | The priority of messages that this mediator will handle. This priority level should also be defined in the priority executor.                                                                                                                                                                                                                                             |
| **Sequence**   | The [sequence](_Mediation_Sequences_) that should be used to process messages with the specified priority. This [sequence](_Mediation_Sequences_) should be saved in the registry before it can be selected here. Click either **Configuration Registry** or the **Governance Registry** to select the required [sequence](_Mediation_Sequences_) from the resource tree. |

  

------------------------------------------------------------------------

### Example

In this example, two Enqueue mediator configurations use the [priority
executor](_Prioritizing_Messages_) `         One        ` based on which
requests are prioritized. A [sequence](_Mediation_Sequences_) named
`         Send        ` applies to requests with priority
`         2        ` , and a [sequence](_Mediation_Sequences_) named
`         LogSend        ` applies to priority `         1        ` .

``` xml
    <inSequence xmlns="http://ws.apache.org/ns/synapse">
       <enqueue executor="One" priority="2" sequence="conf:/Send"></enqueue>
       <enqueue executor="One" priority="1" sequence="conf:/LogSend"></enqueue>
    </inSequence>
```

#### Samples

For another example, see [Sample 652: Priority Based Message
Mediation](https://docs.wso2.com/display/EI6xx/Sample+652%3A+Priority+Based+Message+Mediation)
.
