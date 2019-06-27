# Simulating a Single Event

This section demonstrates how to simulate a single event to be processed
via WSO2 SP.

### Prerequisites

Before simulating events, a Siddhi application should be deployed.

### Simulating an event

To simulate a single event, follow the steps given below.

1.  Access the Stream Processor Studio via the
    `                       http://localhost:/editor                     `
    URL. The Stream Processor Studio opens as shown below.

        !!! info
    
        The default URL is
        `           http://localhost:9090/editor          ` .
    

    ![](attachments/112390871/112390875.png)  
      

2.  Click the **Event Simulator** icon in the left pane of the editor to
    open the **Single Simulation** panel.  
    ![](attachments/112390871/112390874.png){width="50" height="33"}  
    It opens the left panel for event simulation as follows.  
    ![](attachments/112390871/112390873.png)
3.  Enter Information in the **Single Simulation** panel as described
     below.  
    1.  In the **Siddhi App Name** field, select a currently deployed
        Siddhi application.
    2.  In the **Stream Name** field, select the event stream for which
        you want to simulate events. The list displays all the event
        streams defined in the selected Siddhi application.
    3.  If you want to simulate the event for a specific time different
        to the current time, enter a valid timestamp in the
        **Timestamp** field. To select a timestamp, click the time and
        calendar icon next to the **Timestamp** field.  
        ![](attachments/112390871/112390876.png) Then select the
        required date, hour, minute, second and millisecond. Click
        **Done** to select the time stamp entered. If you want to select
        the current time, you can click **Now** .
    4.  Enter values for the attributes of the selected stream.
4.  Click **Send** to start to send the event. The simulated event is
    logged similar to the sample log given below.  
    ![](attachments/112390871/112390872.png){width="900"}
