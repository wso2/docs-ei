# Message Stores and Processors

## Message Stores

A **message store** is used to temporarily store messages before they
are delivered to their destination by a [message
processor](_Message_Processors_) . This approach is useful for serving
traffic to back-end services that can only accept messages at a given
rate, whereas incoming traffic to the ESB profile arrives at different
rates. To store incoming traffic in a message store, use the [Store
mediator](https://docs.wso2.com/display/EI650/Store+Mediator) , and then
use a message processor to deliver messages to the back-end service at a
given rate.

Multiple message processors can use the same message store. For example,
in a clustered environment, each of the nodes would have an instance of
the same message processor, each of which would connect to the same
message store and evenly consume messages from it. The message store
acts as a manager of these consumers and their connections and ensures
that messages are processed by only one message processor, preventing
message duplication. You can further control which nodes a message
processor runs on by specifying pinned servers.

You can implement your own message store by implementing the
`         MessageStore        ` interface and adding the store to the
configuration. The ESB profile ships with the following message store
implementations:

-   [In Memory Message Store](_In_Memory_Message_Store_)
-   [JMS Message Store](_JMS_Message_Store_)
-   [RabbitMQ Message Store](_RabbitMQ_Message_Store_)
-   [JDBC Message Store](_JDBC_Message_Store_)
-   [Resequence Message Store](_Resequence_Message_Store_)
-   [WSO2 MB Message Store](_WSO2_MB_Message_Store_)
-   [Custom Message Store](_Custom_Message_Store_)

### Message Store Configuration

``` java
    <messageStore name="string" class="className">
      <parameter name="string" > "string" </parameter>\*
    </messageStore>
```

You enable a message store in the ESB profile configuration by adding
the `         <messageStore>        ` element. This element is a
top-level entry in the configuration and must have a unique name. The
`         class        ` attribute value is the fully qualified class
name of the underlying message store implementation, and the parameters
section is used to configure the parameters that are needed by that
implementation.

!!! info

Note

Message Store does not work in tenant mode. It is a limitation in the
current implementation.


### Sample

[Sample 700: Introduction to Message
Store](https://docs.wso2.com/display/EI600/Sample+700%3A+Introduction+to+Message+Store)


## Message Processors

A **message processor** is used to deliver messages that have been
temporarily stored in a [message store](_Message_Stores_) . This
approach is useful for serving traffic to back-end services that can
only accept messages at a given rate, whereas incoming traffic to the
ESB profile arrives at different rates. You use the [Store
mediator](https://docs.wso2.com/display/EI650/Store+Mediator) to store
messages in the message store, and then you use a message processor to
deliver messages from the message store to the back-end service at a
given rate. Using message processors and message stores allows you to
implement different messaging and integration patterns.

Multiple message processors can use the same message store. For example,
in a clustered environment, each of the nodes might have an instance of
the same message processor, each of which would connect to the same
message store and evenly consume messages from it. The message store
acts as a manager of these consumers and their connections and ensures
that messages are processed by only one message processor, preventing
message duplication.

![](attachments/119131509/119131510.png){width="650"}

The ESB profile ships with the following message processor
implementations:

-   [Scheduled Message Forwarding
    Processor](_Scheduled_Message_Forwarding_Processor_)
-   [Message Sampling Processor](_Message_Sampling_Processor_)

You can also implement your own message processor by implementing the
[MessageProcessor](https://svn.wso2.org/repos/wso2/carbon/platform/branches/turing/dependencies/synapse/2.1.1-wso2v9/modules/core/src/main/java/org/apache/synapse/message/processor/MessageProcessor.java)
interface and adding the message processor to the configuration.

!!! info

You can increase performance of message processors either by [increasing
the member count](_Message_Processing_in_a_Worker-Manager_Cluster_Mode_)
or by having multiple message processors. If you increase the member
count, it will create multiple child processors of the message
processor.


### Message Processor Configuration

You must have added a message store before you can add a message
processor. You can also configure a message processor in the source view
in the Management Console by adding the
`         <messageProcessor>        ` element as a top-level entry in
the configuration as follows:

###### Forwarding processor

``` java
    <messageProcessor class="classname" name="unique string" targetEndpoint="endpoint name" messageStore="associated message store name">
       <parameter name="string">"string"</parameter>*
    </messageProcessor>
```

###### Sampling and custom processors

``` java
    <messageProcessor class="classname" name="unique string" messageStore="associated message store name">
       <parameter name="string">"string"</parameter>*
    </messageProcessor>
```

!!! info

Note

Message Processor does not work in tenant mode. It is a limitation in
the current implementation.

