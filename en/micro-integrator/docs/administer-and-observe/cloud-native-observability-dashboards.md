!!! note
    This section is still a work in progress and not tested.

# Viewing Cloud Native Observability Dashboards

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
| **Up Time**                    |                                                                                                                   |
| **Service Count**              |
| **All Time Request Count**     |
| **All Time Error Count**       |
| **CPU Utilization**            |
| **JVM Heap Memory**            |
| **Thread Count**               |
| **Open File Descriptor Count** |
| **Services List**              |
| **Request Rate**               |
| **Error Rate**                 |
| **Response Time**              |

## Proxy Service dashboard

In the Proxy service dashboard, we can view information related to a specific Proxy service. We can download this dashboard from here. In this dashboard, it will show us Up Time,, All Request Count, Successful Request Count, Error Count, Error Percentage, Deployed Node Count, Request Rate, Error Rate and Response Time. Basically this dashboard shows us the overall statistics related to a Proxy Service.

![Cluster Dashboard](../assets/img/monitoring-dashboard/grafana-proxy-services-dashboard.png)

### Downloading the dashboard

You can download the dashboard from the [WSO2 EI Git repository]().

### Statistic types

The following is the list of widgets displayed in this dashboard.

| **Widget** | **Description** |
|---------------------------|-------------------------------------------------------------------------------------------------------------------|
|                           |                                                                                                                   |

## API dashboard

In the API dashboard, we can view information related to a specific API. We can download this dashboard from here. In this dashboard, it will show us Up Time,, All Request Count, Successful Request Count, Error Count, Error Percentage, Deployed Node Count, Request Rate, Error Rate and Response Time. Basically this dashboard shows us the overall statistics related to an API.

### Downloading the dashboard

You can download the dashboard from the [WSO2 EI Git repository]().

### Statistic types

The following is the list of widgets displayed in this dashboard.

| **Widget** | **Description** |
|---------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Up Time**            |                                                                                                                   |
| **All Request Count** |
| **Successful Request Count** |
| **Error Count** |
| **Error Percentage** |
| **Deployed Node Count** |
| **Request Rate** |
| **Error Rate and Response Time** |

## Inbound Endpoint dashboard

In the Inbound endpoint dashboard, we can view information related to a specific Inbound endpoint. We can download this dashboard from here. In this dashboard, it will show us Up Time,, All Request Count, Successful Request Count, Error Count, Error Percentage, Deployed Node Count, Request Rate, Error Rate and Response Time. Basically this dashboard shows us the overall statistics related to an Inbound endpoint.

### Downloading the dashboard

You can download the dashboard from the [WSO2 EI Git repository]().

### Statistic types

The following is the list of widgets displayed in this dashboard.

                                                                                                           |