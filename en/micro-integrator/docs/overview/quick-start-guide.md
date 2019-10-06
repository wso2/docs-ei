# Quick Start Guide

Let's get started with WSO2 Micro Integrator by running a simple use case in your local environment. This is a simple service orchestration scenario. The scenario is about a basic health care system where Micro Integrator is used to integrate two backend hospital services to provide information to the client.

Most healthcare centers have a system that is used to make doctor appointments. To check the availability of the doctors for a particular time, users need to visit the hospitals or use each and every online system that is dedicated for a particular healthcare center. Here we are making it easier for patients by orchestrating those isolated systems for each healthcare provider and exposing a single interface to the users.

![alt text](../../assets/img/quick-start-guide/MI-quick-start-guide.png)

In the above scenario, the following takes place:

1. The client makes a call to the Healthcare API created using Micro Integrator.

2. The Healthcare API calls the Pine Valley Hospital backend service and gets the queried information.

3. The Healthcare API calls the Grand Oak Hospital backend service and gets the queried information.

4. The response is returned to the client with the required information.

Both Grand Oak Hospital and Pine Valley Hospital have services exposed over HTTP protocol.

Pine Valley Hospital service accepts a GET request in following service endpoint URL.

```bash
http://<HOST_NAME>:<PORT>/pineValley/doctors
```

Grand Oak Hospital service accepts a GET request in following service endpoint URL.

```bash
http://<HOST_NAME>:<PORT>/grandOak/doctors/<DOCTOR_TYPE>
```

The expected payload should be in the following JSON format.

```bash
{
        "doctorType": "<DOCTOR_TYPE>"
}
```

Let’s implement a simple integration solution that can be used to query for availability of doctors for a particular category from all the available healthcare centers.

## Before you begin

