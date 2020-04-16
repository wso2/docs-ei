# Step 3: Test the Siddhi Application

Any Siddhi application created for a business purpose needs to be tested before it is used to process actual data in order to ensure that it functions as expected. In order to facilitate testing, WSO2 Streaming Integrator Tooling includes an event simulator. 

In [**Step 2: Create a Siddhi Application**](create-the-siddhi-application.md), you created the `TemperatureApp` Siddhi application to calculate the average of every three temperature readings. In this section we are simulating ten temperature readings. The following table shows the temperature read with each simulated event, and the average that should be output for every event.

|**Event No**|**Temperature**|**Calculated Average**<br/>(Calculated by the Siddhi Appllication)|
|------------|---------------|-------------------|
|           1|             26| 26                |
|           2|             27| 26.5              |
|           3|             26| 26.333333333333332|
|           4|             25| 26                |  
|           5|             26| 25.666666666666668|
|           6|             26| 25.666666666666668|
|           7|             27| 26.333333333333332|
|           8|             28| 27                |
|           9|             28| 27.666666666666668|
|          10|             29| 28.333333333333332|

To test the `TemperatureApp` Siddhi application you created in [**Step 2: Create a Siddhi Application**](create-the-siddhi-application.md) via this event simulator, follow the steps below.

!!! tip "Before you begin:" 
    For this scenaro, you need to simulate multiple events from a file. For this purpose, download the [tempReadings.csv file](../resources/tempReadings.csv).

1. In the Streaming Integrator Tooling, click the following icon for event simulation on the side panel.

    ![Event Simulator icon](../images/quick-start-guide-101/event-simulation-icon.png)
    
   The Simulation panel opens as shown below.

   ![Simulation Panel](../images/quick-start-guide-101/Simulation-Panel.png)
    
2. Click on the **Feed Simulation** tab of the simulation panel, and click **Create**. 

    ![Create button](../images/quick-start-guide-101/create-button.png)
    
    This opens a new panel for feed simulation.


3. In the panel for feed simulation that opened, enter information as follows:

    1. In the fields in the top section of the panel, enter information as follows:

        |**Field**              |**Value**                        |
        |-----------------------|---------------------------------|
        |**Simulation Name**    |`Temp Events`                    |
        |**Description**        |`Simulating Temperature Readings`|
        |**Time Interval(ms)**  |`2000`                           |

    2. In the **Simulation Source** field, select **CSV file**. Then click **Add Simulation Source**.
    
        This adds a new section named **CSV file Source 1: TemperatureApp** to the panel.
        
    3. In the **CSV file Source 1: TemperatureApp** section, enter information as follows:
    
        |**Field**          |**Value**                              |
        |-------------------|---------------------------------------|
        |**Siddhi App Name**|`TemperatureApp`                       |
        |**Stream Name**    |`TempStream`                           |
        |**CSV file**       |Click **Upload**. Then click **Choose File** and browse for the [tempReadings.csv file](../resources/tempReadings.csv) that you downloaded.|
        
        Click **Upload** in the **Upload CSV** dialog box.
        
    4. Click Save. The simulation you added is now displayed in the **Feed Simulation** tab as follows.
    
        ![Feed Simulation](../images/quick-start-guide-101/feed-simulation.png)
          
4. Click the play button for the simulation.

    ![Activate Simulation](../images/quick-start-guide-101/play.png))
    
    In the **Start the Siddhi Application** dialog box that appears, click **Start Simulation**.

    The output is logged in the console as follows:
    
    ![Console Log](../images/quick-start-guide-101/output-log.png)
    
## What's Next?

Once you have tested and ensured that the `TemperatureApp` Siddhi application functions as expected, you can deploy it to process actual data. To do this, proceed to [**Step 4: Deploy the Siddhi Application**](deploy-siddhi-application.md).