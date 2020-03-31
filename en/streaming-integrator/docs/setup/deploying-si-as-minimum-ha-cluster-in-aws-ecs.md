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
|3. Direct Events in Queue to Streams                         |The unprocessed events that are currently in the queue of the event sync server are directed into the relevant streams of the Siddhi applications.|
|4. Open Ports, Binary & Thrift Transports,<br/> and REST Endpoints|Once the above steps are completed, the previously passive and now active node opens the ports, starts the thrift and binary transports, and opens the REST endpoints.|

## Setting up the SI cluster in AWS ECS

!!! tip "Before you begin:"
    In order to deploy WSO2 Streaming Integrator in AWS ECS, you need to complete the following requisites:<br/>
     - [create an account in AWS](https://portal.aws.amazon.com).
     - [Download and install AWS CLI Version 2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).
     - Download and install Docker. For instructions, see [Docker Documentation](https://docs.docker.com/install/).

### Step 1: Store SI Docker images in ECR

1. Generate an access key and a secret key.

    1. Sign in to the IAM console via the AWS console. Under **Access Management**, click **Users**.

        ![AWS IAM console](../images/si-as-minimum-ha-cluster-in-aws-ecs/aws-iam-console.png)

        Then click **Add User** in the page that appears, and enter information as follows.

        1. In the **User name** field, enter a valid email address. Under **Access Type**, select the **Programmatic Access** check box. Then click **Next:Permissions**.

            <img src="../../images/si-as-minimum-ha-cluster-in-aws-ecs/add-user.png" alt="Create Group" border="5">


        2. Add the user you are creating to a relevant group. In this example, let's create a new group. To do this, click **Create Group** and open the **CreateGroup** dialog box. Then enter a name for the group, select the **AdministratorAccess** check box, and then click **Create Group**.
            ![Create Group](../images/si-as-minimum-ha-cluster-in-aws-ecs/create-group.png)

            Click **Next:Tags**, and in the next page click **Next:Review**.

        3. Review the information you have entered for the user. If you want to change anything, go back to the relevant previous page. If not, click **Create User** to save the information and create the user.

            Once you create the user, you can get the access key and the secret key as shown below.

            ![Get access keys and secret key](../images/si-as-minimum-ha-cluster-in-aws-ecs/get-access-key-and-secret-key.png)


2. Issue the following command in the terminal to configure AWS.

     `aws configure`

     You are prompted for the `AWS Access Key ID`, `AWS Secret Access Key`, `Default region name`, and `Default output format`. For this example, you can enter the following values.

     | **Requested Information**| **Value**                                                                           |
     |--------------------------|-------------------------------------------------------------------------------------|
     |`AWS Access Key ID`       | Enter the access key that you generated when you created the AWS user.              |
     |`AWS Secret Access Key [None]`| Enter the secret key that you generated when you created the AWS user.|
     |`Default region name`|`us-east-2`.|
     |`Default output format`   |`json`|

2. Create a repository in the ECR (Elastic Container Registry).

    1. Access AWS ECR (Elastic Container Registry) via the AWS Console.

    2. To create a new repository, click **Create a repository** -> **Get Started**. The **Create Repository** page opens.

    3. For this example, enter `wso2` as the repository name and click **Create repository** to create the repository.

        The repository is created as shown below.

        ![Repository Created](../images/si-as-minimum-ha-cluster-in-aws-ecs/created-repository.png)

    4. To retrieve an authentication token and authenticate your Docker client to your registry, do the following:

        1. In the **Repositories** page, click **View push commands**.

        2. Copy the command given under **Retrieve an authentication token and authenticate your Docker client to your registry.**. This command is similar to the following example.
            `aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 627334729308.dkr.ecr.us-east-2.amazonaws.com/wso2`

        3. Issue the command you copied in the AWS CLI. The following message appears in the CLI.

            `Login Succeeded`

3. Build Docker images with SI configurations as follows:

    1. Edit the `<SI_HOME>/conf/server/deployment.yaml` file as follows.

        1. In the `state.persistence` section, update the following parameters.

            |**Parameter**      |**Value**                              |
            |-------------------|---------------------------------------|
            |`enabled`          |`true`                                 |
            |`persistenceStore` |`org.wso2.carbon.streaming.integrator.core.persistence.DBPersistenceStore`|
            |`datasource`       |`PERSISTENCE_DB`|
            |`table`            |`PERSISTENCE_TABLE`|

            ??? tip "Click here to view the updated `state.persistence` section."
                ```
                state.persistence:
                  enabled: true
                  intervalInMin: 1
                  revisionsToKeep: 2
                  persistenceStore: org.wso2.carbon.streaming.integrator.core.persistence.DBPersistenceStore
                  config:
                    datasource: PERSISTENCE_DB
                    table: PERSISTENCE_TABLE
                ```

        2. In the `wso2.datasources:` section, add two data sources named `PERSISTENCE_DB` and `WSO2_CLUSTER_DB` as follows.

            ```
                - name: PERSISTENCE_DB
                  description: The MySQL datasource used for persistence
                  jndiConfig:
                    name: jdbc/PERSISTENCE_DB
                  definition:
                    type: RDBMS
                    configuration:
                      jdbcUrl: 'jdbc:mysql://wso2db.cxtsxcdgcayr.ap-south-1.rds.amazonaws.com:3306/persistencedb?useSSL=false'
                      username: root
                      password: rootroot
                      driverClassName: com.mysql.jdbc.Driver
                      maxPoolSize: 50
                      idleTimeout: 60000
                      connectionTestQuery: SELECT 1
                      validationTimeout: 30000
                      isAutoCommit: false
            ```

            ```
                - name: WSO2_CLUSTER_DB
                  description: The MySQL datasource used for persistence
                  jndiConfig:
                    name: jdbc/WSO2_CLUSTER_DB
                  definition:
                    type: RDBMS
                    configuration:
                      jdbcUrl: 'jdbc:mysql://wso2db.cxtsxcdgcayr.ap-south-1.rds.amazonaws.com:3306/clusterdb?useSSL=false'
                      username: root
                      password: rootroot
                      driverClassName: com.mysql.jdbc.Driver
                      maxPoolSize: 50
                      idleTimeout: 60000
                      connectionTestQuery: SELECT 1
                      validationTimeout: 30000
                      isAutoCommit: false
            ```

        3. To enable clustering and update strategy configuration values, update the `cluster.config` section as follows:

            |**Parameter**      |**Value**                              |
            |-------------------|---------------------------------------|
            |`enabled`          |`true`                                 |
            |`groupId`          |`si`|
            |`coordinationStrategyClass`|`org.wso2.carbon.cluster.coordinator.rdbms.RDBMSCoordinationStrategy`|
            |`datasource`       |`WSO2_CLUSTER_DB`|
            |`heartbeatInterval`|`5000`|
            |`heartbeatMaxRetry`|`5`|
            |`eventPollingInterval`|`5000`|

            ??? tip "Click here to view the updated `cluster.config:` section."
                ```
                    cluster.config:
                      enabled: true
                      groupId:  si
                      coordinationStrategyClass: org.wso2.carbon.cluster.coordinator.rdbms.RDBMSCoordinationStrategy
                      strategyConfig:
                        datasource: WSO2_CLUSTER_DB
                        heartbeatInterval: 5000
                        heartbeatMaxRetry: 5
                        eventPollingInterval: 5000
                ```

        4. Uncomment the `deployment.config:` section and edit the parameters as follows:

           |**Parameter**   |**Value**  |
           |----------------|-----------|
           |`type`          |`ha`       |
           |`host`          |`localhost`|
           |`advertisedHost`|`localhost`|

    2. Clone the `docker-ei` repository by issuing the following command.

        `git clone https://github.com/wso2/docker-ei`

    3. In the `docker-ei/alpine/streaming-integrator/docker-entrypoint.sh` file, enter the following code before the `# start WSO2 server` comment.

        !!! info
            In this example, it is assumed that you are using `alpine`. You can also use `centos` or `ubuntu`.

        ```
        IP=$(ifconfig eth0 | grep "inet addr" | cut -d ':' -f 2 | cut -d ' ' -f 1)
        DEPLOYMENT_YAML=${WSO2_SERVER_HOME}/conf/server/deployment.yaml
        echo "$IP"
        sed -i "s/localhost/$IP/" "$DEPLOYMENT_YAML"
        ```

        This gets the IP address of the host machine and replaces it in the HA configuration in the `<SI_HOME>/conf/server/deployment.yaml` file.

    4. Make a copy of the `<SI_HOME>/conf/server/deployment.yaml` file you edited and paste it in `docker-ei/dockerfiles/alpine/streaming-integrator` directory.

    5. In the `docker-ei/dockerfiles/alpine/streaming-integrator/deployment.yaml` file, under `wso2.carbon:`, change value for the `id` parameter to `wso2-si-1`.

        `id: wso2-si-1`

    6. To build the Docker image for node 1, navigate to the `docker-ei/dockerfiles/alpine/streaming-integrator` directory and issue the following command.

        `docker build -t wso2si:1.0.0-alpine-ha1 .`

        Once the build is successfully executed, a message similar to the following appears in the CLI.

        ![build 1 successfully executed](../images/si-as-minimum-ha-cluster-in-aws-ecs/build-node-1-executed.png)

    7. In the `docker-ei/dockerfiles/alpine/streaming-integrator/deployment.yaml` file, under `wso2.carbon`, change value for the `id` parameter to `wso2-si-2`.

        `id: wso2-si-2`

        This is because, now you are using the same repository to build the Docker image for node 2.

    8. To build the Docker image for node 2, navigate to the `docker-ei/dockerfiles/alpine/streaming-integrator` directory and issue the following command.

        `docker build -t wso2si:1.0.0-alpine-ha2 .`

        Once the build is successfully executed, a message similar to the following appears in the CLI.

        ![build 2 successfully executed](../images/si-as-minimum-ha-cluster-in-aws-ecs/build-node-2-executed.png)

    9. To tag the built images, issue the following commands.

        `docker tag wso2si:1.0.0-alpine-ha2 <AWS_ACCOUNT_NUMBER>.dkr.ecr.us-east-2.amazonaws.com/wso2:ha1`

        `docker tag wso2si:1.0.0-alpine-ha1 <AWS_ACCOUNT_NUMBER>.dkr.ecr.us-east-2.amazonaws.com/wso2:ha2`

    10. To push the images, issue the following commands.

        `docker push <AWS_ACCOUNT_NUMBER>.dkr.ecr.us-east-2.amazonaws.com/wso2:ha1`

        `docker push <AWS_ACCOUNT_NUMBER>.dkr.ecr.us-east-2.amazonaws.com/wso2:ha2`

### STEP 2: Create RDS for persisting and clustering requirements

To create a Amazon RDS (Relational Database Service) for the purpose of persisting data handled by the cluster, follow the procedure below:

1. Access Amazon RDS via the AWS console.

2. To add a parameter group and change the value for the `max_connections` parameter for it, follow the procedure below:

    1. In the left navigator, click **Parameter groups**. Then, in the **Parameter Groups** page, click **Create Parameter Group** to create a new parameter group.

        ![Create New Parameter Group](../images/si-as-minimum-ha-cluster-in-aws-ecs/create-new-parameter-group.png)

        The **Create parameter group** page opens.

    2. Enter details in the **Create parameter group** page to create a new parameter group.

        ![Create New Parameter Group](../images/si-as-minimum-ha-cluster-in-aws-ecs/create-parameter-group.png)

        Then click **Create**. The newly created parameter group appears in the **Parameter groups** page as follows.

        ![Created Parameter Group](../images/si-as-minimum-ha-cluster-in-aws-ecs/parameter-groups.png)

    3. To edit the parameter group, click on it. Then search for the **max_connections** parameter, select it, and click **Edit parameters**. Change the value for the `max_connections` parameter to `300` and then click **Save changes**.

3. Create an RDS instance as follows:

    1. In the left navigator, click **Databases**. Then in the **Databases** page, click **Create database** to open the **Create Database** page.

        ![Create New Database](../images/si-as-minimum-ha-cluster-in-aws-ecs/create-new-database.png)

    2. In the **Create Database** page, enter details as follows.

        1. Select **Standard Create**.

        2. Under **Engine Type**, select **MySQL**.

        3. Under **Templates**, select **Free Tier**.

        4. Under **Settings**, enter details as instructed within the user interface.

            ![RD Instance Settings](../images/si-as-minimum-ha-cluster-in-aws-ecs/RD-instance-settings.png)

        5. Under **Connectivity**, expand the **Additional connectivity configuration**. Under **Publicly accessible**, select **Yes**. This allows you to connect and create the database, and then check on the database values later.

            ![Additional Connectivity](../images/si-as-minimum-ha-cluster-in-aws-ecs/additional-connectivity.png)

        6. Expand the **Additional Configurations** section. In the **DB parameter group** field, select the parameter group that you previously created.

            ![Additional Configurations](../images/si-as-minimum-ha-cluster-in-aws-ecs/additional-configurations.png)

        7. Click **Create database**. The database you created appears in the **Databases** page.

    3. Enable public access to the database as follows:

        1. In the **Databases** page, click on the **wso2** database you created to view details of it in a separate page.

        2. In the **Security** section, click the VPC.

            ![Security VPC](../images/si-as-minimum-ha-cluster-in-aws-ecs/database-vpc.png)

        3. In the **Security Groups** page that opens, click on the relevant security group to view details of it in a separate page.

            ![Security Group](../images/si-as-minimum-ha-cluster-in-aws-ecs/security-group.png)

        4. In the page with details of your security group, click **Edit Inbound Rules**.

            ![Edit Inbound Rules](../images/si-as-minimum-ha-cluster-in-aws-ecs/edit-inbound-rules.png)

            This opens the **Edit inbound rules** page.

        5. In the **Edit inbound rules** page, add two rules as follows.

            |**Type**        |**Source**                                                                            |
            |----------------|--------------------------------------------------------------------------------------|
            |**MYSQL/AURORA**|Select **Custom** for the **Source** field and then select **0.0.0.0/0** as the value.|
            |**MYSQL/AURORA**|Select **Custom** for the **Source** field and then select **::/0** as the value.     |

            Then click **Save rules**.

### Step 3: Set up ECS cluster, tasks and services

1. To create a VPC (Virtual Private Cloud), a subnet, and a security group, follow the procedure below:

    1. Access the VPC Dashboard via the AWS Console.

    2. Click **Launch VPC Wizard**.

    3. In **Step 1: Select a VPC Configuration**, **VPC with a Single Public Subnet** is selected by default in the left pane. Click **Select** for it.

        ![Select VPC Type](../images/si-as-minimum-ha-cluster-in-aws-ecs/edit-inbound-rules.png)

    4. In **Step 2: VPC with a Single Public Subnet**, enter `si-ha-vpc` in the **VPC name** field. Then click **Create VPC**.

        ![Create VPC](../images/si-as-minimum-ha-cluster-in-aws-ecs/create-vpc.png)

    After successfully creating the VPC, you can view it in the **Your VPCs** page as follows:

    ![Created VPC](../images/si-as-minimum-ha-cluster-in-aws-ecs/created-vpc.png)

    To view the public subnet created, click **Subnets** in the left navigator. The subnet connected to the VPC is displayed as shown below.

    ![Created subnet](../images/si-as-minimum-ha-cluster-in-aws-ecs/created-subnet.png)

    To view the security group of the VPC, click **Security Groups** in the left navigator. The security group is displayed as follows.

    ![security group](../images/si-as-minimum-ha-cluster-in-aws-ecs/security-groups.png)

    !!! info
        When you select the security group and view details for it at the bottom of the **Security Groups** page, note that all incoming traffic is currently enabled for now in the **Inbound Rules** tab.

        ![Inbound Rules](../images/si-as-minimum-ha-cluster-in-aws-ecs/inbound-rules.png)

2. Set up the cluster as follows:

