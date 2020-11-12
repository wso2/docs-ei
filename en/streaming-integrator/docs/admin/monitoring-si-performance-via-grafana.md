# Monitoring ETL Statistics with Grafana

!!! tip "Before you begin:"
    To enable WSO2 Streaming Integrator to publish statistics in Grafana, follow the instructions in [Setting up Grafana to Display WSO2 SI Statistics](../admin/setting-up-grafana-dashboards.md).<br/><br/>
    To enable a Siddhi application to publish statistics to publish its statistics to Grafana, you need to specify Prometheus as the the statistics reporter via the `@App:statistics` annotation as shown below.<br/><br/>
    `@App:statistics(reporter = 'prometheus')`<br/><br/>
    For the dashboards to display statistics, at least one Siddhi application with the above configuration needs to be deployed in the Streaming Integrator Server.

## Setting Up Grafana to Monitor WSO2 Streaming Integrator

For the purpose of monitoring the performance of WSO2 Streaming Integrator, it provides ten pre-configured dashboards. To view them in Grafana, follow the steps below:
 
 1. Download the following dashboards (i.e., the JSON file with the dashboard configuration) by clicking on them.
 
    - [Siddhi Overall Statistics](../examples/resources/dashboards/Siddhi%20Overall%20Statistics-1580282204141.json)
    
    - [Siddhi Server Statistics](../examples/resources/dashboards/Siddhi%20Server%20Statistics-1580282223378.json)
    
    - [Siddhi Stream Statistics](../examples/resources/dashboards/Siddhi%20Stream%20Statistics-1580282240055.json)
    
    - [Siddhi Source Statistics](../examples/resources/dashboards/Siddhi%20Source%20Statistics-1580285208526.json)
    
    - [Siddhi Sink Statistics](../examples/resources/dashboards/Siddhi%20Sink%20Statistics-1580285225995.json)
    
    - [Siddhi Query Statistics](../examples/resources/dashboards/Siddhi%20Query%20Statistics-1580282256683.json)
    
    - [Siddhi Window Statistics](../examples/resources/dashboards/Siddhi%20Window%20Statistics-1580285269330.json)
    
    - [Siddhi Trigger Statistics](../examples/resources/dashboards/Siddhi%20Trigger%20Statistics-1580285383360.json)
    
    - [Siddhi Table Statistics](../examples/resources/dashboards/Siddhi%20Table%20Statistics-1580285249969.json)
    
    - [Siddhi Aggregation Statistics](../examples/resources/dashboards/Siddhi%20Aggregation%20Statistics-1580285366634.json)
    
    - [Siddhi On Demand Query Statistics](../examples/resources/dashboards/Siddhi%20On-Demand%20Query%20Statistics-1580285396455.json)
    
 2. Start and access Grafana. Then import the eleven dashboards you downloaded in the previous step. For more information, see [Managing Grafana Dashboards - Importing Dashboards](managing-grafana-dashboards.md#importing-dashboards).
    

 
## Accessing Grafana Dashboards for Monitoring

To navigate through the Grafana dashboards you set up for monitoring WSO2 Streaming Integrator and to analyze statistics, follow the procedure below:

1. Start Grafana and access it via `http://localhost:3000/`.

2. In the left pane, click the **Dashboards** icon, and then click **Manage** to open the **Dashboards** page. Here, you can click on the following dashboards:

    - **Siddhi Overall Statistics**
    
    - **Siddhi Server Statistics**
    
    - **Siddhi Stream Statistics**
    
    - **Siddhi Source Statistics**
    
    - **Siddhi Sink Statistics**
    
    - **Siddhi Query Statistics**
    
    - **Siddhi Window Statistics**
    
    - **Siddhi Trigger Statistics**

    - **Siddhi Table Statistics**

    - **Siddhi Aggregation Statistics**
    
    - **Siddhi On Demand Query Statistics**