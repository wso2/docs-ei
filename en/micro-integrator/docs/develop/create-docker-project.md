# Docker Project

Create a Docker project if you want to deploy your integration solutions inside a Docker environment. This project directory allows you to package multiple integration projects (created as [ESB pojects](../../develop/creating-projects)) into a single Docker image and then build and push to the Docker registries.
    
## Create Docker project
Follow the steps given below.   

1.  There are two ways to create the Docker project:

    -   Open **WSO2 Integration Studio** and click **ESB Project → Create New** in the **Getting Started** view as shown below.
        ![Create ESB project](/assets/img/tutorials/119132413/119132414.png)

        In the **New ESB Solution Project** dialog that opens, enter a name for the ESB config project. Select the **Create Docker/Kubernetes Exporter Project** along with **ESB Config project** and click **Next**.
        
        ![Create Container Project](../../assets/img/create_project/docker_k8s_project/create-container-project.png)

    -   If you already have an ESB Config project with the integration artifacts, click **Miscellaneous → Create New Docker/Kubernetes Project** in the **Getting Started** view as shown below.
        ![Create kubernetes/docker project](../../assets/img/create_project/docker_k8s_project/kubernetes-docker-project.png)

        In the **New Docker/Kubernetes Project** dialog that opens, select **New Docker Project** and click **Next**.
        ![Create Kubernetes Project](../../assets/img/create_project/docker_k8s_project/new_docker_project.png)

2.  In the **New ESB Solution Project** dialog that opens, enter a name for the ESB config project. Select the **Create Docker/Kubernetes Exporter Project** along with **ESB Config project** and click **Next**.

    ![Create Docker Project](../../assets/img/create_project/docker_k8s_project/create-docker.png)

3.  Enter information in the **Docker/Kubernetes Project Information** page as follows:

    ![Docker Configuration](../../assets/img/create_project/docker_k8s_project/docker-details.png)

    -  **Container Type**: Type of the container of the project. Select **Docker**.
    -  **Remote Repository**:  Base image for the docker build.
    -  **Remote Tag**: Base image tag for the docker build.
    -  **Target Repository**:  Target Docker image repository.
    -  **Target Tag**: Target Docker image tag.
    
4.  Click **Finish**. The Docker project is created in the project explorer.
5.  Expand the Docker Exporter Project in the project explorer. The following folders and files are stored within.

    ![Docker Project Structure](../../assets/img/create_project/docker_k8s_project/docker-project.png)
    
    -   **CompositeApps**: Directory to store the composite apps which are selected by the user. During the build time these composite apps will copy to the image.
    -   **Conf**: Directory to store configuration files. During the build time these configuration files inside the **Conf** directory will copy to the image.   
    -   **Libs**: Directory to store libraries. During the build time these libraries inside the **Libs** directory will copy to the image.
    -   **Dockerfile**: The Dockerfile, which contains build details.
    -   **pom.xml**: File for select multiple composite apps and build & push Docker images to the Docker registries.   
    
## Build Docker images

Follow the steps given below,

1.  Open the **pom.xml** file inside the Docker project.

    ![Docker pom View](../../assets/img/create_project/docker_k8s_project/docker-pom.png)
    
2.  In the **Dependencies** section, select the multiple composite applications that you want to package inside the Docker image and save the file.
3.  Click **Build** to start the Docker image build.
4.  It will build the Docker image based on the Dockerfile and the Target details. When the image is created, the following message will display. 

    ![Docker Build Success](../../assets/img/create_project/docker_k8s_project/build.png)