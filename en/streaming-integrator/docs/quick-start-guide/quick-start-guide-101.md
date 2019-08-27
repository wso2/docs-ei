# Quick Start Guide 101

## Introduction to Streaming Integration

The Streaming Integrator is one of the integrators in WSO2 Enterprise Integrator. It reads streaming data from files, cloud-based applications, streaming applications, and databases, and processes them. It also allows downstream applications to access streaming data by publishing information in a streaming manner. It can analyze streaming data, identify trends and patterns, and trigger inte

!!!info"Before you begin:
    - Install [Oracle Java SE Development Kit (JDK) version 1.8](https://www.oracle.com/technetwork/java/javase/downloads/index.html).
    - [Set the Java home](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) environment variable.
    - Download and install the following components of the Streaming Integrator:
        - Streaming Integrator Tooling 
        - Streaming Integrator runtime


## Creating your first Siddhi application

Create a basic Siddhi application for a simple use case.

1. Extract the Streaming Integrator Tooling pack to a preferred location. Hereafter, the extracted location is referred to as `<SI_HOME>`.

2. Navigate to the `<SI_HOME>/bin` directory and issue the following command to start the Streaming Integration tooling.
    -   For Windows: `tooling.bat`
    -   For Linux: `./tooling.sh`
    
3. Access the Streaming Integration Tooling via the `http://<HOST_NAME>:<TOOLING_PORT>/editor` URL. 
    !!!info
        The default URL is `http://<localhost:9390/editor`.
        
   The Streaming Integration Tooling opens as shown below.
   ![Streaming Integrator Tooling Welcome Page](../images/quick-start-guide-101/Welcome-Page.png)
        
4. Open a new Siddhi file by clicking **New**.

    The new file opens as follows.
    
    ![New Siddhi File](../images/quick-start-guide-101/New_Siddhi_File.png)
    
5. Specify a name for the new Siddhi application via the `@App:name` annotation, and a description via the `@App:description` annotation.
    `
    @App:name("TemperatureApp")
    @App:description("This application captures the room temperature and analyzes it, and presents the results as logs in the output console.")
    `
    
6. The details to be captures include the room ID, device ID, and the temperature. To specify this, define an input stream with attributes to capture each of these details. 
    `define stream TempStream(roomNo string, deviceNo long, temp double)`
    
7. The technician wants to know the average temperature with each new temperature reading. To publish this information, define an output stream including these details as attributes in the schema.
    `define stream OutputStream(roomNo string, deviceNo long, avgTemp double)`
    
8. The average temperature needs to be logged. Therefore, connect a sink of the `log` type to the output stream as shown below.
    `
    @sink(type = 'log', 
        @map(type = 'passThrough'))
    define stream OutputStream (roomNo string, deviceID long, avgTemp double);
    `
    `passThrough` is specified as the mapping type because in this scenario, the attribute names are received as they are defined in the stream and do not need to be mapped.

9. To get the input events, calculate the average temperature and direct the results to the output stream, add a query below the stream definitions as follows:
    1. To name the query, add the `@info` annotation and enter `CalculateAverageTemperature` as the query name.
        `@info(name = 'CalculateAvgTemp')`
    2. To indicate that the input is taken from the `TempStream` stream, add the `from` clause as follows:
        `from TempStream`
    3. Specify how the values for the output stream attributes are derived by adding a `select` clause as follows.
        `select roomNo, deviceNo, avg(temp)`
    4. To insert the results into the output stream, add the `insert into` clause as follows.
        `insert into OutputStream;`
        
The completed Siddhi application is as follows:
        
```
@App:name('TemperatureApp')
@App:description('This application captures the room temperature and analyzes it, and presents the results as logs in the output console.')
define stream TempStream (roomNo string, deviceID long, temp double);
@sink(type = 'log', 
	@map(type = 'passThrough'))
define stream OutputStream (roomNo string, deviceID long, avgTemp double);

@info(name = 'CalculateAvgTemp')
from TempStream 
select roomNo, deviceID, avg(temp) as avgTemp 
insert into OutputStream;
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

   The first line that is marked as a debug point is highlighted as shown below when they are hit.




## Deploying Siddhi applications

After creating and testing the `TemperatureApp` Siddhi application, you need to deploy it in the server so that you can use it to process actual data. To do this, follow the procedure below:

1. Open the `TemperatureApp` Siddhi application.
2. Click **Deploy** and then click **Deploy to Server**.
    ![Deploy to Server](../images/quick-start-guide-101/Deploy-to Server.png)

As a result, the `TemperatureApp` Siddhi application is saved in the `<SI_HOME>/deployment/siddhi-files` directory.


## Advanced streaming integrations

Now, let's assume that the technicians require more advanced processing carried out for the temperature readings which are as follows:

- Instead of calculating the average temperature based on all the readings done up to the given point, it is required to calculate the average temperature per hour.
- The technicians also need to identify the temperature peaks within each hour.
- The average temperature needs to be monitored only for the rooms in a specific wing.

To address the above requirements, let's update the `TemperatureApp` Siddhi application as follows:

1. You need to consider a time window of one hour when reporting the average temperature per hour as well as when reporting the temperature peaks per hour. Therefore, let's add a window definition that you can use in multiple queries.
    `define window OneHourTempWindow(roomNo string, deviceID long, temp double) time(1 min);`

2. Now lets write a query to insert all the events in the `TempStream` into the `OneHourTempWindow` you defined

1. To calculate the average temperature per hour, the Streaming Integrator needs to identify each subset of events that
belong to a specific hour. This can be managed by adding a time window of one hour as to the `from` clause of the `CalculateAvgTemp` query as shown below.

`from TempStream#window.time(1 hour)`

The complete `CalculateAvgTemp` query is now as follows.

```
@info(name = 'CalculateAvgTemp')
from TempStream#window.time(1 hour)
select roomNo, deviceID, avg(temp) as avgTemp
insert into OutputStream;
```

!!!info
    For the complete list of windows supported by Siddhi, see [Siddhi Extensions - siddhi-execution-unique](https://siddhi-io.github.io/siddhi-execution-unique/).

2. To produce the identified temperature peaks as an output, follow the sub steps below.
    1. To report the temperature peaks identified as logs, define an output stream with a `log` sink as follows.
        ```
        @sink(type='log', prefix='TemperaturePeak]:')
        define stream PeakTempStream(initialTemp double, peakTemp double);
        ```
        Here, you are specifying the initial temperature and the peak temperature to be logged in the console with the `TemperaturePeak` prefix.

    2. Create a separate query for this purpose. You can name it `IdentifyPeaks`.
        `@info(name = 'IdentifyPeaks')`

        1. From the input stream, you need to get only the peak temperatures and those need to be identified by the hour. Therefore, add a `from` clause as follows:
        `from TempStream#window.time(1 hour)`

        2. To derive the initial temperature and the peak temperature, add the `select` clause as follows.
            ``

3. To filter information that is only relevant to the rooms within the range, apply a filter as follows

##Extending the Streaming Integrator
Steps to download, install and use Siddhi extensions that are not packed with the Streaming Integrator by defaults.




