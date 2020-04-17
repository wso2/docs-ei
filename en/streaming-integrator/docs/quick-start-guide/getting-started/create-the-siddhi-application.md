# Step 2: Create the Siddhi Application

Let's create your first Siddhi application.

For this purpose, you can use the same example of temperature readings via a sensor mentioned in [Understanding Streaming Integration](getting-started-guide-overview.md). You can consider a simple use case where you need to do view the temperature readings on your terminal. Also, instead of viewing all the temperature readings, you want to view the average of every three readings in a sliding manner.

The following image depicts the procedure to be followed by the Siddhi application you create.

![Siddhi Application Flow](../../images/quick-start-guide-101/siddhi-application-flow.png)

1. Extract the Streaming Integrator Tooling pack to a preferred location. Hereafter, the extracted location is referred to as `<SI_TOOLING_HOME>`.

2. Navigate to the `<SI_TOOLING_HOME>/bin` directory and issue the appropriate command depending on your operating system to start the Streaming Integration tooling.

    -   For Windows: `tooling.bat`

    -   For Linux/MacOS: `./tooling.sh`
    
3. Access the Streaming Integration Tooling via the `http://<HOST_NAME>:<TOOLING_PORT>/editor` URL.

    !!!info
        The default URL is `http://<localhost:9390/editor`.
        
   The Streaming Integration Tooling opens as shown below.

   ![Welcome Page](../../images/Creating-Siddhi-Applications/Welcome-Page.png)
        
4. Open a new Siddhi file by clicking **New**.

    The new file opens as follows.
    
    ![New Siddhi File](../../images/Creating-Siddhi-Applications/New_Siddhi_File.png)
    
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

        `from TempStream#window.length(3)`
        
        Here, `#window.length(3)` is a length window connected to the `TempStream` input stream indicates that the calculations are applied to every three input events in a sliding manner.
        
        !!! tip
            For more information about all the supported window types, see [Siddhi Query Guide - Siddhi Execution Unique](https://siddhi-io.github.io/siddhi-execution-unique).

    3. Specify how the values for the output stream attributes are derived by adding a `select` clause as follows.

        `select roomNo, deviceNo, avg(temp)`
        
        Here, you are applying the `avg()` function to the `temp` attribute in order to calculate the average for that attribute. However, this average calculation is applied to every subset of three events due to the length window connected to the `from` clause.

    4. To insert the results into the output stream, add the `insert into` clause as follows.

        `insert into AverageTempStream;`
        
The completed Siddhi application is as follows:
        
```
@App:name('TemperatureApp')

@App:description('This application captures the room temperature and analyzes it, and presents the results as logs in the output console.')
define stream TempStream (roomNo string, deviceID string, temp double);
@sink(type = 'log', 
	@map(type = 'passThrough'))
define stream AverageTempStream (roomNo string, deviceID string, avgTemp double);

@info(name = 'CalculateAvgTemp')
from TempStream#window.length(3)
select roomNo, deviceID, avg(temp) as avgTemp
insert into AverageTempStream;
```

## What's Next?

To test the `TemperatureApp` Siddhi application you created, proceed to [Step 3: Test the Siddhi Application](test-siddhi-application.md).