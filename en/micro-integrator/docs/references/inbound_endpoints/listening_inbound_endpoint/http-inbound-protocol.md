# HTTP Inbound Protocol

The HTTP inbound protocol is used to separate endpoint listeners for
each HTTP inbound endpoint so that messages are handle separately. The
HTTP inbound endpoint can bypass the inbound side axis2 layer and
directly inject messages to a given sequence or API. For proxy services,
messages will be routed through the axis2 transport layer in a manner
similar to normal transports. You can start dynamic HTTP inbound
endpoints without restarting the server.

Following is a sample HTTP inbound endpoint configuration:

``` html/xml
    <inboundEndpoint name="HttpListenerEP" protocol="http" suspend="false" sequence="TestIn" onError="fault" >
        <p:parameters xmlns:p="http://ws.apache.org/ns/synapse">
            <p:parameter  name="inbound.http.port">8081</p:parameter>
        </p:parameters>
    </inboundEndpoint>
```

### HTTP inbound endpoint parameters

| Parameter                                    | Description                                                | Required |
|----------------------------------------------|------------------------------------------------------------|----------|
| `             inbound.http.port            ` | The port on which the endpoint listener should be started. | Yes      |

!!! info

Note

-   A sequence should be designed with a call/respond or send/receive
    sequence. It is not recommended to use a sequence that has in and
    out mediators.
-   If a send mediator is used within the inbound endpoint sequence,
    specify a receiving sequence. If you do not specify a receiving
    sequence, the response will dispatch to the
    `           main          ` sequence.


#### Worker pool configuration parameters

By default inbound endpoints share the PassThrough transport worker pool
to handle incoming requests. If you need a separate worker pool for the
inbound endpoint, you need to configure the following parameters:

| Parameter                                                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Default Value                           |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
| `              inbound.worker.pool.size.core             `        | The initial number of threads in the worker thread pool. This value can be changed accordingly based on the number of messages to be processed. The maximum value that can be specified here is the value of the `             i             nbound.worker.pool.size.max            ` parameter.                                                                                                                                                                          | 400                                     |
| `              inbound.worker.pool.size.max             `         | The maximum number of threads in the worker thread pool. Specify a maximum limit in order to avoid performance degradation that can occur due to context switching.                                                                                                                                                                                                                                                                                                       | 500                                     |
| `              inbound.worker.thread.keep.alive.sec             ` | The keep-alive time for extra threads in the worker pool. This value should be less than the socket timeout. When this time is elapsed for an extra thread, it will be destroyed. The purpose of this parameter is to optimize the usage of resources by avoiding wastage that results from having extra threads that are not utilized.                                                                                                                                   | `             60            `           |
| `              inbound.worker.pool.queue.length             `     | The length of the queue that is used to hold runnable tasks to be executed by the worker pool. The thread pool starts queuing jobs when all existing threads are busy and the pool has reached the maximum number of threads. The value for this parameter should be -1 to use an unbounded queue. If a bound queue is used and the queue gets filled to its capacity, and any further attempt to submit jobs will fail causing synapse to drop some messages.            | -1                                      |
| `              inbound.thread.group.id             `              | Unique Identifier of the thread group.                                                                                                                                                                                                                                                                                                                                                                                                                                    | PassThrough inbound worker thread group |
| `              inbound.thread.id             `                    | Unique Identifier of the thread.                                                                                                                                                                                                                                                                                                                                                                                                                                          | PassThroughInboundWorkerThread          |
| `             dispatch.filter.pattern            `                | The regular expression that defines the proxy services and API's to expose via the inbound endpoint. Provide the `             .*            ` expression to expose all proxy services and API's or provide an expression similar to `             ^(/foo|/bar|/services/MyProxy)$            ` to define a set of services to expose via the inbound endpoint. If you do not provide an expression only the defined sequence of the inbound endpoint will be accessible. | blank                                   |

  

Following is a sample configuration for an inbound endpoint with
a separate worker pool:

``` html/xml
    <inboundEndpoint name="HttpListenerEP" protocol="http" suspend="false" sequence="TestIn" onError="fault" >
        <p:parameters xmlns:p="http://ws.apache.org/ns/synapse">
            <p:parameter  name="inbound.http.port">8081</p:parameter>
            <p:parameter  name="api.dispatching.enabled">false</p:parameter> 
            <p:parameter name="inbound.thread.group.id">Pass_Through Inbound worker Pool</p:parameter>
            <p:parameter name="inbound.worker.pool.size.max">500</p:parameter>
            <p:parameter name="inbound.thread.id">PassThroughInboundWorkerPool</p:parameter>
            <p:parameter name="inbound.worker.pool.queue.length">-1</p:parameter>
            <p:parameter name="inbound.worker.pool.size.core">400</p:parameter>
            <p:parameter name="inbound.worker.thread.keep.alive.sec">60</p:parameter>
        </p:parameters>
     </inboundEndpoint>
```

#### Inbound endpoint parameter for proxy services

If a proxy service is to be exposed only via inbound endpoints, the
following service parameter has to be set in the proxy configuration.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Service Parameter</th>
<th>Description</th>
<th>Default Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             inbound.only            </code></td>
<td><p>Whether the proxy service needs to be exposed only via inbound endpoints.</p>
<p>If set to <code>              true             </code> all requests that the proxy service receives via normal transport will be rejected. The proxy service will process only the requests that are received via inbound endpoints.</p></td>
<td>false</td>
</tr>
</tbody>
</table>

Following is a sample proxy configuration:

``` java
    <proxy xmlns="http://ws.apache.org/ns/synapse" name="InboundProxy" transports="https,http" statistics="disable" trace="disable" startOnLoad="true">
        <target>
            <outSequence>
                <send/>
            </outSequence>
            <endpoint>
                <address uri="http://localhost:9773/services/HelloService/"/>
            </endpoint>
        </target>
        <parameter name="inbound.only">true</parameter>
    </proxy>
```

### Samples

For a sample that demonstrates how a HTTP inbound endpoint can act as a
dynamic http listener, see [Sample 902: HTTP Inbound Endpoint
Sample](https://docs.wso2.com/display/EI611/Sample+902%3A+HTTP+Inbound+Endpoint+Sample)
.
