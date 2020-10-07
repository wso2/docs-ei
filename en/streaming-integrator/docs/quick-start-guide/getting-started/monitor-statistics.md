# Step 8: Monitor Statistics

This step shows how you can monitor the CDC and file statistics of the WSO2 Streaming Integrator deployment you started and the `SweetFactoryApp` Siddhi application you created and deployed in the previous steps.

!!! tip "Before you begin:"
    As instructed in [# Step 1: Download Streaming Integrator and Dependencies](download-install-and-start-si.md), you need to first start the Prometheus server and then the Grafana.
    
1. To enable the `SweetFactoryApp` Siddhi application to publish statistics to Prometheus, add the `@App:statistics(reporter = 'prometheus')` annotation to it below the `@App:name` annotation as shown below:

    ```text
        @App:name('SweetFactoryApp')
        @App:statistics(reporter = 'prometheus')
    ```

2. Start WSO2 Streaming Integrator.

3. To generate statistics, insert as many events as you want into the `SweetProductionTable` MySQL table that you created for this scenario in [Step 1: Download Streaming Integrator and Dependencies](download-install-and-start-si.md). Also, manually add as many rows as you want in the `/Users/foo/productioninserts.csv` file.

4. Access Grafana via the `localhost:3000` URL.

5. In the side panel, click the **Dashboards** icon and click **Dashboards**.

    ![Access Dashboards](../../images/quick-start-guide-101/access-grafana-dashboards.png)
    
    Then click on the **WSO2 Streaming Integrator - Overall Statistics** dashboard. It opens as follows.
    
    !!! info
        The statistics displayed will be different based on the number of records you inserted to the `SweetProductionTable` MySQL table and the number of rows you added in the `/Users/foo/productioninserts.csv` file during the last 30 minutes. You can also change the time interval for which statistics are displayed via the field for selecting the time interval in the top panel.
    
    ![overall-statistics](../../images/quick-start-guide-101/overall-staistics.png)
    
6. Under **Overview Statistics**, click **SweetFactoryApp**. The **overview-statistics / WSO2 Streaming Integrator App Statistics** dashboard opens.

    ![app-statistics](../../images/quick-start-guide-101/app-staistics.png)
    
7. Scroll down to the **Sources** section. The following is displayed.

    ![source-statistics](../../images/quick-start-guide-101/sources.png)
    
    The two entries displayed above represent the `file` source and the `cdc` source used in the `SweetFactoryApp` Siddhi application.
    
8. Scroll down further to the **Destinations** section. The `file` sink in the `SweetFactoryApp` Siddhi application is displayed as shown below.

    ![destination-statistics](../../images/quick-start-guide-101/destination.png)
    
9. Under **Sources**, click on the link to the `productioninserts.csv` file. The **WSO2 Streaming Integrator - File Statistics** dashboard opens. The contents of the `productioninserts.csv` file is the output of one query and the input of another. Therefore, it is a source as well as a destination, statistics are displayed for it under **Source** and **Sink** as shown below.

    **Source Statistics**
    
    ![File Statistics - Source](../../images/quick-start-guide-101/file-statistics-source.png)
    
    **Sink Statistics**
    
    ![File Statistics - Sink](../../images/quick-start-guide-101/file-statistics-sink.png)
    
10. Under **WSO2 Streaming Integrator - File Statistics** dashboard -> **Sources**, click on the file link. The **file-statistics / WSO2 Streaming Integrator / File Source Statistics**  dashboard opens displaying detailed statistics for the file when it is functioning as a source.

    ![File Source Statistics](../../images/quick-start-guide-101/file-statistics-sink.png)

11. Under **WSO2 Streaming Integrator - File Statistics** dashboard -> **Sources**, click on the file link. The **file-statistics / WSO2 Streaming Integrator / File Sink Statistics**  dashboard opens displaying detailed statistics for the file when it is functioning as a source.

    ![File Sink Statistics](../../images/quick-start-guide-101/file-statistics-sink.png)
                                                                                   
12. In the **overview-statistics / WSO2 Streaming Integrator App Statistics** dashboard -> **CDC** section, click on the **SweetProductionTable** link. The **cdc-statistics / WSO2 Streaming Integrator / CDC Statistics**  dashboard opens with statistics generated for the `cdc` source in the `SweetFactoryApp` Siddhi application.

    ![CDC Statistics](../../images/quick-start-guide-101/cdc-statistics.png)
    
    Under **Streaming**, click on the **SweetProductionTable** link. The **cdc-statistics / WSO2 Streaming Integrator / CDC Streaming Statistics** dashboard opens as follows.
    
    ![CDC Streaming Statistics](../../images/quick-start-guide-101/cdc-streaming-statistics.png)
    
## What's Next

- To learn more about the key concepts of WSO2 Streaming Integrator, see [Key Concepts](../../concepts/concepts.md).

- For more hands-on experience with WSO2 Streaming Integrator, try the [Tutorials](../../examples/tutorials-overview.md).

- For more guidance as you use WSO2 Streaming Integrator for your Streaming Integration use cases, see [Use Cases](../../guides/use-cases.md).

    

    