1. [Download Micro Integrator](https://www.wso2.com/integration/micro-integrator) for your Operating System.
   <table>
    <tr>
    <th>Operating System</th>
    <th>Location</th>
    </tr>
    
    <tr>
    <td>Mac OSX</td>
    <td>/Library/WSO2/MicroIntegrator/1.0.0</td>
    </tr>
    
    <tr>
    <td>Windows</td>
    <td>C:\Program Files\WSO2\MicroIntegrator\1.0.0</td>
    </tr>
    
    <tr>
    <td>Ubuntu</td>
    <td>/usr/lib/wso2/MicroIntegrator/1.0.0</td>
    </tr>
    
    <tr>
    <td>CentOS</td>
    <td>/usr/lib64/MicroIntegrator/1.0.0</td>
    </tr>
   </table> 

2. Download the sample files from [here](https://github.com/wso2/docs-ei/tree/7.0.0/en/micro-integrator/docs/assets/attach/quick-start-guide). From this point onwards, let's refer to this folder as `<MI_QSG_HOME>`.

3. Download [curl](https://curl.haxx.se/) or a similar tool that can call an endpoint.

## Start backend mock services

Two mock hospital information services are available in the `DoctorInfo.jar` file located at `<MI_QSG_HOME>/BackendService/` directory. 

Open a command line window, navigate to `<BI_QSG_HOME>/BackendService/`, and use the following command to start the services.

```java
java -jar DoctorInfo.jar
```

You will see following printed in the command line.

```bash
[ballerina/http] started HTTP/WS listener 0.0.0.0:9090
[ballerina/http] started HTTP/WS listener 0.0.0.0:9091
```

## Create a Project, Add a Template, and Invoke the Service

Create a new project by navigating to a directory of your choice and running the following command. 

```bash
$ ballerina new healthcare-service
```

You see a response confirming that your project is created.

Let's use a predefined module from Ballerina Central, which is a public directory that allows you to host templates and modules. A module is a directory that contains Ballerina source code files, while a template is a predefined code that solves a particular integration scenario. In this case, we use the `healthcare_service` module. Navigate into the project directory you created and run the following command.

```
$ ballerina pull wso2/healthcare_service
```

Now navigate into the above module directory you created. The following command enables you to apply a predefined template you pulled.

```bash
$ ballerina add -t wso2/healthcare_service doctors
```

This automatically creates a healthcare service for you inside an `src` directory. A Ballerina service represents a collection of network accessible entry points in Ballerina. A resource within a service represents one such entry point. The generated sample service exposes a network entry point on port 9090.

Build the service using the `ballerina build` command.

```bash
$ ballerina build doctors
```

You get the following output.

```bash
Compiling source
	wso2/doctors:0.1.0

Creating balos
	target/balo/doctors-2019r3-any-0.1.0.balo

Running tests
    wso2/doctors:0.1.0
	No tests found


Generating executables
	target/bin/doctors.jar
```

Run the following Java command to run the executable .jar file that is created once you build your module.

```bash
$ java -jar target/bin/doctors.jar
```

Your service is now up and running. You can invoke the service using an HTTP client. In this case, we use cURL.

> **Tip**: If you do not have cURL installed, you can download it from [https://curl.haxx.se/download.html](https://curl.haxx.se/download.html).

```bash
$ curl http://localhost:9090/healthcare/doctor/physician
```

You get the following response.

```json
[
   {
      "name":"Shane Martin",
      "time":"07:30 AM",
      "hospital":"Grand Oak"
   },
   {
      "name":"Geln Ivan",
      "time":"08:30 AM",
      "hospital":"Grand Oak"
   },
   {
      "name":"Geln Ivan",
      "time":"05:30 PM",
      "hospital":"pineValley"
   },
   {
      "name":"Daniel Lewis",
      "time":"05:30 PM",
      "hospital":"pineValley"
   }
]
```

You just started Ballerina Integrator, created a project, started a service, invoked the service you created, and received a response.

To have a look at the code, navigate to the `hospital_service.bal` file found inside your module.

```
import ballerina/http;
import ballerina/log;

http:Client grandOakHospital = new("http://localhost:9091/grandOak");
http:Client pineValleyHospital = new("http://localhost:9092/pineValley");

@http:ServiceConfig {
    basePath: "/healthcare"
}
service healthcare on new http:Listener(9090) {

    @http:ResourceConfig {
        path: "/doctor/{doctorType}"
    }
    resource function getDoctors(http:Caller caller, http:Request request, string doctorType) returns error? {
        json grandOakDoctors = {};
        json pineValleyDoctors = {};
        var grandOakResponse = grandOakHospital->get("/doctors/" + doctorType);
        var pineValleyResponse = pineValleyHospital->post("/doctors", {doctorType: doctorType});
        // Extract doctors array from grand oak hospital response
        if (grandOakResponse is http:Response) {
            json result = check grandOakResponse.getJsonPayload();
            grandOakDoctors = check result.doctors.doctor;
        } else {
            handleError(caller, <@untained> grandOakResponse.reason());
        }
        // Extract doctors array from pine valley hospital response
        if (pineValleyResponse is http:Response) {
            json result = check pineValleyResponse.getJsonPayload();
            pineValleyDoctors = check result.doctors.doctor;
        } else {
            handleError(caller, <@untained> pineValleyResponse.reason());
        }
        // Aggregate grand oak hospital's doctors with pine valley hospital's doctors
        if (grandOakDoctors is json[] && pineValleyDoctors is json[]) {
            foreach var item in pineValleyDoctors {
                grandOakDoctors.push(item);
            }
        }
        // Respond back to the caller with aggregated json response
        http:Response response = new();
        response.setJsonPayload(<@untained> grandOakDoctors);
        var result = caller->respond(response);

        if (result is error) {
            log:printError("Error sending response", err = result);
        }
    }
}

function handleError(http:Caller caller, string errorMsg) {
    http:Response response = new;

    json responsePayload = {
        "error": {
            "message": errorMsg
        }
    };
    response.setJsonPayload(responsePayload, "application/json");
    var result = caller->respond(response);
    if (result is error) {
        log:printError("Error sending response", err = result);
    }
}
```

## What's Next

- Try out the tutorials available in the [Learn section of our documentation](../../learn/use-cases/).
- You can easily deploy the projects you create by following our documentation on [Docker](../../learn/deploy-on-docker/) and [Kubernetes](../../learn/deploy-on-kubernetes/).
















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
    1.  Download the <a href="../../assets/attach/MI_QSG_HOME.zip">project file</a>
        for this guide, and extract it to a known location. Let's call
        this **MI_QSG_HOME**.
    2.  Go to the **MI_GS_HOME** directory.
        The following project files, and Docker/Kubernetes configuration
        files are available.
        <details>
            <summary>hello-world-config-project</summary>
            This is the ESB Config Project folder with the integration artifacts (synapse artifacts) for the HelloWorld service (HelloWorld.xml). This service consists of the following REST API:
            ```xml
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
            ```toml
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

Once you have [set up your workspace](#set-up-the-workspace), you can run the HelloWorld service on Docker:

#### Build a Docker image

Open a terminal, navigate to the **MI_QSG_HOME** directory (which stores the Dockerfile), and execute the following
command to build a Docker image with WSO2 Micro Integrator and the
integration artifacts.

```bash
docker build -t hello_world_docker_image .
```

This command executes the following tasks:

1.  The base Docker image of WSO2 Micro Integrator is downloaded from
    **DockerHub** and a custom, deployable Docker image of the Micro
    Integrator is created in a Docker container.
2.  The CAR file with the integration artifacts that define the
    HelloWorld service is deployed.

#### Run Docker container

From the **<MI_QSG_HOME>** directory, execute the following command to start a Docker container for the Micro Integrator.

```bash
docker run -d -p 8290:8290 hello_world_docker_image
```

The Docker container with the Micro Integrator is started.

#### Invoke the Micro Integrator (on Docker)

Open a terminal, and execute the following command to invoke the
HelloWorld service:  

```bash
curl http://localhost:8290/hello-world
```

Upon invocation, you should be able to observe the following response:

`{"Hello":"World"}`

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

    ```bash
    minikube start
    ```

3.  Execute the command given below to start using Minikube's built-in
    Docker daemon. You need this Docker daemon to be able to create
    Docker images for the Minikube environment.

    ```bash
    eval $(minikube docker-env)
    ```

Now you can build a Docker image for your HelloWorld service in Minikube
as explained below.

#### Build a Docker image

Open a terminal, navigate to the **<MI_QSG_HOME>**
directory (which stores the Dockerfile), and execute the following
command to build a Docker image (with WSO2 Micro Integrator and the
integration artifacts) **in the Minikube environment** .

```bash
docker build -t wso2-mi-hello-world .
```

#### Run container (on Minikube)

Follow the steps given below to start a Docker container for Docker
image on Minikube.

1.  Navigate to the **<MI_QSG_HOME>** directory
    (which stores the `k8s-deployment.yaml` file),
    and execute the following command:

    ```bash
    kubectl create -f k8s-deployment.yaml
    ```

2.  Check whether all the Kubernetes artifacts are deployed successfully
    by executing the following command:  

    ```bash
    kubectl get all
    ```

    You will get a result similar to the following. Be sure that the
    deployment is in 'Running' state.

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

#### Invoke the Micro Integrator (on Minikube)

Open a terminal, and execute the command given below to invoke the
HelloWorld service. Be sure to replace MINIKUBE\_IP with the IP of your
Minikube installation.

```bash
curl http://MINIKUBE_IP:32100/hello-world
```

Upon invocation, you should be able to observe the following response:

`{"Hello":"World"}`

### Run on a Virtual Machine

Follow the steps given below to run the HelloWorld service on a Micro Integrator instance that is installed on a VM.

##### Download and install the Micro Integrator

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

#### Deploy the HelloWorld service

Copy the CAR file of the HelloWorld service (**hello-world-config-projectCompositeApplication_1.0.0.car**), from the MI_QSG_HOME/hello-world-config-projectCompositeApplication/target/ directory to the MI_HOME/repository/deployment/server/carbonapps directory.

#### Start the Micro Integrator

Follow the steps relevant to your OS:

- On **MacOS/Linux/CentOS**, open a terminal and execute the following commands:
  ```bash
  sudo wso2mi-1.0.0
  ```
- On **Windows**, go to **Start Menu -> Programs -> WSO2 -> Micro Integrator**. This will open a terminal and start the relevant profile.

#### Invoke the HelloWorld service

Open a terminal and execute the following curl command to invoke the service:

```bash
curl http://localhost:8290/hello-world
```

Upon invocation, you should be able to observe the following response:

`{"Hello":"World"}`
