# Viewing Statistics for Parent Siddhi Applications

When you open the WSO2 SP Status Dashboard, the [Node
Overview](_Node_Overview_) page is displayed by default. To view
information specific to an active manager, click on the required active
manager node in the **Distributed Deployments** section. This opens a
page with parent Siddhi applications deployed in that manager node as
shown in the example below.

![](attachments/112391075/112391076.png){width="900"}

This page provides a summary of information relating to each parent
Siddhi application as described in the table below. If a parent Siddhi
application is active, it is indicated with a green dot that appears
before the name of the Siddhi application. Similarly, an orange dot is
displayed for inactive parent Siddhi applications.

<table>
<thead>
<tr class="header">
<th>Detail</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Groups</strong></td>
<td>This indicates the number of execution groups of the parent Siddhi application. In the above example, the <code>             Testing            </code> Siddhi application has only one execution group.</td>
</tr>
<tr class="even">
<td><strong>Child Apps</strong></td>
<td>This indicates the number of child applications of the parent Siddhi application. The number of active child applications is displayed in green, and the number of inactive child applications are displayed in red.</td>
</tr>
<tr class="odd">
<td><strong>Worker Nodes</strong></td>
<td><p>The number displayed in yellow indicates the total number of worker nodes in the resource cluster. In the above example, there are two worker nodes in the cluster.</p>
<p>The number displayed in green indicates the number of worker nodes in which the Siddhi application is deployed. In the above example, the <code>              Testing             </code> parent Siddhi application is deployed only in one worker node although there are two worker nodes in the resource cluster.</p></td>
</tr>
</tbody>
</table>

If you click on a parent Siddhi application, detailed information is
displayed as shown below.

![](attachments/112391075/112391080.png){width="900"}

The following are the widgets displayed.

### Code View

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391075/112391078.png" width="900" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><p>This displays the queries defined in the Parent Siddhi file of the application.</p>
<p>This allows you to check the queries of the Siddhi application if any further investigations are needed based on the kafka diagrams and performance.<br />
For detailed instructions to write a Siddhi application, see <a href="https://docs.wso2.com/display/SP440/Converting+to+a+Distributed+Streaming+Application">Converting to a Distributed Streaming Application</a> .</p>
<p>For detailed information about the Siddhi logic, see the <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/">Siddhi Query Guide</a> .</p></td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to check the queries of the Siddhi application if any further investigations are needed based on the observations of the performance of the distributed cluster to which it belongs.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><p>Edit the Siddhi file if any changes that can improve the performance of the Siddhi application are identified.<br />
For detailed instructions to write a Siddhi application, see <a href="https://docs.wso2.com/display/SP440/Converting+to+a+Distributed+Streaming+Application">Converting to a Distributed Streaming Application</a> .<br />
For detailed information about the Siddhi logic, see the <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/">Siddhi Query Guide</a> .</p></td>
</tr>
</tbody>
</table>

### Distributed Siddhi App Deployment

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391075/112391079.png" width="900" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This is a graphical representation of how Kafka topics are connected to the child Siddhi applications of the selected parent Siddhi application . Kafka topics are represented by boxes with red margins, and the child applications are represented by boxes with blue margins.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This is displayed for you to understand how the flow of information takes place.</td>
</tr>
</tbody>
</table>

### Child App Details

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391075/112391077.png" width="900" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><p>This table displays the complete list of child Siddhi applications of the selected parent Siddhi application. The status is displayed in green for active Siddhi applications, and in red for inactive Siddhi applications. In addition, the following is displayed for each Siddhi application:</p>
<ul>
<li><strong>Group Name</strong> : The name of the execution group to which the child application belongs.</li>
<li><strong>Child App Status</strong> : This indicates whether the child application is currently active or not.</li>
<li><strong>Worker Node</strong> : The HTTPS host and The HTTPS port of the worker node in which the child siddhi application is deployed.</li>
</ul></td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>To identify the currently active child applications.</td>
</tr>
</tbody>
</table>
