#Single Node Deployment

Wso2 streaming integrator single node deployment can be used to achieve most of the usecases which would commonly arise
in streaming integration world. The other deployment options which are namely Minimum High Available(HA) Deployment and 
Scalable High Available(HA) Deployment are mainly introduced to handle high availability , scalability and resiliency. 
But with the single node also you can achieve resilient deployment as explained below.

####Resilient deployment

Resiliency guarantees the ability to withstand or recover from any system failure and carry out the process without 
loosing any data. Streaming integrator has the capability to achieve above using a broker which can replay data from a 
certain checkpoint. Kafka is one such broker which can configure with wso2 Streaming integrator to achieve said 
advantage. The only additional configuration which need to be done on SI is the state persistence.For detailed 
instructions, see <a target="_blank" href="configuring-Database-and-File-System-State-Persistence">state persistence.

##![overview](../images/singleNodeDeployment.jpg?)


With this capability if the streaming integrator node failed to receive any incoming events, with the state persistence 
configured in SI , once the server comes back online it will start working from the latest snapshot which retrieved from 
database and ask the broker to send events which were not able to process due to the failure.

The only draw back of this approach is once the system fails there should be a mechanism to restart the server. 
