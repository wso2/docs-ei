# Quick Start with WSO2 Micro Integrator

Let's get started with WSO2 Micro Integrator by running a simple use
case on your local environment. In this example, we use a REST API to
simulate a simple HTTP service ( **HelloWorld** service) deployed in an
instance of WSO2 Micro Integrator. You can deploy this HelloWorld
service on a **Docker** container, on **Kubernetes**, or on a **VM**. When invoked,
the **HelloWorld** service will return the following response: `{"Hello":"World"}`

![Micro Integrator Deployment Pattern](../assets/img/micro-integrator-01.png)

### Set up the workspace

-   Install [Docker](https://www.docker.com/) (version 17.09.0-ce or
    higher).
-   Install [Curl](https://curl.haxx.se/) .
-   To set up the integration workspace for this quick guide, we will
    use an integration project that was built using [WSO2 Integration
    Studio](../develop/working-with-WSO2-Integration-Studio.md) :
    1.  Download the <a href="../assets/attach/MI_QSG_HOME.zip">project file</a>
        for this guide, and extract it to a known location. Let's call
        this **MI_QSG_HOME**.
    2.  Go to the **MI_GS_HOME** directory.
        The following project files, and Docker/Kubernetes configuration
        files are available.
        <details>
            <summary>hello-world-config-project</summary>
            This is the ESB Config Project folder with the integration artifacts (synapse artifacts) for the HelloWorld service (HelloWorld.xml). This service consists of the following REST API:
    
            ```
            <api context="/hello-world" name="HelloWorld" xmlns="http://ws.apache.org/ns/synapse">
                <resource methods="GET">
                    <inSequence>
                        <payloadFactory media-type="json">
                            <format>{"Hello":"World"}</format>
                            <args/>
                        </payloadFactory>
                         <respond/>
                    </inSequence>
                    <outSequence/>
                    <faultSequence/>
                </resource>
            </api>
            ```
        </details>
        <details>
            <summary>hello-world-config-projectCompositeApplication</summary>
            This is the Composite Application Project folder, which contains the packaged CAR file of the HelloWorld service.
        </details>
        <details>
            <summary>Dockerfile</summary>
            This Docker configuration file is configured to build a Docker image for WSO2 Micro Integrator with the HelloWorld service.
    
            ```
            FROM wso2/micro-integrator:1.0.0
            COPY hello-world-config-projectCompositeApplication/target/hello-world-config-projectCompositeApplication_1.0.0.car /home/wso2carbon/wso2mi/repository/deployment/server/carbonapps

            ```
            
           **Note** that this file is configured to use the community version of the WSO2 Micro Integrator base Docker image (from DockerHub ). If you want to use the Micro Integrator that includes the latest product updates, you can update the image name in this Docker file as explained here .
        </details>
        <details>
            <summary>k8s-deployment.yaml</summary>
            This is sample Kubernetes configuration file that is configured to deploy WSO2 Micro Integrator in a Kubernetes cluster.
    
            ```
            apiVersion: apps/v1
            kind: Deployment
            metadata:
            name: mi-helloworld-deployment
            labels:
                event: mi-helloworld
            spec:
                strategy:
                type: Recreate
                replicas: 2
            selector:
                matchLabels:
                event: mi-helloworld
            template:
                metadata:
                labels:
                event: mi-helloworld
            spec:
            containers:
            -     
                image: wso2-mi-hello-world
                name: helloworld
                imagePullPolicy: IfNotPresent
                ports:
            -
                name: web
                containerPort: 8290 
            ---
            apiVersion: v1
            kind: Service
            metadata:
                name: mi-helloworld-service
                labels:
                    event: mi-helloworld
            spec:
                type: NodePort
            ports:
            -
                name: web
                port: 8290
                targetPort: 8290 
                nodePort: 32100
            selector:
                event: mi-helloworld

            ```
        </details>

### Run on Docker

Once you have [set up your
workspace](#set-up-the-workspace), you
can run the HelloWorld service on Docker:

#### Build a Docker image

Open a terminal, navigate to the **MI_QSG_HOME** directory (which stores the Dockerfile), and execute the following
command to build a Docker image with WSO2 Micro Integrator and the
integration artifacts.

``` java
docker build -t hello_world_docker_image .
```

This command executes the following tasks:

1.  The base Docker image of WSO2 Micro Integrator is downloaded from
    **DockerHub** and a custom, deployable Docker image of the Micro
    Integrator is created in a Docker container.
2.  The CAR file with the integration artifacts that define the
    HelloWorld service is deployed.

#### Run Docker container

From the **MI_QSG_HOME>** directory, execute the following command to start a Docker container for the Micro Integrator.

``` java
docker run -d -p 8290:8290 hello_world_docker_image
```

The Docker container with the Micro Integrator is started.

#### Invoke the Micro Integrator (on Docker)

Open a terminal, and execute the following command to invoke the
HelloWorld service:  

``` java
curl http://localhost:8290/hello-world
```

Upon invocation, you should be able to observe the following response:

``` java
{"Hello":"World"}
```

### Run on Kubernetes

Once you have [set up your workspace](#set-up-the-workspace), you can run the HelloWorld service on Kubernetes:

#### Install Minikube

Let's use [Minikube](https://github.com/kubernetes/minikube) to run a
Kubernetes cluster for this example.

1.  [Install
    Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)
    . Once you have completed this step, you should have **kubectl**
    configured to use Minikube from your terminal.
2.  Start Minikube from your terminal:

    ``` java
    minikube start
    ```

3.  Execute the command given below to start using Minikube's built-in
    Docker daemon. You need this Docker daemon to be able to create
    Docker images for the Minikube environment.

    ``` java
    eval $(minikube docker-env)
    ```

Now you can build a Docker image for your HelloWorld service in Minikube
as explained below.

#### Build a Docker image

Open a terminal, navigate to the **<MI_QSG_HOME>**
directory (which stores the Dockerfile), and execute the following
command to build a Docker image (with WSO2 Micro Integrator and the
integration artifacts) **in the Minikube environment** .

``` java
docker build -t wso2-mi-hello-world .
```

#### Run container (on Minikube)

Follow the steps given below to start a Docker container for Docker
image on Minikube.

1.  Navigate to the **MI_QSG_HOME>** directory
    (which stores the `k8s-deployment.yaml` file),
    and execute the following command:

    ``` java
    kubectl create -f k8s-deployment.yaml
    ```

2.  Check whether all the Kubernetes artifacts are deployed successfully
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

#### Invoke the Micro Integrator (on Minikube)

Open a terminal, and execute the command given below to invoke the
HelloWorld service. Be sure to replace MINIKUBE\_IP with the IP of your
Minikube installation.

``` java
curl http://MINIKUBE_IP:32100/hello-world
```

Upon invocation, you should be able to observe the following response:

``` java
{"Hello":"World"}
```

## Run on a Virtual Machine

Follow the steps given below to run the HelloWorld service on a Micro Integrator instance that is installed on a VM.

### Download and install the Micro Integrator

1. Go to the WSO2 Micro Integrator [product page](https://wso2.com/integration/micro-integrator/) and click **Download** to download the product installer (**.pkg** file).
2. Double-click the installer to open the installation wizard, which will guide you through the installation. The installation location (**MI_HOME**) will depend on your OS:
    <table style="width:100%;">
    <colgroup>
      <col style="width: 9%" />
      <col style="width: 90%" />
    </colgroup>
    <thead>
      <tr class="header">
         <th>OS</th>
         <th>Home directory</th>
      </tr>
    </thead>
    <tbody>
      <tr class="odd">
         <td>Mac OS</td>
         <td><code>/Library/WSO2/MicroIntegrator/1.0.0</code></td>
      </tr>
      <tr class="even">
         <td>Windows</td>
         <td><code>C:\Program Files\WSO2\MicroIntegrator\1.0.0</code></td>
      </tr>
      <tr class="odd">
         <td>Ubuntu</td>
         <td><code>/usr/lib/wso2/MicroIntegrator/1.0.0</code></td>
      </tr>
      <tr class="even">
         <td>CentOS</td>
         <td><code>/usr/lib64/MicroIntegrator/1.0.0</code></td>
      </tr>
    </tbody>
    </table>

### Deploy the HelloWorld service

Copy the CAR file of the HelloWorld service (**hello-world-config-projectCompositeApplication_1.0.0.car**), from the MI_QSG_HOME/hello-world-config-projectCompositeApplication/target/ directory to the MI_HOME/repository/deployment/server/carbonapps directory.

### Start the Micro Integrator

Follow the steps relevant to your OS:

- On **MacOS/Linux/CentOS**, open a terminal and execute the following commands:
  ```
  sudo wso2mi-1.0.0
  ```
- On **Windows**, go to **Start Menu -> Programs -> WSO2 -> Micro Integrator**. This will open a terminal and start the relevant profile.

### Invoke the HelloWorld service

Open a terminal and execute the following curl command to invoke the service:

```
curl http://localhost:8290/hello-world
```

Upon invocation, you should be able to observe the following response:

``` java
{"Hello":"World"}
```
