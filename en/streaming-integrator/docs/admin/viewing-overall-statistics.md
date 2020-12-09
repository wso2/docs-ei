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
 
 This also provides a link to the **Siddhi Server Statistics** dashboard to [view server statistics](viewing-server-statistics.md).
 
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

To evaluate the performance of each server as follows:

- By analysing the efficiency of the server by comparing its events received with the overhead it incurrs in terms of the system load average and the memory used.
- By comparing the events received, system load average and the memory usage of each server with that of other servers.
 
## Overall Throughput
 
 ![Overall throughput](../images/streaming-integrator-grafana-dashboard/overall_throughput_graph.png)
 
 This shows the overall throughput of your current Streaming Integrator deployment. 
 
### Purpose

 To monitor the overall throughput and evaluate it against other statistics such as the system load average, memory used, the number of Siddhi applications deployed in the system etc.
 
 
## System Load Average
 
 ![System load average](../images/streaming-integrator-grafana-dashboard/system_load_average_graph.png)
 
 This shows the average system load of your current Streaming Integrator deployment.
 
### Purpose

 To monitor the system load average and take appropriate measures to reduce it if it is too high, and to optimize the system better if it is relatively low.
 
## CPU Usage
 
 ![CPU usage](../images/streaming-integrator-grafana-dashboard/cpu_usage_graph.png)
 
  This shows the CPU usage of your current Streaming Integrator deployment.
  
### Purpose

 To monitor the CPU usage of your current Streaming Integrator deployment and make changes to the CPU resources allocated as appropriate.
 
## Memory Usage
 
 ![Memory usage](../images/streaming-integrator-grafana-dashboard/memory_usage_graph.png)
 
 This shows the memory usage of your current Streaming Integrator deployment.
  
### Purpose

 To monitor the memory usage of your Streaming Integrator deployment and allocate more memory when needed.
 
## Thread Count

![Thread Count](../images/viewing-overall-statistics/thread-count.png)

This shows the number of JVM threads that are currently active.

## Threads Blocked

![Threads Blocked](../images/viewing-overall-statistics/blocked-threads.png)

This shows the number of JVM threads that are currently blocked.

## Memory Heap Used

![Memory Heap Used](../images/viewing-overall-statistics/memory-heap-used.png)

This shows the JVM memory heap that is currently consumed by your Streaming Integrator deployment.

## File Descriptors Open

The number of JVM file descriptors that are currently open.

![File Descriptors Open](../images/viewing-overall-statistics/file-descriptors.png)

## Stream Statistics

![Stream Statistics](../images/viewing-overall-statistics/stream-statistics.png)

This indicates the following:

- The complete list of streams in all the Siddhi applications that you are monitoring in your Streaming Integrator deployment.

- The throughput of each stream listed.

- The total stream count in all the Siddhi applications you have selected to monitor.

The **Stream Statistics** widget also provides a link to open the **Siddhi Stream Statistics** dashboard where you can [view stream statistics](viewing-stream-statistics.md).

## Query Statistics

![Query Statistics](../images/viewing-overall-statistics/query-statistics.png)

This indicates the following:

- The complete list of queries in all the Siddhi applications that you are monitoring in your Streaming Integrator deployment.

- The throughput of each query listed.

- The total query count in all the Siddhi applications you have selected to monitor.

The **Query Statistics** widget also provides a link to the **Siddhi Query Statistics** dashboard where you can [view query statistics](viewing-query-statistics.md).

## Source and Source Mapper Statistics

![Source and Source Mapper Statistics](../images/viewing-overall-statistics/source-statistics.png)

This indicates the following:

- The complete list of sources in all the Siddhi applications that you are monitoring in your Streaming Integrator deployment.

- The throughput of each source listed.

- The total source count in all the Siddhi applications you have selected to monitor.

The **Source and Source Mapper Statistics** widget also provides a link to the **Siddhi Source Statistics** dashboard where you can [view source statistics](viewing-source-statistics.md).

## Sink Statistics

![Sink Statistics](../images/viewing-overall-statistics/sink-statistics.png)

This indicates the following:

- The complete list of sinks in all the Siddhi applications that you are monitoring in your Streaming Integrator deployment.

- The throughput of each sink listed.

- The total sink count in all the Siddhi applications you have selected to monitor.

The **Sink Statistics** widget also provides a link to the **Siddhi Sink Statistics** dashboard where you can [view sink statistics](viewing-sink-statistics.md).

## Table Statistics

![Table Statistics](../images/viewing-overall-statistics/table-statistics.png)

This indicates the following:

- The complete list of tables in all the Siddhi applications that you are monitoring in your Streaming Integrator deployment.

- The throughput of each table listed.

- The total table count in all the Siddhi applications you have selected to monitor.

The **Table Statistics** widget also provides a link to the **Siddhi Table Statistics** dashboard where you can [view table statistics](viewing-table-statistics.md).

## Window Statistics

![Window Statistics](../images/viewing-overall-statistics/window-statistics.png)

This indicates the following:

- The complete list of windows in all the Siddhi applications that you are monitoring in your Streaming Integrator deployment.

- The throughput of each window listed.

- The total window count in all the Siddhi applications you have selected to monitor.

The **Window Statistics** widget also provides a link to the **Siddhi Window Statistics** dashboard where you can [view window statistics](viewing-window-statistics.md).

## Aggregation Statistics

![Aggregation Statistics](../images/viewing-overall-statistics/aggregation-statistics.png)

This indicates the following:

- The complete list of aggregations in all the Siddhi applications that you are monitoring in your Streaming Integrator deployment.

- The throughput of each aggregation listed.

- The total aggregation count in all the Siddhi applications you have selected to monitor.

The **Aggregation Statistics** widget also provides a link to the **Siddhi Aggregation Statistics** dashboard where you can [view aggregation statistics](viewing-aggregation-statistics.md).

## Trigger Statistics

![Trigger Statistics](../images/viewing-overall-statistics/trigger-statistics.png)

This indicates the following:

- The complete list of triggers in all the Siddhi applications that you are monitoring in your Streaming Integrator deployment.

- The throughput of each trigger listed.

- The total trigger count in all the Siddhi applications you have selected to monitor.

The **Trigger Statistics** widget also provides a link to the **Siddhi Trigger Statistics** dashboard where you can [view trigger statistics](viewing-trigger-statistics.md).

## On Demand Query Statistics

![On Demand Query Statistics](../images/viewing-overall-statistics/on-demand-query-statistics.png)

This indicates the following:

- The complete list of on demand queries you have carried out via REST API for the Siddhi applications you have selected to monitor.

- The throughput of each on-demand query listed.

- The total on-demand query count.

The **On-Demand Query Statistics** widget also provides a link to the **Siddhi On-Demand Query Statistics** dashboard where you can [view on-demand query statistics](viewing-on-demand-query-statistics.md).


