# Creating a Docker Project

Create a Docker project if you want to deploy your integration solutions inside a Docker environment. This project directory allows you to package multiple [integration projects](../../develop/creating-projects) into a single Docker image and then build and push to the Docker registries.

!!! Tip
    If you want to simultaneously create all the projects required for your use case, read about the [integration project solution](../../develop/creating-project-solution).

## Prerequisites

It is recommended to [get the latest updates](../../develop/installing-WSO2-Integration-Studio#get-the-latest-updates) for your [WSO2 Integration Studio](../../develop/installing-WSO2-Integration-Studio) before trying these instructions.

## Creating the Docker project
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
                <b>Required</b>. Give a name for the Docker project.
            </td>
        </tr>
        <tr>
            <td>
                Base Image Repository
            </td>
            <td>
                <b>Required</b>. Select the base Micro Integrator Docker image for your solution. Use one of the following options:
                <ul>
                    <li>
                        <b>wso2/micro-integrator</b>: This is the community version of the Micro Integrator Docker image, which is stored in the <a href="https://hub.docker.com/r/wso2/micro-integrator">public WSO2 Docker registry</a>. This is selected by default.
                    </li>
                    <li>
                        <b>docker.wso2.com</b>: This is the Micro Integrator Docker image that includes <b>product updates</b>. This image is stored in the <a href="https://docker.wso2.com/tags.php?repo=wso2mi">private WSO2 Docker registry</a>.
                        Note that you need a valid <a href="https://wso2.com/subscription/free-trial">WSO2 subscription</a> to use the Docker image with updates.
                    </li>
                    <li>
                        You can also use a custom Docker image from a custom repository.
                    </li>
                </ul>
            </td>
        </tr>
            <td>
                Base Image Tag
            </td>
            <td>
                <b>Required</b>. Specify the tag of the base image that you are using.
            </td>
        <tr>
        </tr>
        <tr>
            <td>
                Target Image Repository
            </td>
            <td>
                <b>Required</b>. The Docker repository to which you will later push this Docker image. 
                <ul>
                    <li>
                        If your repository is in <b>Docker Hub</b>, use the <b>docker_user_name/repository_name</b> format.
                    </li>
                    <li>
                        If you are using any other repository, use the <b>repository_url/repository_user_name/repository_name</b> forrmat.
                    </li>
                </ul> 
                If required, you can update this information later when you build the Docker image or when you push the image to the relevant repository.
            </td>
        </tr>
        <tr>
            <td>
                Target Image Tag
            </td>
            <td>
                <b>Required</b>. Give a tag name for the Docker image.
            </td>
        </tr>
        <tr>
            <td>
                Environment Variables
            </td>
            <td>
                You can enter multiple environment variables as key-value pairs.
            </td>
        </tr>
    </table>

3.  Optionally, click **Next** and configure Maven details for the Docker project.

    <img src="../../assets/img/create_project/docker_k8s_project/new_docker_project_maven_info.png" width="500">

4.  Click **Finish**. The Docker project is created in the project explorer.

## The Docker project directory

Expand the **Docker Exporter Project** in the project explorer. See that the following folders and files are created:

<img src="../../assets/img/create_project/docker_k8s_project/proj_explorer_docker_project.png" width="400">

<table>
    <tr>
        <th>
            Directory
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
            This folder stores libraries that should be copied to the Docker image. During the build time, the libraries inside this directory will be copied to the image.
        </td>
    </tr>
    <tr>
        <td>
            Resources
        </td>
        <td>
            This folder stores additional files and resources that should be copied to the Docker image. During the build time, the resources inside this directory will be copied to the image.
        </td>
    </tr>
    <tr>
        <td>
            deployment.toml
        </td>
        <td>
            The <a href="../../references/config-catalog">product configuration file</a>.
        </td>
    </tr>
    <tr>
        <td>
            Dockerfile
        </td>
        <td>
            The Dockerfile containing the build details.
        </td>
    </tr>
    <tr>
        <td>
            pom.xml
        </td>
        <td>
            The file for selecting the relevant composite applications that should be included in the Docker image. This information is also used when you later build and push Docker images to the Docker registries.
        </td>
    </tr>
</table>
    
## Build Docker images

!!! Info
    **Before you begin**, you need to create your integration artifacts in a [Config](../../develop/creating-projects/#esb-config-proje) project and package the artifacts in a [Composite Application project](../../develop/packaging-artifacts). For example, see the HelloWorld sample given below.
     <img alt="Integration artifacts for Docker" src="../../assets/img/create_project/docker_k8s_project/integration-projects-for-docker.png" width="300">

Follow the steps given below.

1.  Open the **pom.xml** file inside the Docker project and click **Refresh** on the top-right. Your composite application project with integration artifacts will be listed under **Dependencies** as follows:

    <img alt="Docker Pom view" src="../../assets/img/create_project/docker_k8s_project/docker-pom.png" width="700">
    
2.  Select the composite applications that you want to package inside the Docker image.
3.  If required, you can update the **Target Repository** to which the image should be pushed and the **Target Tag**.
4.  Save the POM file and click **Build** to start the Docker image build.
5.  It will build the Docker image based on the Dockerfile and the Target details. When the image is created, the following message will display. 

    ![Docker Build Success](../assets/img/create_project/docker_k8s_project/build.png)
