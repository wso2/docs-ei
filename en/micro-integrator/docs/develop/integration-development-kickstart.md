# Developing Your First Integration Solution

Integration developers need efficient tools to build and test all the
integration use cases required by the enterprise before pushing them
into a production environment. The following topics will guide you
through the process of building and running an example integration use
case using **WSO2 Integration Studio** . This tool contains an embedded
**WSO2 Micro Integrator** instance as well as other capabilities that
allows you to conveniently design, develop, and test your integration
artifacts before deploying them in your production environment.


## Example use case

You will set up a service (named **integration-service** ), which can
communicate with a backend service (named **order-processing-be** ) that
processes book orders. The **integration-service** consists of a REST
API artifact. This REST API ( `         forwardOrderApi)        `
receives the request that is sent from a client, and the **Call**
mediator in the API forwards the request to the URL of the backend
service ( **order-processing-be** ).

![Running a REST API on Docker](../assets/img/developer_workflow.png)

The **order-processing-be** receives the message, processes the request,
and returns the response by appending the 'orderID', 'price', and
'status' to the order details.

![Order processing backend service](../assets/img/backend.png)

## Running the use case

Follow the steps given below and get started.

### Step 1: Set up the workspace

In this guide, you will see how this use case can be executed by running
the Micro Integrator on Docker, on Kubernetes, or on a VM.

-   Install [WSO2 Integration
    Studio](https://wso2.com/integration/tooling/) .
-   Install [Docker](https://www.docker.com/) (version 17.09.0-ce or
    higher).
-   Install [Curl](https://curl.haxx.se/) .
-   [Install
    Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)
    (only required for running the use case on Kubernetes) . Once you
    have completed this step, you should have **kubectl** configured to
    use Minikube from your terminal.

### Step 2: Develop the integration artifacts

We use **WSO2 Integration Studio** to develop the integration artifacts. To run this use case, you need two REST API artifacts for the frontend service ( **integration** service) and a backend service (**order-processing** service) respectively. The synapse artifacts of these two services are given below. To run this tutorial, let's import
the pre-built artifacts of the two services to WSO2 Integration Studio, and proceed from there. If you want to build the artifacts from scratch, see the instruction in [Using WSO2 Integration Studio](../develop/creating-artifacts/creating-an-api.md).

``` xml tab="Integration Service"
<?xml version="1.0" encoding="UTF-8"?>
<api context="/forward" name="forwardOrderApi" xmlns="http://ws.apache.org/ns/synapse">
    <resource methods="POST">
        <inSequence>
            <call>
                 <endpoint>
                    <address uri="http://backed:8290/order"/>
                </endpoint>
            </call>
            <respond/>
        </inSequence>
        <outSequence/>
        <faultSequence/>
    </resource>
</api>
```

``` xml tab="Backend Service"
<?xml version="1.0" encoding="UTF-8"?>
<api context="/order" name="OrderProcessApi" xmlns="http://ws.apache.org/ns/synapse">
    <resource methods="POST">
        <inSequence>
            <payloadFactory description="order placement payload" media-type="json">
                <format>{
                "orderDetails":$1,
                "orderID":"1a23456",
                "price":25.65,
                "status":"successful"
                }</format>
                <args>
                    <arg evaluator="json" expression="$"/>
                </args>
            </payloadFactory>
            <log description="order result log" level="full"/>
            <respond description="respond with order placement details"/>
        </inSequence>
        <outSequence/>
        <faultSequence/>
    </resource>
</api>
```

To import the pre-built artifacts:

1.  Download the <a href="../../assets/attach/tutorial/MI_Tutorial.zip">project file</a> with the integration artifacts.
2.  Open WSO2 Integration Studio, and [import the project files](../../develop/importing-artifacts).
3.  The project files of the frontend (integration service) and the backend (order-processing-be service) are listed in the project explorer:

    ![project explorer](../../assets/img/developer-kickstart-proj-explorer.png)

    The **BackendService** and the **IntegrationService** project folders contain the synapse configurations of the backend service and integration service respectively. The **BackendServiceCompositeApplication** and the **IntegrationServiceCompositeApplication** project folders are the composite application projects that are used for packaging the synapse artifacts.

4.  Open the REST API of the integration service and verify that the
    endpoint URL correctly points to the backend service:
    1.  When you run the two services on Docker, the endpoint URL of the
        backend should be as follows:

        ``` xml
        <endpoint>
            <address uri="http://backend:8290/order"/>
        </endpoint>
        ```

    2.  When you run the two services on Kubernetes, the endpoint URL of
        the backend should be as follows:

        ``` xml
        <endpoint>
            <address uri="http://order-process-be-service:8290/order"/>
        </endpoint>
        ```

    3.  When you run the two services on your VM, the endpoint URL of
        the backend should be as shown below. Note that the port of the
        backend server is incremented by 1 (8291), and we are using
        localhost as the IP address.

        ``` xml
        <endpoint>
            <address uri="http://localhost:8291/order"/>
        </endpoint>
        ```

You can now build and run the artifacts in your local environment.

### Step 3: Build and Run the integration artifacts

#### Using Docker

To Build the backend (Docker image):
    
1. Right-click the **BackendServiceCompositeApplication** (application project of the Backend service) in the project explorer, and click **Generate Docker Image**.

2.  In the dialog that opens, enter the following details, and click **Next**.

    | Parameter                   |                                Description                                                                                                                                   |
    |-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Name of the application     | The name of the composite application with the artifacts created for your EI project. The name of the EI project is displayed by default, but it can be changed if required. |
    | Application version         | Enter **1.0.0** as the version of the composite application.                                                                                                                 |
    | Name of Docker Image        | Enter **backend_docker_image** as the name of the Docker image.                                                                                                            |
    | Docker Image Tag            | Enter **latest** as the tag for the Docker image to be used for reference.                                                                                                   |
    | Export Destination          | Browse for the preferred location in your machine to export the Docker image.                                                                                                |

3.  Select the integration artifacts in the **BackendService** project
    folder, and click **Finish** .  
    The Docker image of the WSO2 Micro Integrator (with the artifacts of
    the backend service) is now created and deployed your local Docker
    registry.

To build the integration service (Docker image):

1.  Right-click the **IntegrationServiceCompositeApplication**
    (application project of the integration service) in the project
    explorer, and click **Generate Docker Image** .
2.  In the dialog that opens, enter the following details, and click
    **Next**.

    |         Parameter           |                     Description                                                                                                                                              |
    |-----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | **Name of the application** | The name of the composite application with the artifacts created for your EI project. The name of the EI projecty is displayed by default, but it can be changed if required. |
    | Application version         | Enter **1.0.0** as the version of the composite application.                                                                                                                  |
    | Name of Docker Image        | Enter **integration_docker_image** as the name of the Docker image.                                                                                                         |
    | Docker Image Tag            | Enter **latest** as the tag for the Docker image to be used for reference.                                                                                                    |
    | Export Destination          | Browse for the preferred location in your machine to export the Docker image.                                                                                                 |

3.  Select the integration artifacts in the **IntegrationService**
    project folder, and click **Finish** .  
    The Docker image of the WSO2 Micro Integrator (with the artifacts of
    the integration service) is now created and deployed your local
    Docker registry.

To compose and run the Docker images:

1.  Download the <a href="../../assets/attach/tutorial/docker-compose.yml">docker-compose.yml</a> file
    (shown below) and save it to a known directory. According to the
    contents of this file, the Docker container with the backend service
    will start on port **8291** and the Docker container with the
    integration service will start on port **8290**.

    <details>
        <summary>docker-compose.yml</summary>    
        ```toml
        version: "3.7"
        services:
          service:
            image: integration_docker_image:latest
            ports:
              - 8290:8290
          backend:
            image: backend_docker_image:latest
            expose:
              - 8290
            ports:
              - 8291:8290
        ```
    </details>

2.  Open a terminal, navigate to the directory with the
    docker-compose.yml file, and execute the following command. This
    will compose the two Docker images (of the backend and the
    integration service), and start two Docker containers with the two
    images.

    ```bash
    docker-compose up -d
    ```

The two Docker containers are now running. You can now [test the integration flow](#step-4-test-the-integration-flow) .

#### Using Kubernetes

To **build the backend (Docker image)** :

1.  Right-click the **BackendServiceCompositeApplication** (application
    project of the Backend service) in the project explorer, and click
    **Generate Docker Image** .
2.  In the dialog that opens, enter the following details, and click
    **Next** .

    |    Parameter                |                                             Description                                                                                                                     |
    |-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Name of the application | The name of the composite application with the artifacts created for your EI project. The name of the EI project is displayed by default, but it can be changed if required. |
    | Application version         | Enter **1.0.0** as the version of the composite application.                                                                                                                 |
    | Name of Docker Image        | Enter **backend_docker_image_k8** as the name of the Docker image.                                                                                                        |
    | Docker Image Tag            | Enter **latest** as the tag for the Docker image to be used for reference.                                                                                                   |
    | Export Destination          | The .tar file of the Docker image will be saved to this location.                                                                                                            |

3.  Select the integration artifacts in the **BackendService** project folder, and click **Finish** .

To **build the integration service (Docker image)**:

!!! Info
    Before you build the integration service, be sure that you have changed the endpoint URL of your integration service to the following:
    ```xml
    <endpoint>
        <address uri="http://order-process-be-service:8290/order"/>
    </endpoint>
    ```

1.  Right-click the **IntegrationServiceCompositeApplication** (application project of the integration service) in the project explorer, and click **Generate Docker Image** .
2.  In the dialog that opens, enter the following details, and click **Next**.

    |    Parameter                |                                             Description                                                                                                                     |
    |-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Name of the application | The name of the composite application with the artifacts created for your EI project. The name of the EI project is displayed by default, but it can be changed if required. |
    | Application version         | Enter **1.0.0** as the version of the composite application.                                                                                                                 |
    | Name of Docker Image        | Enter **integration_docker_image_k8** as the name of the Docker image.                                                                                                    |
    | Docker Image Tag            | Enter **latest** as the tag for the Docker image to be used for reference.                                                                                                   |
    | Export Destination          | The .tar file of the Docker image will be saved to this location.                                                                                                            |

3.  Select the integration artifacts in the **IntegrationService**
    project folder, and click **Finish** .

To set up a **Minikube** cluster:

1.  Start Minikube on your terminal:

    ```bash
    minikube start
    ```

2.  Execute the command given below to start using Minikube's built-in
    Docker daemon. You need this Docker daemon to be able to create
    Docker images for the Minikube environment.

    ```bash
    eval $(minikube docker-env)
    ```

To **load the two Docker images** to the Minikube environment execute
the commands given below. Be sure to replace the file path with the
directory paths to the .tar files of your Docker images.

1.  To load the backend service, execute the following command:

    ```bash
    docker load --input <file_path-tar_file_of_backend_service>
    ```

    Verify (by running the '
    `               docker image ls              ` ' command) that the
    docker image has been loaded to Minikube without a tag.  
    ![load docker image](../assets/img/load-docker-img.png)

    Use the image ID and tag your image in Minikube:

    ``` java
    docker tag 3fd3399caa63 backend_docker_image_k8
    ```

2.  To load the integration service execute the following command:

    ```bash
    docker load --input <file_path-tar_file_of_integration_service>
    ```

    Verify (by running the '`docker image ls`' command) that the docker image has been loaded to Minikube without a tag.

    ![tag docker image](../assets/img/tag-docker-img.png) 

    Use the image ID and tag your image in Minikube:  

    ```bash
    docker tag 10d6e28754fd integration_docker_image_k8
    ```

To **compose and run the Docker images** on Minikube:

1.  Download the <a href="../../assets/attach/tutorial/docker-compose.yml">k8-deployment.yaml</a> file and save it to a know location.
2.  Navigate to the location of the k8-deployment.yaml file, and execute the following command:

    ```bash
    kubectl create -f k8s-deployment.yaml
    ```

3.  Check whether all the Kubernetes artifacts are deployed successfully by executing the following command:

    ```bash
    kubectl get all
    ```

    You will get a result similar to the following. Be sure that the deployment is in 'Running' state.

    ```bash
    NAME                                            READY   STATUS    RESTARTS   AGE
    pod/mi-helloworld-deployment-56f58c9676-djbwh   1/1     Running   0          14m
    pod/mi-helloworld-deployment-56f58c9676-xj4fq   1/1     Running   0          14m
             
    NAME                            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
    service/kubernetes              ClusterIP   10.96.0.1       <none>        443/TCP          25m
    service/mi-helloworld-service   NodePort    10.110.50.146   <none>        8290:32100/TCP   14m
    NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/mi-helloworld-deployment   2/2     2            2           14m
             
    NAME                                                  DESIRED   CURRENT   READY   AGE
    replicaset.apps/mi-helloworld-deployment-56f58c9676   2         2         2       14m
    ```

The two Docker containers are now running. You can now [test the integration flow](#step-4-test-the-integration-flow).

#### Using a VM

**Build and Run the backend service**

1.  Install the [binary of WSO2 Micro Integrator](../../setup/installation/install_in_vm/#using-the-binary-distribution). The installation location of this server instance will be referred to as MI1_HOME.
2.  Setup the backend server:
    1.  Create a CAR file from the artifacts in your BackendService
        project: Right-click the **BackendServiceCompositeApplication**
        and click **Export Composite Application Project** . Follow the
        steps on the wizard to save the CAR file.
    2.  Copy the CAR file to the `MI1_HOME/repository/deployment/server/carbonapps/` directory.
    3.  Start the Micro Integrator instance (MI1) with a port offset
        of 11. This changes the effective port of the Micro Integrator
        to **8291** . Note that the following commands are applicable if
        the product is set up [using the **installer**](../../setup/installation/install_in_vm/#using-the-installer).

        - On **MacOS/Linux/CentOS**:

            Open a terminal and execute the following command:

            ```bash 
            sudo wso2mi-1.0.0 -DportOffset=11
            ```

        - On **Windows**:
    
            First, open the `deployment.toml` file (stored in the `MI1_HOME/conf/` directory) and set the port offset to 11:

            ```toml
            [port_offset]
            offset=11
            ```

            Next, go to **Start Menu -> Programs -> WSO2 -> Micro Integrator.** This will open a terminal and start the relevant profile.

**Build and run the integration service**

!!! Info
    Before you build the integration service, be sure that you have changed the endpoint URL of your integration service to the following:

```xml
<endpoint>
    <address uri="http://localhost:8291/order"/>
</endpoint>
```

Let's run the integration service in the Micro Integrator that is
embedded in WSO2 Integration Studio: Right-click the **IntegrationServiceCompositeApplication**, and click **Export Project
Artifacts and Run** .

The two services are now running on two instances of WSO2 Micro Integrator.

### Step 4: Test the integration flow

-   If the Micro Integrator is running on Docker or a VM, open a
    terminal and send the following request using **Curl.**

    ```bash
    curl --header "Content-Type: application/json" --request POST   --data '{"store": {"book": [{"author": "Nigel Rees","title": "Sayings of the Century"},{"author": "J. R. R. Tolkien","title": "The Lord of the Rings","isbn": "0-395-19395-8"}]}}' http://localhost:8290/forward
    ```

-   If the Micro Integrator is running on Kubernetes, open a terminal
    and send the following request using curl. Be sure to replace
    MINIKUBE\IP with the IP of your Minikube installation.

    ```bash
    curl --header "Content-Type: application/json" --request POST   --data '{"store": {"book": [{"author": "Nigel Rees","title": "Sayings of the Century"},{"author": "J. R. R. Tolkien","title": "The Lord of the Rings","isbn": "0-395-19395-8"}]}}' http://MINIKUBE_IP:32100/forward
    ```

The following response will be received:

```json
{
"orderDetails":{"store": {"book": [{"author": "Nigel Rees","title": "Sayings of the Century"},{"author": "J. R. R. Tolkien","title": "The Lord of the Rings","isbn": "0-395-19395-8"}]}},
"orderID":"1a23456",
"price":25.65,
"status":"successful"
}
```

### Step 5: Publish to production

You can now publish your integration artifacts into your production environment. It is recommended to use a [CICD pipeline](../develop/using-cicd-pipeline.md).

### What's next?

-   Use [WSO2 Micro Integrator's CLI Tool](../administer-and-observe/using-the-command-line-interface.md) to check the integration artifacts deployed in the Micro Integrator instance.
-   Use **Prometheus** to [monitor your Micro Integrator](../administer-and-observe/monitoring_with_prometheus.md) instance.
