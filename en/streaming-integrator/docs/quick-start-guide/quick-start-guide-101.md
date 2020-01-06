# Quick Start Guide 101

## Introduction to Streaming Integration

The Streaming Integrator is one of the integrators in WSO2 Enterprise Integrator. It reads streaming data from files,
cloud-based applications, streaming applications, and databases, and processes them. It also allows downstream
applications to access streaming data by publishing information in a streaming manner. It can analyze streaming data,
identify trends and patterns, and trigger integration flows.

The purpose of this guide if for you to understand the basic functions of the Streaming Integrator in 30 minutes.

To learn how to use the key functions of the Streaming Integrator, consider a laboratory that is observing the temperature
of a range of rooms in a building via a sensor and needs to use the temperature readings as the input to derive other information.

!!!info "Before you begin:"
    - Install [Oracle Java SE Development Kit (JDK) version 1.8](https://www.oracle.com/technetwork/java/javase/downloads/index.html).<br/>
    - [Set the Java home](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) environment variable.<br/>
    - Download and install the following components of the Streaming Integrator:<br/>
        - Streaming Integrator Tooling<br/>
        - Streaming Integrator runtime


## Creating your first Siddhi application

Create a basic Siddhi application for a simple use case.

1. Extract the Streaming Integrator Tooling pack to a preferred location. Hereafter, the extracted location is referred to as `<SI_TOOLING_HOME>`.

2. Navigate to the `<SI_TOOLING_HOME>/bin` directory and issue the appropriate command depending on your operating system to start the Streaming Integration tooling.

    -   For Windows: `tooling.bat`

    -   For Linux/MacOS: `./tooling.sh`
    
3. Access the Streaming Integration Tooling via the `http://<HOST_NAME>:<TOOLING_PORT>/editor` URL.

    !!!info
        The default URL is `http://<localhost:9390/editor`.
        
   The Streaming Integration Tooling opens as shown below.

   ![Streaming Integrator Tooling Welcome Page](../images/quick-start-guide-101/Welcome-Page.png)
        
4. Open a new Siddhi file by clicking **New**.

    The new file opens as follows.
    
    ![New Siddhi File](../images/quick-start-guide-101/New_Siddhi_File.png)
    
5. Specify a name for the new Siddhi application via the `@App:name` annotation, and a description via the `@App:description` annotation.

    ```
    @App:name("TemperatureApp")
    @App:description("This application captures the room temperature and analyzes it, and presents the results as logs in the output console.")
    ```
    
6. The details to be captures include the room ID, device ID, and the temperature. To specify this, define an input stream with attributes to capture each of these details.

    `define stream TempStream(roomNo string, deviceNo long, temp double)`
    
7. The technicians need to know the average temperature with each new temperature reading. To publish this information, define an output stream including these details as attributes in the schema.

    `define stream AverageTempStream(roomNo string, deviceNo long, avgTemp double)`
    
8. The average temperature needs to be logged. Therefore, connect a sink of the `log` type to the output stream as shown below.

    ```
    @sink(type = 'log', 
        @map(type = 'passThrough'))
    define stream AverageTempStream (roomNo string, deviceID long, avgTemp double);
    ```

    `passThrough` is specified as the mapping type because in this scenario, the attribute names are received as they are defined in the stream and do not need to be mapped.

9. To get the input events, calculate the average temperature and direct the results to the output stream, add a query below the stream definitions as follows:

    1. To name the query, add the `@info` annotation and enter `CalculateAverageTemperature` as the query name.

        `@info(name = 'CalculateAvgTemp')`

    2. To indicate that the input is taken from the `TempStream` stream, add the `from` clause as follows:

        `from TempStream`

    3. Specify how the values for the output stream attributes are derived by adding a `select` clause as follows.

        `select roomNo, deviceNo, avg(temp)`

    4. To insert the results into the output stream, add the `insert into` clause as follows.

        `insert into AverageTempStream;`
        
The completed Siddhi application is as follows:
        
```
@App:name('TemperatureApp')
@App:description('This application captures the room temperature and analyzes it, and presents the results as logs in the output console.')
define stream TempStream (roomNo string, deviceID long, temp double);
@sink(type = 'log', 
	@map(type = 'passThrough'))
define stream AverageTempStream (roomNo string, deviceID long, avgTemp double);

@info(name = 'CalculateAvgTemp')
from TempStream 
select roomNo, deviceID, avg(temp) as avgTemp 
insert into AverageTempStream;
```

## Testing your Siddhi application

The application you created needs to be tested before he uses it to process the actual data received. You can test it in the following methods:


### Simulating events

To simulate events for the Siddhi application, you can use the event simulator available with in the Streaming Integration Tooling as explained in the procedure below.

1. In the Streaming Integrator Tooling, click the following icon for event simulation on the side panel.

    ![Event Simulator icon](../images/quick-start-guide-101/event-simulation-icon.png)
    
   The Simulation panel opens as shown below.

   ![Simulation Panel](../images/quick-start-guide-101/Simulation-Panel.png)
    
2. In the **Single Simulation** tab of the simulation panel, select **TemperatureApp** from the list for the **Siddhi App Name** field.

3. You need to send events to the input stream. Therefore, select **TempStream" in the **Stream Name** field. As a result, the attribute list appears in the simulation panel.

4. Then enter values for the attributes as follows:

    <style type="text/css">
    .tg  {border-collapse:collapse;border-spacing:0;}
    .tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
    .tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
    .tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
    </style>
    <table class="tg">
      <tr>
        <th class="tg-0pky"><span style="font-weight:bold">Attribute</span></th>
        <th class="tg-0pky"><span style="font-weight:bold">Value</span></th>
      </tr>
      <tr>
        <td class="tg-0pky"><span style="font-weight:bold">roomNo</span></td>
        <td class="tg-0pky">NW106</td>
      </tr>
      <tr>
        <td class="tg-0pky"><span style="font-weight:bold">deviceID</span></td>
        <td class="tg-0pky">262626367171371717</td>
      </tr>
      <tr>
        <td class="tg-0pky"><span style="font-weight:bold">temp</span></td>
        <td class="tg-0pky">26</td>
      </tr>
    </table>
    
    ![Single Simulation](../images/quick-start-guide-101/single-simulation.png)
    
5. Click **Start and Send**.

The output is logged in the console as follows:
![Console Log](../images/quick-start-guide-101/output-console.png)

### Debugging

To debug your Siddhi application, you need to mark debug points, and then simulate events as you did in the previous section. The complete procedure is as follows:

1. Open the `TemperatureApp` Siddhi application.
2. To run the Siddhi application in the debug mode, click **Run** => **Debug**, or click the following icon for debugging.

    ![Debug icon](../images/quick-start-guide-101/debug-icon.png)

    As a result, the **Debug** console opens in a new tab below the Siddhi application as follows.

    ![Debug Console](../images/quick-start-guide-101/debug-console.png)

3. Apply debug points in the lines with the `from` and `insert into` clauses. To mark a debug point, you need to click on the left of the required line number so that it is marked with a dot as shown in the image below.

    !!!info
        You can only mark lines with from or insert into clauses as debug points.

   ![Debug Points](../images/quick-start-guide-101/debug-points.png)

4. Now simulate a single event the same way you simulated it in the previous section, with the following values.
   <style type="text/css">
   .tg  {border-collapse:collapse;border-spacing:0;}
   .tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
   .tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
   .tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
   </style>
   <table class="tg">
     <tr>
       <th class="tg-0pky"><span style="font-weight:bold">Attribute</span></th>
       <th class="tg-0pky"><span style="font-weight:bold">Value</span></th>
     </tr>
     <tr>
       <td class="tg-0pky"><span style="font-weight:bold">roomNo</span></td>
       <td class="tg-0pky">NW106</td>
     </tr>
     <tr>
       <td class="tg-0pky"><span style="font-weight:bold">deviceID</span></td>
       <td class="tg-0pky">262626367171371717</td>
     </tr>
     <tr>
       <td class="tg-0pky"><span style="font-weight:bold">temp</span></td>
       <td class="tg-0pky">26</td>
     </tr>
   </table>

   Click **Send** to send the event.

   When a debug point is hit, the line marked as a debug point is highlighted as shown below. The status of the event and the query is displayed in the debug console below the Siddhi application.

   ![Debugging](../images/quick-start-guide-101/debug-points-and-console.png)


## Deploying Siddhi applications

After creating and testing the `TemperatureApp` Siddhi application, you need to deploy it in the Streaming Integrator server, export it as a Docker image, or deploy in Kubernetes.

### Deploying in Streaming Integrator server

To deploy your Siddhi application in the Streaming Integrator server, follow the procedure below:

!!!info
    To deploy the Siddhi application, you need to run both the Streaming Integrator server and Streaming Integrator Tooling. The home directories of the Streaming Integrator server is referred to as `<SI_HOME>` and the home directory of Streaming Integrator Tooling is referred to as `<SI_TOOLING_HOME>`.

1. Start the Streaming Integrator server by navigating to the `<SI_HOME>/bin` directory from the CLI, and issuing the appropriate command based on your operating system:</br>
   - For Windows: `server.bat --run`</br>
   - For Linux/Mac OS:  `./server.sh`

2. In the Streaming Integrator Tooling, click **Deploy** and then click **Deploy to Server**.

    ![Deploy to Server Menu Option](../images/quick-start-guide-101/deploy-to-server-menu.png)

    The **Deploy Siddhi Apps to Server** dialog box opens as follows.

    ![Deploy Siddhi Apps to Server](../images/quick-start-guide-101/deploy-to-server-dialog-box.png)

3. In the **Add New Server** section, enter information as follows:

    | Field           | Value                            |
    |-----------------|----------------------------------|
    | **Host**        | Your host                        |
    | **Port**        | `9443`                           |
    | **User Name**   | `admin`                          |
    | **Password**    | `admin`                          |

    ![Add Server](../images/quick-start-guide-101/add-server.png)

    Then click **Add**.

4. Select the check boxes for the **TemperatureApp.siddhi** Siddhi application and the server you added as shown below.

    ![Deploy Siddhi Apps to Server](../images/quick-start-guide-101/select-siddhi-app-and-server.png)

5. Click **Deploy**.

    As a result, the `TemperatureApp` Siddhi application is saved in the `<SI_HOME>/deployment/siddhi-files` directory, and the following is message displayed in the dialog box.

    ![Siddhi App successfully deployed](../images/quick-start-guide-101/siddhi-app-successfully-deployed.png)


### Deploying in Docker

To export the `TemperatureApp` Siddhi application as a Docker artifact, follow the procedure below:

1. Open the Streaming Integrator Tooling.

2. Click **Export** in the top menu, and then click **For Docker**.

    ![Export Siddhi Application as Docker](../images/quick-start-guide-101/export-as-docker.png)

    As a result, **Step 1** of the **Export Siddhi Apps for Docker image** wizard opens as follows.
    
    ![Export as Docker dialog box](../images/quick-start-guide-101/export-as-docker-dialog-box.png)
    
3. Select the **TemperatureApp.siddhi** check box and click **Next**.
    
4. In **Step 2**, you can template values of the Siddhi Application.
    
    ![Template Siddhi Apps dialog box](../images/quick-start-guide-101/template-siddhi-apps-dialog-box.png)
    
    Click **Next** without templating any value of the Siddhi application.

    !!!info
        For detailed information about templating the values of a Siddhi Application, see [Exporting Siddhi Apps for Docker Image](../develop/exporting-Siddhi-Files.md#exporting-siddhi-apps-for-docker-image).
    
5. In **Step 3**, you can update configurations of the Streaming Integrator.
    
    ![Update Streaming Integrator Configurations dialog box](../images/quick-start-guide-101/update-streaming-integrator-configurations-dialog-box.png)
    
    Leave the default configurations, and click **Next**.
    
6. In **Step 4**, you can provide arguments for the values that were templated in **Step 2**.
    
    ![Populate Arguments Template dialog box](../images/quick-start-guide-101/populate-arguments-template-dialog-box.png)
    
    There are no values to be configured because you did not template any values in **Step 2**. Therefore click **Next**.
    
7. In **Step 5**, you can choose additional dependencies to be bundled. This is applicable when Sources, Sinks and etc. with additional dependencies are used in the Siddhi Application (e.g., a Kafka Source/Sink, or a MongoDB Store).
    In this scenario, there are no such dependencies. Therefore nothing is shown as additional JARs.
    
    ![Bundle Additional Dependencies dialog box](../images/quick-start-guide-101/bundle-additional-dependencies-dialog-box.png)
    
    Click **Export**. The Siddhi application is exported as a Docker artifact in a zip file to the default location in your machine, based on your operating system and browser settings.


##Extending the Streaming Integrator

The Streaming Integrator is by default shipped with most of the available Siddhi extensions by default. If a Siddhi extension you require is not shipped by default, you can download and install it.

In this scenario, let's assume that the laboratories require the siddhi-execution-extrema extension to carry out more advanced calculations for different types of time windows. To download and install it, follow the procedure below:

1. Open the [Siddhi Extensions page](https://store.wso2.com/store/assets/analyticsextension/list). The available Siddhi extensions are displayed as follows.
    ![Extensions Home Page](../images/quick-start-guide-101/siddhi-extensions.png)

2. Search for the siddhi-execution-extrema extension.
    ![Searched Extension](../images/quick-start-guide-101/search-for-extension.png)

3. Click on the **V4.1.1** for this scenario. As a result, the following page opens.
    ![Extrema Extension Page](../images/quick-start-guide-101/extension-page.png)

4. To download the extension, click **Download Extension**. Then enter your email address in the dialog box that appears, and click **Submit**. As a result, a JAR fil is downloaded to a location in your machine (the location depends on your browser settings).

5. To install the siddhi-execution-extrema extension in your Streaming Integrator, place the JAR file you downloaded in the `<SI_HOME>/lib directory`.


## Further references

- For a quicker demonstration of the Streaming Integrator, see [Getting Started with the Streaming Integrator in Five Minutes](getting-started-with-si.md).
- For a quick guide on how the Streaming Integrator works with the Micro Integrator to trigger integration flows, see [Getting SI Running with MI in 5 Minutes].
- To get the Streaming Integrator running with Docker in five minutes, see [Getting SI Running with Docker in 5 Minutes](hello-world-with-docker.md).
- To get the Streaming Integrator running in a Kubernetes cluster in five minutes, see [Getting SI Running with Kubernetes in 5 Minutes](hello-world-with-kubernetes.md).