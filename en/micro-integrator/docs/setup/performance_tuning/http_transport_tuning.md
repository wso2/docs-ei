# Tuning the HTTP Transport

The HTTP transport of the WSO2 Enterprise Integrator(WSO2 EI) can be
used to handle blocking and non-blocking calls. This section describes
how you can tune this transport for better performance in WSO2 EI.

### Improving the non-blocking invocation performance

WSO2 EI supports two non-blocking transports, namely the passthrough
transport and the nhttp transport. The passthrough transport is the
default transport of WSO2 EI, but you can set the NHTTP transport as the
default transport by renaming the
`         <EI_HOME>/conf/axis2/axis2_nhttp.xml        ` file to
`         axis2.xml        ` .

You can improve the non-blocking invocation performance by either
configuring properties related to the HTTP Pass Through transport or
by configuring the properties related to the NHTTP transport, based on
which transport you are using as the default WSO2 EI transport in your
production environment.

##### Configuring passthru-http.properties

You can configure the following properties as required in the
`         <EI_Home>/conf/passthru-http.properties        ` file:

<table>
<thead>
<tr class="header">
<th>Property</th>
<th>Description</th>
<th>Default Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>              worker_pool_size_core             </code></p></td>
<td><p>WSO2 EI uses a thread pool executor to create threads and to handle incoming requests. This parameter controls the number of core threads used by the executor pool. If you increase this parameter value, the number of requests received that can be processed by EI increases, hence, the throughput also increases.</p>
<p>The n ature of the integration scenario and the number of concurrent requests received by the ESB are the main factors that helps to determine worker_pool_size_core.</p></td>
<td>400</td>
</tr>
<tr class="even">
<td><p><code>              worker_pool_size_max             </code></p></td>
<td>This is the maximum number of threads in the worker thread pool. Specifying a maximum limit avoids performance degradation that can occur due to context switching. If the specified value is reached, you will see the error “SYSTEM ALERT - HttpServerWorker threads were in BLOCKED state during last minute”. This can occur due to an extraordinarily high number of requests sent at a time when all the threads in the pool are busy, and the maximum number of threads is already reached.</td>
<td>500</td>
</tr>
<tr class="odd">
<td><p><code>              http.socket.timeout             </code></p></td>
<td>This is the maximum period of inactivity between two consecutive data packets, specified in milliseconds.</td>
<td>120000</td>
</tr>
<tr class="even">
<td><p><code>              worker_thread_keepalive_sec             </code></p></td>
<td>This defines the keep-alive time for extra threads in the worker pool. The value specified here should be less than the socket timeout value. Once this time has elapsed for an extra thread, it will be destroyed. The purpose of this parameter is to optimize the usage of resources by avoiding wastage that results by having unutilized extra threads.</td>
<td>60</td>
</tr>
<tr class="odd">
<td><p><code>              worker_pool_queue_length             </code></p></td>
<td>This defines the length of the queue that is used to hold runnable tasks to be executed by the worker pool. The thread pool starts queuing jobs when all the existing threads are busy, and the pool has reached the maximum number of threads. The value for this parameter should be -1 to use an unbound queue. If a bound queue is used and the queue gets filled to its capacity, any further attempts to submit jobs fail causing some messages to be dropped by Synapse.</td>
<td>-1</td>
</tr>
<tr class="even">
<td><p><code>              io_threads_per_reactor             </code></p></td>
<td>This defines the number of IO dispatcher threads used per reactor. The value specified should not exceed the number of cores in the server.</td>
<td>The default value is equal to number of cores in the server.</td>
</tr>
<tr class="odd">
<td><p><code>              io_buffer_size             </code></p></td>
<td>This is the value of the memory buffer allocated when reading data into the memory from the underlying socket/file channels. You should leave this property set to the default value.</td>
<td>16384</td>
</tr>
<tr class="even">
<td><p><code>              http.max.connection.per.host.port             </code></p></td>
<td>This defines the maximum number of connections allowed per host port.</td>
<td>32767</td>
</tr>
<tr class="odd">
<td><p><code>              http.socket.reuseaddr             </code></p></td>
<td>If this parameter is set to true, it is possible to open another socket on the same port as the socket that is currently used by the EI server to listen to connections. This is useful when recovering from a crash. In such instances, if the socket is not properly closed, a new socket can be opened to listen to connections.</td>
<td><code>             true            </code></td>
</tr>
<tr class="even">
<td><p><code>              http.socket.buffer-size             </code></p></td>
<td>This is used to configure the SessionInputBuffer size of http core. The SessionInputBuffer is used to fill data that is read from the OS socket. This parameter does not affect the OS socket buffer size.</td>
<td>8192</td>
</tr>
<tr class="odd">
<td><p><code>              http.block_service_list             </code></p></td>
<td>If this parameter is set to true, all services deployed to WSO2 EI cannot be accessed via the http <code>             :&lt;EI&gt;:8240/services/            </code> and <code>             https:&lt;EI&gt;:8243/services/            </code> URls.</td>
<td><code>             true            </code></td>
</tr>
<tr class="even">
<td><code>             http.user.agent.preserve            </code></td>
<td>If this parameter is set to true, the user-agent HTTP header of messages passing through the ESB is preserved and printed in the outgoing message.</td>
<td><code>             false            </code></td>
</tr>
<tr class="odd">
<td><p><code>              http.headers.preserve             </code></p></td>
<td><div class="content-wrapper">
<p>This parameter allows you to specify the header field/s of messages passing through the EI that need to be preserved and printed in the outgoing message such as <code>               Location, Keep-Alive, Date, Server, User-Agent              </code> and <code>               Host.              </code> E.g., <code>               http.headers.preserve = Location, Date, Server              </code> .</p>
!!! note
<p>When uploading files using this property, if you run into any header dropping issues such as content type (or any other headers) not passing to back end or media type (charset) being missing at the Pass Through Transport level, add the <code>               http.headers.preserve = Content-Type              </code> property to the <code>               &lt;EI_HOME&gt;/conf/passthru-http.properties              </code> file and restart the server.</p>
!!! info
<p>When you add the <code>               http.headers.preserve=Content-Length              </code> property, if the client sends a chunked request (with the <code>               Transfer-Encoding: chunked              </code> header), the ESB profile forwards it to the backend. Else, if the client sends a request with the <code>               Content-Length              </code> header, ESB profile forwards that header to the backend. Thus, if you are changing the message payload before sending it to the backend, the request will fail as the content length you sent is not the actual content length of the message.</p>
<div>
The main difference between using this property and using the <a href="https://docs.wso2.com/display/EI650/HTTP+Transport+Properties"><code>                FORCE_HTTP_CONTENT_LENGTH               </code> and <code>                COPY_CONTENT_LENGTH_FROM_INCOMING               </code></a> properties together in an API/proxy service is that <code>               http.headers.preserve=Content-Length              </code> property applies at a global (server) level, whereas, you can use the other two properties to have this behaviour locally in the API/proxy service.
</div>

