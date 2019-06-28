# Viewing Statistics for Siddhi Applications

When you open the WSO2 SP Status Dashboard, the [Node
Overview](_Node_Overview_) page is displayed by default.

1.  To view information specific to a selected worker node, click on the
    relevant gadget. This opens the [page specific to the
    worker](_Viewing_Node-specific_Pages_) .
2.  To view information specific to a Siddhi application deployed in the
    Siddhi node, click on the relevant Siddhi application in the Siddhi
    Applications table. This opens a page with information specific to
    the selected Siddhi application as shown in the example below.  
    ![](attachments/112391064/112391065.png){width="900"}

The following statistics can be viewed for an individual Siddhi
Application.

### Latency

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391064/112391068.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the latency of the selected Siddhi application. Latency is the time taken to complete processing a single event in the event flow.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to assess the performance of the selected Siddhi application.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>If the latency of the Siddhi application is too high, check the Siddhi queries and rewrite them to optimise performance.</td>
</tr>
</tbody>
</table>

  

### Overall Throughput

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391064/112391067.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This shows the overall throughput of a selected Siddhi application over time.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to assess the performance of the selected Siddhi application.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>If the throughput of a Siddhi application varies greatly overtime, investigate reasons for any slumps in the throughput (e.g., errors in the deployment of the application).</li>
<li>If the throughput of the Siddhi application is lower than expected, investigate reasons, and take corrective action to improve the throughput (e.g., check the Siddhi queries in the application and rewrite them with best practices to achieve greater efficiency in the processing of events.</li>
</ul></td>
</tr>
</tbody>
</table>

### Memory Used

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391064/112391066.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the memory usage (In MB) of a selected Siddhi application over time.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to monitor the memory consumption of individual Siddhi applications.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><p>If there are major fluctuations in the memory consumption of a Siddhi application, investigate the reasons (e.g., Whether the Siddhi application has been inactive at any point of time).</p></td>
</tr>
</tbody>
</table>

### Code View

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391064/112391071.png" height="250" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the queries defined in the Siddhi file of the application.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to check the queries of the Siddhi application if any further investigations are needed based on the observations of its latency, throughput and the memory consumption.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><p>Edit the Siddhi file if any changes that can improve the performance of the Siddhi application are identified.<br />
For detailed instructions to write a Siddhi application, see <a href="https://docs.wso2.com/display/SP440/Creating+a+Siddhi+Application">Creating a Siddhi Application</a> . |<br />
For detailed information about the Siddhi logic, see the <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/">Siddhi Query Guide</a> .</p>
<p><br />
</p></td>
</tr>
</tbody>
</table>

### Design View

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391064/112391069.png" width="900" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the graphical view for queries defined in the Siddhi file of the application.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to check the flow of the queries of the Siddhi application in the graphical way.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>Edit the Siddhi file if any changes that can improve the performance of the Siddhi application are identified.</td>
</tr>
</tbody>
</table>

  

### Siddhi App Component Statistics

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="attachments/112391064/112391070.png" height="250" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><p>This table displays performance statistics related to dfferent components within a selected Siddhi application (e.g., queries). The columns displayed are as follows:</p>
<ul>
<li><strong>Type</strong> : The type of the Siddhi application component to which the information displayed in the row applies. The component type can be queries, streams, tables, windows and aggregations. For more information, see <a href="https://docs.wso2.com/display/SP440/Siddhi+Application+Overview#SiddhiApplicationOverview-Components">Siddhi Application Overview - Common components of a Siddhi application</a> .</li>
<li><strong>Name</strong> : The name of the Siddhi component within the application to which the information displayed in the row apply.</li>
<li><strong>Metric Type</strong> : The metric type for which the statistics are displayed. This can be either the latency (in milliseconds), throughput the number of events per second), or the amount of memory consumed (in bytes). The metric types based on which the performance of a Siddhi component is measured depends on the component type.</li>
<li><strong>Attribute</strong> : The attribute to which the given value applies.</li>
<li><strong>Value</strong> : The value for the metric type given in the row.</li>
</ul></td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to carry out a detailed analysis of the performance of a selected Siddhi application and identify components that have a negative impact on the overall performance of the Siddhi application.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>Identify the componets in a Siddhi application that have a negative impact on the performance, and rewrite them to improve performance. To understand Siddhi concepts in order to rewrite the components, see the <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/">Siddhi Query Guide</a> .</td>
</tr>
</tbody>
</table>
