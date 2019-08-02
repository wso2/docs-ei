# Generating Docker images

To generate Docker images, follow the steps below:

> **Before you begin:**

> 1.  Install Docker from the [Docker Site](https://docs.docker.com/) .
> 2.  Create a Docker Account at [Docker Hub](https://hub.docker.com) and log in.
> 3.  Start the Docker server.


1.  Open the WSO2 Integration Studio interface.
2.  Open an existing project. Right-click on **Composite Project** and
    then click **Generate Docker Image**.  
    ![Create docker](../../assets/img/create_project/open-docker_image_generation_wizard.png) 

    The **WSO2 Platform Distribution - Generate Docker Image** wizard
    opens.
    
3.  Enter information in the wizard as follows:

    1.  In the **Generate Docker Image** page, enter the following
        details:  
        ![Create docker image dialog](../../assets/img/create_project/generate_docker_image_dialog.png) 

        -   **Name of the application** : The name of the composite
            application with the artifacts created for your EI project.
            The name of the EI projecty is displayed by default, but it
            can be changed if required.
        -   **Application version** : The version of the composite
            application.
        -   **Name of the Docker Image** : A name for the Docker image.
        -   **Docker Image Tag** : A tag for the Docker image to be used
            for reference.
        -   **Export Destination** : Browse for the preferred location
            in your machine to export the Docker image.

    2.  Once you have entered the required details, click **Next** .
    3.  In the next page, select the EI projects that you want to
        include in the Docker image and click **Finish** .  
        ![Create docker image](../../assets/img/create_project/select_artifact_docker.png)  
        Once the Docker image is successfully created, a message similar
        to the following appears in your screen.  
        ![Create docker image](../../assets/img/create_project/docker_image_successful.png)