</div></td>
<td><code>             Content-Type            </code></td>
</tr>
<tr class="even">
<td><p><code>              http.connection.disable.keepalive             </code></p></td>
<td>If this parameter is set to true, the HTTP connections with the back end service are closed soon after the request is served. It is recommended to set this property to false so that WSO2 EI does not have to create a new connection every time it sends a request to a back-end service. However, you may need to close connections after they are used if the back-end service does not provide sufficient support for keep-alive connections.</td>
<td><code>             false            </code></td>
</tr>
</tbody>
</table>

#####  Configuring nhttp.properties

You can configure the NHTTP properties as required in the
`         <EI_Home>/conf/nhttp.properties        ` file:

-   Following are the properties used by the non-blocking HTTP
    transport:  

    <table>
    <colgroup>
    <col style="width: 33%" />
    <col style="width: 33%" />
    <col style="width: 33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Property</th>
    <th>Description</th>
    <th>Default Value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p><code>                http.socket.timeout.receiver               </code></p></td>
    <td>This is the maximum period of inactivity between two consecutive data packets on the transport listener side. This is the socket timeout value for connection between the client and the WSO2 EI server.</td>
    <td>60000</td>
    </tr>
    <tr class="even">
    <td><p><code>                http.socket.timeout.sender               </code></p></td>
    <td>This is the maximum period of inactivity between two consecutive data packets for the transport sender side. This is the socket timeout value for connection between the WSO2 EI server and the backend server.</td>
    <td>60000</td>
    </tr>
    <tr class="odd">
    <td><p><code>                nhttp_buffer_size               </code></p></td>
    <td>This is the size of the buffer through which data passes when receiving/transmitting NHTTP requests.</td>
    <td>8192</td>
    </tr>
    <tr class="even">
    <td><p><code>                http.tcp.nodelay               </code></p></td>
    <td>This determines whether Nagle's algorithm (for improving the efficiency of TCP/IP by reducing the number of packets sent over the network) is used. Use the value 0 to enable this algorithm and the value 1 to disable it. The algorithm should be enabled if you need to reduce bandwidth consumption.</td>
    <td>1</td>
    </tr>
    <tr class="odd">
    <td><p><code>                http.connection.stalecheck               </code></p></td>
    <td>This determines whether stale connection check is used. Use the value 0 to enable stale connection check and the value 1 to disable it. When this parameter is enabled, connections that are no longer used are identified and disabled before each request execution. Stale connection check should be disabled when performing critical operations.</td>
    <td>0</td>
    </tr>
    <tr class="even">
    <td><p><code>                http.block_service_list               </code></p></td>
    <td>If this parameter is set to <code>               true              </code> , all services deployed to WSO2 EI cannot be viewed via the http <code>               :&lt;EI&gt;:8240/services/              </code> and <code>               https:&lt;EI&gt;:8243/services/              </code> URls.</td>
    <td><code>               false              </code></td>
    </tr>
    <tr class="odd">
    <td><code>               http.headers.preserve              </code></td>
    <td>This parameter allows you to specify the header field/s of messages passing through WSO2 EI that need to be preserved and printed in the outgoing message (e.g., <code>               http.headers.preserve = Location, Date, Server              </code> ). Supported header fields are: <code>               Date, Server              </code> and <code>               User-Agent.              </code></td>
    <td><br />
    </td>
    </tr>
    </tbody>
    </table>

