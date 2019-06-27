# Generating Random Data

This section explains how to generate random data to be analyzed via
WSO2 SP.

### Prerequisites

Before simulating events, a Siddhi application should be deployed.

### Simulating events

To simulate random events, follow the steps given below:

1.  Access the Stream Processor Studio via the
    `                       http://localhost:/editor                     `
    URL. The Stream Processor Studio opens as shown below.

        !!! info
    
        The default URL is
        `                       http://localhost:9090/editor                     `
    

    ![](attachments/112390905/112390911.png){height="250"}

2.  Click the **Event Simulator** icon in the left pane of the editor.  
    ![](attachments/112390905/112390912.png)
3.  Click the **Feed** tab to open the **Feed Simulation** panel.  
    ![](attachments/112390905/112390913.png)
4.  To create a new simulation, click **Create** . This opens the
    following panel.  
    ![](attachments/112390905/112390914.png)
5.  Enter values for the displayed fields as follows.
    1.  In the **Simulation Name** field, enter a name for the event
        simulation.
    2.  In the **Description** field, enter a description for the event
        simulation.
    3.  If you want to include only events that belong to a specific
        time interval in the simulation feed, enter the start time and
        the end time  in the **Starting Event's Timestamp** and **Ending
        Event's Timestamp** fields respectively. To select a timestamp,
        click the time and calendar icon next to the field.  
        ![](attachments/112390905/112390915.png)  
        Then select the required date, hour, minute, second and
        millisecond. Click **Done** to select the time stamp entered. If
        you want to select the current time, you can click **Now** .
    4.  If you want to restrict the event simulation feed to a specific
        number of events, enter the required number in the **No of
        Events** field.
    5.  If you want to receive events only during a specific time
        interval, enter that time interval in the **Time Interval**
        field.
    6.  In the **Simulation Source** field, select **Random** .
    7.  If the random simulation source from which you want to simulate
        events does not already exist in the **Feed Simulation** pane,
        click **Add New** to open the following section.  
        ![](attachments/112390905/112390910.png)
    8.  Enter information relating to the random source as follows:

        1.  In the **Siddhi App Name** field, s elect the name of the
            Siddhi App with the event stream for which the events are
            simulated.
        2.  In the **Stream Name** field, select the event stream for
            which you want to simulate events. All the streams defined
            in the Siddhi Application you selected are available to be
            selected.
        3.  In the **Timestamp Interval** field, enter the number of
            milliseconds by which you want to increase the timestamp of
            each event.
        4.  To enter values for the stream attributes, follow
            the instructions below.
            -   To enter a custom value for a stream attribute, select
                **Custom data based** from the list. When you select
                this value, data field in which the required value can
                be entered appears as shown in the example below.  
                ![](attachments/112390905/112390909.png){width="235"
                height="88"}
            -   To enter a primitive based value, select **Primitive
                based** from the list. The information to be entered
                varies depending on the data type of the attribute. The
                following table explains the information you need to
                enter when you select **Primitive based** for each data
                type.  

                <table>
                <thead>
                <tr class="header">
                <th>Data Type</th>
                <th>Values to enter</th>
                </tr>
                </thead>
                <tbody>
                <tr class="odd">
                <td><code>                     STRING                    </code></td>
                <td>Specify a length in the <strong>Length</strong> field that appears. This results in a value of the specified length being auto-generated.</td>
                </tr>
                <tr class="even">
                <td><code>                     FLOAT                    </code> or <code>                     DOUBLE                    </code></td>
                <td>The value generated for the attribute is based on the following values specified.
                <ul>
                <li><strong>Min</strong> : The minimum value.</li>
                <li><strong>Max</strong> : The maximum value.</li>
                <li><strong>Precision</strong> : The precise value. The number of decimals included in the auto-generated values are the same as that of the value specified here.</li>
                </ul></td>
                </tr>
                <tr class="odd">
                <td><code>                     INT                    </code> or <code>                     LONG                    </code></td>
                <td>The value generated for the attribute is based on the following values specified.
                <ul>
                <li><strong>Min</strong> : The minimum value.</li>
                <li><strong>Max</strong> : The maximum value.</li>
                </ul></td>
                </tr>
                <tr class="even">
                <td><code>                     BOOL                    </code></td>
                <td>No further information is required because <code>                     true                    </code> and <code>                     false                    </code> values are randomly generated.</td>
                </tr>
                </tbody>
                </table>

                  

            -   To randomly assign values based on a pre-defined set of
                meaningful values, select **Property based** from the
                list. When you select this value, a field in which the
                set of available values are listed appears as shown in
                the example below.  
                ![](attachments/112390905/112390908.png)
            -   To assign a regex value, select **Regex based** from the
                list.

        5.  To enter values for the stream attributes, follow
            the instructions below.
            -   To enter a custom value for a stream attribute, select
                **Custom data based** from the list. When you select
                this value, data field in which the required value can
                be entered appears as shown in the example below.  
                ![](attachments/112390905/112390909.png){width="235"
                height="88"}
            -   To enter a primitive based value, select **Primitive
                based** from the list. The information to be entered
                varies depending on the data type of the attribute. The
                following table explains the information you need to
                enter when you select **Primitive based** for each data
                type.  

                <table>
                <thead>
                <tr class="header">
                <th>Data Type</th>
                <th>Values to enter</th>
                </tr>
                </thead>
                <tbody>
                <tr class="odd">
                <td><code>                     STRING                    </code></td>
                <td>Specify a length in the <strong>Length</strong> field that appears. This results in a value of the specified length being auto-generated.</td>
                </tr>
                <tr class="even">
                <td><code>                     FLOAT                    </code> or <code>                     DOUBLE                    </code></td>
                <td>The value generated for the attribute is based on the following values specified.
                <ul>
                <li><strong>Min</strong> : The minimum value.</li>
                <li><strong>Max</strong> : The maximum value.</li>
                <li><strong>Precision</strong> : The precise value. The number of decimals included in the auto-generated values are the same as that of the value specified here.</li>
                </ul></td>
                </tr>
                <tr class="odd">
                <td><code>                     INT                    </code> or <code>                     LONG                    </code></td>
                <td>The value generated for the attribute is based on the following values specified.
                <ul>
                <li><strong>Min</strong> : The minimum value.</li>
                <li><strong>Max</strong> : The maximum value.</li>
                </ul></td>
                </tr>
                <tr class="even">
                <td><code>                     BOOL                    </code></td>
                <td>No further information is required because <code>                     true                    </code> and <code>                     false                    </code> values are randomly generated.</td>
                </tr>
                </tbody>
                </table>

                  

            -   To randomly assign values based on a pre-defined set of
                meaningful values, select **Property based** from the
                list. When you select this value, a field in which the
                set of available values are listed appears as shown in
                the example below.  
                ![](attachments/112390905/112390908.png)
            -   To assign a regex value, select **Regex based** from the
                list.

6.  Click **Save** to save the simulation information.  The saved random
    simulation appears in the **Feed** tab of the left panel.
7.  To simulate events, click the arrow to the right os the saved
    simulation (shown in the example below).  
    ![](attachments/112390905/112390906.png)  
    The simulated events are logged in the CLI as shown in the extract
    below.  
    ![](attachments/112390905/112390907.png)  
8.  Click **Save** to save the simulation information.  The saved random
    simulation appears in the **Feed** tab of the left panel.
9.  To simulate events, click the arrow to the right os the saved
    simulation (shown in the example below).  
    ![](attachments/112390905/112390906.png)  
    The simulated events are logged in the CLI as shown in the extract
    below.  
    ![](attachments/112390905/112390907.png)  
