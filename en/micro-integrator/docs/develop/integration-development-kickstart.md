# Developing Cloud Native Integration

Integration developers need efficient tools to build and test all the
integration use cases required by the enterprise before pushing them
into a production environment. The following topics will guide you
through the process of building and running an example integration use
case using **WSO2 Integration Studio** . This tool contains an embedded
**WSO2 Micro Integrator** instance as well as other capabilities that
allows you to conveniently design, develop, and test your integration
artifacts before deploying them in your production environment.

-   [Example use case](#DevelopingCloudNativeIntegration-Exampleusecase)
-   [Running the use
    case](#DevelopingCloudNativeIntegration-Runningtheusecase)
    -   [Step 1: Set up the
        workspace](#DevelopingCloudNativeIntegration-Step1:Setuptheworkspace)
    -   [Step 2: Develop the integration
        artifacts](#DevelopingCloudNativeIntegration-Step2:Developtheintegrationartifacts)
    -   [Step 3: Build and Run the integration
        artifacts](#DevelopingCloudNativeIntegration-Step3:BuildandRuntheintegrationartifacts)
    -   [Step 4: Test the integration
        flow](#DevelopingCloudNativeIntegration-testingStep4:Testtheintegrationflow)
    -   [Step 5: Publish to
        production](#DevelopingCloudNativeIntegration-Step5:Publishtoproduction)

### Example use case

You will set up a service (named **integration-service** ), which can
communicate with a backend service (named **order-processing-be** ) that
processes book orders. The **integration-service** consists of a REST
API artifact. This REST API ( `         forwardOrderApi)        `
receives the request that is sent from a client, and the **Call**
mediator in the API forwards the request to the URL of the backend
service ( **order-processing-be** ).

![Running a REST API on
Docker](attachments/119136105/119136106.png "Running a REST API on Docker"){width="550"}

The **order-processing-be** receives the message, processes the request,
and returns the response by appending the 'orderID', 'price', and
'status' to the order details.

![Order processing backend
service](attachments/119136105/119136107.png "Order processing backend service"){width="650"}  

### Running the use case

Follow the steps given below and get started.

#### Step 1: Set up the workspace

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

#### Step 2: Develop the integration artifacts

We use **WSO2 Integration Studio** to develop the integration artifacts.
To run this use case, you need two REST API artifacts for the frontend
service ( **integration** service) and a backend service (
**order-processing** service) respectively. The synapse artifacts of
these two services are given below. To run this tutorial, let's [import
the pre-built
artifacts](#DevelopingCloudNativeIntegration-import_artifacts) of the
two services to WSO2 Integration Studio, and proceed from there. If you
want to build the artifacts from scratch, see [Working with WSO2
Integration
Studio](https://docs.wso2.com/display/EI650/Working+with+WSO2+Integration+Studio)
.

![](images/icons/grey_arrow_down.png){.expand-control-image} Integration
service

-   [**Design View**](#59299674bbaa4a9fb5619dd4d60cec99)
-   [**Source View**](#0c639e4d49284ca0a98501cf206b93bd)

![](attachments/119136105/119136108.png){width="700"}

``` java
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

![](images/icons/grey_arrow_down.png){.expand-control-image} Backend
service

-   [**Design View**](#00e4a2c0a07f4a78ba59af1adfd6e92b)
-   [**Source View**](#5272304b87e64862ab8bec8e000cf04d)

![](attachments/119136105/119136109.png){width="800" height="414"}

``` java
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

To export the pre-built artifacts:

1.  Download the [project files](attachments/119136105/119136889.zip)
    with the integration artifacts.
2.  Open WSO2 Integration Studio, and [import the project
    files](https://docs.wso2.com/display/EI650/Working+with+WSO2+Integration+Studio#WorkingwithWSO2IntegrationStudio-ImportingESBprojects)
    .

3.  The project files of the frontend (integration service) and the
    backend (order-processing-be service) are listed in the project
    explorer:

    ![](attachments/119136105/119136110.png){width="300" height="184"}

    The **BackendService** and the **IntegrationService** project
    folders contain the synapse configurations of the backend service
    and integration service respectively. The
    **BackendServiceCompositeApplication** and the
    **IntegrationServiceCompositeApplication** project folders are the
    composite application projects that are used for packaging the
    synapse artifacts.

4.  Open the REST API of the integration service and verify that the
    endpoint URL correctly points to the backend service:
    1.  When you run the two services on Docker, the endpoint URL of the
        backend should be as follows:

        ``` java
                <endpoint>
                    <address uri="http://backend:8290/order"/>
                </endpoint>
        ```

    2.  When you run the two services on Kubernetes, the endpoint URL of
        the backend should be as follows:

        ``` java
                    <endpoint>
                        <address uri="http://order-process-be-service:8290/order"/>
                    </endpoint>
        ```

    3.  When you run the two services on your VM, the endpoint URL of
        the backend should be as shown below. Note that the port of the
        backend server is incremented by 1 (8291), and we are using
        localhost as the IP address.

        ``` java
                    <endpoint>
                        <address uri="http://localhost:8291/order"/>
                    </endpoint>
        ```

You can now build and run the artifacts in your local environment.

#### Step 3: Build and Run the integration artifacts

![](images/icons/grey_arrow_down.png){.expand-control-image} Build and
Run on Docker

To **Build the backend (Docker image)** :

1.  Right-click the **BackendServiceCompositeApplication** (application
    project of the Backend service) in the project explorer, and click
    **Generate Docker Image** .
2.  In the dialog that opens, enter the following details, and click
    **Next** .

    |                             |                                                                                                                                                                              |
    |-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | **Name of the application** | The name of the composite application with the artifacts created for your EI project. The name of the EI project is displayed by default, but it can be changed if required. |
    | Application version         | Enter **1.0.0** as the version of the composite application.                                                                                                                 |
    | Name of Docker Image        | Enter **backend\_docker\_image** as the name of the Docker image.                                                                                                            |
    | Docker Image Tag            | Enter **latest** as the tag for the Docker image to be used for reference.                                                                                                   |
    | Export Destination          | Browse for the preferred location in your machine to export the Docker image.                                                                                                |

3.  Select the integration artifacts in the **BackendService** project
    folder, and click **Finish** .  
    The Docker image of the WSO2 Micro Integrator (with the artifacts of
    the backend service) is now created and deployed your local Docker
    registry.

To **build the integration service (Docker image)** :

1.  Right-click the **IntegrationServiceCompositeApplication**
    (application project of the integration service) in the project
    explorer, and click **Generate Docker Image** .
2.  In the dialog that opens, enter the following details, and click
    **Next** .

    |                             |                                                                                                                                                                               |
    |-----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | **Name of the application** | The name of the composite application with the artifacts created for your EI project. The name of the EI projecty is displayed by default, but it can be changed if required. |
    | Application version         | Enter **1.0.0** as the version of the composite application.                                                                                                                  |
    | Name of Docker Image        | Enter **integration\_docker\_image** as the name of the Docker image.                                                                                                         |
    | Docker Image Tag            | Enter **latest** as the tag for the Docker image to be used for reference.                                                                                                    |
    | Export Destination          | Browse for the preferred location in your machine to export the Docker image.                                                                                                 |

3.  Select the integration artifacts in the **IntegrationService**
    project folder, and click **Finish** .  
    The Docker image of the WSO2 Micro Integrator (with the artifacts of
    the integration service) is now created and deployed your local
    Docker registry.

To **compose and run the Docker images** :

1.  Download the
    [docker-compose.yml](attachments/119136105/119136934.yml) file
    (shown below) and save it to a known directory. According to the
    contents of this file, the Docker container with the backend service
    will start on port **8291** and the Docker container with the
    integration service will start on port **8290** .

    ![](images/icons/grey_arrow_down.png){.expand-control-image}
    docker-compose.yml

    ``` java
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

2.  Open a terminal, navigate to the directory with the
    docker-compose.yml file, and execute the following command. This
    will compose the two Docker images (of the backend and the
    integration service), and start two Docker containers with the two
    images.

    ``` java
            docker-compose up -d
    ```

The two Docker containers are now running. You can now [test the
integration flow](#DevelopingCloudNativeIntegration-testing) .

![](images/icons/grey_arrow_down.png){.expand-control-image} Build and
Run on Kubernetes

To **build the backend (Docker image)** :

1.  Right-click the **BackendServiceCompositeApplication** (application
    project of the Backend service) in the project explorer, and click
    **Generate Docker Image** .
2.  In the dialog that opens, enter the following details, and click
    **Next** .

    |                             |                                                                                                                                                                              |
    |-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | **Name of the application** | The name of the composite application with the artifacts created for your EI project. The name of the EI project is displayed by default, but it can be changed if required. |
    | Application version         | Enter **1.0.0** as the version of the composite application.                                                                                                                 |
    | Name of Docker Image        | Enter **backend\_docker\_image\_k8** as the name of the Docker image.                                                                                                        |
    | Docker Image Tag            | Enter **latest** as the tag for the Docker image to be used for reference.                                                                                                   |
    | Export Destination          | The .tar file of the Docker image will be saved to this location.                                                                                                            |

3.  Select the integration artifacts in the **BackendService** project
    folder, and click **Finish** .

To **build the integration service (Docker image)** :

!!! tip

Before you build the integration service, be sure that you have changed
the endpoint URL of your integration service to the following:

``` java
    <endpoint>
        <address uri="http://order-process-be-service:8290/order"/>
    </endpoint>
```
    

1.  Right-click the **IntegrationServiceCompositeApplication**
    (application project of the integration service) in the project
    explorer, and click **Generate Docker Image** .
2.  In the dialog that opens, enter the following details, and click
    **Next** .

    |                             |                                                                                                                                                                              |
    |-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | **Name of the application** | The name of the composite application with the artifacts created for your EI project. The name of the EI project is displayed by default, but it can be changed if required. |
    | Application version         | Enter **1.0.0** as the version of the composite application.                                                                                                                 |
    | Name of Docker Image        | Enter **integration\_docker\_image\_k8** as the name of the Docker image.                                                                                                    |
    | Docker Image Tag            | Enter **latest** as the tag for the Docker image to be used for reference.                                                                                                   |
    | Export Destination          | The .tar file of the Docker image will be saved to this location.                                                                                                            |

3.  Select the integration artifacts in the **IntegrationService**
    project folder, and click **Finish** .

To set up a **Minikube** cluster:

1.  Start Minikube on your terminal:

    ``` java
        minikube start
    ```

2.  Execute the command given below to start using Minikube's built-in
    Docker daemon. You need this Docker daemon to be able to create
    Docker images for the Minikube environment.

    ``` java
            eval $(minikube docker-env)
    ```

To **load the two Docker images** to the Minikube environment execute
the commands given below. Be sure to replace the file path with the
directory paths to the .tar files of your Docker images.

1.  To load the backend service, execute the following command:

    ``` java
            docker load --input <file_path-tar_file_of_backend_service>
    ```

    Verify (by running the '
    `               docker image ls              ` ' command) that the
    docker image has been loaded to Minikube without a tag.  
    ![](attachments/119136105/119136446.png){width="1000" height="38"}

    Use the image ID and tag your image in Minikube:

    ``` java
            docker tag 3fd3399caa63 backend_docker_image_k8
    ```

2.  To load the integration service execute the following command:

    ``` java
            docker load --input <file_path-tar_file_of_integration_service>
    ```

    Verify (by running the '
    `               docker image ls              ` ' command) that the
    docker image has been loaded to Minikube without a tag .

    ![](attachments/119136105/119136447.png){width="1000" height="55"}  

    Use the image ID and tag your image in Minikube:  

    ``` java
            docker tag 10d6e28754fd integration_docker_image_k8
    ```

To **compose and run the Docker images** on Minikube:

1.  Download the
    [k8-deployment.yaml](attachments/119136105/119136409.yaml) file and
    save it to a know location.

2.  Navigate to the location of the k8-deployment.yaml file , and
    execute the following command:

    ``` java
            kubectl create -f k8s-deployment.yaml
    ```

      

3.  Check whether all the Kubernetes artifacts are deployed successfully
    by executing the following command:
      

    ``` java
            kubectl get all
    ```

    You will get a result similar to the following. Be sure that the
    deployment is in 'Running' state.

    ``` java
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

The two Docker containers are now running. You can now [test the
integration flow](#DevelopingCloudNativeIntegration-testing) .

![](images/icons/grey_arrow_down.png){.expand-control-image} Build and
Run on a VM

**Build and Run the backend service**

1.  Install the [binary of WSO2 Micro
    Integrator](https://docs.wso2.com/display/EI650/Installing+WSO2+Micro+Integrator#InstallingWSO2MicroIntegrator-MicroIntegratoronaVM)
    . The installation location of this server instance will be referred
    to as \<MI1\_HOME\>.
2.  Setup the backend server
    1.  Create a CAR file from the artifacts in your BackendService
        project: Right-click the **BackendServiceCompositeApplication**
        and click **Export Composite Application Project** . Follow the
        steps on the wizard to save the CAR file.
    2.  Copy the CAR file to the
        `                <MI1_HOME>/repository/deployment/server/carbonapps/               `
        directory.
    3.  Start the Micro Integrator instance (MI1) with a port offset
        of 11. This changes the effective port of the Micro Integrator
        to **8291** . Note that the following commands are applicable if
        the product is set up [using the
        **installer**](https://docs.wso2.com/display/EI650/Installing+WSO2+Micro+Integrator#InstallingWSO2MicroIntegrator-RunningtheMicroIntegrator)
        .

        -   [**On
            MacOS/Linux/CentOS**](#a823082378094767aea79d0b5dcd67a5)
        -   [**On Windows**](#c44f97607dde40aebacf783382156201)

        Open a terminal and execute the following command:

        ``` java
                    sudo wso2mi-1.0.0 -DportOffset=11
        ```

        First, open the
        `                         carbon.xml                        `
        file (stored in the
        `                         <MI1_HOME>/conf/                        `
        directory) and set the port offset to 11:

        ``` java
                    <Offset>11</Offset>
        ```

        Next, go to **Start Menu -\> Programs -\> WSO2 -\> Micro
        Integrator.** This will open a terminal and start the relevant
        profile.

**Build and run the integration service**

!!! tip

Before you build the integration service, be sure that you have changed
the endpoint URL of your integration service to the following:

``` java
    <endpoint>
        <address uri="http://localhost:8291/order"/>
    </endpoint>
```
    

Let's run the integration service in the Micro Integrator that is
embedded in WSO2 Integration Studio: Right-click the
IntegrationServiceCompositeApplication, and click **Export Project
Artifacts and Run** .

The two services are now running on two instances of WSO2 Micro
Integrator.

#### Step 4: Test the integration flow

-   If the Micro Integrator is running on Docker or a VM, open a
    terminal and send the following request using **Curl.**

    ``` java
        curl --header "Content-Type: application/json" --request POST   --data '{"store": {"book": [{"author": "Nigel Rees","title": "Sayings of the Century"},{"author": "J. R. R. Tolkien","title": "The Lord of the Rings","isbn": "0-395-19395-8"}]}}' http://localhost:8290/forward
    ```

-   If the Micro Integrator is running on Kubernetes, open a terminal
    and send the following request using curl. Be sure to replace
    MINIKUBE\_IP with the IP of your Minikube installation.

    ``` java
            curl --header "Content-Type: application/json" --request POST   --data '{"store": {"book": [{"author": "Nigel Rees","title": "Sayings of the Century"},{"author": "J. R. R. Tolkien","title": "The Lord of the Rings","isbn": "0-395-19395-8"}]}}' http://MINIKUBE_IP:32100/forward
    ```

The following response will be received:

``` java
    {
        "orderDetails":{"store": {"book": [{"author": "Nigel Rees","title": "Sayings of the Century"},{"author": "J. R. R. Tolkien","title": "The Lord of the Rings","isbn": "0-395-19395-8"}]}},
        "orderID":"1a23456",
        "price":25.65,
        "status":"successful"
    }
```

#### Step 5: Publish to production

You can now publish your integration artifacts into your production
environment.

**What's next?**

-   Use [WSO2 Micro Integrator's CLI
    Tool](_Using_the_Command-Line_Interface_) to check the integration
    artifacts deployed in the Micro Integrator instance.
-   Use **Prometheus** and other tools to [monitor your Micro
    Integrator](https://docs.wso2.com/display/EI650/Monitoring+WSO2+Micro+Integrator)
    instance.
