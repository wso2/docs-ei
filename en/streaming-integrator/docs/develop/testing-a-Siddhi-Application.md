# Testing a Siddhi Application

WSO2 Stream Processor Studio allows the following tasks to be carried
out to ensure that the Siddhi applications you create and deploy are
validated before they are run in an actual production environment.

-   Validate Siddhi applications that are written in the Stream
    Processor Studio.
-   Run Siddhi applications that were written in the Stream Processor
    Studio in either Run or Debug mode.
-   Simulate events to test the Siddhi applications and analyze events
    that are received and sent. This allows you to analyze the status of
    each query within a Siddhi application at different execution
    points.

### Validating a Siddhi application

To validate a Siddhi application, follow the procedure below:

1.  Start and access the Stream Processor Studio. For detailed
    instructions, see [Starting Stream Processor
    Studio](Stream-Processor-Studio-Overview_112390916.html#StreamProcessorStudioOverview-StartingStreamProcessorStudio)
    .
2.  In this example, let's use an existing sample as an example. Click
    on the **ReceiveAndCount** sample to open it.
3.  Sample opens in a new tab. This sample does not have errors, and
    therefore, no errors are displayed in the editor. To create an error
    for demonstration purposes, change the
    `           count()          ` function in the
    `           query1          ` query
    `           tocountNew()          ` as shown below.

    `             @info(name='query1') from SweetProductionStream select                           countNew()                          as totalCount insert into TotalCountStream;            `

    Now, the editor indicates that there is a syntax error. If you move
    the cursor over the error icon, it indicates that
    `           countNew          ` is an invalid function name as shown
    below.  
    ![](attachments/112390856/112390865.png){height="250"}

### Running or debugging a Siddhi application

You can run or debug a siddhi application to verify whether the logic
you have written is correct. To start a Siddhi application in the
run/debug mode, follow the procedure below:

1.  Start and access the Stream Processor Studio. For detailed
    instructions, see [Starting Stream Processor
    Studio](Stream-Processor-Studio-Overview_112390916.html#StreamProcessorStudioOverview-StartingStreamProcessorStudio)
    .
2.  For this example, click the **existing** sample **ReceiveAndCount**
    . It opens in a new untitled tab.
3.  Save the Siddhi file sothat you can run it in the Run or Debug mode.
     To save it, click **File** =\> **Save** .  Once the file is saved,
    you can see the **Run** and **Debug** menu options enables as shown
    below.
4.  Go to **File** and click **Save** . (Default saved in default
    workspace folder). Once saved you can see the above options are now
    enabled.  
    ![](attachments/112390856/112390859.png){height="250"}
    1.  To start the application in Run mode, click **Run** =\> **Run**
        . This logs the following output in the console.  
        ![](attachments/112390856/112390864.png){height="250"}
    2.  Start the application in the Debug mode, click **Run** =\>
        **Debug** . As a result, the following mesage is logged in the
        console. You can also note that another console tab is opened
        with debug options.  
        ![](attachments/112390856/112390863.png){width="913"}
5.  To create an error for demonstration purposes, change the
    `          count()         ` function in the
    `          query1         ` query to `          countNew()         `
    , and save. Then click **Run** =\> **Run** . As a result, the
    following output is logged in the console.  
    ![](attachments/112390856/112390858.png)

### Simulating events

This section demonstrates how to test a Siddhi application via event
simulation. There are multiple methods to simulate events. In this
example, you can use single simulation. For more information about all
available methods, see [Simulating Events](_Simulating_Events_) .

To simulate a single event in order to check the status of your Siddhi
queries, follow the procedure below:

!!! tip

Before you simulate events for a Siddhi application, you need to run or
debug it. Therefore, before you try this section, see [Run or debug a
Siddhi application](#TestingaSiddhiApplication-RunorDebug) .


1.  Run the `           ReceiveAndCount          ` Siddhi application.

2.  Click the following icon for event simulation.  
    ![](attachments/112390856/112390862.png){width="50"}  
    It opens the left panel for event simulation as follows.  
    ![](attachments/112390856/112390857.png)
3.  In the **Siddhi App Name** field, select the name of the application
    for which you want to simulate events. For this example, select
    **ReceiveAndCount** .
4.  In the **Stream Name** field, select the stream defined in the
    Siddhi application for which you want to simulate events. For this
    example, select **SweetProductionStream** .
5.  Enter values for the attributes of the selected stream that appear
    in the pane, and click **Send** .
6.  In this Siddhi application, you have a sink of the log
    type connected to the `          TotalCountStream         ` output
    stream. Therefore, once the events are received at the input stream,
    a log ise printed in the output console as shown below.  
    ![](attachments/112390856/112390861.png){width="913"}
