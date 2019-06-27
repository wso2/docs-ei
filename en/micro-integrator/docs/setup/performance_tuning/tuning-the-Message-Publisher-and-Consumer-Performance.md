# Tuning the Message Publisher and Consumer Performance

This section describes the impact of different parameters in the
`         <EI_HOME>/wso2/broker/conf/broker.xml        ` file on the
rate at which messages are published by publishers, as well as the rate
at which the messages are consumed by the subscribers.

Improvement Area

Parameter

Description

Performance Recommendations

Reading messages from database and allocation of slots to consumers.

`             windowSize            `

This parameter specifies the size of a slot. A publisher returns its
last message ID to the slot manager each time he/she publishes a number
of messages equal to the number specified in this parameter.

Increasing the window size would increase the number of messages
received per consumer. Thus, the subscribers who are the earliest to be
allocated slots would receive a higher number of messages. A lower
window size would result in an even load of messages received by each
subscriber. The default/recommended value is 1000.

  

`             workerThreadCount            `

This parameter specifies the number of slots that should be initialized
when the Message Broker profile is started.

A higher worker thread count would result in a higher rate of message
consumption, as well as a higher system overhead. Increase the thread
count only if you have sufficient hardware resources.

`             deleteThreadCount            `

This value specifies the number of parallel threads that should be
included in a slot deletion task.

Increasing this value will remove slots where messages are
read/delivered to consumers/acknowledged faster, thereby reducing heap
memory used by the server.

`             maxSubmitDelay            `

This parameter specifies the time interval in which, broker checks for
slots that can be marked as 'ready to deliver' (i.e. slots that age more
than the time specified in
`             messageAccumulationTimeout            ` ). The nodes,
which will have subscribers for a given queue/topic will periodically
poll for slots during this interval.

If the consumers are slow, then increasing the
`             maxSubmitDelay            ` would result in reduced CPU
and I/O overhead on the DB. If the consumers are fast, there will be an
increase in latency.

`             SlotDeleteQueueDepthWarningThreshold            `

The maximum number of slots (with pending messages) to delete per Slot
Deleting Task. This configuration is used to raise a warning when the
scheduled number of pending slots exceeds this limit. This indicates
issues that can lead to message accumulation on the server.

\-

`             thriftClientPoolSize            `

The maximum number of thrift client connections that should be created
in the thrift connection pool.

\-

**Message Delivery** : This is the phase where messages read from the
database are delivered to consumers.

`             maxNumberOfReadButUndeliveredMessages            `

This parameter specifies the maximum number of undelivered messages that
are allowed to be retained in the memory.

  

The default value for the
`              maxNumberOfReadButUndeliveredMessages             `
parameter is 1000. Increasing this value can cause out-of-memory
exceptions, but performance will be improved because the number of
database calls will be reduced. Therefore, your allocated server memory
capacity should be considered when configuring this parameter.

`             ringBufferSize            `

This parameter specifies the thread pool size of the queue delivery
workers.

  

