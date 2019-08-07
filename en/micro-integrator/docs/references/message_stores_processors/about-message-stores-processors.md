# Message Stores and Processors

A **message store** is used to temporarily store messages before they
are delivered to their destination by a **message
processor**. This approach is useful for serving
traffic to back-end services that can only accept messages at a given
rate, whereas incoming traffic to the Micro Integrator arrives at different
rates. To store incoming traffic in a message store, use the Store
mediator, and then
use a message processor to deliver messages to the back-end service at a
given rate. Using message processors and message stores allows you to
implement different messaging and integration patterns.

Multiple message processors can use the same message store. For example,
in a clustered environment, each of the nodes would have an instance of
the same message processor, each of which would connect to the same
message store and evenly consume messages from it. The message store
acts as a manager of these consumers and their connections and ensures
that messages are processed by only one message processor, preventing
message duplication. You can further control which nodes a message
processor runs on by specifying pinned servers.

!!! Info
    You can increase performance of message processors either by [increasing
the member count](_Message_Processing_in_a_Worker-Manager_Cluster_Mode_)
or by having multiple message processors. If you increase the member
count, it will create multiple child processors of the message
processor.

## Sample configurations

### Message Stores

``` java
<messageStore name="string" class="className">
  <parameter name="string" > "string" </parameter>\*
</messageStore>
```
You can implement your own message store by implementing the
`         MessageStore        ` interface and adding the store to the
configuration.

### Message Processors

The Micro Integrator ships with the following message processor implementations:

``` java tab='Scheduled Message Forwarding Processor'
<messageProcessor class="classname" name="unique string" targetEndpoint="endpoint name" messageStore="associated message store name">
   <parameter name="string">"string"</parameter>*
</messageProcessor>
```

``` java tab='Message Sampling Processor'
<messageProcessor class="classname" name="unique string" messageStore="associated message store name">
   <parameter name="string">"string"</parameter>*
</messageProcessor>
```

You can also implement your own message processor by implementing the
[MessageProcessor](https://svn.wso2.org/repos/wso2/carbon/platform/branches/turing/dependencies/synapse/2.1.1-wso2v9/modules/core/src/main/java/org/apache/synapse/message/processor/MessageProcessor.java)
interface and adding the message processor to the configuration.

You must have added a message store before you can add a message
processor. You can also configure a message processor in the source view
in the Management Console by adding the
`         <messageProcessor>        ` element as a top-level entry in
the configuration.