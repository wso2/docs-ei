# Creating a Project Solution

An integration solution consists of one or several project directories. These directories store the various artifacts that you create for your integration sequence. 

WSO2 Integration Studio allows you to simultaneously create all the project directories that are required for your solution by using a single wizard.

Follow the steps given below.

## Prerequisites
    
It is recommended to [get the latest updates](../../develop/installing-WSO2-Integration-Studio#get-the-latest-updates) for your [WSO2 Integration Studio](../../develop/installing-WSO2-Integration-Studio) before trying these instructions.

## Step 1: Select the required projects

1.  Open **WSO2 Integration Studio** and click **Integration → Create New Integration Project** in the **Getting Started** view as shown below.
    <img src="../../assets/img/create_project/create-integration-solution.png">

2.  In the **New Integration Project** dialog that opens, provide the following information:

    <img src="../../assets/img/create_project/new_soln_vm_deployment.png" width="500">

    <table>
        <tr>
            <th>
                Project Type
            </th>
            <th>
                Description
            </th>
        </tr>
        <tr>
            <td>
                Integration Project Name
            </td>
            <td>
                <b>Required</b>. Enter a name for the main project (**Config** project) in this field. The Config project stores the mediation sequence and related mediation artifacts.
            </td>
        </tr>
        <tr>
            <td>
                Create Composite Application Project
            </td>
            <td>
                You will need this project when you <a href="../../develop/packaging-artifacts">package your solution</a>.
            </td>
        </tr>
        <tr>
            <td>
                Create Registry Resources Project
            </td>
            <td>
                Select this option if you will use <a href="../../develop/creating-artifacts/creating-registry-resources">registry resources</a> in your integration solution.
            </td>
        </tr>
        <tr>
            <td>
                Create Connector Exporter project
            </td>
            <td>
                Select this option if you will <a href="../../develop/creating-artifacts/adding-connectors">add Connectors</a> in your integration solution.
            </td>
        </tr>
        <tr>
            <td>
                Create Docker Exporter project
            </td>
            <td>
                You will need this project if you are deploying the solution on Docker.
            </td>
        </tr>
        <tr>
            <td>
                Create Kubernetes Exporter project
            </td>
            <td>
                You will need this project if you are deploying the solution on Kubernetes.
            </td>
        </tr>
    </table>

3.  Click **Next** to go to the next step or click **Finish** to end the process.

## Step 2: Enter Docker project information (Optional)

!!! Tip
    The following step is only available if you have selected **Create Docker Exporter Project** in **step 1**.

In the **Docker Project Information** page, enter the details required for the Docker project.

<img src="../../assets/img/create_project/new_soln_docker_info.png" width="500">

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

Click **Next** to go to the next step or click **Finish** to end the process.

## Step 3: Enter Kubernetes project information (Optional)

!!! Tip
    The following step is only available if you have selected **Create Kubernetes Exporter Project** in **step 1**.

In the **Kubernetes Project Information** page, enter the details required for the Kuberntes project.

<img src="../../assets/img/create_project/new_soln_k8s_info.png" width="500">

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
            <b>Required</b>. This name will be used to identify the integration solution in the kubernetes custom resource. The custom resource file (<b>integration_cr.yaml</b>) for this solution will be generated along with the other artifacts.
        </td>
    </tr>
    <tr>
        <td>
            Number of Replicas
        </td>
        <td>
            <b>Required</b>. Specify the number of pods that should be created in the kubernetes cluster.
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
                    <b>docker.wso2.com/wso2mi</b>: This is the Micro Integrator Docker image that includes <b>product updates</b>. This image is stored in the <a href="https://docker.wso2.com/tags.php?repo=wso2mi">private WSO2 Docker registry</a>.
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
            If required, you can update this information later when you build and push the Docker image to the relevant repository.
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
            Automatically deploy configurations
        </td>
        <td>
            This check box indicates that you are using WSO2 EI 7 Micro Integrator as the base image. It is recommended to leave this check box selected when you use WSO2 EI 7.
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

Click **Next** to go to the next step or click **Finish** to end the process.

## Step 4: Update Maven information (Optional)

In the **Additional Information** page, update the Maven information:
<img src="../../assets/img/create_project/new_soln_k8s_maven.png" width="500">

## Step 5: Finish the process

Once you have added all the mandatory information, click **Finish** to save the projects. 

<img src="../../assets/img/create_project/click-finish.png" width="500">

The projects are created and saved in the project explorer. See the example given below.

<img src="../../assets/img/create_project/project-solution-explorer.png" width="400">

Go to [Docker project directory](../../develop/create-docker-project#the-docker-project-directory) or [Kubernetes project directory](../../develop/create-kubernetes-project#the-kubernetes-project-directory) for more details about the directory structure.

## What's Next?

You can now start creating the [integration artifacts](../develop/creating-artifacts/creating-an-api.md).