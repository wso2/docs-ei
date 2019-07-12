# Exporting Siddhi Files
The Siddhi files you create in the Stream Processor Studio are saved in the `<SI_HOME>/wso2/editor/deployment/workspace` 
directory by default. If you want to save a Siddhi file in a different location, you can export it to the required 
location. You can also enable Siddhi applications to be run in a Docker environment by exporting them as Docker 
artifacts.

## Exporting a single Siddhi file
If you want to copy a specific Siddhi application to multiple Streaming Integrator instances, you can export it as follows:

1. Start Streaming Integrator Tooling by issuing one of the following commands from the `<SI_HOME>/bin` directory.
    - For Windows: `streaming-integrator-tooling.bat`
    - For Linux: `./streaming-integrator-tooling.sh` <br/>
    
    The Streaming Integrator Tooling opens as shown below.  
    ![Streaming Integrator Welcome Page](../images/exporting-Siddhi-Applications/SI-Welcome_Page.png)
2. Open the Siddhi application you want to export. You can click **Open** and select a Siddhi application that is 
    already saved in the `<SI_HOME>/wso2/server/deployment/workspace` directory.
3. Click **File** =\> **Export File**.  
    ![Exporting Siddhi File](../images/exporting-Siddhi-Applications/Export_Siddhi_File.png)  
    This opens the native file browser opens as shown below.  
    ![Browse Export Location](../images/exporting-Siddhi-Applications/fileExport.png)
4. Browse for the required directory path and click **Save**.

!!! info
    This functionality differs based on the web browser you are using and its settings. e.g., if you have set a default
    download location and disabled the **Ask where to save each file before downloading** feature as shown below, the 
    file is downloaded to the default location without prompting you for any further input.
    ![Download Settings](../images/exporting-Siddhi-Applications/Download_Settings.png)  
    
## Exporting Siddhi Files as Docker Artifacts
The Streaming Integrator Tooling allows you to export one or more selected Siddhi files as Docker artifacts in order to
 make it possible to run those Siddhi applications in a Docker environment.

To export  Siddhi files as Docker artifacts, follow the steps below:

!!! tip
    The exported Docker artifacts use docker-compose for this purpose. Therefore to run the artifacts, you need to 
    install the following in the running environment.
    - [Docker](https://www.docker.com/)
    - [Docker Compose](https://docs.docker.com/compose/)


### Configuring Docker in Streaming Integrator

By default, the Streaming Integrator uses the latest docker image hosted in [wso2.docker.com](http://wso2.docker.com/).
 If you want to use the different version of an image, follow the steps below:

1.  Update the `Configurations for Docker export` section of the `<SI_HOME>/conf/editor/deployment.yaml` file as
    follows.

    ``` xml
        # Configurations for Docker export feature
        docker.configs:
          productVersion: <PREFERED_PRODUCT_VERSION>
    ```

    You need to replace the `<PREFERED_PRODUCT_VERSION>` with your preferred image version e.g., 4.3.0.

2.  Restart the Streaming Integrator server.

    !!! info
        This feature also uses the following configuration files with the content as shown below:

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
        

### Exporting Siddhi files

To export multiple Siddhi files, follow the steps below:

1. Start Streaming Integrator Tooling by issuing one of the following commands from the `<SI_HOME>/bin` directory.
     - For Windows: `streaming-integrator-tooling.bat`
     - For Linux: `./streaming-integrator-tooling.sh` <br/>
2. Click **File** , and then click **Export as Docker**.  
   ![Export Docker file menu](../images/exporting-Siddhi-Applications/File_Menu_Export_Docker.png) 
   As a result, the **Export as Docker** dialog box opens as follows.  
   ![Export as Docker dialog box](../images/exporting-Siddhi-Applications/Export_As_Docker.png)
3. In the **Profile** field, select either **Editor** or **Worker** depending on the profile in which the Siddhi file
   you want to export is located.
4. Under **Files to be included** , expand the **workspace** directory to view all the Siddhi applications that are 
   available to be exported.  
   ![Select required Siddhi files](../images/exporting-Siddhi-Applications/Select_Siddhi_Files.png)
   Select the speific Siddhi applications you want to export by selecting the relevant check boxes. If you want to 
   select all the Siddhi applications, select the check box for the **workspace** directory.
5.  Click **Export**. The Siddhi applications are exported as docker artifacts in a zip file to the default location 
    in your machine, based on your operating system and browser settings.