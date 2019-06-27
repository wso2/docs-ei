# Simulating Multiple Events via CSV Files

This section explains how to generate multiple events via CSV files to
be analyzed via WSO2 SP.

### Prerequisites

Before simulating events, a Siddhi application should be deployed.

### Simulating events

To simulate multiple events from a CSV file, follow the steps given
below.

1.  Access the Stream Processor Studio via the
    `                       http://localhost:/editor                     `
    URL. The Stream Processor Studio opens as shown below.

        !!! info
    
        The default URL is
        `                       http://localhost:9090/editor                     `
    

    ![](attachments/112390880/112390886.png)

2.  Click the **Event Simulator** icon in the left pane of the editor.  
    ![](attachments/112390880/112390885.png){width="50"}
3.  In the event simulation left panel that opens, click on the **Feed
    Simulation** tab.  
    ![](attachments/112390880/112390884.png)  
      
4.  To create a new simulation, click **Create** . This opens the
    following panel.  
    ![](attachments/112390880/112390883.png){height="250"}
5.  Enter values for the displayed fields as follows.
    1.  In the **Simulation Name** field, enter a name for the event
        simulation.
    2.  In the **Description** field, enter a description for the event
        simulation.
    3.  If you want to receive events only during a specific time
        interval, enter that time interval in the **Time Interval**
        field.
    4.  Click **Advanced Configurations** if you want to enter detailed
        specifications to filter events from the CSV file. Then enter
        information as follows.
        1.  If you want to include only events that belong to a specific
            time interval in the simulation feed, enter the start time
            and the end time  in the **Starting Event's Timestamp** and
            **Ending Event's Timestamp** fields respectively. To select
            a timestamp, click the time and calendar icon next to the
            field.  
            ![](attachments/112390880/112390889.png)  
            Then select the required date, hour, minute, second and
            millisecond. Click **Done** to select the time stamp
            entered. If you want to select the current time, you can
            click **Now** .
        2.  If you want to restrict the event simulation feed to a
            specific number of events, enter the required number in the
            **No of Events** field.
    5.  In the **Simulation Source** field, select **CSV File** .
    6.  Click **Add Simulation Source** to open the following section.  
        ![](attachments/112390880/112390882.png)  
        In the **Siddhi App Name** field, select the required Siddhi
        application. Then more fields as shown below.  
        ![](attachments/112390880/112390881.png)  
        Enter details as follows:
        1.  In the **Stream Name** field, select the stream for which
            you want to simulate events. All the streams defined in the
            Siddhi App you selected are available in the list.
        2.  In the **CSV File** field, select an available CSV file. If
            no CSV files are currently uploaded, select **Upload File**
            from the list. This opens the **Upload File** dialog box.  
            ![](attachments/112390880/112390887.png)  
            Click **Choose File** and browse for the CSV file you want
            to upload. Then click **Upload** .
        3.  In the **Delimiter** field, enter the character you want to
            use in order to separate the attribute values in each row of
            the CSV file.
        4.  If you want to enter more detailed specificiations, click
            **Advanced Configuration** . Then enter details as follows.
            1.  To use the index value as the event timestamp, select
                the **Timestamp Index** option. Then enter the relevant
                index.
            2.  If you want to increase the value of the timestamp for
                each new event, select the **Increment event time
                by(ms)** option. Then enter the number of milliseconds
                by which you want to increase the timestamp of each
                event.
            3.  If you want the events to arrive in order based on the
                timestamp, select **Yes** under the **Timestamp
                Interval** option.
        5.  Click **Save** to save the information relating to the CSV
            file. The name os the CSV file appears in the **Feed
            Simulation** tab in the left panel.
6.  To simulate a CSV file that is uploaded and visible in the **Feed
    Simulation** tab in the left panel, click on the arrow to its right.
    The simulated events are logged n the output console.
