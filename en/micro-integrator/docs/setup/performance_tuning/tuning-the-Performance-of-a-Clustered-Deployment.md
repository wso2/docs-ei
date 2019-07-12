# Tuning the Performance of a Clustered Deployment

This section describes how to tune the performance of the Message Broker
profile in terms of deployment in a clustered environment.

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Improvement area</th>
<th>Parameter</th>
<th>Description</th>
<th>Location</th>
<th>Default/Recommended Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Memory</strong></td>
<td><div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>-Xms256m -Xmx1024m -XX:MaxPermSize=256m</span></code></pre></div>
</div>
</div></td>
<td><p>The memory allocated for the broker.</p></td>
<td><p><strong>For Windows</strong> : <code>              &lt;EI_HOME&gt;/wso2/broker/bin/wso2server.bat             </code> file</p>
<p><strong>For Linux</strong> : <code>              &lt;EI_HOME&gt;/wso2/broker/bin/wso2server.sh             </code> file</p></td>
<td>Generally, at least 2 GB memory is recommended for production instances.</td>
</tr>
<tr class="even">
<td><strong>Cluster health</strong></td>
<td>hazelcast.max.no.heartbeat.seconds</td>
<td><p>The maximum time period that should elapse between pings received from a worker node in a broker cluster before acknowledging that worker node to be dead. This value is specified in seconds.</p>
<p>This parameter prevents the allocation of resources to inactive worker nodes, thereby avoiding unnecessary system overheads.</p>
<p>A short time duration can be specified in environments with a very high message flow and it is important to reallocate resources from inactive nodes to active nodes fast. A longer time duration can be specified in environments with lower message flows.</p></td>
<td><code>             &lt;EI_HOME&gt;/wso2/broker/conf/hazlecast.properties            </code> file</td>
<td>600</td>
</tr>
<tr class="odd">
<td><strong>Cluster Recovery</strong></td>
<td>concurrentStorageQueueReads</td>
<td>The number of storage queue reads carried out concurrently at a given time. This number should be set based on the number of storage queues that currently exist.</td>
<td><code>             &lt;EI_HOME&gt;/wso2/broker/conf/broker.xml            </code> file</td>
<td>5</td>
</tr>
</tbody>
</table>
