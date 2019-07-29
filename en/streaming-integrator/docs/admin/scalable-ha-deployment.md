#Scalable High Availability (HA) Deployment

Scalable high availability deployment predominantly focused on scaling the system according to the load or the TPS of
the system. This is achieved With the help of horizontal scalability.

Streaming Integration underneath uses siddhi as the streaming language. In siddhi one can write siddhi logic in a 
stateless and stateful way. 

Stateless operations include filters , database operations etc and stateful operations include window operations , 
aggregations etc which keep data in memory to carry out calculations.

The deployment options for a scalable streaming integrator depends on the stateless and statefulness of siddhi apps. 
Below are the detail descriptions of two approaches.

#### Stateless Scalable High Availability (HA) Deployment

As described in the stateless scenario in order to scale we can keep adding SI servers to the system and front them with
 a load balancer which will publish the events in round robin way. 
 
See below the architecture depicted 

##![overview](../images/statelessDeploymentOverview.jpg?)


#### Stateful Scalable High Availability (HA) Deployment

As described earlier stateful operations keep state data in memory thus inorder to scale such system we need to process 
particular data on same node without processing same state data in different servers. So to achieve this we can use data 
partitioning so that one bucket of partitioned data will only process in one particular server. 

!!! note

In order to scale stateful operations it is a must to have some kind of partitioning attribute available so that 
partitioned data can be processed independently.

See below the high level diagram of event flow and components to achieve scalable stateful high available deployment.

##![overview](../images/statefulDeploymentOverview.jpg?)

Below describes each component in detail and how to configure them with streaming integrator.

#####Partitioning Layer

As depicted above first we need to have a partitioning layer. Here we are using a SI server to achieve it. This layer 
responsible of consuming events from output sources and then partition the events based on a partitioning condition. 

In order to partition you can leverage the Distributed sink extension in Streaming integration. Consider following 
sample siddhi app syntax which defines a stream and how the distributed sink can be applied to partition data and for
this example data are partitioned from tenant domain. To see more check on 
<a target="_blank" href="https://siddhi.io/en/v5.0/docs/query-guide/#distributed-sink">distributed sink</a>

!!! note

Below configuration(Request stream) only consist on how to send events out for load balancers via http for each 
partition. In addition there should be a logic to consume events coming from outside and pass it to Request stream.

Eg:-

    // Stream defined with distributed sink with partitioned stratergy      
    @Sink(type = 'http',
              @map(type='json'),
              @distribution(strategy='partitioned', partitionKey='userTenantDomain',
              @destination(publisher.url='http://Ip1:8009/Request'),
              @destination(publisher.url='http://Ip2:8009/Request')))
    define stream Request (meta_clientType string,
         applicationConsumerKey string,
         applicationName string,
         applicationId string,
         applicationOwner string,
         apiContext string,
         apiName string,
         apiVersion string,
         apiCreator string,,
         apiTier string,
         apiHostname string,
         username string,
         userTenantDomain string,,
         requestTimestamp long,
         throttledOut bool,
         responseTime long,
         backendTime long);
         
    
According to above distributed sink configuration, events which comes to Request stream will be partitioned based on 
userTenantDomain attribute. So if there are two tenant domain values "fooDomain" and "barDomain" , then events belong 
"fooDomain" might publish to Ip1 and the events belong to "barDomain" will publish into Ip2.Distributed sink will make
sure that unique partitioned events will not distribute across the cluster.Here Ip1 and Ip2 represents the load balancer
IP's. Reason for using load balancers because stateful layer also contain two SI servers to handle the high 
availability. Hence we need a load balancer to send traffic in fail over manner.

According to above diagram there are four partitions hence the use of four load balancers.

Since we need the high availability in the partitioning layer we can use two SI servers (minimum) as depicted in the 
diagram.

#####Stateful Layer

Responsibility of this layer is to consume events according to each partition and carry out rest of the stateful 
operations.Since we have partitioned data we can seamlessly handle the scalability of the system. Also we have to address 
the high availability of the system as well. Thus for each partition we can deploy the system as mentioned in two node 
minimum high available deployment section. Basically for each partition or partitions there will be a separate cluster 
of two SI nodes. So if active node fails for a particular partition the other node in the cluster will carry out the 
work.

In order to configure the stateful layer you can follow the minimum high availability deployment guide. The only 
difference in configurations regarding this layer would be , as mentioned since we maintain separate clusters for each
partition the Cluster Configuration **group id** should be different for each cluster.

    - Sample cluster configuration can be as below  :
          
      - cluster.config:
          enabled: true
          groupId:  group 1
          coordinationStrategyClass: org.wso2.carbon.cluster.coordinator.rdbms.RDBMSCoordinationStrategy
          strategyConfig:
            datasource: WSO2_CLUSTER_DB
            heartbeatInterval: 5000
            heartbeatMaxRetry: 5
            eventPollingInterval: 5000
            
            
// do we need to mention the advance configuring of stateful siddhi apps by putting a consuming siddhii apps which will
put to a inmemory sink so that multiple inmemory sources can subscribe for them 


  

 




