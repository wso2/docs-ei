# Sending a Simple Message to a Service

## What you'll build

Let’s try a simple scenario where a patient makes an inquiry specifying the doctor's specialization (category) to retrieve a list of doctors that match the specialization. The required information is available in a  back-end microservice. 

To implement this use case, you will create a REST API resource and other artifacts using WSO2 Integration Studio, and then deploy them in the embedded WSO2 Micro Integrator instance. The default API resource will be configured to receive the client request in place of the back-end service, thereby decoupling the client and the back-end service. The response message with the requested doctor details will be routed back to the client through the same API resource.

## Let's get started!

### Step 1: Set up the workspace

- Download the relevant [WSO2 Integration Studio](https://wso2.com/integration/tooling/) based on your operating system. The path to the extracted/installed folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.
- Optionally, you can set up the **CLI tool** for artifact monitoring. This will later help you get details of the artifacts that you deploy in your Micro Integrator.

    1.  Go to the [WSO2 Micro Integrator website](https://wso2.com/integration/#). 
    2.  Click **Download -> Other Resources** and click **CLI Tooling** to download the tool. 
    3.  Extract the downloaded ZIP file. This will be your `MI_CLI_HOME` directory. 
    4.  Export the `MI_CLI_HOME/bin` directory path as an environment variable. This allows you to run the tool from any location on your computer using the `mi` command. Read more about the [CLI tool](../../../administer-and-observe/using-the-command-line-interface).

### Step 2: Develop the integration artifacts

Follow the instructions given in this section to create and configure the required artifacts.

#### Create the project directories

To create the required projects for this integration scenario:

1.  Open **WSO2 Integration Studio**.
2.  Go to **Integration** and click **Create Integration Project**.
    ![](../../assets/img/create_project/create-integration-project.png)

3.  Enter `SampleServices` as the project name and select the following check boxes.
    -   **Create Registry Resources Project**
    -   **Create Composite Application Project**
    -   **Create Connector Exported Project**

    !!! Note
        This will create a **Config** project named `SampleServices` along with the **Registry Resource** project, **Composite Application** project, and a **Connector Exporter** project with the specified names.

    ![](../../assets/img/tutorials/119132413/create-simple-message-project.png)

4.  Click **Finish**. 

    You can see the projects listed in the **Project Explorer** as shown below:

    ![](../../assets/img/tutorials/119132413/project-explorer-simple-service.png)

#### Create an Endpoint

An Endpoint artifact is required for the purpose of exposing the URL that connects to the back-end service.

1. Right-click **SampleServices** in the Project Explorer and navigate to **New -> Endpoint**.
2. Ensure that **Create a New Endpoint** is selected and click **Next**.
3. Enter the information given below to create the new endpoint.  
    <table>
    <thead>
      <tr class="header">
         <th>Property</th>
         <th>Value</th>
         <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr class="odd">
         <td>Endpoint Name</td>
         <td><code>               QueryDoctorEP              </code></td>
         <td>The name of the endpoint.</td>
      </tr>
      <tr class="even">
         <td>Endpoint Type</td>
         <td><code>HTTP Endpoint</code></td>
         <td>Indicates that the back-end service is HTTP.</td>
      </tr>
      <tr class="odd">
         <td>URI Template</td>
         <td>
            <code>http://localhost:9090/healthcare/{uri.var.category}</code>
         </td>
         <td>The template for the request URL expected by the back-end service. In this case, the variable 'category' that needs to be included in the request for querying doctors is represented as <code>{uri.var.category}</code> in the template.</td>
      </tr>
      <tr class="even">
         <td>Method</td>
         <td><code>GET</code></td>
         <td>Indicates that we are creating this endpoint for GET requests that are sent to the back-end service.</td>
      </tr>
      <tr class="odd">
         <td>Static Endpoint</td>
         <td><br/>
         </td>
         <td>Select this option because we are going to use this endpoint only in this <code>SampleServices</code> project and will not reuse it in other projects.</br/></br/> <b>Note</b>: If you need to create a reusable endpoint, save it as a Dynamic Endpoint in either the Configuration or Governance Registry.</td>
      </tr>
      <tr class="even">
         <td>Save Endpoint in</td>
         <td><code>SampleServices</code></td>
         <td>This is the <b>Config</b> project where the artifact will be saved.</td>
      </tr>
     </tbody>
    </table>

    ![](../../assets/img/tutorials/119132413/create-endpoint-artifact.png)

4.  Click **Finish**.  
    The **QueryDoctorEP** endpoint is saved in the `endpoints` folder within the **Config** project you created.  
    ![](../../assets/img/tutorials/119132413/endpoint-project-explorer.png)

#### Create a REST API

A REST API is required for receving the client response and the REST resource within the API will define the mediation logic that will send requests to the Healthcare back-end service and retrieve the available doctor information.

1.  In the Project Explorer, right-click **SampleServices** and navigate to **New -> REST API**.
2.  Ensure **Create A New API Artifact** is selected and click **Next**.
3.  Enter the details given below to create a new REST API.
    <table>
      <tr>
        <th>Property</th>
        <th>Value</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>Name</td>
        <td><code>HealthcareAPI</code></td>
        <td>
          The name of the REST API.
        </td>
      </tr>
      <tr>
        <td>Context</td>
        <td><code>/healthcare </code></td>
        <td>
          Here you are anchoring the API in the <code>/healthcare </code> context. This will become part of the name of the generated URL used by the client when sending requests to Healthcare service. For example, setting the context to /healthcare defines that the API will only handle HTTP requests where the URL path starts with <code>http://host:port/healthcare<code>.
        </td>
      </tr>
      <tr>
        <td>Save location</td>
        <td>
          SampleServices
        </td>
        <td>
          This is the <b>Config</b> project where the artifact will be saved.
        </td>
      </tr>
    </table>
                                                                                                                                                                                                                       |
    ![](../../assets/img/tutorials/119132413/create-rest-api.png)

4.  Click **Finish**.

#### Define the mediation flow 

Once the API resource is created, the design view of the `           HealthcareAPI.xml          ` file will appear as shown below.

!!! Note
    - The top part of the canvas is the **In sequence**, which controls how incoming messages are mediated.
    - The middle part of the canvas is the **Out sequence**, which controls how responses are handled. In this case, a **Send** mediator is already in place to send responses back to the requesting client.
    - The bottom part of the canvas is the **Fault sequence**, which allows you to configure how to handle messages when an error occurs (for more information, see [Error Handling](../../../references/error_handling)).

![](../../assets/img/tutorials/119132413/119132425.png)

You can now start configuring the API resource.

1.  Double-click the **Resource** icon on the left side of the canvas.  
    The properties for the API resource appear on the **Properties** tab at the bottom of the window. If they do not appear, you can right-click the **Resource** icon and click **Show Properties View**.
2.  On the **Properties** tab, provide the following as **Basic** properties:

    <table>
      <tr>
        <th>Property</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>Url Style</td>
        <td>
          Click the respective <b>Value</b> field, click the down arrow, and then select <b>URI_TEMPLATE</b> from the list.
        </td>
      </tr>
      <tr>
        <td>URI-Template</td>
        <td>
          Enter <code>/querydoctor/{category}</code> . This defines the request URL format. In this case, the full request URL format is <code>http://host:port/querydoctor/{category}</code> where <code>{category}</code> is a variable.
        </td>
      </tr>
      <tr>
        <td>Methods</td>
        <td>
          Check if the value of <b>Get</b> is set to true. This defines that the API resource only handles the requests where the HTTP method is GET.
        </td>
      </tr>
    </table>

    ![](../../assets/img/tutorials/119132413/119132424.png)

3.  You can now configure the In sequence to handle requests from the client:

    1.  From the **Mediators** palette, click and drag a **Log** mediator to the In sequence (the top of the canvas).

        !!! Note
            The Log mediator logs messages when the request is received by the In sequence of the API resource. In this scenario, let's configure the Log mediator to display the following message: “Welcome to the HealthcareService”.

    2. With the Log mediator selected, access the **Property** tab and fill in the information in the table below:
        <table>
      <tr class="header">
         <th>Field</th>
         <th>Value</th>
         <th>Description</th>
      </tr>
   <tbody>
      <tr class="odd">
         <td>Log Category</td>
         <td><code>                 INFO                </code></td>
         <td>Indicates that the log contains an informational message.</td>
      </tr>
      <tr class="even">
         <td>Log Level</td>
         <td><code>                 Custom                </code></td>
         <td>When <code>                 Custom                </code> is selected, only specified properties will be logged by this mediator.
         </td>
      </tr>
      <tr class="odd">
         <td>Log Separator</td>
         <td><code>                 (blank)                </code></td>
         <td>Since there is only one property that is being logged, you do not require a separator. Therefore, leave this field blank.</td>
      </tr>
      <tr class="even">
         <td>Properties</td>
         <td><br />
         </td>
         <td>
            <div class="content-wrapper">
               To extract the stock symbol from the request and print a welcome message in the log, click the '+' icon in the <strong>Properties</strong> section, and then add the following values:<br />
               <ul>
                  <li><strong>Name</strong>: <code>Log Property message</code></li>
                  <li><strong>Type</strong>: <code>LITERAL</code><br />
                     (We select LITERAL because the required log message is a static value.)
                  </li>
                  <li><strong>Value/Expression</strong> : <code>"Welcome to HealthcareService"</code></li>
               </ul>
               <p><img src="../../../assets/img/tutorials/119132413/119132423.png"/></p>
            </div>
         </td>
      </tr>
      <tr class="odd">
         <td>Description</td>
         <td><code>Request Log</code></td>
         <td>The <strong>Description</strong> field provides the name that appears for the Log mediator icon in the design view.</td>
        </tr>
        </tbody>
        </table>

        ![](../../assets/img/tutorials/119132413/119132422.png)

    3.  Click **OK** to save the Log mediator configuration.
    4.  Configure the **Send** mediator to send the request message to the `HealthcareService` endpoint.
        1. From the **Mediators** palette, click and drag a **Send** mediator to the In sequence adjoining the Log mediator you added above.
        2. From the **Defined EndPoints** palette, click and drag the **QueryDoctorEP** endpoint, which we created, right next to the empty space of the **Send** mediator.

        ![](../../assets/img/tutorials/119132413/119132421.png)

4.  Configure the Out sequence to send the response from the Healthcare service back to the client. 

    For this, we use a **Send** mediator with no output endpoint defined, which defaults to sending the response back to the requesting client. From the **Mediators** palette, click and drag a **Send** mediator to the Out Sequence (the bottom part of the canvas).  

    ![](../../assets/img/tutorials/119132413/119132420.png)

You have successfully created all the artifacts that are required for sending a request through the Micro Integrator to the back-end service. 

### Step 3: Package the artifacts

Package the artifacts in your composite application project (SampleServicesCompositeApplication project) to be able to deploy the artifacts in the server.

1.  Open the `pom.xml` file in the composite application project POM editor.
2.  Ensure that the following artifacts are selected in the POM file.

    -   `HealthcareAPI`
    -   `QueryDoctorEP`

3.  Save the project.

### Step 4: Build and run the artifacts

To test the artifacts, deploy the [packaged artifacts](#step-3-package-the-artifacts) in the embedded Micro Integrator:

1.  Right-click the composite application project and click **Export Project Artifacts and Run**.
2.  In the dialog that opens, select the composite application project that you want to deploy.  
4.  Click **Finish**. The artifacts will be deployed in the embedded Micro Integrator and the server will start. See the startup log in the **Console** tab.

### Step 5: Test the use case

Let's test the use case by sending a simple client request that invokes the service.

#### Start the back-end service

1. Download the JAR file of the back-end service from [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0-EI7.jar).
2. Open a terminal, navigate to the location where your saved the [back-end service](#step-1-set-up-the-workspace).
3. Execute the following command to start the service:

    ```bash
    java -jar Hospital-Service-2.0.0-EI7.jar
    ```

#### Get details of deployed artifacts (Optional)

Let's use the **CLI Tool** to find the URL of the REST API (that is deployed in the Micro integrator) to which you will send a request.

!!! Tip
    Be sure to set up the CLI tool for your work environment as explained in the [first step](#step-1-set-up-the-workspace) of this tutorial.

1.  Open a terminal and execute the following command to start the tool:
    ```bash
    mi
    ```
    
2.  Log in to the CLI tool. Let's use the server administrator user name and password:
    ```bash
    mi remote login admin admin
    ```

    You will receive the following message: *Login successful for remote: default!*

3.  Execute the following command to find the APIs deployed in the server:
    ```bash
    mi api show
    ```

    You will receive the following information:

    *NAME : HealthcareAPI*            
    *URL  : http://localhost:8290/healthcare* 

Similarly, you can get details of other artifacts deployed in the server. Read more about [using the CLI tool](../../../administer-and-observe/using-the-command-line-interface).

#### Send the client request

Let's send the request to the API. You can use the embedded <b>HTTP Client</b> of WSO2 Integration Studio as follows:

1. Open the <b>HTTP Client</b> of WSO2 Integration Studio.

    !!! Tip
        If you don't see the <b>HTTP Client</b> pane, go to <b>Window -> Show View - Other</b> and select <b>HTTP Client</b> to enable the client pane.

    <img src="../../../assets/img/tutorials/common/http4e-client-empty.png" width="800">
    
2. Enter the request information as given below and click the <b>Send</b> icon (<img src="../../../assets/img/tutorials/common/play-head-icon.png" width="20">).
    
    <table>
        <tr>
            <th>Method</th>
            <td>
               <code>GET</code> 
            </td>
        </tr>
        <tr>
            <th>URL</th>
            <td>
                <code>http://localhost:8290/healthcare/querydoctor/surgery</code></br></br>
            </td>
        </tr>
     </table>
     
     <img src="../../../assets/img/tutorials/119132413/http4e-config.png" width="800">

If you want to send the client request from your terminal:

1. Install and set up [cURL](https://curl.haxx.se/) as your REST client.
2. Execute the following command.
    ```bash
    curl -v http://localhost:8290/healthcare/querydoctor/surgery
    ```

#### Analyze the response

You will see the response message from the HealthcareService with a list of available doctors and the relevant details.

```json
[
  {"name":"thomas collins",
  "hospital":"grand oak community hospital",
  "category":"surgery",
  "availability":"9.00 a.m - 11.00 a.m",
  "fee":7000.0},
  {"name":"anne clement",
   "hospital":"clemency medical center",
   "category":"surgery",
   "availability":"8.00 a.m - 10.00 a.m",
   "fee":12000.0},
  {"name":"seth mears",
   "hospital":"pine valley community hospital",
   "category":"surgery",
   "availability":"3.00 p.m - 5.00 p.m",
   "fee":8000.0}
]
```

Now, check the **Console** tab of WSO2 Integration Studio and you will see the following message:
`INFO - LogMediator message = "Welcome to HealthcareService"`

You have now created and deployed an API resource in the Micro Integrator, which receives requests, logs a message using the Log mediator, sends the request to a back-end service using the Send mediator, and returns a response to the requesting client.
