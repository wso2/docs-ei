!!! note
    **This page is still a work in progress!**

# Viewing Overall Statistics

This dashboard displays the overall Siddhi statistics of the Siddhi applications currently deployed in your WSO2 Streaming Integrator instance.

There are nine other dashboards linked to the Siddhi overall statistics dashboard which represent the Siddhi components and their statistics. The Siddhi Grafana Dashboard represents the memory, latency and throughput statistics for the Siddhi components as follows.

<table>
<thead>
<tr class="header">
<th>No.</th>
<th>Element</th>
<th>Memory</th>
<th>Latency</th>
<th>Throughput</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>               01              </td>
<td>               Stream              </td>
<td>               -              </td>
<td>               -              </td>
<td>               ✔️              </td>
</tr>
<tr class="even">
<td>               02              </td>
<td>               Query              </td>
<td>               ✔️              </td>
<td>               ✔️              </td>
<td>               -              </td>
</tr>
<tr class="odd">
<td>               03              </td>
<td>               Source              </td>
<td>               -              </td>
<td>               -              </td>
<td>               ✔️              </td>
</tr>
<tr class="even">
<td>               04              </td>
<td>               Sink              </td>
<td>               -              </td>
<td>               -              </td>
<td>               ✔️              </td>
</tr>
<tr class="odd">
<td>               05              </td>
<td>               Source Mapper              </td>
<td>               -              </td>
<td>               ✔️              </td>
<td>               -              </td>
</tr>
<tr class="even">
<td>               06              </td>
<td>               Sink Mapper              </td>
<td>               -              </td>
<td>               ✔️              </td>
<td>               -              </td>
</tr>
<tr class="odd">
<td>               07              </td>
<td>               Table              </td>
<td>               -              </td>
<td>               ✔️              </td>
<td>               ✔️              </td>
</tr>
<tr class="even">
<td>               08              </td>
<td>               Window              </td>
<td>               ✔️              </td>
<td>               ✔️              </td>
<td>               ✔️              </td>
</tr>
<tr class="odd">
<td>               09              </td>
<td>               Aggregation              </td>
<td>               ✔️              </td>
<td>               ✔️              </td>
<td>               ✔️              </td>
</tr>
<tr class="even">
<td>               10              </td>
<td>               Trigger              </td>
<td>               -              </td>
<td>               -              </td>
<td>               ✔️              </td>
</tr>
<tr class="odd">
<td>               11              </td>
<td>               On-Demand Query              </td>
<td>               -              </td>
<td>               ✔️              </td>
<td>               -              </td>
</tr>
</tbody>
</table>
 
The following information is displayed in this dashboard.
 
## Servers Up/Down
 
 ![Servers up/down](../images/streaming-integrator-grafana-dashboard/active_servers_graph.png)
 
 This indicates the number of active servers against time. When a new server is started, it is indicated by a vertical line. You can move the cursor over this vertical line to check the host and port at which the new active server is running.
 
 ### Purpose
 
 This allows you to identify the following:
 
 - The times at which a specific server started and stopped.
 
 - The duration of time for which a specific server was active.
 
 - The number of servers that were active at a specific time.
 
## Siddhi App Count
 
 ![Siddhi app count](../images/streaming-integrator-grafana-dashboard/siddhi_app_count.png)
 
 This indicates the total number of Siddhi applications deployed in the currently active servers.
 
### Purpose

This allows you to get an overall understanding of the level of activity carried out by the currently active servers.
 
## Server Statistics Summary Table
 
 ![Server statistics summary](../images/streaming-integrator-grafana-dashboard/server_statistics_summary.png)
 
 This lists the currently active Streaming Integrator servers and displays the following for each server:
  - The total events received by the server
  - The system load average
  - The total memory used by each server.
  
### Purpose

This allows you to get an overall idea of the performance of your Streaming Integrator servers.
 
## Overall Throughput
 
 ![Overall throughput](../images/streaming-integrator-grafana-dashboard/overall_throughput_graph.png)
 
 This shows the overall throughput of your current Streaming Integrator deployment.
 
## System Load Average
 
 ![System load average](../images/streaming-integrator-grafana-dashboard/system_load_average_graph.png)
 
 This shows the average system load of your current Streaming Integrator deployment.
 
## CPU Usage
 
 ![CPU usage](../images/streaming-integrator-grafana-dashboard/cpu_usage_graph.png)
 
  This shows the CPU usage of your current Streaming Integrator deployment.
 
## Memory Usage
 
 ![Memory usage](../images/streaming-integrator-grafana-dashboard/memory_usage_graph.png)
 
  This shows the memory usage of your current Streaming Integrator deployment.
 
## JVM Physical Memory
 
 ![JVM physical memory](../images/streaming-integrator-grafana-dashboard/jvm_physical_memory_usage.png)
 
  This shows the JVM physical memory usage of your current Streaming Integrator deployment.
 
## JVM Threads
 
 ![JVM threads](../images/streaming-integrator-grafana-dashboard/jvm_threads.png)
 
## JVM Class Load
 
 ![JVM class load](../images/streaming-integrator-grafana-dashboard/jvm_class_load.png)
 
## JVM Swap Space
 
 ![JVM swap space](../images/streaming-integrator-grafana-dashboard/jvm_swap_space.png)


