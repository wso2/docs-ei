# Custom Message Processor

Existing message processor implementations are created using the
[Quartz](http://quartz-scheduler.org/) enterprise job scheduler. If
needed, you can create your own implementations of message processors by
creating a Java class that implements the
`         MessageProcessor        ` interface. You then select the **Add
Custom Message Processor** option when adding a message processor and
specify your implementation class.

Note that message processors go through several life-cycle stages, so
you must take great care when creating your own implementation. Because
existing implementations are tested and proven under high loads, the
best practice is to use the existing implementations whenever possible.

  
