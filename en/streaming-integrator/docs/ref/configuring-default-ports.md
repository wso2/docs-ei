# Configuring Default Ports

This page describes the default ports that are used for each runtime
when the port offset is 0 .

-   [Common Ports](#ConfiguringDefaultPorts-CommonPortsCommonPorts)
-   [Worker
    Runtime](#ConfiguringDefaultPorts-WorkerRuntimeWorkerRuntime)
-   [Manager
    Runtime](#ConfiguringDefaultPorts-ManagerRuntimeManagerRuntime)
-   [Dashboard
    Runtime](#ConfiguringDefaultPorts-DashboardRuntimeDashboardRuntime)
-   [Editor
    Runtime](#ConfiguringDefaultPorts-EditorRuntimeEditorRuntime)
-   [Clustering
    Ports](#ConfiguringDefaultPorts-ClusteringPortsClusteringPorts)
    -   [Distributed
        deployment:](#ConfiguringDefaultPorts-Distributeddeployment:)
        -   [Manager Node:](#ConfiguringDefaultPorts-ManagerNode:)
        -   [Worker Node(Resource
            Node):](#ConfiguringDefaultPorts-WorkerNode(ResourceNode):)
    -   [Minimum High Availability (HA)
        Deployment:](#ConfiguringDefaultPorts-MinimumHighAvailability(HA)Deployment:)
        -   [Worker Node:](#ConfiguringDefaultPorts-WorkerNode:)
    -   [Multi Datacenter High Availability
        Deployment](#ConfiguringDefaultPorts-MultiDatacenterHighAvailabilityDeployment)

#### Common Ports

The following ports are common to all runtimes.

|      |                                                                         |
|------|-------------------------------------------------------------------------|
| 7611 | Thrift TCP port to receive events from clients.                         |
| 7711 | Thrift SSL port for secure transport where the client is authenticated. |
| 9611 | Binary TCP port to receive events from clients.                         |
| 9711 | Binary SSL port for secure transport where the client is authenticated. |

You can offset binary and thrift by configuring the offset parameter in
the `         <SP_HOME>/conf/<PROFILE>/deployment.yaml        ` file.
The following is a sample configuration.

``` xml
    # Carbon Configuration Parameters
    wso2.carbon:
      # value to uniquely identify a server
      id: wso2-sp
      # server name
      name: WSO2 Stream Processor
      # server type
      # type: wso2-sp
      # ports used by this server
      ports:
        # port offset
        offset: 1
```

  

#### Worker Runtime

|      |                       |
|------|-----------------------|
| 9090 | HTTP netty transport  |
| 9443 | HTTPS netty transport |

####  Manager Runtime

|      |                       |
|------|-----------------------|
| 9190 | HTTP netty transport  |
| 9543 | HTTPS netty transport |

####  Dashboard Runtime

|      |                       |
|------|-----------------------|
| 9290 | HTTP netty transport  |
| 9643 | HTTPS netty transport |

####  Editor Runtime

|      |                       |
|------|-----------------------|
| 9390 | HTTP netty transport  |
| 9743 | HTTPS netty transport |

!!! tip

The following example shows how to overide the default netty port for
the Editor profile by updating the required parameters in the
`         <SP_HOME>/conf/editor/deployment.yaml        ` file.

``` xml
    wso2.transport.http:
     transportProperties:
      listenerConfigurations:
     -
          id: "default"
     port: 9390
        -
          id: "msf4j-https"
     port: 9743
```
    

  

  

#### Clustering Ports

Ports that are required for clustering deployment:

##### Distributed deployment:

###### Manager Node:

|      |                                                                                                                                                                                                                                     |
|------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 9190 | HTTP netty transport.                                                                                                                                                                                                               |
| 9190 | Specify the port of the node for the `             httpInterface            ` parameter in the `             cluster.config            ` section . The HTTP netty transport port is considered the default port for HTTP interface. |
| 9543 | HTTPS netty transport.                                                                                                                                                                                                              |
| 9092 | The Kafka Server bootstrapURLs Port. This is the default port where the external kafka server starts and the manager communicates.                                                                                                  |
| 2181 | Zookeeper Server zooKeeperURLs Port. This is the default port where the external zookeeper server starts and the manager communicates.                                                                                              |

###### Worker Node(Resource Node):

|      |                                                                                                                                                                                                                                    |
|------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 9090 | HTTP netty transport.                                                                                                                                                                                                              |
| 9090 | Specify the port of the node for the `             httpInterface            ` parameter in the `             cluster.config            ` section. The HTTP netty transport port is considered the default port for HTTP interface. |
| 9191 | Specify the port of the resource manager for the `             resourceManagers            ` parameter in the cluster.config section. The netty transport port of the manager node is considered the default port.                 |
| 9443 | HTTPS netty transport.                                                                                                                                                                                                             |

##### Minimum High Availability (HA) Deployment:

###### Worker Node:

|      |                                                                                                                                                                                 |
|------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 9090 | HTTP netty transport.                                                                                                                                                           |
| 9090 | Specify the port of the node for the advertisedPort parameter in the `             liveSync            ` section. The HTTP netty transport port is considered the default port. |
| 9443 | HTTPS netty transport.                                                                                                                                                          |

##### Multi Datacenter High Availability Deployment

Other than the ports used in clustering setups (i.e., a Minimum HA
Deployment or a Fully Distributed Deployment) , the following is
required:

|      |                                                                                                                                                                                                                                                                                     |
|------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 9092 | Ports of the two separate instances of the broker deployed in each data center. (e.g., `             bootstrap.servers=            ` `             'host1:9092, host2:9092',             default             9092             where the external kafka servers start            ` ) |
