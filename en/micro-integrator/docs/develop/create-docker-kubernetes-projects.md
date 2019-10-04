# Docker/Kubernetes Exporter Project

Create this project directory if you want to deploy your integration solutions inside the Docker or Kubernetes environments.
This project directory allows you to package multiple composite to a single Docker image and build and push to the Docker registries.
Also, for the ease of Kubernetes deployment, Kubernetes based exporter projects will generate file called **kubernetes_cr.yaml** which can use to do a Kubernetes deployments based on our **k8s-ei-operator**.

## Kubernetes Project

### Create Kubernetes Project
Follow the steps given below,

1.  Open **WSO2 Integration Studio** and click **ESB Project → Create New** in the **Getting Started** view as shown below.

2.  In the **New ESB Solution Project** dialog that opens, enter a name for the ESB config project. Select the **Create Docker/Kubernetes Exporter Project** along with **ESB Config project** and click **Next**.

    ![Create Kubernetes Project](../../assets/img/create_project/docker_k8s_project/create-container-project.png)

3.  Enter information in the **Docker/Kubernetes Project Information** page as follows:

    ![Kubernetes Configuration](../../assets/img/create_project/docker_k8s_project/k8s-details.png)

    1.  **Container Type**: Type of the container of the project. Here select Kubernetes.
    2.  **Container Name**: Name of the Kubernetes cluster.
    3.  **Number of Replicas**: Number of desired Kubernetes pods.
    4.  **Remote Repository**:  Base image for the docker build.
    5.  **Remote Tag**: Base image tag for the docker build.
    6.  **Target Repository**:  Target Docker image repository as in `<url>/<username>/<repository>` or `<username>/<repository>`.
    7.  **Target Tag**: Target Docker image tag.
    8.  **Expose Port**:    Service expose port in Kubernetes cluster.
    9.  **Environment Variables**:   Multiple environment variable as in key-value pairs.
    
4.  Once you filled the details, click **Finish**. Integration Studio will create a Kubernetes project for you.

5.  Expand the Kubernetes Exporter Project in the project explorer. You can find the following folders and files inside it.

    ![Kubernetes Project Structure](../../assets/img/create_project/docker_k8s_project/k8s-project.png)
    
    1.  **CompositeApps**:  Directory to store the composite apps which are selected by the user. During the build time these composite apps will copy to the image.
    2.  **Conf**:   Directory to store configuration files. During the build time these configuration files inside the Conf will copy to the image.   
    3.  **Libs**:   Directory to store libraries. During the build time these libraries inside the Libs will copy to the image.
    4.  **Dockerfile**: Dockerfile which contains build details.
    5.  **kubernetes_cr.yaml**: Kubernetes configuration file generated based on the user inputs.
    6.  **pom.xml**:    File for select multiple composite apps and build & push Docker images to the Docker registries.
       
### Build & Push Docker Images in Kubernetes Project

Follow the steps given below,

1.  Open the **pom.xml** file inside the Kubernetes project.

    ![Kubernetes pom View](../../assets/img/create_project/docker_k8s_project/k8s-pom.png)
    
2.  Select multiple composite applications which wants to pack inside the Docker image under the Dependencies section, and save the file.
3.  Click **Build & Push**, to start the Docker image build and push process.

    ![Docker Registry Auth Details](../../assets/img/create_project/docker_k8s_project/k8s-auth.png)

    The **Enter Docker Registry Credentials** wizard opens.
    
4.  Enter information in the wizard as follows:
    
    1.  **Registry URL Type**:  Docker image registry wants to push. Docker Hub or Other.
    2.  **Enter Registry URL**: If above step it Other, enter the registry url of the private registry.
    3.  **Email**:  Email of the target registry repository.
    4.  **Username**:   Username of the target registry repository.
    5.  **Password**:   Password of the target registry repository.
    
5.  Once you enter the above details, click **Push Image**.

6.  First it will build the Docker image based on the Dockerfile and the Target details. Once it done it will show a popup message regarding the status of it.

    ![Docker Build Success](../../assets/img/create_project/docker_k8s_project/build.png)

7.  Finally, it will start to push the built image to the given registry. Once it done it will show a popup message regarding the status of it.

    ![Docker Push Success](../../assets/img/create_project/docker_k8s_project/push.png)

## Docker Project
    
### Create Kubernetes Project
Follow the steps given below,   

1.  Open **WSO2 Integration Studio** and click **ESB Project → Create New** in the **Getting Started** view as shown below.
    
    ![Create Docker Project](../../assets/img/create_project/docker_k8s_project/create-docker.png)
    
2.  In the **New ESB Solution Project** dialog that opens, enter a name for the ESB config project. Select the **Create Docker/Kubernetes Exporter Project** along with **ESB Config project** and click **Next**.

3.  Enter information in the **Docker/Kubernetes Project Information** page as follows:

    ![Docker Configuration](../../assets/img/create_project/docker_k8s_project/docker-details.png)

    1.  **Container Type**: Type of the container of the project. Here select Docker.
    2.  **Remote Repository**:  Base image for the docker build.
    3.  **Remote Tag**: Base image tag for the docker build.
    4.  **Target Repository**:  Target Docker image repository.
    5.  **Target Tag**: Target Docker image tag.
    
4.  Once you filled the details, click **Finish**. Integration Studio will create a Docker project for you.

5.  Expand the Docker Exporter Project in the project explorer. You can find the following folders and files inside it.

    ![Docker Project Structure](../../assets/img/create_project/docker_k8s_project/docker-project.png)
    
    1.  **CompositeApps**:  Directory to store the composite apps which are selected by the user. During the build time these composite apps will copy to the image.
    2.  **Conf**:   Directory to store configuration files. During the build time these configuration files inside the Conf will copy to the image.   
    3.  **Libs**:   Directory to store libraries. During the build time these libraries inside the Libs will copy to the image.
    4.  **Dockerfile**: Dockerfile which contains build details.
    6.  **pom.xml**:    File for select multiple composite apps and build & push Docker images to the Docker registries.   
    
### Build Docker Images in Docker Project

Follow the steps given below,

1.  Open the **pom.xml** file inside the Docker project.

    ![Docker pom View](../../assets/img/create_project/docker_k8s_project/docker-pom.png)
    
2.  Select multiple composite applications which wants to pack inside the Docker image under the Dependencies section, and save the file.
3.  Click **Build**, to start the Docker image build.
4.  It will build the Docker image based on the Dockerfile and the Target details. Once it done it will show a popup message regarding the status of it. 

    ![Docker Build Success](../../assets/img/create_project/docker_k8s_project/build.png)