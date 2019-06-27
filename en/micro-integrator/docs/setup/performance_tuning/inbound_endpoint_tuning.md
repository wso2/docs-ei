# Tuning Inbound Endpoints

An inbound endpoint is a message entry point that can inject messages
directly from the transport layer to the mediation layer, without going
through the Axis engine. This section describes how you can tune HTTP,
HTTPS as well as the Kafka inbound endpoint for better performance.

### Improving the performance of HTTP/HTTPS inbound

The HTTP inbound protocol is used to separate endpoint listeners for
each HTTP inbound endpoint, so that messages are handle separately. The
HTTP inbound endpoint can bypass the inbound side axis2 layer and
directly inject messages to a given sequence or API. For proxy services,
messages are routed through the axis2 transport layer in a manner
similar to normal transports. You can start dynamic HTTP inbound
endpoints without restarting the server.  
By default inbound endpoints share the PassThrough transport worker pool
to handle incoming requests. If you need a separate worker pool for the
inbound endpoint to increase the performance, you need to configure the
following parameters:

| Parameter                                                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Default Value                           |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
| `              inbound.worker.pool.size.core             `        | The initial number of threads in the worker thread pool. This value can be changed accordingly based on the number of messages to be processed. The maximum value that can be specified here is the value of the `             i             nbound.worker.pool.size.max            ` parameter.                                                                                                                                                                          | 400                                     |
| `              inbound.worker.pool.size.max             `         | The maximum number of threads in the worker thread pool. Specify a maximum limit in order to avoid performance degradation that can occur due to context switching.                                                                                                                                                                                                                                                                                                       | 500                                     |
| `              inbound.worker.thread.keep.alive.sec             ` | The keep-alive time for extra threads in the worker pool. This value should be less than the socket timeout. When this time is elapsed for an extra thread, it will be destroyed. The purpose of this parameter is to optimize the usage of resources by avoiding wastage that results from having extra threads that are not utilized.                                                                                                                                   | `             60            `           |
| `              inbound.worker.pool.queue.length             `     | The length of the queue that is used to hold runnable tasks to be executed by the worker pool. The thread pool starts queuing jobs when all existing threads are busy and the pool has reached the maximum number of threads. The value for this parameter should be -1 to use an unbounded queue. If a bound queue is used and the queue gets filled to its capacity, and any further attempt to submit jobs will fail causing synapse to drop some messages.            | -1                                      |
| `              inbound.thread.group.id             `              | Unique Identifier of the thread group.                                                                                                                                                                                                                                                                                                                                                                                                                                    | PassThrough inbound worker thread group |
| `              inbound.thread.id             `                    | Unique Identifier of the thread.                                                                                                                                                                                                                                                                                                                                                                                                                                          | PassThroughInboundWorkerThread          |
| `             dispatch.filter.pattern            `                | The regular expression that defines the proxy services and API's to expose via the inbound endpoint. Provide the `             .*            ` expression to expose all proxy services and API's or provide an expression similar to `             ^(/foo|/bar|/services/MyProxy)$            ` to define a set of services to expose via the inbound endpoint. If you do not provide an expression only the defined sequence of the inbound endpoint will be accessible. | blank                                   |

### Improving the performance of Kafka inbound

WSO2 EI kafka inbound endpoint acts as a message consumer. It creates a
connection to zookeeper and requests messages for a topic, topics or
topic filters.

You can follow the recommendations described below to gain the maximum
performance with the Kafka inbound.

-   Set the `           sequential          ` parameter to
    `           false          ` to use the Kafka inbound in a
    non-sequential mode as it allows better performance than the
    sequential mode.

-   Change the inbound thread pool size based on your use case.
    Recommended values are specified below. These parameters can be
    configured in the esb.toml file.

    ``` java
    [mediation]
    inbound.threads.core = 200 
    inbound.threads.max = 1000          
    ```
