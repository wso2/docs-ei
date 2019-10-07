# Exporting Siddhi Apps

The Streaming Integrator Tooling allows you to export one or more siddhi files as a Docker or Kubernetes artifact, to make it possible to run those Siddhi applications within a Docker or Kubernetes environment.

To export Siddhi files as Docker or Kubernetes artifacts, follow the steps given below.

## Exporting Siddhi Apps for Docker Image

1. Start the Streaming Integrator Tooling buy issueing one of the following commands from the `<SI_HOME>/bin` directory.
    - For Windows: `tooling.bat`
    - For Linux: `./tooling.sh`
    The Streaming Integrator Tooling opens as shown below.
    ![Streaming Integrator Welcome Page](../images/exporting-Siddhi-Applications/SI-Welcome_Page.png)

2. Create required Siddhi files. The rest of the documentation assumes that you have set of Siddhi apps to be exported as a Docker image.

3. From the **Export** menu, select **For Docker**. The following wizard will be popped up.

![Export as Docker/Kubernetes Menu](../images/exporting-Siddhi-Applications/Export_Docker_k8s_Menu.png)

![Export Siddhi App for Docker image dialog](../images/exporting-Siddhi-Applications/Export_Docker_1.png)

4. From the **Step 1: Select Siddhi Apps** select a set of Siddhi apps to be included in the docker image. You can select one or more Siddhi apps.

5. Click **Next**.

6. From the **Step 2: Template Siddhi Apps**, you can template the Siddhi app. You can use `${...}` pattern to define a template within the Siddhi app. (eg. `${http.port}`).

    Example:
    ```
    @source(type='http', receiver.url='http://localhost:${http.port}/${http.context}',
        @map(type = 'json'))
    define stream StockQuoteStream(symbol string, price double, volume int);
    ```

7. Once templates are defined, click **Next**.

8. From **Step 3: Update Streaming Integrator configurations**, you can update the configuration file (`deployment.yaml`) of the Streaming Integration specific to the Docker image. Similar to the previous step, you can template the configurations as well. Use the similar notation i.e. `${...}` to specify a template within the configuration file.

9. Once the configuration is complete, click **Next**.

10. The next step, **Step 4: Populate arguments template** is to define the values for the template fields defined in Step 2 and Step 3. This step will list down all the template fields, therefore you can set the relevant values for each field.

11. Once the template values are set, click **Next**.

12. The last step **Step 5: Bundle additional dependencies** allows you to select additional JARs to be shipped within the Docker image.

13. Once the everything is complete, click **Export** to export a ZIP file with the following directory structure.

    ```
    .
    ├── Dockerfile
    ├── configurations.yaml
    └── siddhi-files
        ├── <SIDDHI_FILE>.siddhi
        ├── ...
        └── <SIDDHI_FILE>.siddhi
    ```

    For more information on Siddhi Docker artifacts, please refer [Siddhi 5.1 as a Docker Microservice](https://siddhi.io/en/v5.1/docs/siddhi-as-a-docker-microservice/) documentation.<br /><br />

    !!! info
        This functionality differs based on the web browser you are using and its settings. e.g., if you have set a default
        download location and disabled the **Ask where to save each file before downloading** feature as shown below, the
        file is downloaded to the default location without prompting you for any further input.<br/>
        ![Download Settings](../images/exporting-Siddhi-Applications/Download_Settings.png)

## Exporting Siddhi Apps for Kubernetes

1. Start the Streaming Integrator Tooling buy issueing one of the following commands from the `<SI_HOME>/bin` directory.
    - For Windows: `tooling.bat`
    - For Linux: `./tooling.sh`
    The Streaming Integrator Tooling opens as shown below.
    ![Streaming Integrator Welcome Page](../images/exporting-Siddhi-Applications/SI-Welcome_Page.png)

2. Create required Siddhi files. The rest of the documentation assumes that you have set of Siddhi apps to be exported as a Kubernetes artifacts.

3. From the **Export** menu, select **For Kubenetes**. The following wizard will be popped up.

![Export as Docker/Kubernetes Menu](../images/exporting-Siddhi-Applications/Export_Docker_k8s_Menu.png)

![Export Siddhi App for Docker image dialog](../images/exporting-Siddhi-Applications/Export_k8s_1.png)

4. From the **Step 1: Select Siddhi Apps** select a set of Siddhi apps to be included in the docker image. You can select one or more Siddhi apps.

5. Click **Next**.

6. From the **Step 2: Template Siddhi Apps**, you can template the Siddhi app. You can use `${...}` pattern to define a template within the Siddhi app. (eg. `${http.port}`).

    Example:
    ```
    @source(type='http', receiver.url='http://localhost:${http.port}/${http.context}',
        @map(type = 'json'))
    define stream StockQuoteStream(symbol string, price double, volume int);
    ```

7. Once templates are defined, click **Next**.

8. From **Step 3: Update Streaming Integrator configurations**, you can update the configuration file (`deployment.yaml`) of the Streaming Integration specific to the Docker image. Similar to the previous step, you can template the configurations as well. Use the similar notation i.e. `${...}` to specify a template within the configuration file.

9. Once the configuration is complete, click **Next**.

10. The next step, **Step 4: Populate arguments template** is to define the values for the template fields defined in Step 2 and Step 3. This step will list down all the template fields, therefore you can set the relevant values for each field.

11. Once the template values are set, click **Next**.

12. The next step **Step 5: Bundle additional dependencies** allows you to select additional JARs to be shipped within the Docker image.

13. Once the JARs are selected, click **Next**.

14. In the last step **Step 7: Add Kubernetes config**, click **Export**. This will download the ZIP file with following directory structure.

    ```
    ├── Dockerfile
    ├── configurations.yaml
    ├── siddhi-files
    │   ├── <SIDDHI_FILE>.siddhi
    │   ├── ...
    │   └── <SIDDHI_FILE>.siddhi
    └── siddhi-process.yaml
    ```

15. Once the ZIP is downloaded, you can extract and open the `<ZIP_HOME>/siddhi-process.yaml` via a text editor to modify the SiddhiProcess configuration.<br /><br />

    For more information on **SiddhiProcess** Kubernetes configuration, please refer [Siddhi 5.1 as a Kubernetes Microservice](https://siddhi.io/en/v5.1/docs/siddhi-as-a-docker-microservice/) documentation.<br /><br />

    !!! info
        This functionality differs based on the web browser you are using and its settings. e.g., if you have set a default
        download location and disabled the **Ask where to save each file before downloading** feature as shown below, the
        file is downloaded to the default location without prompting you for any further input.<br/>
        ![Download Settings](../images/exporting-Siddhi-Applications/Download_Settings.png)