-   Following are the HTTP sender thread pool properties:

    | Parameter Name                                | Description                                                                                                                                                                                                                                                                                                                                                                                 |  Default Value|
    |-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------:|
    | `               snd_t_core              `     | Transport listener worker pool's initial thread count.                                                                                                                                                                                                                                                                                                                                      |             20|
    | `               snd_t_max              `      | Transport listener worker pool's maximum thread count. Once this limit is reached and all the threads in the pool are busy, they will be in a `               BLOCKED              ` state. In such situations an increase in the number of messages would fire the error `               SYSTEM ALERT - HttpServerWorker threads were in BLOCKED state during last minute              ` . |            100|
    | `               snd_alive_sec              `  | Listener-side keep-alive seconds.                                                                                                                                                                                                                                                                                                                                                           |              5|
    | `               snd_qlen              `       | The listener queue length.                                                                                                                                                                                                                                                                                                                                                                  |             -1|
    | `               snd_io_threads              ` | Listener-side IO workers.                                                                                                                                                                                                                                                                                                                                                                   |              2|

    When there is an increased load, it is recommended to increase the
    number of threads mentioned in the properties above to balance it.  


-   Following are the HTTP listener thread pool properties:

    > Listener-side properties generally have the same values as the
        sender-side properties .


    | Property                                                 | Description                                                                                                                                                                                                                                                                                                                                                                               |  Default Value|
    |----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------:|
    | `               lst_t_core                             ` | Transport sender worker pool's initial thread count.                                                                                                                                                                                                                                                                                                                                      |             20|
    | `               lst_t_max              `                 | Transport sender worker pool's maximum thread count. Once this limit is reached and all the threads in the pool are busy, they will be in a `               BLOCKED              ` state. In such situations an increase in the number of messages would fire the error `               SYSTEM ALERT - HttpServerWorker threads were in BLOCKED state during last minute              ` . |            100|
    | `               lst_alive_sec              `             | Sender-side keep-alive seconds.                                                                                                                                                                                                                                                                                                                                                           |              5|
    | `               lst_qlen              `                  | The sender queue length.                                                                                                                                                                                                                                                                                                                                                                  |             -1|
    | `               lst_io_threads              `            | Sender-side IO workers.                                                                                                                                                                                                                                                                                                                                                                   |              2|

-   Following is a property for AIX-based deployment:

    | Property                                                      | Description                                                                | Default Value                       |
    |---------------------------------------------------------------|----------------------------------------------------------------------------|-------------------------------------|
    | `               http.nio.interest-ops-queueing              ` | Determines whether interestOps() queueing is enabled for the I/O reactors. | `               true              ` |

### Improving the blocking invocation performance

The [Callout
mediator](https://docs.wso2.com/display/EI650/Callout+Mediator) as well
as the [Call
mediator](https://docs.wso2.com/display/EI650/Call+Mediator) in blocking
mode uses the axis2 `         CommonsHTTPTransportSender        `
internally to invoke services. It uses the
`         MultiThreadedHttpConnectionManager        ` to handle
connections, but by default it only allows two simultaneous connections
per host. So if there are more than two requests per host, the requests
have to wait until a connection is available. Therefore if the backend
service is slow, many requests have to wait until a connection is
available from the `         MultiThreadedHttpConnectionManager        `
.This can lead to a significant degrade in the performance of WSO2 EI.

In order to overcome this issue, you can edit
the CommonsHTTPTransportSender configuration in the
`         <EI_HOME>/conf/axis2/axis2_blocking_client.xml        `
file, and increase the value of the
`         defaultMaxConnectionsPerHost        ` parameter.

For example, if you need to set 100 simultaneous connections per second,
you can set the value of the
`         defaultMaxConnectionsPerHost        ` parameter as follows:

``` xml
    <transportsender class="org.apache.axis2.transport.http.CommonsHTTPTransportSender" name="http">
            <parameter name="PROTOCOL">HTTP/1.1</parameter>
            <parameter name="Transfer-Encoding">chunked</parameter>
            <parameter name="cacheHttpClient">true</parameter>
            <parameter name="defaultMaxConnectionsPerHost">100</parameter>
    </transportsender>
```
