!!! note
    This section is still a work in progress!

# Deploying the Streaming Integrator as a Minimum HA Cluster in AWS ECS

## Introduction

This section explains how to run WSO2 Streaming Integrator as a minimum HA (High Availability) cluster in AWS (Amazon Web Services) ECS(Elastic Container Service).

The minimum HA cluster typically has two nodes where one node operates as the active node and the other as the passive node. Each node maintains a communication link with the other node as well as with the database.

### Assigning the Active and Passive statuses to nodes

![Assign Node Active or Passive Status](../images/si-as-minimum-ha-cluster-in-aws-ecs/assigning-node-status.png)

When a node is started in the minimum HA cluster mode, it checks the tables in the `WSO2_CLUSTER_DB` database. This check covers checking whether there are existing members in the cluster. If other nodes already exist as members of the cluster, it checks whether there are heartbeats from the existing member(s) for the last time interval that is of the same length as the specified heartbeat interval. If no heartbeat exists for the specified time interval, the node is added to the cluster as the active node. If not, it is added as the passive node.

Once a node becomes the active node, it performs the following:

- Inserts its own details in the `WSO2_CLUSTER_DB` database or updates them if they already exist. The details include `nodeId`, `clusterGroup`, `propertyMap`, and `heartbeatTimestamp`.<br/>
- Periodically updates the appropriate table of the `WSO2_CLUSTER_DB` database with its heartbeat (timestamp).<br/>
- Starts the Siddhi applications in runtime and opens all the ports mentioned in the Siddhi applications.<br/>
- Starts the binary and thrift transports.<br/>
- Starts the REST endpoints.<br/>
- Once a new member (i.e., passive node) joins the cluster, the active node checks the `WSO2_CLUSTER_DB` database for the host and port of ther other member's event syncing server. Once this information is retrieved, it sends the input events received by the cluster to that event syncing server.

### Operating the nodes

When the cluster is running with both the nodes, the active node sends each event received by the cluster to the passive node's event sync server with the event timestamp. It also persists the state (windows, sequences, patterns, aggregations, etc.,) in the database named `PERSISTENSE_DB`.
Each time the state is persisted, the active node sends a control message to the passive node.

The passive node saves the events sent to its event sync server in a queue. When it receives the control message from the active node, it trims the queue to keep only the latest events that were not used by the active node to build the state of Siddhi applications at the time of persisting the state.

### Handling the failure of the active node

The passive node continuously monitors the heartbeat of the active node. If the active node fails, the passive node follows the process shown below to start functioning as the active node so that data is not lost due to node failure.

![Passive Node Becomes Active](../images/si-as-minimum-ha-cluster-in-aws-ecs/passive-node-becomes-active-process.png)

The following table explains the above steps.

|**Step**|**Description**|
|----------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
|1. Start Siddhi Application Runtime                          |This is done without opening any ports mentioned in Siddhi applications. This is because the Siddhi application statuses need to be restored to what they were before the node failure so that unprocessed events before the failure are not lost.|
|2. Restore State                                             |This is done by retrieving the states persisted in the `PERSISTENSE_DB` database. |
|3. Direct Events in Queue to Streams                         |The unprocessed eevents that are currently in the queue of the event sync server are directed into the relevant streams of the Siddhi applications.|
|4. Open Ports, Binary & Thrift Transports,<br/> and REST Endpoints|Once the above steps are completed, the previously passive and now active node opens the ports, starts the thrift and binary transports, and opens the REST endpoints.|
