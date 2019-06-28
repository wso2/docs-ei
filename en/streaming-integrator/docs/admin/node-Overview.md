# Node Overview

Once you login to the status dashboard, the nodes that are already added
to the Status Dashboard are displayed as shown in the following example:

### Adding a node to the dashboard

If no nodes are displayed, you can add the nodes for which you want to
view the status by following the procedure below:

1.  Click **ADD NEW NODE** .  
    ![](attachments/112391028/112391047.png)  
    This opens the following dialog box.  
      
    ![](attachments/112391028/112391046.png){height="250"}
2.  Enter the following information in the dialog box and click **ADD
    NODE** to add a gadget for the required node in the
    **Node Overview** page.
    1.  In the **Host** parameter, enter the host ID of the node you
        want to add.
    2.  In the **Port** parameter, enter the port number of the node you
        want to add.
3.  If the node you added is currently unreachable, the following dialog
    box is displayed.  
    ![](attachments/112391028/112391037.png)  
    Click either **WORKER** or **MANAGER.** If you click **WORKER** ,
    the node is displayed under **Never Reached** . If you click
    **Manager** , the node is displayed under **Distributed
    Deployments** as shown below.  
    ![](attachments/112391028/112391042.png)

!!! info

The following basic details are displayed for each node.

-   **CPU Usage** : The CPU resources consumed by the SP node out of the
    available CPU resources in the machine in which it is deployed is
    expressed as a percentage.
-   **Memory Usage** : The memory consumed by the node as a percentage
    of the total memory available in the system.
-   **Load Average** :
-   **Siddhi Apps** : The total number of Siddhi applications deployed
    in the node.


  

### Viewing status details

The following is a list of sections displayed in the **Node Overview**
page to provide information relating to the status of the nodes.

#### Distributed Deployments

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391028/112391041.png" width="900" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><div class="content-wrapper">
<p>The nodes that are connected in the distributed deployment are displayed under the relevant group ID in the status dashboard (e.g., <code>               sp              </code> in the above example). Both managers and workers are displayed under separate labels.</p>
<p><strong>Managers</strong> : The active manager node in the cluster is indicated by a green dot that is displayed with the host name and the port of the node. Similarly, a grey dot is displayed for passive manager nodes in the cluster.</p>
<p><strong>Workers</strong> : When you add an active manager node, it automatically retrieves the worker node details that are connected with that particular deployment. If the worker node is already registered in the Status Dashboard, you can view the metrics of that node as follows:<br />
<img src="attachments/112391028/112391035.png" /></p>
</div></td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td><ul>
<li>To determine whether the request load is efficiently allocated between the nodes of a cluster.</li>
<li>To determine whether the cluster has sufficient resources to handle the load of requests.</li>
<li>To identify the nodes connected with the particular deployment.</li>
</ul></td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>If there is a disparity in the CPU usage and the memory consumption of the nodes, redeploy the Siddhi applications between the nodes to balance out the workload.</li>
<li>If the CPU and memory are fully used and and the request load is increasing, allocate more resources (e.g., more memory, more nodes, etc.).</li>
</ul></td>
</tr>
</tbody>
</table>

  

#### Clustered nodes

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391028/112391033.png" width="900" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><p>The nodes that are clustered together in a high-availability deployment are displayed under the relevant cluster ID in the Status Dashboard (e.g., under <code>              WSO2_A_1             </code> in the above example). The active node in the cluster (i.e., the active worker in a minimum HA cluster or the active manager in a fully distributed cluster) are indicated by a green dot that is displayed with the hostname and the port of the node. Similarly, a grey dot is displayed for passive nodes in the cluster.</p></td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td><p>This allows you to determine the following:</p>
<ul>
<li>Whether the request load is efficiently allocated between the nodes of a cluster.</li>
<li>Whether the cluster has sufficient resources to handle the load of requests.</li>
</ul></td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>If there is a disparity in the CPU usage and the memory consumption of the nodes, redeploy the Siddhi applications between the nodes to balance out the workload.</li>
<li>If the CPU and memory are fully used and and the request load is increasing, allocate more resources (e.g., more memory, more nodes, etc.).</li>
</ul></td>
</tr>
</tbody>
</table>

#### Single nodes

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391028/112391030.png" width="900" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This section displays statistics for SP servers that operate as single node setups.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to compare the performance of single nodes agaisnt each other.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>If the CPU usage of a node is too high, investigate the causes for it and take corrective action (e.g., undeploy unnecessary Siddhi applications).</li>
<li>If any underutilized single nodes are identified, you can either deploy more Siddhi applications thatare currrently deployed in other nodes with a high request load. Alternatively, you can redeploy the Siddhi applications of the underutilized node to other nodes, and then shut it down.</li>
</ul></td>
</tr>
</tbody>
</table>

  

#### Nodes that cannot be reached

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391028/112391029.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>When a node is newly added to the Status dashboard and it is unavailable, it is displayed as shown in the above examples.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to identify nodes that cannot be reached at specific hosts and ports.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>Check whether the host and port of the node you added is correct.</li>
<li>Check whether any authentication errors have occured for the node.</li>
</ul></td>
</tr>
</tbody>
</table>

  

#### Nodes that are currently unavailable

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391028/112391034.png" width="900" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>When a node that could be viewed previously is no longer available, its status is displayed in red as shown in the example above. The status displayed for such nodes is applicable for the last time at which the node had been reachable.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to identify previously available nodes that have become unreachable.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>Check whether the node is inactive.</li>
<li>Check whether any authentication errors have occured for the node.</li>
</ul></td>
</tr>
</tbody>
</table>

  

#### Nodes for which metrics is disabled

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391028/112391031.png" width="900" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>When a node for which metrics is disabled is added to the Status dashboard, you can view the number of active and inactive Siddhi applications deployed in it. However, you cannot view the CPU usage, memory usage and the load average.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to identify nodes for which metrics is not enabled.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>Enable metrics for the required nodes to view statistics about their status in the Status Dashboard. For instructions to enable metrics, see <a href="Monitoring-Stream-Processor_112391023.html#MonitoringStreamProcessor-Confi">Monitoring the Stream Processor - Configuring the Status Dashboard</a> .</td>
</tr>
</tbody>
</table>

  

#### Nodes with JMX reporting disabled

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391028/112391032.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>When a node with JMX reporting disabled is added to the Status dashboard, you can view the number of active and inactive Siddhi applications deployed in it. However, you cannot view the CPU usage, memory usage and the load average.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to identify nodes for which JMX reporting is disabled</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>Enable JMX reporting for the required nodes to view statistics about their status in the Status Dashboard. For instructions to enable JMX reporting, see <a href="Monitoring-Stream-Processor_112391023.html#MonitoringStreamProcessor-Confi">Monitoring the Stream Processor - Configuring the Status Dashboard</a> .</td>
</tr>
</tbody>
</table>

  

#### Statistics trends

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391028/112391036.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This dispalys the change that has taken taken place in the CPU usage, memory usage and the load average of nodes since the status was last viewed in the status dashboard. Positive changes are indicated in green (e.g., a decrease in the CPU usage in the above example), and egative changes are indicated in red (an increase in the memory usage and the load average in the above example).</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to view a summary of the performance trends of your SP clusters and single nodes.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>Based on the performance trend observed, add more resources to your SP clusters/single nodes to handle more events, or shutdown one or more nodes if there is excess resources.</td>
</tr>
</tbody>
</table>
