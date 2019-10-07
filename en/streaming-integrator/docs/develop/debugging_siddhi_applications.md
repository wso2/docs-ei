# Debugging Siddhi Applications

WSO2 Streaming Integrator Studio allows debugging tasks to be carried out to
ensure that the Siddhi applications you create and deploy are validated
before they are run on an actual production environment. To debug a
Siddhi application, you can run it in the debug mode, apply debug point
and then run event simulation so that the specific debug points are
analyzed.

To run a Siddhi application in the debug mode, follow the procedure
below:

!!! info
    A Siddhi application can be run in the debug mode only in the source
    view.


1.  Start the Streaming Integrator Studio following the instructions in
    [Starting Stream Integration Studio](streaming-integrator-studio-overview.md#Starting-Streaming-Integration-Studio).  
2.  You are directed to the welcome-page. In this scenario, let's use
    the existing sample Siddhi application named
    `ReceiveAndCount` to demostrate the debugging
    functionality. To open this Siddhi application, click on the
    sample.  
    ![Streaming Integrator Studio Welcome Page](../images/Debugging-Siddhi-Applications/Stream_Processor_Studio_Welcome_Page.png)  
    The `ReceiveAndCount` Siddhi application opens inca new tab.
3.  In order to debug the Siddhi file, you need to first save it in the
    `workspace` directory. To do this, click **File**
    =\> **Save** . In the **Save to Workspace** dialog box that appears,
    click **Save** .
4.  To run the Siddhi application in the debug mode, click **Run** =\>
    **Debug** .
    !!! info
        This menu option is enabled only when the Siddhi file is saved in the `workspace` directory as 
        specified in the previous step.
    
    As a result, the following log is printed in the console.  
    ![Siddhi application successfully started in debug mode](../images/Debugging-Siddhi-Applications/Debug-Mode-Console-Message.png)  
    Also, another console tab is opened with debug options as shown
    below.  
    ![Debug console tab](../images/Debugging-Siddhi-Applications/Console_Tab.png)  

5.  Apply debug points for the required queries. To mark a debug point,
    you need to click on the left of the required line number so that it
    is marked with a dot as shown in the image below.

    ![Marking debug points](../images/Debugging-Siddhi-Applications/Debug-Points.png)

    !!! info
        You can only mark lines with `from` or `insert into` statements as debug points.
    

6.  Simulate one or more events for the `SweetProductionStream` stream in the Siddhi
    application. The first line that is marked as a debug point is highlighted as shown below when
     they are hit.

    !!! info
        For detailed instructions to simulate events, see the following
        sections:
    
        - [Simulating a Single Event](_Simulating_a_Single_Event_)
        - [Simulating Multiple Events via CS Files](_Simulating_Multiple_Events_via_CSV_Files_)
        - [Simulating Multiple Events via Databases](_Simulating_Multiple_Events_via_Databases_)
        - [Generating Random Data](_Generating_Random_Data_)
    
    ![Debug points being hit](../imagess/Debugging-Siddhi-Applications/Hitting-Debug-Points.png) 
    Two viewing options are provided under both **Event State** and the
    **Query State** sections of the **Debug** tab for each debug point
    hit as shown above.

7.  To expand the tree and understand the details of the processed
    attributes  and their values etc., click the following icon for the
    relevant query.  
    ![Expand icon](../images/Debugging-Siddhi-Applications/Expand-Icon.png)

    When you observe the details, note that the value for
    `outputData` in the **Event State** section is
    null. This is because the debug point is still at beginning of the
    query. Also note that the value calculated via the
    `count()` function is still displayed as
    `0` in the **Query State** section.  
    ![Debug results](../images/Debugging-Siddhi-Applications/Debug-Results.png) 
    The following icons are displayed in the **Debug** tab of the
    console:

    <table>
    <thead>
    <tr class="header">
    <th>Icon</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><div class="content-wrapper">
    <p><img src="../../images/Debugging-Siddhi-Applications/Resume.png" /></p>
    </div></td>
    <td>Click this to proceed from the current debug point to the next available debug point. If there is no debug point marked after the current debug point, the existing debug point continues to be displayed in the tab.</td>
    </tr>
    <tr class="even">
    <td><div class="content-wrapper">
    <p><img src="../../images/Debugging-Siddhi-Applications/Step_Over.png" /></p>
    </div></td>
    <td>Click this to proceed from the current debug point even if no debug point exists after it.</td>
    </tr>
    </tbody>
    </table>

      

   Once you navigate to next debug point and see the details by clicking the plus signs as mentioned
    above you can further analyze the processed attributes and its values as shown below. Note that
    after the `count()` aggregate function, a value of 1 has been
    calculated.  
    ![Debugging results](../images/Debugging-Siddhi-Applications/Debug-Results-2.png)
