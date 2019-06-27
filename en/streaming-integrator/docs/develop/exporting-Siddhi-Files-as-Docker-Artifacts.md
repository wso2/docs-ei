# Exporting Siddhi Files as Docker Artifacts

The Stream Processor Studio allows you to export one or more selected
Siddhi files as Docker artifacts in order to make it possible to run
those Siddhi applications in a Docker environment.

To export  Siddhi files as Docker artifacts, follow the steps below:

!!! tip

Before you begin:

The exported Docker artifacts use docker-compose for this purpose.
Therefore to run the artifacts, you need to install the following in the
running environment.

-   [Docker](https://www.docker.com/)

-   [Docker Compose](https://docs.docker.com/compose/)


## Configuring Docker in SP

By default, WSO2 SP uses the latest docker image hosted in
[wso2.docker.com](http://wso2.docker.com/) . If you want to use the
different version of an image, follow the steps below:

1.  Update the `           Configurations for Docker export          `
    section of the
    `           <SP_HOME>/conf/editor/deployment.yaml          ` file as
    follows.

    ``` xml
        # Configurations for Docker export feature
        docker.configs:
          productVersion: <PREFERED_PRODUCT_VERSION>
    ```

      
    You need to replace the
    `           <PREFERED_PRODUCT_VERSION>          ` with your
    preferred image version e.g., 4.3.0.

2.  Restart the WSO2 SP server.

!!! info

This feature also uses the following configuration files with the
content as shown below:

-   `           docker-compose.editor.yaml          `

    ``` xml
        version: '2.3'
        services:
          editor:
            image: docker.wso2.com/wso2sp-editor:{{PRODUCT_VERSION}}
            container_name: wso2sp-editor
            ports:
              - "9390:9390"
            healthcheck:
              test: ["CMD", "nc", "-z","localhost", "9390"]
              interval: 10s
              timeout: 120s
              retries: 5
            volumes:
              - ./siddhi-files:/home/wso2carbon/wso2-artifact-volume/wso2/editor/deployment/workspace
    ```
    
    -   `           docker-compose.worker.yml          `
    
    ``` xml
            version: '2.3'
            services:
              editor:
                image: docker.wso2.com/wso2sp-worker:{{PRODUCT_VERSION}}
                container_name: wso2sp-worker
                ports:
                  - "9090:9090"
                healthcheck:
                  test: ["CMD", "nc", "-z","localhost", "9090"]
                  interval: 10s
                  timeout: 120s
                  retries: 5
                volumes:
                  - ./siddhi-files:/home/wso2carbon/wso2-artifact-volume/wso2/worker/deployment/siddhi-files
    ```
        

## Exporting Siddhi files

To export multiple Siddhi files, follow the steps below:

1.  Start and access the Stream Processor Studio as instructed in
    [Stream Processor Studio Overview - Starting the Stream Processor
    Studio](Stream-Processor-Studio-Overview_112390916.html#StreamProcessorStudioOverview-Start)
    .
2.  Click **File** , and then click **Export as Docker** .  
    ![](attachments/112391019/112391022.png){width="258"}  
    As a result, the **Export as Docker** dialog box opens as follows.  
    ![](attachments/112391019/112391021.png){width="480"}
3.  In the **Profile** field, select either **Editor** or **Worker**
    depending on the profile in which the Siddhi file you want to export
    is located.
4.  Under **Files to be included** , expand the **workspace** directory
    to view all the Siddhi applications that are available to be
    exported.  
    ![](attachments/112391019/112391020.png){height="250"} Select the
    speific Siddhi applications you want to export by selecting the
    relevant check boxes. If you want to select all the Siddhi
    applications, select the check box for the **workspace** directory.
5.  Click **Export** . The Siddhi applications are exported as docker
    artifacts in a zip file to the default location in your machine,
    based on your operating system and browser settings.
