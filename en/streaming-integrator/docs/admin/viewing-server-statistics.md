!!! note
    **This page is still a work in progress!**
    
# Viewing Server Statistics

Siddhi Server Statistics Dashboard represents a detailed view of the active server instances. It also includes the JVM metrics related to the active servers.

The information displayed is as follows.

## Servers Up/Down

![Servers up/down](../images/streaming-integrator-grafana-dashboard/active_servers_graph.png)

 This indicates the number of active servers against time. When a new server is started, it is indicated by a vertical line. You can move the cursor over this vertical line to check the host and port at which the new active server is running.

## Siddhi App Count

![Siddhi app count](../images/streaming-integrator-grafana-dashboard/siddhi_app_count.png)

This indicates the total number of Siddhi applications deployed in the currently active servers.

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

 This shows the overall throughput of all the servers in your current Streaming Integrator deployment. 
 
### Purpose
 
To monitor the overall throughput and evaluate it against other statistics such as the system load average, memory used, the number of Siddhi applications deployed in the system etc.

## System Load Average

![System load average](../images/streaming-integrator-grafana-dashboard/system_load_average_graph.png)

 This shows the average system load of your current Streaming Integrator deployment.

## CPU Usage

![CPU usage](../images/streaming-integrator-grafana-dashboard/cpu_usage_graph.png)

### Purpose

 To monitor the system load average and take appropriate measures to reduce it if it is too high, and to optimize the system better if it is relatively low.

## Memory Usage

![Memory usage](../images/streaming-integrator-grafana-dashboard/memory_usage_graph.png)

 This shows the memory usage of your current Streaming Integrator deployment.
  
### Purpose

 To monitor the memory usage of your Streaming Integrator deployment and allocate more memory when needed.

## JVM Physical Memory

![JVM physical memory](../images/streaming-integrator-grafana-dashboard/jvm_physical_memory_usage.png)

The amount of JVM physical memory consumed by your WSO2 Streaming Integrator deployment over time.

## JVM Threads

![JVM threads](../images/streaming-integrator-grafana-dashboard/jvm_threads.png)

This shows the number of JVM threads that are currently active.

## JVM Class Load

![JVM class load](../images/streaming-integrator-grafana-dashboard/jvm_class_load.png)

## JVM Swap Space

![JVM swap space](../images/streaming-integrator-grafana-dashboard/jvm_swap_space.png)

