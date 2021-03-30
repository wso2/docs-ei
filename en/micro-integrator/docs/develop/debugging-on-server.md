# Debugging Class Mediators

You can use the debugging features in WSO2 Integration Studio to troubleshoot errors in the Class mediator.

You can use the following two methods (same as for debugging mediations):

-   Instant debugging using the embedded Micro Integrator of WSO2 Integration Studio.
-   Deploy artifacts to an external Micro Integrator server and debug.

Above two approaches are discussed in detail below.

## Using the embedded Micro Integrator

1.  Open the `pom.xml` of the <b>Composite Application Project</b> and check that all the modules, including the Class Mediator module, are added to the CApp project.

    <img src="../../assets/img/debug-on-server/add-class-mediator.png" width="700" alt="add class mediators to capp">

2.  Select the Composite Application project and click **Run** -> **Debug As** -> **Debug on Server**.

    <img src="../../assets/img/debug-on-server/select-debug-on-server.png" width="700" alt="select debug on server">

3.  In the <b>Debug On Server </b> dialog box that opens, select the required server and click **Next**.

    <img src="../../assets/img/debug-on-server/select-server.png" width="500" alt="select debugging">

4.  Add the CAR file with the artifacts that need to be deployed to the embedded Micro Integrator and click **Finish**.

    !!! Note
        Note that the Micro Integrator is started with the artifacts deployed.</br>
        HTTP traffic is listened on the 8290 port.

    <img src="../../assets/img/debug-on-server/add-artifacts.png" width="500" alt="add artifacts to server">

5.  Let's add breakpoints to the Java class of the class mediator.

    1.  Open the Java class used in the class mediator.
    2.  Right-click the required point in the code and click <b>Add breakpoint</b>.

        <img src="../../assets/img/debug-on-server/add-breakpoints.png" width="700" alt="add breakpoints">

6.  Invoke the service using the inbuilt HTTP client or some external client. 

    As soon as a request comes to the class mediator, the first break point will be triggered.

    <img src="../../assets/img/debug-on-server/debug-class-mediator-view-properties.png" width="700" alt="select debugging">
    
7.  Hover over the breakpoint to view the message context that comes into the mediator.

8.  If you get the "**Source not found**" error, follow the instructions below to add the class mediator source.
        
    1.  Click **Edit Source Lookup Path** in the <b>Source not found</b> window that appears.
    2.  Click **Add** in the <b>Edit Source Lookup Path</b> dialog box that appears.
    3.  Select <b>Java Project</b> in the <b>Add source</b> dialog box.
    4.  Select your mediator project from the <b>Project Selection</b> dialog box and click **ok**.

9.  Click **Continue** to resume the message flow.

## Using a remote Micro Integrator

Follow the steps below to debug custom class mediators.

1.  Open the `pom.xml` of the <b>Composite Application Project</b> and check that all the modules, including the Class Mediator module, are added to the CApp project.

    <img src="../../assets/img/debug-on-server/add-class-mediator.png" width="700" alt="add class mediators to capp">

2.  Select the Composite Application project and click **Run** -> **Debug Configurations..**.

3.  In the <b>Debug Configurations</b> window that opens, follow the instructions below. 

    1.  Double-click <b>Remote Java Application</b> to create a new connection.

        <img src="../../assets/img/debug-on-server/run-debug-configs.png" width="700" alt="select debug configurations">

    2.  Provide the following details related to the external Micro Integrator.

        <table>
            <tr>
                <th>
                    Name
                </th>
                <td>
                    Give a name to identify the external Micro Integrator server.
                </td>
            </tr>
            <tr>
                <th>
                    Project
                </th>
                <td>
                    Click <b>Browse</b> and select the class mediator project from the workspace.
                </td>
            </tr>
            <tr>
                <th>
                    Host
                </th>
                <td>
                    Specify the host on which the external Micro Integrator will be running.
                </td>
            </tr>
            <tr>
                <th>
                    Port
                </th>
                <td>
                    Specify the port on which the external Micro Integrator will be running.
                </td>
            </tr>
        </table>

        <img src="../../assets/img/debug-on-server/add-java-remote-server.png" width="700" alt="add external Micro Integrator connection details">

    3.  Click <b>Apply</b>.
      
4.  Optinally, you can add the new configuration to the <b>Debug</b> menu as shown below and click <b>Apply</b>. 

    !!! Info
        You can then access the configuation easily. 

    <img src="../../assets/img/debug-on-server/add-java-remote-server-to-fav.png" width="700" alt="add configuration to debug menu"> 
      
5.  Execute the following command to start the external Micro Integrator server in debug mode:  

    !!! Note
        Be sure to pass the same port number (which you used in step 2 above) as a system variable.

    ```bash tab='On Windows'
    MI_HOME\bin\micro-integrator.bat --run -debug 8000
    ```

    ```bash tab='On Linux/Solaris'
    sh MI_HOME/bin/micro-integrator.sh -debug 8000
    ```

    You will receive the following printed on the terminal:

    <img src="../../assets/img/debug-on-server/java-remote-server-waiting.png" width="700" alt="java remote server waiting - message">

6.  Go back to WSO2 Integration Studio, click the **down** arrow next to **Debug**, and then select the new server configuration that you have added.

    <img src="../../assets/img/mediation-debugging/debugging-11.png" width="400" alt="select new configuration">

7.  In WSO2 Integration Studio, right-click and add breakpoints on the desired class mediators to start debugging as shown in the example below.

    <img src="../../assets/img/debug-on-server/add-breakpoints.png" width="700" alt="add breakpoints">

Now you can send a request to the external Micro Integrator and debug the class mediators as discussed under "Instant debugging using Micro Integrator".
