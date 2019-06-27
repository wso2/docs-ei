# Simulating Multiple Events via Databases

This section explains how to generate multiple events via databases to
be analyzed via WSO2 SP.

### Prerequisites

Before simulating events via databases, the following prerequisites must
be completed:

-   A Siddhi application must be created.
-   The database from which you want to simulate events must be already
    configured for WSO2 SP.

### Simulating events

To simulate multiple events from a database, follow the procedure below:

1.  Access the Stream Processor Studio via the
    `                       http://localhost:/editor                     `
    URL. The Stream Processor Studio opens as shown below.

        !!! info
    
        The default URL is
        `                       http://localhost:9090/editor                     `
    

    ![](attachments/112390893/112390902.png)

2.  Click the **Event Simulator** icon in the left pane of the editor.  
    ![](attachments/112390893/112390901.png){width="50"}
3.  Click the **Feed** tab to open the **Feed Simulation** panel.  
    ![](attachments/112390893/112390900.png)
4.  To create a new simulation, click **Create** . This opens the
    following panel.  
    ![](attachments/112390893/112390899.png)
5.  Enter values for the displayed fields as follows.
    1.  In the **Simulation Name** field, enter a name for the event
        simulation.
    2.  In the **Description** field, enter a description for the event
        simulation.
    3.  If you want to simulate events at time intervals of a specific
        length, enter that length in milliseconds in the **Time
        Interval(ms)** field.
    4.  If you want to enter more advanced conditions to simulate the
        events, click **Advanced Configurations** . As a result, the
        following section is displayed.  
        ![](attachments/112390893/112390898.png){width="442"
        height="191"}  
        Then enter details as follows.
        1.  If you want to include only events that belong to a specific
            time interval in the simulation feed, enter the start time
            and the end time  in the **Starting Event's Timestamp** and
            **Ending Event's Timestamp** fields respectively. To select
            a timestamp, click the time and calendar icon next to the
            field.  
            ![](attachments/112390893/112390904.png)  
            Then select the required date, hour, minute, second and
            millisecond. Click **Done** to select the time stamp
            entered. If you want to select the current time, you can
            click **Now** .
        2.  If you want to restrict the event simulation feed to a
            specific number of events, enter the required number in the
            **No of Events** field.
    5.  In the **Simulation Source** field, select **Database** . To
        connect to a new database, click **Add Simulation Source** to
        open the following section.  
        ![](attachments/112390893/112390897.png)  
        Enter information as follows:

        | Field               | Description                                                                                                                                                 |
        |---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
        | **Siddhi App Name** | Select the Siddhi Application in which the event stream for which you want to simulate events is defined.                                                   |
        | **Stream Name**     | Select the event stream for which you want to simulate events. All the streams defined in the Siddhi Application you selected are available to be selected. |
        | **Data Source**     | The JDBC URL to be used to access the required database.                                                                                                    |
        | **Driver Class**    | The driver class name of the selected database.                                                                                                             |
        | **Username**        | The username that must be used to access the database.                                                                                                      |
        | **Password**        | The password that must be used to access the database.                                                                                                      |

    6.  Once you have entered the above information, click **Connect to
        Database** . If the datasource is correctly configured, the
        following is displayed to indicate that WSO2 SP can successfully
        connect to the database.  
        ![](attachments/112390893/112390896.png)
    7.  To use the index value as the event timestamp, select the
        **Timestamp Index** option. Then enter the relevant index. If
        you want the vents in the CSV file to be sorted based on the
        timestamp, select the **Yes** option under **CSV File is Ordered
        by Timestamp** .
    8.  To increase the timestamp of the published events, select the
        **Timestamp Interval** option. Then enter the number of
        milliseconds by which you want to increase the timestamp of each
        event.

6.  Click **Save** . This adds the fed simulation you created as an
    active simulation in the **Feed Simulation** tab of the left panel
    as shown below.  
    ![](attachments/112390893/112390895.png)
7.  Click on the play button of this simulation to open the **Run or
    Debug** dialog box.  
    ![](attachments/112390893/112390894.png){height="250"}
8.  If you want to run the Siddhi application you previously selected
    and simulate events for it, select **Run** . If you want to simulate
    events in the **Debug** mode, select **Debug** . Once you have
    selected the required mode, click **Start Simulation** . A message
    appears to inform you that the feed simulation started successfully.
    Similarly, when the simulation is completed, a message appears to
    inform you that the event simulation has finished.

  
