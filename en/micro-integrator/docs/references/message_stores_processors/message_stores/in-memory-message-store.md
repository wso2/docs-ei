# In Memory Message Store

In memory message store is a basic [Message Store](_Message_Stores_)
that stores messages in an in-memory queue. Since the messages are
stored in an in-memory queue in case of a ESB profile restart, all the
messages stored will be lost.

The in memory message store is lot faster than a persistent message
store implementation, so it can be used to temporarily store messages
for use cases such as the implementation of a high-speed store and
forwarded pattern where message persistence is not a requirement.

!!! Note
	In memory message stores are not recommended for use in production as
well as in scenarios where large scale message storing is required. You
can use an external message store (e.g., [JMS message
store](_JMS_Message_Store_) ) for such scenarios.

## Sample configuration

Following is a sample in memory message store configuration:

``` xml
<messageStore name="InMemoryMS" class="org.apache.synapse.message.store.impl.memory.InMemoryStore" xmlns="http://ws.apache.org/ns/synapse"></messageStore>
```