The default value of 4096 for the
`              ringBufferSize             ` parameter can be increased
if there are a lot of unique queues in the system. An increased ring
buffer size is also required if the [slot window
size](#TuningtheMessagePublisherandConsumerPerformance-window) is large
and therefore, there is a large number of messages to be delivered.

`              parallelContentReaders             `

This parameter specifies the number of parallel readers used to read
content from the message store.

The default value for the
`              parallelContentReaders             ` parameter, the
`              parallelDeliveryHandlers             ` parameter and the
`              parallelDecompressionHandlers             ` parameter is
5. Increasing this value would increase the speed of the message sending
mechanism, however, the load on the message store would also be
increased. A higher number of cores is required to increase these
values.

`             parallelDeliveryHandlers            `

This parameter specifies the number of parallel delivery handlers used
to deliver messages to subscribers.

`             parallelDecompressionHandlers            `

This parameter specifies the number of parallel decompression handlers
used to decompress messages before they are sent to subscribers.

`             contentReadBatchSize            `

This parameter specifies the number of messages included in the content
retrieval query.

The `             contentReadBatchSize            ` parameter is used to
minimise the number of I/O operations. When large messages (i.e. larger
than 1MB) are exchanged, the batch size should be reduced to avoid
network latency.

**Acknowledgment Handling** : This is the phase where the delivery of
messages that have reached the consumers is acknowledged.

`             ackHandlerCount            `

This parameter specifies the number of message acknowledgment handlers
to process acknowledgments concurrently.

  

The default value of 1 for the
`              ackHandlerCount             ` parameter should be
increased in a high throughput scenario with a relatively high amount of
messages being delivered to the consumers. The value for this parameter
can be decreased when the value specified for the
`              ackHandlerBatchSize             ` parameter is high and
as a result, each individual acknowledgment handler can handle a higher
number of acknowledgments. Note that increasing the number of
acknowledgment handlers when the number of messages being delivered and
acknowledged is low, or when the value specified for the
`              ackHandlerBatchSize             ` parameter is high can
result in idle acknowledgment handlers incurring an unnecessary system
overhead.

`             ackHandlerBatchSize            `

This parameter specifies the maximum number of acknowledgments that can
be handled by an acknowledgment handler.

  

The default value of 100 for the
`              ackHandlerBatchSize             ` parameter should be
increased when there is an increase in the number of messages being
delivered to consumers. If the number of acknowledgements that can be
handled by each individual acknowledgment handler is too low in a high
throughput scenario, it will be required to increase the number of
acknowledgment handlers. This would increase the number of calls made to
the database, thereby increasing the system overhead.

`             maxUnckedMessages            `

The message delivery from the Message Broker profile to the client will
be temporarily paused if the number of messages of which the delivery is
not acknowledged reaches the number specified for the
`             maxUnckedMessages            ` parameter.

Increasing the value specified for the
`             maxUnckedMessages            ` parameter can cause
out-of-memory exceptions, but the performance will be improved due to
the reduction in the number of calls to the database. Therefore, your
allocated server memory capacity should be considered when configuring
this parameter.

**Content Handling** : Handling incoming content chunks.

`             contentChunkHandlerCount            `

This parameter specifies the number of handlers that should be available
to handle content chunks concurrently.

  

The default value of 3 for the
`              contentChunkHandlerCount             ` parameter should
be increased when there is a significant number of large messages being
published. A low value can be specified when the value for the
`              maxContentChunkSize             ` parameter is high in
such situations, each individual handler will be able to handle a higher
amount of content chunks. Note that increasing this value in a scenario
where there are not many large messages published or when the maximum
content chunk size is high can result in idle handlers causing an
unnecessary system overhead.

`             maxContentChunkSize            `

This parameter specifies the maximum column size of a content chunk
handler.

The default value of 65500 for the
`             maxContentChunkSize            ` parameter should be
increased if there is an increased number of large messages being
published. If the value specified for this parameter is too low, the
value for the `             contentChunkHandlerCount            `
parameter will have to be increased. This would increase the system
overhead due to an increased number of calls made to the database.

`             allowCompression            `

This parameter specifies whether or not content compression is enabled.
If enabled, messages published to the Message Broker profile will be
compressed before storing in the DB, to reduce the content size.

This parameter is set to false by default. To increase the performance
of the Message Broker, you can change this value in 'true'. Note that
enabling content compression would increase the speed of the message
sending mechanism. However, if message contents are very small, enabling
content compression will reduce the performance of the Message Broker.
This is because the message content will expand due to this setting.

`             contentCompressionThreshold            `

This parameter specifies the maximum message content size of an
uncompressed message.

The default value of contentCompressionThreshold is 1000 in bytes. It is
not recommended to use values less than 13 bytes. If the value specified
for this parameter is too low, it will cause expansion of the message
size, due to the lack of repeated content. Thus, it will reduce the
performance of the Message Broker. Values greater than 1000 would
increase performance.

**Inbound Events** : These are messaging events relating to publishers

`             bufferSize            `

This parameter specifies the size of the Disruptor ring buffer for
inbound event handling.

It is recommended to increase the value for the
`              bufferSize             ` parameter when there is an
increase in the rate of publishing. The default/recommended value is
65536.

  

`             parallelMessageWriters            `

This parameter specifies the number of parallel writers used to write
content to the message store.

Increasing the value for the
`              parallelMessageWriters             ` parameter increases
the speed of the message receiving mechanism. However, it would also
increase the load on the data store. A higher number of cores are
required to increase this value. The default/recommended value is 1.

`             messageWriterBatchSize            `

This parameter specifies the maximum batch size of the batch write
operation for inbound messages.

The `              messageWriterBatchSize             ` parameter should
be used in high throughput scenarios to avoid database requests with a
high load. A higher number of cores are required to increase this value.
The default/recommended value is 70.

`             purgedCountTimeout            `

This parameter specifies the number of milliseconds to wait for a queue
to complete a purge event in order to update the purge count. If the
time specified here elapses before the queue completes the purge event,
the purge count will not be updated to include the event.

The value for the `             purgedCountTimeout            `
parameter can be increased to ensure that the purge count is accurate.

**Failover**

`             vHostSyncTaskInterval            `

This parameter specifies the time interval after which the virtual host
syncing task can sync host details across the cluster.

This interval should be reduced if frequent failures occur in the
system. Reducing this interval to make more frequent checks in
situations when there are not many failovers would result in an
unnecessary increase in the system overhead. The default/recommended
value is 3600.

**Message Counter** : This is used by the management console to display
the number of messages in a queue.

`             counterTaskInterval            `

This parameter specifies the delay which should occur between the end of
one execution and the start of another, in milliseconds.

Reducing the counter task interval would result in more database calls
(in non-RDBMS databases, this could be a very costly operation). The
default/recommended value is 5 milliseconds.

**Message Deletion**

`             contentRemovalTaskInterval            `

This parameter specifies the ask interval for the content removal task
which will remove the actual message content from the store in the
background.

If the publish/consumer rate is very high, a low value should be entered
for the `              contentRemovalTaskInterval             `
parameter to increase the number of delete requests per task.

  

**Transactions**

`             maxWaitTimeout            `

Maximum wait time for a transaction to be successfully committed. If the
transaction commit request is not completed within this time period, an
error is thrown.

Be sure to configure the wait time based on the speed of your database.
If it takes longer than 30 seconds to respond, you need to change the
default value.
