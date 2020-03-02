# Creating a Kubernetes Project

Create a Kubernetes project directory if you want to deploy your integration solutions in a Kubernetes environment. 

The Kubernetes project allows you to package multiple integration projects (created as [ESB projects](../../develop/creating-projects)) into a single Docker image. Also, a file called **integration_cr.yaml** is generated, which can be used to carry out Kubernetes deployments based on our [k8s-ei-operator](../../setup/deployment/kubernetes_deployment/#ei-kubernetes-k8s-operato).

!!! Tip
    If you want to simultaneously create all the projects required for your use case, read about the [ESB project solution](../../develop/creating-esb-solution).

## Prerequisites
    
-   It is recommended to [get the latest updates](../../develop/installing-WSO2-Integration-Studio#get-the-latest-updates) for your [WSO2 Integration Studio](../../develop/installing-WSO2-Integration-Studio) before trying these instructions.
-   You need a Docker registry before you try these instructions. For example, set up [DockerHub](https://docs.docker.com/).

## Create Kubernetes project

Follow the steps given below.   

1.  Open **WSO2 Integration Studio** and click **Miscellaneous → Create New Kubernetes Project** in the **Getting Started** view as shown below.
    
    <img src="../../assets/img/create_project/docker_k8s_project/get_started_k8s_project.png" width="1000">

2.  In the **New Kubernetes Project** dialog that opens, enter a name for the Kubernetes project and other parameters as shown below.

    <img src="../../assets/img/create_project/docker_k8s_project/new_k8s_project_info.png" width="500">

    Enter the following information:

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
                Integration Name
            </td>
            <td>
                This name will be used to identify the integration solution in the kubernetes custom resource.
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
        <tr>
            <td>
                Environment Variables
            </td>
            <td>
                Multiple environment variable as key-value pairs.
            </td>
        </tr>
    </table>

4.  Optionally, click **Next** and configure Maven details for the Kubernetes project.

    <img src="../../assets/img/create_project/docker_k8s_project/new_k8s_project_maven_info.png" width="500">
    
5.  Click **Finish**. The Kubernetes project is created in the project explorer.
6.  Expand the **Kubernetes Exporter Project** in the project explorer. The following folders and files are stored within.

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
                Libs
            </td>
            <td>
                Directory to store libraries. During the build time, these libraries inside the **Libs** directory will copy to the image.
            </td>
        </tr>
        <tr>
            <td>
                deployment.toml
            </td>
            <td>
                The product configuration file.
            </td>
        </tr>
        <tr>
            <td>
                Dockerfile
            </td>
            <td>
                The Dockerfile, which contains build details.
            </td>
        </tr>
        <tr>
            <td>
                integration_cr.yaml
            </td>
            <td>
                Kubernetes configuration file generated based on the user inputs.
            </td>
        </tr>
        <tr>
            <td>
                pom.xml
            </td>
            <td>
                File for selecting multiple composite apps. This information is also used to build and push Docker images to the Docker registries.
            </td>
        </tr>
    </table>
       
## Build and Push Docker images

!!! Info
    **Before you begin**, you need to create your integration artifacts in an [ESB Config](../../develop/creating-projects/#esb-config-proje) project and package the artifacts in a [Composite Application project](../../develop/packaging-artifacts). For example, see the HelloWorld sample given below.
     <img alt="Integration artifacts for Docker" src="../../assets/img/create_project/docker_k8s_project/integration-projects-for-k8s.png" width="300">

Follow the steps given below.

1.  Open the **pom.xml** file inside the Kubernetes project and click **Refresh** on the top-right. Your composite application project with integration artifacts will be listed under **Dependencies** as follows:

    <img alt="Kubernetes pom view" src="../../assets/img/create_project/docker_k8s_project/k8s-pom.png" width="600">
    
2.  Select the composite applications that should be packed inside the Docker image under **Dependencies** and save the file.
3.  Click **Build & Push** on the top-right to start the Docker image build and push process. The **Enter Docker Registry Credentials** wizard opens.

    <img alt="Docker Registry Auth Details" src="../../assets/img/create_project/docker_k8s_project/k8s-auth.png" width="500">
    
4.  Enter the following details in the wizard:

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
                Registry URL Type
            </td>
            <td>
                The Docker image registry to which the image will be pushed: <b>Docker Hub</b> or <b>Other</b>.
            </td>
        </tr>
        <tr>
            <td>
                Username
            </td>
            <td>
                Username of the target registry repository.
            </td>
        </tr>
        <tr>
            <td>
                Password
            </td>
            <td>
                Password of the target registry repository.
            </td>
        </tr>
    </table>
    
5.  Once you enter the above details, click **Push Image**.
6.  First, it will build the Docker image based on the Dockerfile and the Target details. When the image is created, you will see the following message.

    ![Docker Build Success](../assets/img/create_project/docker_k8s_project/build.png)

7.  Finally, it will start to push the image to the given registry. Once the process is completed, you will see the following message.

    ![Docker Push Success](../assets/img/create_project/docker_k8s_project/push.png)
