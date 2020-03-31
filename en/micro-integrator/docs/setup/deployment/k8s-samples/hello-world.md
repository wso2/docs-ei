# K8s Deployment Sample 1: Hello World Scenario
Let's define a basic Hello World scenario using WSO2 Micro Integrator and deploy it on your Kubernetes environment.

## Prerequisites

-   Install and set up [WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
    
    !!! Tip
        Be sure to [get the latest updates](../../../../develop/installing-WSO2-Integration-Studio#get-the-latest-updates) before trying this example.
        
-   Install a [Kubernetes](https://kubernetes.io/docs/setup/) cluster and **v1.11+** client. Alternatively, you can [run Kubernetes locally via Minikube](https://kubernetes.io/docs/setup/learning-environment/minikube/).
-   Install [Docker](https://docs.docker.com/).
-   Install the [EI Kubernetes operator](../../../../setup/deployment/kubernetes_deployment#install-the-ei-k8s-operator).

## Step 1: Create the integration solution

Follow the steps given below.

1.  Create a Maven Multi Module project using WSO2 Integration Studio.

    ![Create Maven Multi Module Project](../../../assets/img/create_project/docker_k8s_project/create-maven-project.png) 
    
2.  Create an **ESB Config Project** inside the Maven Multi Module project:
    
    Right-click the Maven Multi Module project in the project explorer, go to **New → Project**, and select **ESB Config Project** to open the **New ESB Config Project** dialog.

    <img src="../../../../assets/img/create_project/docker_k8s_project/esb-config.png" alt="Create ESB Config Project" width="500">
        
3.  Add the following proxy service configuration inside the ESB Config Project. This will return the "{"Hello":"World"}" response payload for the service request.

    1.  Right-click the ESB Config project in the project explorer, go to **New -> Proxy Service** and create a custom proxy service named `HelloWorld`. 
        <img src="../../../../assets/img/create_project/docker_k8s_project/custom-proxy-service-hello-world.png" alt="Create ESB Config Project" width="500">

    2.  You can then use the **Source View** to copy the following configuration.
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <proxy name="HelloWorld" startOnLoad="true" transports="http https" xmlns="http://ws.apache.org/ns/synapse">
            <target>
                <inSequence>
                    <payloadFactory media-type="json">
                        <format>{"Hello":"World"}</format>
                        <args/>
                    </payloadFactory>
                    <respond/>
                </inSequence>
                <outSequence/>
                <faultSequence/>
            </target>
        </proxy>
        ```

4.  Create a **Composite Application Project** with the above proxy service inside the Maven Multi Module project.

    1.  Right-click the maven multi modiule project, go to **New → Project**, select **Composite Application Project**, and click **Next**.
    2.  Be sure to select the proxy service under **Dependencies** as shown below.
        <img src="../../../../assets/img/create_project/docker_k8s_project/composite-proj-hello-world.png" alt="Create Composite Application Project" width="500">

    3.  Click **Finish**.

5.  Create a **Kubernetes Project** inside the Maven Multi Module Project. 

    1.  Right-click the Maven Multi Module project, go to **New → Project**, select **Kubernetes Exporter Project**, and click **Next**.
        <img src="../../../../assets/img/create_project/docker_k8s_project/k8s-proj.png" alt="Create Kubernetes Project" width="500">
    2.  In the **Kubernetes Project Information** dialog that opens, enter the following details:
        <table>
            <tr>
                <th>
                    Parameter
                </th>
                <th>
                    Description
                </th>
            </tr>
            <tr>
                <td>
                    Kubernetes Project Name
                </td>
                <td>
                   Give a unique name for the project. 
                </td>
            </tr>
            <tr>
                <td>
                    Integration Name
                </td>
                <td>
                    This name will be used to identify the integration solution in the kubernetes custom resource. Let's use <code>hello-world</code> as the integration name for this example.
                </td>
            </tr>
            <tr>
                <td>
                    Number of Replicas
                </td>
                <td>
                    Specify the number of pods that should be created in the kubernetes cluster.
                </td>
            </tr>
            <tr>
                <td>
                    Base Image Repository
                </td>
                <td>
                    Specify the base Micro Integrator Docker image for your solution. For this example, let's use the Micro Integrator docker image from the WSO2 public docker registry: <b>wso2/micro-integrator</b>.</br></br>
                    Note that the image value format should be 'docker_user_name/repository_name'.
                </td>
            </tr>
            <tr>
                <td>
                    Base Image Tag
                </td>
                <td>
                    Give a tag name for the base Docker image.
                </td>
            </tr>
            <tr>
                <td>
                    Target Image Repository
                </td>
                <td>
                    The Docker repository to which the Docker image will be pushed: 'docker_user_name/repository_name'.
                </td>
            </tr>
            <tr>
                <td>
                    Target Image Tag
                </td>
                <td>
                    Give a tag name for the Docker image.
                </td>
            </tr>
        </table>
        
    6.  This step is only required if you already have a Docker image (in your local Docker repository) with the same name as the base image specified above. 
    
        !!! Info
            In this scenario, WSO2 Integration Studio will first check if there is a difference in the two images before pulling the image specified in the **Base Image Repository** field. If the given base image is more updated, the existing image will be overwritten by this new image. Therefore, if you are currently using an older version, or if you have custom changes in your existing image, they will be replaced. 
        
        To avoid your existing custom/older images from being replaced, add the following property under **dockerfile-maven-plugin -> executions -> execution -> configurations** in the `pom.xml` file of your Docker Exporter project. This configuration will ensure that the base image will not be pulled when a Docker image already exists with the same name.
            
        ```xml
        <pullNewerImage>false</pullNewerImage>
        ```

        Example plugin configuration after adding the property:

        ```xml
          <plugin>
            <groupId>com.spotify</groupId>
            <artifactId>dockerfile-maven-plugin</artifactId>
            <version>1.4.3</version>
            <extensions>true</extensions>
            <executions>
              <execution>
                <goals>
                  <goal>build</goal>
                  <goal>push</goal>
                </goals>
                <configuration>
                  <username>${username}</username>
                  <password>${password}</password>
                  <repository>docker/helloworld</repository>
                  <tag>1.1.0</tag>
                  <pullNewerImage>false</pullNewerImage> 
                </configuration>
              </execution>
            </executions>
            <configuration/>
          </plugin>
        ```
         
Finally, the created Maven Multi Module project should look as follows:

<img src="../../../../assets/img/create_project/docker_k8s_project/hello_world_project.png" alt="Hello World Project" width="300">

## Step 2: Package and build the solution  

You need to build a Docker image of the integration solution and push it to your Docker registry.
      
1.  Start the Docker daemon in the host machine.
2.  Open the **pom.xml** file in the Kubernetes project and ensure that the composite application is selected under **Dependencies**.
    ![Select composite projects](../../../assets/img/create_project/docker_k8s_project/select-dependency-hello-world.png)
   
3.  Leave the **Automatically deploy configurations** check box selected. This ensures that deployment configurations are automatically deployed to the base image.
4.  Click **Build and Push**.
    
    In the dialog that opens, enter the credentials of your Docker registry to which the image should be pushed.

    <img src="../../../../assets/img/create_project/docker_k8s_project/docker-registry-credentials.png" alt="docker registry credentials" width="500">

    !!! Info
        Alternatively, you can build the Docker image and push it to the Docker registry as follows:

        1.  Navigate to the Maven Multi Module project and run the following command to build the project. It will create a docker image with the provided target repository and tag once the build is successfull.
            ```bash
            mvn clean install -Dmaven.test.skip=true
            ```
        2.  Navigate to the Kubernetes project inside the MavenParentProject and run the following command to push the docker image to the remote docker registry.
            ```bash
            mvn dockerfile:push -Ddockerfile.username={username} -Ddockerfile.password={password}
            ``` 

3.  Run the `docker image ls` command to verify that the Docker image is created.
    
## Step 3: Deploy the solution in K8s

!!! Info
    **Before you begin**, be sure that the [system requrements](../../../../setup/deployment/kubernetes_deployment#prerequisites-system-requirements) are in place, and that the [EI Kubernetes Operator](../../../../setup/deployment/kubernetes_deployment#install-the-ei-k8s-operator) is installed.

Follow the steps given below.

1.  Open the `integration_cr.yaml` file from the Kubernetes project in WSO2 Integration Studio.
2.  See that the **integration** details of the `hello-world` solution are updated:

    ```yaml
    apiVersion: "integration.wso2.com/v1alpha2"
    kind: "Integration"
    metadata:
      name: "hello-world"
    spec:
      replicas: 1
      image: "<Docker image for the Hello World Scenario>"
    ```

3.  Open a terminal, navigate to the location of your `integration_cr.yaml` file, and execute the following command to deploy the integration solution in the Kubernetes cluster:
    ```bash
    kubectl apply -f integration_cr.yaml
    ``` 

When the integration is successfully deployed, it should create the `hello-world` integration, `hello-world-deployment`, `hello-world-service`, and `ei-operator-ingress` as follows:

!!! Tip
    The `ei-operator-ingress` will not be created if you have [disabled the ingress controller](../../../../setup/deployment/kubernetes_deployment#disable-ingress-controller).

```bash
kubectl get integration

NAME          STATUS    SERVICE-NAME    AGE
hello-world   Running   hello-service   2m

kubectl get deployment

NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
hello-world-deployment   1/1     1            1           2m

kubectl get services
NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)       AGE
hello-world-service      ClusterIP   10.101.107.154   <none>        8290/TCP      2m
kubernetes               ClusterIP   10.96.0.1        <none>        443/TCP       2d
k8s-ei-operator          ClusterIP   10.98.78.238     <none>        443/TCP       1d

kubectl get ingress
NAME                  HOSTS     ADDRESS     PORTS     AGE
ei-operator-ingress   wso2ei    10.0.2.15   80, 443   2m
```

## Step 4: Test the deployment

Let's invoke the service without going through the ingress controller.

1.  Apply port forwarding as shown below. This will allow you to invoke the service without going through the Ingress controller:
    ```bash
    kubectl port-forward service/hello-world-service 8290:8290
    ```

2.  Invoke the service as follows:
    ```bash
    curl http://localhost:8290/services/HelloWorld
    ```  

You will receive the following response:

```bash
{"Hello":"World"}%
```
