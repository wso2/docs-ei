# Kubernetes Deployment Patterns

!!! Warning
	**This content is currently work in progress**

These are the deployment patterns you can use when deploying your WSO2 Micro Integrator-based integration solutions in a Kubernetes environment. 

<!--
The following Kubernetes concepts 

-   Kuberentes cluster
-   Worker nodes
-   Pods
-   Replicas
-   High availablity in Kuberenetes
-   Load balancing in Kuberentes
-->

When you deploy your integrations, the main concern is to ensure high availability and scalability of your system. Therefore, you need to decide upon the number of worker nodes and the number of replicas that are required to scale the deployment and to ensure high availability. 

## Single Replica

The following diagram depicts a single worker node deployment, which contains a single pod (single replica).

<img src="../../../assets/img/k8s_deployment/k8s-single-pod.png">

Designing with a single-node will give you less management overhead if you are building an on-premises cluster (this does not apply to cloud instances). Also, there will be lower cost and resource requirements when compared to a multiple node cluster. 

### Load balancing

This single replica deployment is not recommended if you expect a high amount of traffic to your deployment. With only one replica, it is not possible to balance a high workload across multiple instances.

### High availability

The failure of this single worker node will take down the entire Micro Integrator deployment (depending on how many resources you have additionally). Also, if there is a failure in the single pod, there will be a downtime before a new pod is spawned again. This downtime can vary depending on the server_startup_time + artifact_deployment_time, which could lead to a significant downtime in your cluster. If this downtime is acceptable and does not negatively impact your requirements, this approach may be the easiest way for you to get started. Otherwise, to achieve high availability, you require more than one worker node (which avoids single point of failure) and multiple replicas of the pod (which avoids downtime).

## Multiple Replicas

The following diagram depicts a kubernetes cluster with multiple replicas of an integration deployment, scaled across multiple worker nodes.

<img src="../../../assets/img/k8s_deployment/k8s-muliple-workers-single-pod.png" width="1500">

Running multiple instances of an application will require a way to distribute the traffic to all of them. In this case, the cluster should be fronted by an Ingress or an external load balancer service (given that your Kubernetes environment supports external load balancers) that will distribute network traffic to all pods of an exposed deployment.

### Load balancing

This deployment pattern is suitable for handling high incoming traffic because the workload is shared by multiple instances (replicas) of the deployment. The ingress or external load balancer that fronts the deployment distributes the workload across replicas. 

### High availability

This approach ensures high availability in your cluster. If one worker node fails, the traffic will be routed to another worker node. Simillarly, if one pod replica fails, the traffic will be routed to another replica that runs concurrently at a given point of time. This avoids the impact of pod downtime. 

### Rolling updates

Because there are multiple replicas (i.e., multiple instances of the same deployment) running, you can apply rolling updates with zero downtime.

## Multiple Replicas (with Coordination)

Most of the integration solutions that you develop can be deployed using a single Micro Integrator container. That is, as explained in the previous deployment pattern, you can have multiple replicas of a single pod. Because most of these integration flows are stateless (does not need to persist status) the multiple instances (replicas) are not require to coordinate with one other. 

However, the following set of artifacts are stateful and requires coordination among themselves if they are deployed in more than a single instance.

-   Scheduled Tasks
-   Message Processors
-   Polling Inbound Endpoints
    -   File Inbound Endpoint
    -   JMS Inbound Endpoint
    -   Kafka Inbound Endpoint
-   Event-based Inbound Endpoints
    -   MQTT Inbound Endpoint
    -   RabbitMQ Inbound Endpoint

As long as you maintain a single artifact deployment for each of these artifacts (all these artifacts), no coordination is required. You can arrange your cluster in the following manner to ensure that there is no duplicate deployment of the same task in multiple containers/pods in the cluster.

<img src="../../../assets/img/k8s_deployment/k8s-muliple-workers.png" width="1500">

### Load balancing

Because these artifacts cannot be deployed in multiple containers/pods, the workload for the artifacts cannot be distributed among multipe instances when there is high traffic. However, load balancing can be achieved by isolating each of the artiacts in individual containers/pod to reduce the workload for a single instance. 

For example, as shown in the above diagram, if `recurringOrder_Task` and s`chedulOrder_Task` are highly utilized scheduled tasks in your deployment, you can distribute the tasks across multiple containers.

### High availability

Because the artifacts are deployed in one container/pod in one worker node, if the node fails or if the pod fails, the pod will be spawned again in one of the running working nodes. This avoids single point of failure. However, there will be a downtime until the pod deployment becomes active again.
