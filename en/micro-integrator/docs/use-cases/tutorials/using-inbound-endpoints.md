# Using Inbound Endpoints

## What you'll build

In this sample scenario, you will use an **Inbound Endpoint** to expose an already defined REST API through a different port. You can reuse the REST API that was defined in the [Sending a Simple Message to a Service](integration/sending-a-simple-message-to-a-service.md) tutorial. See [Creating an Inbound Endpoint](../../develop/creating-artifacts/creating-an-inbound-endpoint.md) for details on how to work with inbound endpoints using WSO2 Integration Studio.

## Let's get started!

### Step 1: Set up the workspace

To set up the tools:

-   Go to the [product page](https://wso2.com/integration/) of **WSO2 Micro Integrator**, download the product installer and run it to set up the product.
-   Select the relevant [WSO2 Integration Studio](https://wso2.com/integration/tooling/) based on your operating system and extract the
    ZIP file.  The path to this folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.
-   Download the CLI Tool for monitoring artifact deployments.

To set up the previous artifacts:

1.  If you did not try the [Sending a Simple Message to a Service](integration/sending-a-simple-message-to-a-service.md) tutorial yet, open WSO2 Integration Studio, click **File**, and click **Import**. Next, expand the **WSO2** category and select **Existing WSO2 Projects into workspace**, click **Next** and upload the [pre-packaged
project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/SimpleMessageToServiceTutorial.zip). 
2.  Download the JAR file of the back-end service from [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0-EI7.jar).

### Step 2: Develop the Inbound Endpoint

1.  Once you have exported the ESB project as described in above, the
    project directory will appear with the artifacts as shown below.
    Note the 'HealthcareAPI' that is already included.

    ![](/assets/img/tutorials/inbound-project-explorer.png)

2.  Right-click on **SampleServices** and navigate to **New -> Inbound
    Endpoint**. Select **Create A New Inbound Endpoint** and click
    **Next**.

3.  Enter the following details and click **Finish**.
    
    <table>
        <tr>
            <th>Parameter</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Inbound Endpoint Name</td>
            <td>
                QueryDoctorInboundEndpoint
            </td>
        </tr>
        <tr>
            <td>Inbound Endpoint Creation Type</td>
            <td>
                HTTP 
            </td>
        </tr>
    </table>

    ![](https://lh3.googleusercontent.com/CYLJoSvCMhZfVYSZc73iRyAHhzgVWwjCqfkNgjPDlVs2qAs6QhsbDKt8mbIzEk8ojpONkEl2nemszzeNLPSAW3ogSs0eHqbGQMmw7WSlhx3b3Nbvfp0xGJ2Xbwl-Qbi0NxMGrSJB)

4.  Go to the **Properties** tab in the **Design** view and enter the following:

    <table>
        <tr>
            <th>Parameter</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Inbound HTTP port</td>
            <td>
                8285
            </td>
        </tr>
        <tr>
            <td>Dispatch Filter Pattern</td>
            <td>
                /healthcare/querydoctor/.\*
            </td>
        </tr>
    </table>

    ![](https://lh5.googleusercontent.com/KUBGiYXSzSVkZo_mDb0y9yzGFp6Fts7FdUrvrH_QlvpGDtaTiwnivjvCsMBpzhGDyRrJvCeBysYQNBFL3ndpEXwUB-U5TDsBbNS2actK3_8ie2RWULV6-g1LY3Q9XWaWOHsZNc7O)

The endpoint will now get mapped to any URL that matches the above pattern provided. You will be exposing the health care API on a new port
through this inbound endpoint.

### Step 3: Package the artifacts

Package the artifacts in your composite application project (SampleServicesCompositeApplication project) to be able to deploy the artifacts in the server.

1.  Open the `pom.xml` file in the composite application project POM editor.
2.  Ensure that the following artifacts are selected in the POM file.

    -   `HealthcareAPI`
    -   `QueryDoctorInboundEndpoint`

3.  Save the project.

### Step 4: Build and run the artifacts

To test the artifacts, deploy the [packaged artifacts](#step-3-package-the-artifacts) in the embedded Micro Integrator:

1.  Right-click the composite application project and click **Export Project Artifacts and Run**.
2.  In the dialog that opens, select the composite application project that you want to deploy.  
4.  Click **Finish**. The artifacts will be deployed in the embedded Micro Integrator and the server will start. See the startup log in the **Console** tab. 

### Step 5: Test the use case

Let's test the use case by sending a simple client request that invokes the service.

#### Start the backend service

First, start the back-end service.

#### Send the client request

Let's use the **CLI Tool** to find the URL of the REST API that is deployed in the Micro Integrator:

1.  Open a terminal and navigate to the `CLI_HOME` directory.
2.  Execute the following command to start the tool:
    `./mi`
3.  Execute the following command to find the APIs deployed in the server:
    `mi show api`

Let's send a message to the **healthcare** REST API on the 8285 port.

1.  Open a command line terminal and execute the following command:

    ```
    curl -v http://localhost:8285/healthcare/querydoctor/surgery
    ```

2.  You will get the response shown below. The inbound endpoint has successfully invoked the REST API, and further, the response received by the REST API has been routed back to client through the inbound endpoint.

    ``` 
    [{"name":"thomas collins","hospital":"grand oak community 
    hospital","category":"surgery","availability":"9.00 a.m - 11.00 a.m","fee":7000.0},
    {"name":"anne clement","hospital":"clemency medical center","category":"surgery","availability":"8.00 a.m - 10.00 A.m","fee":12000.0},
    {"name":"seth mears","hospital":"pine valley community hospital","category":"surgery","availability":"3.00 p.m - 5.00 p.m","fee":8000.0}]
    ```
