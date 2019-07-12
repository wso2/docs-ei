# In Memory Message Store

In memory message store is a basic [Message Store](_Message_Stores_)
that stores messages in an in-memory queue. Since the messages are
stored in an in-memory queue in case of a ESB profile restart, all the
messages stored will be lost.

The in memory message store is lot faster than a persistent message
store implementation, so it can be used to temporarily store messages
for use cases such as the implementation of a high-speed store and
forwarded pattern where message persistence is not a requirement.

!!! note

Note

In memory message stores are not recommended for use in production as
well as in scenarios where large scale message storing is required. You
can use an external message store (e.g., [JMS message
store](_JMS_Message_Store_) ) for such scenarios.


### UI Configuration

Following is the **Add In Memory Message Store** screen that you will
see on the Management Console.

![](attachments/119131485/119131486.png){width="651" height="162"}

When you add an in memory message store, you need to specify a unique
name for the message store.

For instructions on adding a required type of message store via the
Management Console, see [Adding a Message
Store](_Creating_a_Message_Store_) .

Following is a sample in memory message store configuration:

``` xml
    <messageStore name="InMemoryMS" class="org.apache.synapse.message.store.impl.memory.InMemoryStore" xmlns="http://ws.apache.org/ns/synapse"></messageStore>
```
