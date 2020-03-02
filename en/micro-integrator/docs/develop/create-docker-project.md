# Creating a Docker Project

Create a Docker project if you want to deploy your integration solutions inside a Docker environment. This project directory allows you to package multiple integration projects (created as [ESB projects](../../develop/creating-projects)) into a single Docker image and then build and push to the Docker registries.

!!! Tip
    If you want to simultaneously create all the projects required for your use case, read about the [ESB project solution](../../develop/creating-esb-solution).

## Prerequisites

-   It is recommended to [get the latest updates](../../develop/installing-WSO2-Integration-Studio#get-the-latest-updates) for your [WSO2 Integration Studio](../../develop/installing-WSO2-Integration-Studio) before trying these instructions.
-   You need a Docker registry before you try these instructions. For example, set up [DockerHub](https://docs.docker.com/).

## Create Docker project
Follow the steps given below.   

1.  Open **WSO2 Integration Studio** and click **Miscellaneous → Create New Docker Project** in the **Getting Started** view as shown below.
    
    <img src="../../assets/img/create_project/docker_k8s_project/get_started_docker_project.png" width="1000">

2.  In the **New Docker Project** dialog that opens, enter a name for the Docker project and other parameters as shown below.

    <img src="../../assets/img/create_project/docker_k8s_project/new_docker_project_info.png" width="500">

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
                Docker Project Name
            </td>
            <td>
                Give a name for the Docker project.
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

4.  Optionally, click **Next** and configure Maven details for the Docker project.

    <img src="../../assets/img/create_project/docker_k8s_project/new_docker_project_maven_info.png" width="500">
    
5.  Click **Finish**. The Docker project is created in the project explorer.
6.  Expand the **Docker Exporter Project** in the project explorer. The following folders and files are stored within.

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
                Directory that store libraries. During the build time, the libraries inside the **Libs** directory will copy to the image.
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
                pom.xml
            </td>
            <td>
                File for selecting multiple composite apps. This information is also used to build and push Docker images to the Docker registries.
            </td>
        </tr>
    </table>
    
## Build Docker images

!!! Info
    **Before you begin**, you need to create your integration artifacts in an [ESB Config](../../develop/creating-projects/#esb-config-proje) project and package the artifacts in a [Composite Application project](../../develop/packaging-artifacts). For example, see the HelloWorld sample given below.
     <img alt="Integration artifacts for Docker" src="../../assets/img/create_project/docker_k8s_project/integration-projects-for-docker.png" width="300">

Follow the steps given below,

1.  Open the **pom.xml** file inside the Docker project and click **Refresh** on the top-right. Your composite application project with integration artifacts will be listed under **Dependencies** as follows:

    <img alt="Docker Pom view" src="../../assets/img/create_project/docker_k8s_project/docker-pom.png" width="700">
    
2.  Select the multiple composite applications that you want to package inside the Docker image and save the file.
3.  Click **Build** to start the Docker image build.
4.  It will build the Docker image based on the Dockerfile and the Target details. When the image is created, the following message will display. 

    ![Docker Build Success](../assets/img/create_project/docker_k8s_project/build.png)