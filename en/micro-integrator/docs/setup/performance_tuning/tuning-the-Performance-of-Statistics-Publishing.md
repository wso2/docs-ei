# Tuning the Performance of Statistics Publishing

This section describes the parameters you need to configure to tune the
performance of the Analytics profile when it is affected by high load,
network traffic etc. You need to tune these parameters based on the
deployment environment.

### Tuning carbon.xml parameters

The following parameter is configured in the
`         <EI_HOME>/conf/carbon.xml        ` file.

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th style="text-align: right;">Default Value</th>
<th>Tuning Recommendation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             AnalyticsServerPublishingInterval            </code></td>
<td>The number of milliseconds that should elapse after a batch of statistical data is processed to be published in the Analytics Dashboard before sending another batch.</td>
<td style="text-align: right;">2000</td>
<td><p>The default value of 2000 milliseconds (i.e. 5 seconds) is recommended.</p>
<p>When WSO2 EI is handling a high load of requests, this value can be reduced to increase the frequency with which the resulting statistics published in the Analytics Dashboard. This helps to avoid storing too much data in WSO2 EI causing an overconsumption of memory.</p>
<p>When the load of requests handled by WSO2 EI is comparatively low, this time interval can be increased to reduce the system overhead incurred by frequent processing.</p></td>
</tr>
</tbody>
</table>

### Tuning data-agent parameters

The parameters in the
`         <EI-ANALYTICS_HOME>/conf/data-bridge/data-agent-config.xml        `
file can be tuned as follows.

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th style="text-align: right;">Default Value</th>
<th>Tuning Recommendation</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             QueueSize            </code></td>
<td>The number of messages that can be stored in WSO2 EI at a given time before they are sent to be published in the Analytics Dashboard.</td>
<td style="text-align: right;">32768</td>
<td><div class="content-wrapper">
<p>This value should be increased when the Analytics profile is busy due to a request overload or if there is high network traffic. This prevents the generation of the <code>               queue full, dropping message              </code> error.</p>
<p>When the Analytics profile is not very busy and when the network traffic is relatively low, the queue size can be reduced to avoid an overconsumption of memory.</p>
!!! info
<p>The number specified for this parameter should be a power of 2.</p>

</div></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>             BatchSize            </code></td>
<td>The WSO2 EI statistical data sent to the Analytics profile to be published in the Analytics Dashboard are grouped into batches. This parameter specifies the number of requests to be included in a batch.</td>
<td style="text-align: right;">200</td>
<td>This value should be tuned in proportion to the volume of requests sent from WSO2 EI to the Analytics profile. This value should be reduced if you want to reduce the system overhead of the Analytics profile. This value should be increased if WSO2 EI is generating a high amount of statistics and if the <code>             QueueSize            </code> cannot be further increased without causing an overconsumption of memory.</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>             CorePoolSize            </code></td>
<td>The number of threads allocated to publish WSO2 EI statistical data to the Analytics Dashboard via Thrift at the time WSO2 EI is started. This value increases when the throughput of statistics generated increases. However, the number of threads will not exceed the number specified for the <code>             MaxPoolSize            </code> parameter.</td>
<td style="text-align: right;">1</td>
<td>The number of available CPU cores should be taken into account when specifying this value. Increasing the core pool size may improve the throughput of statistical data published in the Analytics Dashboard, but latency will also be increased due to context switching.</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>             MaxPoolSize            </code></td>
<td>The maximum number of threads that should be allocated at any given time to publish WSO2 EI statistical data in the Analytics Dashboard.</td>
<td style="text-align: right;">1</td>
<td>The number of available CPU cores should be taken into account when specifying this value. Increasing the maximum core pool size may improve the throughput of statistical data published in the Analytics Dashboard, since more threads can be spawned to handle an increased number of events. However, latency will also increase since a higher number of threads would cause context switching to take place more frequently.</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>             MaxTransportPoolSize            </code></td>
<td>The maximum number of transport threads that should be allocated at any given time to publish WSO2 EI statistical data to the Analytics Server.</td>
<td style="text-align: right;">250</td>
<td>This value must be increased when there is an increase in the throughput of events handled by WSO2 EI Analytics.<br />
<br />
The value of the <code>             tcpMaxWorkerThreads            </code> parameter in the <code>             &lt;EI-ANALYTICS_HOME&gt;/repository/conf/data-bridge/data-bridge-config.xml            </code> must change based on the value specified for this parameter and the number of data publishers publishing statistics. e.g., When the value for this parameter is <code>             250            </code> and the number of data publishers is 7, the value for the <code>             tcpMaxWorkerThreads            </code> parameter must be <code>             1750            </code> (i.e., 7 * 250). This is because you need to ensure that there are enough receiver threads to handle the number of messages published by the data publishers.</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>             SecureMaxTransportPoolSize            </code></td>
<td>The maximum number of secure transport threads that should be allocated at any given time to publish WSO2 EI statistical data to the Analytics Server.</td>
<td style="text-align: right;">250</td>
<td><p>This value must be increased when there is an increase in the throughput of events handled by WSO2 EI Analytics.</p>
<p>The value of the <code>                             sslMaxWorkerThreads                           </code> parameter in the <code>              &lt;EI-ANALYTICS_HOME&gt;/repository/conf/data-bridge/data-bridge-config.xml             </code> must change based on the value specified for this parameter and the number of data publishers publishing statistics. e.g., When the value for this parameter is <code>              250             </code> and the number of data publishers is 7, the value for the <code>                             sslMaxWorkerThreads                           </code> parameter must be <code>              1750             </code> (i.e., 7 * 250). This is because you need to ensure that there are enough receiver threads to handle the number of messages published by the data publishers.</p></td>
<td><br />
</td>
</tr>
</tbody>
</table>

![](images/icons/grey_arrow_down.png){.expand-control-image} Click here
to view a list of errors that can be avoided by tuning the above
parameters

-   `            TID: [-1234] [] [2016-05-05 19:12:02,393]  WARN {org.wso2.carbon.databridge.agent.DataPublisher} -  Event queue is full, unable to process the event for endpoint group [ ( Receiver URL :                         tcp://127.0.0.1:7611                        , Authentication URL :                         ssl://127.0.0.1:7711                        ) ], dropping the event. {org.wso2.carbon.databridge.agent.DataPublisher}           `
-   `            TID: [-1] [] [2016-05-05 19:11:18,642] ERROR {org.wso2.carbon.databridge.agent.endpoint.DataEndpoint} -  Unable to send events to the endpoint.  {org.wso2.carbon.databridge.agent.endpoint.DataEndpoint}           `  
    `            org.wso2.carbon.databridge.agent.exception.DataEndpointException: Cannot send Events           `
-   `            TID: [-1] [] [2016-05-05 19:11:18,642] ERROR {org.wso2.carbon.databridge.agent.endpoint.DataEndpoint} -  Unable to send events to the endpoint.  {org.wso2.carbon.databridge.agent.endpoint.DataEndpoint}           `  
    `            org.wso2.carbon.databridge.agent.exception.DataEndpointException: Cannot send Events           `
-   `            2018-01-18 12:00:19,362  WARN {org.wso2.carbon.databridge.agent.DataPublisher}  -  Event queue is full, unable to process the event for endpoint group [  ( Receiver URL :                         tcp://10.36.239.67:7611                        , Authentication URL :                         ssl://10.36.239.67:7711                        ),( Receiver URL :                         tcp://10.36.239.70:7611                        ,  Authentication URL :                         ssl://10.36.239.70:7711                        ) ], 54995 events dropped so  far.           `
