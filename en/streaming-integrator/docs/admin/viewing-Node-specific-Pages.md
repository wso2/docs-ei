# Viewing Node-specific Pages

When you open the WSO2 SP Status Dashboard, the [Node
Overview](_Node_Overview_) page is displayed by default. To view
information specific to a selected worker node, click on the relevant
widget. This opens a separate page for the worker node as shown in the
example below.

![](attachments/112391048/112391055.png)

### Status indicators

Thw following gadgets can be viewed for the selected worker.

#### Server General Details

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391048/112391054.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This gadget displays general information relating to the selected worker node.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to understand the distribution of nodes in terms of the location, the time zone, operating system used etc., and to locate them.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>In a distributed set up, you can use this information to evaluate the clustered setup and make changes to optimize the benefits of deploying WSO2 SP as a cluster (e.g., making them physically available in different locations to minimize the risk of all the nodes failing at the same time etc.).</td>
</tr>
</tbody>
</table>

  

#### CPU Usage

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391048/112391053.png" height="250" /></p>
<p><br />
</p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the CPU usage of the selected node.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to observe the CPU usage of a selected node over time.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>Identify sudden slumps in the CPU usage, and investigate the reasons (e.g., such as authentication errors that result in requests not reaching the SP server).</li>
<li>Identify continuous increases in the CPU usage and check whether the node is overloaded. If so, reallocate some of the Siddhi applications deployed in the node.</li>
</ul></td>
</tr>
</tbody>
</table>

  

#### Memory Used

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><br />
</p>
<p><img src="attachments/112391048/112391052.png" height="250" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the memory usage of the selected node.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to observe the memory usage of a selected node over time.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>Identify sudden slumps in the memory usage, and investigate the reasons (e.g., a reduction in the requests recived due to system failure).</li>
<li>If there are continous increases in the memory usage, check whether there is an increase in the requests handled, and whether you have enough memory resources to handle the increased demand. If not, add more memory to the node or reallocate some of the Siddhi applications deployed in the node to other nodes.</li>
</ul></td>
</tr>
</tbody>
</table>

#### System Load Average

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><br />
</p>
<p><img src="attachments/112391048/112391049.png" height="250" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the system load average for the selected node.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to observe the system load of the node over time.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><p>Observe the trends of the node's system load, and adjust the allocation of resources (e.g., memory) and work load (i.e., the number of Siddhi applications deployed) accordingly.</p></td>
</tr>
</tbody>
</table>

#### Overall Throughput

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><br />
</p>
<p><img src="attachments/112391048/112391051.png" height="250" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the overall throughput of the selected node.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to assess the performance of the selected node in terms of the throughput over time.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>Compare the throughput of the node against that of other nodes with the same amount of CPU and memory resources. If there are significant variations, investigate the causes (e.g., the differences in the number of requests received by different Siddhi applications deployed in the nodes).</li>
<li>Observe changes in the throughput over time. If there are significant variances, investigate the causes (e.g., whether the node has been unavaialable to receive requests during a given time).</li>
</ul></td>
</tr>
</tbody>
</table>

#### Siddhi Applications

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><br />
</p>
<p><img src="attachments/112391048/112391050.png" height="250" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><p>This table displays the complete list of Siddhi applications deployed in the selected node. The status is displayed in green for active Siddhi applications, and in red for inactive Siddhi applications. In addition, the following is displayed for each Siddhi application:</p>
<ul>
<li><strong>Age</strong> : The age of the Siddhi application in milliseconds.</li>
<li><strong>Latency</strong> : The time (in milliseconds) taken by the Siddhi application to process one request.</li>
<li><strong>Throughput:</strong> The number of requests processed by the Siddhi application since it has been active.</li>
<li><strong>Memory</strong> : The amount of memory consumed by the Siddhi application during its current active session, expressed in milliseconds.</li>
</ul></td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to assess the performance of each Siddhi application deployed in the selected node.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>Identify the inactive Siddhi applications that are required to be active and take the appropriate corrective action.</li>
<li>Identify Siddhi applications that consume too much memory, and identify ways in which the memory usage can be optimized (e.g., use incremental processing).</li>
</ul></td>
</tr>
</tbody>
</table>
