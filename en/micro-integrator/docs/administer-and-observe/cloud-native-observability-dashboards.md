!!! note
    This section is still a work in progress and not tested.

# Viewing Cloud Native Observability Dashboards

## Overview

The cloud native observability deployment is an observability solution that allows you to monitor and troubleshoot you WSO2 Enterprise Integrator system with ease. This solution is based on proven projects in Cloud Native Computing Foundation to be cloud native and future proof. Also, this technology stack allows you to view and correlate all three pillars of observability (i.e., metrics, logging, and tracing) from a single location instead of using several tools, thereby unifying the monitoring experience. 

Following are the technologies used in the current solution.

| **Feature**   | **Technology**              |
|---------------|-----------------------------|
| Metrics       | Prometheus                  |
| Visualization | Grafana                     |
| Logging       | Fluent-Bit and Grafana Loki |
| Tracing       | Jaeger                      |

The WSO2 Enterprise Integrator team expects to add support for more technologies in the future based on the demands of the customers and industry. 

The following diagram depicts the high level architecture of the solution.

![Cloud Native Deployment Architecture](../../assets/img/monitoring-dashboard/cloud-native-deployment-architecture.png)

As illustrated above, the main feature of this solution is the ability to view logs, metrics and traces from a single location. This is achieved by tagging logs and traces with same set of labels as Prometheus and then using those labels to correlate them within Grafana. This allows you to correlate all three aspects within a given time frame and analyze abnormalities in an effective manner. 

Following sections explains the features and usage of this solution and how to get the maximum use out of this observability solution.


## Cluster dashboard

In the Cluster dashboard visualizes the overall statistics relating to your WSO2 Micro Integrator cluster. we can view information related to our MI Cluster. 

![Cluster Dashboard](../assets/img/monitoring-dashboard/grafana-cluster-dashboard.png)

### Downloading the dashboard

You can download the dashboard from the [WSO2 EI Git repository]().

### Statistic types

The following is the list of widgets displayed in this dashboard.

| **Widget**                | **Description**                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|**Node Count**             |The total number of nodes in the cluster.                                                                                                                                                                                                                                                                                                                                                                 |
|**Service Count**          |The total number of services deployed in the cluster.                                                                                                                                                                                                                                                                                                                                                     |
|**Node List**              |The list of nodes in the cluster. The time at which the node started is displayed together with the node name. <br/>You can click on a node to open the **MI Node Metrics** dashboard which displays statistics specific to the selected node.                                                                                                                                                                 |
|**Service List**           |The list of services deployed in the cluster. The service type and the deployment time is displayed for each service. The service can be a proxy service or a REST API.<br/><br/> You can click on a proxy service to view statistics specific for it in the **WSO2 Proxy Service Metrics** dashboard.<br/><br/> You can click on a REST API service to view statistics specific to it in the **WSO2 API Metrics** dashboard. |
|**All Time Request Count** |The total number of requests handled by the cluster.                                                                                                                                                                                                                                                                                                                                                      |
|**All Time Error Count**   |The total number of errors that have occurred for requests handled by the cluster.                                                                                                                                                                                                                                                                                                                        |
|**Request Rate**           |This is a graphical representation of the number of requests handled by the cluster against time.                                                                                                                                                                                                                                                                                                         |
|**Error Rate**             |This is a graphical representation of the number of errors that have occurred for the cluster against time.                                                                                                                                                                                                                                                                                               |
|**Response Time**          |The amount of time taken by the cluster to respond to a request against time.                                                                                                                                                                                                                                                                                                                             |


## Node dashboard

In the Node dashboard, we can view information related to a specific MI instance. We can download this dashboard from here. In this dashboard, it will show us Up Time, CPU Utilization, Thread Count,  JVM Heap Memory, Thread Count, Open File Descriptor Count, Service Count, Service List, All Time Request Count,  All Time Error Count, All Time Error Count, Request Rate, Error Rate and Response Time. Basically this dashboard shows us the overall statistics related to a MI Instance.

![Node Dashboard](../assets/img/monitoring-dashboard/grafana-node-metrics.png)

### Downloading the dashboard

You can download the dashboard from the [WSO2 EI Git repository]().

