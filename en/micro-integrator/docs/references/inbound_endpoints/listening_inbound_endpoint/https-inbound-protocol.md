# HTTPS Inbound Protocol

The HTTPS inbound protocol is used to separate endpoint listeners for
each HTTPS inbound endpoint so that messages are handle separately. This
transport can bypass the inbound side axis2 layer and directly inject
messages to a given sequence or API. You can start dynamic HTTPS inbound
endpoints without restarting the server.

Configuration parameters for a HTTPS inbound endpoint are XML fragments
that represent various properties.

Following is a sample HTTPS inbound endpoint configuration:

``` html/xml
    <inboundEndpoint name="HttpListenerEP" protocol="https" suspend="false" sequence="TestIn" onError="fault" >
        <p:parameters xmlns:p="http://ws.apache.org/ns/synapse">
            <p:parameter  name="inbound.http.port">8081</p:parameter>
            <p:parameter name="keystore">
                <KeyStore>
                    <Location>repository/resources/security/wso2carbon.jks</Location>
                    <Type>JKS</Type>
                    <Password>wso2carbon</Password>
                    <KeyPassword>wso2carbon</KeyPassword>
                </KeyStore>
            </p:parameter>
            <p:parameter name="truststore">
                <TrustStore>
                    <Location>repository/resources/security/client-truststore.jks</Location>
                    <Type>JKS</Type>
                    <Password>wso2carbon</Password>
                </TrustStore>
            </p:parameter>
            <p:parameter name="SSLVerifyClient">require</p:parameter>
            <p:parameter name="HttpsProtocols">TLSv1,TLSv1.1,TLSv1.2</p:parameter>
            <p:parameter name="SSLProtocol">SSLV3</p:parameter>
            <p:parameter name="CertificateRevocationVerifier">
                <CertificateRevocationVerifier enable="true">
                   <CacheSize>10</CacheSize>
                   <CacheDelay>2</CacheDelay>
                </CertificateRevocationVerifier>
             </p:parameter>
         </p:parameters>
    </inboundEndpoint>
```

### HTTPS inbound endpoint parameters

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Parameter</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>inbound.http.port</code></pre></td>
<td><p>The port on which the e ndpoint listener should be started .</p></td>
<td>Yes</td>
</tr>
<tr class="even">
<td><pre><code>keystore</code></pre></td>
<td>The KeyStore location where keys are stored.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><pre><code>truststore</code></pre></td>
<td>The TrustStore location where keys are stored.</td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>SSLVerifyClient</code></pre></td>
<td><p>Used when enabling mutual verification.</p></td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>HttpsProtocols</code></pre></td>
<td>The supporting protocols.</td>
<td>No</td>
</tr>
<tr class="even">
<td><pre><code>SSLProtocol</code></pre></td>
<td>The supporting SSL protocol.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><pre><code>CertificateRevocationVerifier</code></pre></td>
<td><p>When the <code>              enable             </code> attribute is set to <code>              true             </code> , this validates and verifies the revocation status of the host certificates using OCSP/CRL, when making HTTPS connections.<br />
If the <code>              enable             </code> attribute of this parameter is set to <code>              true             </code> , you also need to specify the <code>              CacheSize             </code> and <code>              CacheDelay             </code> .<br />
<code>              CacheSize -             </code> The maximum size of the cache.<br />
<code>              CacheDelay -             </code> The time duration between two consecutive scheduled cache managing tasks that perform housekeeping work for the cache.</p></td>
<td>No</td>
</tr>
</tbody>
</table>

!!! info

Note

-   A sequence should be designed with a call/respond or send/receive
    sequence. It is not recommended to use a sequence that has in and
    out mediators.
-   If a send mediator is used within the inbound endpoint sequence, s
    pecify a receiving sequence . If you do not specify a receiving
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

### Samples

For a sample that demonstrates how an HTTPS inbound endpoint can act as
a dynamic https listener, see [Sample 903: HTTPS Inbound Endpoint
Sample](https://docs.wso2.com/display/EI650/Sample+903%3A+HTTPS+Inbound+Endpoint+Sample)
.