### Statistic types

The following is the list of widgets displayed in this dashboard.

| **Widget**                     | **Description**   |
|--------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Up Time**                    | The time duration that has elapsed since the node became active for the current session.|                                                                                                              |
| **Service Count**              | The number of services (i.e., proxy services and REST API services) that are currently deployed in the node.|
| **All Time Request Count**     | The total number of requests received by the node after it became a member of the current WSO2 EI setup.|
| **All Time Error Count**       | The total number of requests handled by the node that have resulted in errors.|
| **CPU Utilization**            | A visualization of the node's CPU consumption over time.|
| **JVM Heap Memory**            | A visualization of the amount of JVM heap memory consumed by the node over time.|
| **Thread Count**               | A visualization of the number of threads allocated to the node over time. |
| **Open File Descriptor Count** ||
| **Services List**              | The complete list of services (i.e., proxy services and REST API services) that are currently deployed in the node.|
| **Request Rate**               | A visualization of the total number of requests received by the node over time. |
| **Error Rate**                 | A visualization of the total number of requests handled by the node that have resulted in errors over time. |
| **Response Time**              | A visualization of the amount of time taken by the node to respond to requests over time. |

## Proxy Service dashboard

In the Proxy service dashboard, we can view information related to a specific Proxy service.

![Cluster Dashboard](../assets/img/monitoring-dashboard/grafana-proxy-services-dashboard.png)

### Downloading the dashboard

You can download the dashboard from the [WSO2 EI Git repository]().

### Statistic types

The following is the list of widgets displayed in this dashboard.

| **Widget**                    | **Description**                                                                                       |
|-------------------------------|-------------------------------------------------------------------------------------------------------|
| **Up Time**                   | The time duration that has elapsed since the proxy service started running during the current session.|
| **All Request Count**         | The total number of requests received and handled by the proxy service.                               |
| **Successful Request Count**  | The total number of requests that were successfully executed by the proxy service.                    |
| **Error Count**               | The total number of requests handled by the proxy service that have resulted in errors.               |
| **Error Percentage**          | The percentage of requests handled by the proxy service that have resulted in errors.                 |
| **Deployed Node Count**       | The number of nodes in which the proxy service is deployed.                                           |
| **Request Rate**              | A visualization of the total number of requests handled by the proxy service over time.               |
| **Error Rate**                | A visualization of the total number of errors that have occurred for the proxy service over time.     |
| **Response Time**             | A visualization of the time taken by the proxy service to respond to requests over time.              |
                                                                                                                 

## API dashboard

This dashboard displays overall statistics related to a specific API. 

### Downloading the dashboard

You can download the dashboard from the [WSO2 EI Git repository]().

### Statistic types

The following is the list of widgets displayed in this dashboard.

| **Widget**                    | **Description**                                                                                      |
|-------------------------------|------------------------------------------------------------------------------------------------------|
| **Up Time**                   | The time duration that has elapsed since the API service started running during the current session. |                                                                                                            |
| **All Time Request Count**    | The total number of requests received and handled by the API service.                                |
| **Approx. Request Count**     |                                                                                                      |
| **All Time Error Count**      | The total number of requests handled by the proxy service that have resulted in errors.              |
| **Approx. Error Count**       |                                                                                                      |
| **Deployed Node Count**       | The number of nodes in which the API service is deployed.                                            |
| **Request Rate**              | A visualization of the total number of requests handled by the API service over time.                |
| **Error Rate**                | A visualization of the total number of errors that have occurred for the API service over time.      |
| **Response Time**             | A visualization of the time taken by the API service to respond to requests over time.               |

## Inbound Endpoint dashboard

In the Inbound endpoint dashboard, we can view information related to a specific Inbound endpoint. We can download this dashboard from here. In this dashboard, it will show us Up Time,, All Request Count, Successful Request Count, Error Count, Error Percentage, Deployed Node Count, Request Rate, Error Rate and Response Time. Basically this dashboard shows us the overall statistics related to an Inbound endpoint.

### Downloading the dashboard

You can download the dashboard from the [WSO2 EI Git repository]().

### Statistic types

The following is the list of widgets displayed in this dashboard.

                                                                                                           |