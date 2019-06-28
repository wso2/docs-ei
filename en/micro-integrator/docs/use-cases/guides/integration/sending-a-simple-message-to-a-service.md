# Sending a Simple Message to a Service

Let’s try a simple scenario where a patient makes an inquiry specifying
the doctor's specialization (category) to retrieve a list of doctors
that match the specialization. The required information is available in
a  backend micro service deployed in the [MSF4J
profile](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-msf4j)
of WSO2 Enterprise Integrator (WSO2 EI). We configure an API resource in
the ESB profile of WSO2 EI that receives the client request, instead of
the client sending messages directly to the back-end service, thereby
decoupling the client and the back-end service.

  

**In this tutorial** , you create a REST API in ESB profile to connect
to a REST back-end service (microservice) that is defined as an HTTP
endpoint in the ESB. If you want to see how information stored in a
database (instead of a microservice) can be used, see the tutorial on
[sending a simple message to
a datasource](_Sending_a_Simple_Message_to_a_Datasource_) .

  

![](attachments/119132413/119132450.png){height="250"}

!!! tip

**Before you begin** ,

-   Install Oracle Java SE Development Kit (JDK) version 1.8.\* and set
    the `          JAVA_HOME         ` environment variable.
-   Go to the [product page](https://wso2.com/integration/) of **WSO2
    Enterprise Integrator** , select **Other Installation Options** ,
    and download the **Binary** distribution . Extract the ZIP file of
    the binary. This will be your `          <EI_HOME>         `
    directory.
-   Select and download the relevant WSO2 Integration Studio ZIP file
    based on your operating system from
    [here](https://wso2.com/integration/tooling/) and then extract the
    ZIP file.  
    The path to this folder is referred to as
    `           <EI_TOOLING>          ` throughout this tutorial.

    !!! info

    Getting an error message? See the troubleshooting tips given under
    [Installing WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio#InstallingWSO2IntegrationStudio-Troubleshooting)
    .


-   Download the MSF4J service from
    [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0.jar)
    and copy the JAR file to the
    `          <EI_HOME>/wso2/msf4j/deployment/microservices         `
    folder. The back-end service is now deployed in the MSF4J profile of
    WSO2 EI.


**Let's get started!**

This tutorial includes the following sections:

-   [Creating the message mediation
    artifacts](#SendingaSimpleMessagetoaService-Creatingthemessagemediationartifacts)
    -   [Creating the deployable artifacts
        project](#SendingaSimpleMessagetoaService-Creatingthedeployableartifactsproject)
    -   [Connecting to the back-end
        service](#SendingaSimpleMessagetoaService-Connectingtotheback-endservice)
    -   [Mediating requests to the back-end
        service](#SendingaSimpleMessagetoaService-Mediatingrequeststotheback-endservice)
-   [Packaging the
    artifacts](#SendingaSimpleMessagetoaService-Packagingtheartifacts)
-   [Starting the ESB profile and deploying the
    artifacts](#SendingaSimpleMessagetoaService-StartingtheESBprofileanddeployingtheartifacts)
-   [Starting the MSF4J
    profile](#SendingaSimpleMessagetoaService-StartingtheMSF4Jprofile)
-   [Sending requests to the
    ESB](#SendingaSimpleMessagetoaService-SendingrequeststotheESB)

### Creating the message mediation artifacts

Requests going through the ESB profile of WSO2 EI are called messages,
and [message
mediation](https://docs.wso2.com/display/EI650/ESB+Mediators) is a
fundamental part of any ESB. In this section, we configure the message
mediation process in the ESB profile that routes all received messages
to the back-end service (healthcare service). We use the Eclipse-based
WSO2 Integration Studio to create the message mediation artifacts and
then deploy them to the ESB profile of WSO2 EI.

!!! note

See the following topics for a description of the **concepts** that you
need to know when creating ESB artifacts:

-   [REST
    API](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-restAPI)
-   [Endpoints](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Messageexitpoints)
-   [Sequences  
    ](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Sequences)


#### Creating the deployable artifacts project

Follow the steps given below to create an ESB solution project.

1.  Open **WSO2 Integration Studio** .

2.  Go to **ESB Project** and click **Create New** .

    ![](attachments/119132413/119132414.png){width="800" height="384"}

3.  Enter `           SampleServices          ` as the project name. Be
    sure to select the following check boxes so that the relevant
    projects will be created.

    -   **Create Registry Resources Project**
    -   **Create Connector Exporter Project**
    -   **Create Composite Application Project**

    ![](attachments/119132413/119132430.png){width="550" height="650"}

4.  Click **Finish** .  
    You have now created the following projects as shown in the Project
    Explorer:

    ![](attachments/119132413/119132429.png){width="350" height="127"}

Next, we create an endpoint inside the SampleServices ESB Solution
project that will allow the ESB to connect to the back-end service.

#### Connecting to the back-end service

To connect to the back-end service, we must expose a URL that can be
used to connect to the service. To do this, we will create an endpoint
for this service.

!!! info

The sample back-end service (i.e. the
`                   Hospital-Service-2.0.0.jar                 ` file)
we are using in this tutorial is a sample [MSF4J
service](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-msf4j)
.


1.  Right-click **SampleServices** in the Project Explorer and navigate
    to **New -\> Endpoint** .

2.  Ensure **Create a New Endpoint** is selected and click **Next** .  

3.  Enter the information given below to create the new endpoint.  

    <table>
    <thead>
    <tr class="header">
    <th>Field</th>
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
    <td><code>               HTTP Endpoint              </code></td>
    <td>Indicates that we are connecting to REST back-end service.</td>
    </tr>
    <tr class="odd">
    <td>URI Template</td>
    <td><p><code>                                                   http://localhost:9090/healthcare/{uri.var.category                                                 }               </code></p></td>
    <td>The template for the request URL expected by the MSF4J service deployed in the MSF4J profile. In this case, the variable 'category' that needs to be included in the request for querying doctors, is represented as <code>               {uri.var.category}              </code> in the template.</td>
    </tr>
    <tr class="even">
    <td>Method</td>
    <td><code>               GET              </code></td>
    <td>Indicates that we are creating this endpoint for GET requests that are sent to the back-end service.</td>
    </tr>
    <tr class="odd">
    <td>Static Endpoint</td>
    <td><br />
    </td>
    <td>Select this option because we are going to use this endpoint only in this ESB Solution project and will not re-use it in other projects. If you need to create a reusable endpoint, you create it as a Dynamic Endpoint and save the endpoint in either the Configuration or Governance Registry. For more information, see the documentation on <a href="https://docs.wso2.com/display/ADMIN44x/Working+with+the+Registry">registries</a> .</td>
    </tr>
    <tr class="even">
    <td>Save Endpoint in</td>
    <td><code>               SampleServices              </code></td>
    <td>This is the ESB Solution project we created in the last section</td>
    </tr>
    </tbody>
    </table>

    ![](attachments/119132413/119132428.png){width="550" height="506"}

4.  Click **Finish** .  
    The QueryDoctorEP endpoint is saved in the
    `           endpoints          ` folder within the ESB
    Solution Project you created.  
    ![](attachments/119132413/119132427.png){width="363" height="330"}

Now that you have created the endpoint for the back-end service, it’s
time to create the REST API and the relevant API resource that will
receive requests from client applications, mediate them and send them to
the endpoint, and return the results to the client.

#### Mediating requests to the back-end service

We use WSO2 Integration Studio to create a REST API named
`         HealthcareAPI        ` . Next, we create a resource within
this API for the GET HTTP method that is used to send requests to the
Healthcare back-end service and retrieve available doctor information.

1.  In the Project Explorer, right-click **SampleServices** and navigate
    to **New -\> REST API** .

2.  Ensure **Create A New API Artifact** is selected and click **Next.**

3.  Enter the details given below to create a new REST API.

    | Field         | Value                                         | Description                                                                                                                                                                                                                                                                                                                                                                                                        |
    |---------------|-----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Name          | `               HealthcareAPI              `  | The name of the REST API in WSO2 EI.                                                                                                                                                                                                                                                                                                                                                                               |
    | Context       | `               /healthcare              `    | Here we are anchoring the API in the `               /healthcare              ` context. This will become part of the name of the generated URL used by the client when sending requests to Healthcare service. For example, setting the context to /healthcare defines that the API will only handle HTTP requests where the URL path starts with `               http://<host>:<port>/healthcare.              ` |
    | Save location | `               SampleServices              ` | This is the ESB Solution project we have already created previously.                                                                                                                                                                                                                                                                                                                                               |

    ![](attachments/119132413/119132426.png){width="500" height="455"}

4.  Click **Finish** .  
    Once the API resource is created, the design view of the
    `           HealthcareAPI.xml          ` file will appear as shown
    below. You can now start configuring the API resource.

        !!! note
    
        -   The top part of the canvas is the **In sequence** , which
            controls how incoming messages are mediated.
        -   The middle part of the canvas is the **Out sequence** , which
            controls how responses are handled. In this case, a Send
            mediator is already in place to send responses back to the
            requesting client.
        -   The bottom part of the canvas is the **Fault sequence** , which
            allows you to configure how to handle messages when an error
            occurs (for more information, see [Error
            Handling](https://docs.wso2.com/display/EI650/Error+Handling) ).
    

    ![](attachments/119132413/119132425.png){width="1000" height="428"}

5.  Double-click the **Resource** icon on the left side of the canvas.  
    The properties for the API resource appear on the **Properties** tab
    at the bottom of the window. If they do not appear, you can
    right-click the **Resource** icon and click **Show Properties View**
    .

6.  On the **Properties** tab, provide the following as **Basic**
    properties:

    |                  |                                                                                                                                                                                                                                                                                            |
    |------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | **Url Style**    | Click the respective **Value** field, click the down arrow, and then select **URI\_TEMPLATE** from the list                                                                                                                                                                                |
    | **URI-Template** | Enter `               /querydoctor/{category}              ` . This defines the request URL format. In this case, the full request URL format is `               http://<host>:<port>/querydoctor/{category}              ` where `               {category}              ` is a variable. |
    | **Methods**      | Check if the value of **Get** is set to true. This defines that the API resource only handles the requests where the HTTP method is GET.                                                                                                                                                   |

    ![](attachments/119132413/119132424.png){width="660" height="458"}  
    We are now ready to configure the In sequence to handle requests
    from the client.

7.  Configure the In sequence:

    1.  From the **Mediators** palette, click and drag a Log mediator to
        the In sequence (the top of the canvas).

                !!! note
        
                You can use the [Log
                mediator](https://docs.wso2.com/display/EI650/Log+Mediator) to
                log a message when the request is received by the In sequence of
                the API resource. In this scenario, we will configure the Log
                mediator to display the following message: “Welcome to the
                HealthcareService”.
        

    2.  With the Log mediator selected, access the Property tab and fill
        in the information in the table below:

        <table>
        <thead>
        <tr class="header">
        <th>Field</th>
        <th>Value</th>
        <th>Description</th>
        </tr>
        </thead>
        <tbody>
        <tr class="odd">
        <td>Log Category</td>
        <td><code>                 INFO                </code></td>
        <td>Indicates that the log contains an informational message.</td>
        </tr>
        <tr class="even">
        <td>Log Level</td>
        <td><code>                 Custom                </code></td>
        <td>When <code>                 Custom                </code> is selected, only specified properties will be logged by this mediator.<br />
        For more information on the available log levels, see the <a href="https://docs.wso2.com/display/EI650/Log+Mediator#LogMediator-log-level">Log Mediator</a> .</td>
        </tr>
        <tr class="odd">
        <td>Log Separator</td>
        <td><code>                 (blank)                </code></td>
        <td>Since there is only one property that is being logged, we do not require a separator, so this field can be left blank.</td>
        </tr>
        <tr class="even">
        <td>Properties</td>
        <td><br />
        </td>
        <td><div class="content-wrapper">
        <p>Follow the steps given below to extract the stock symbol from the request and print a welcome message in the log</p>
        <ol>
        <li>Click the <strong>Value</strong> field of the <strong>Properties</strong> property, and then click the <strong>browse (...)</strong> icon that appears.</li>
        <li>In the Log Mediator Configuration dialog, click <strong>New</strong> , and then add a property called "message" as follows:<br />

        <ul>
        <li><strong>Name</strong> : <code>                      message                     </code></li>
        <li><strong>Type</strong> : <code>                      LITERAL                     </code><br />
        (We select LITERAL because the required log message is a static value.)</li>
        <li><strong>Value/Expression</strong> : <code>                      "Welcome to HealthcareService"                     </code></li>
        </ul></li>
        </ol>
        <p><img src="attachments/119132413/119132423.png" width="546" height="369" /></p>
        </div></td>
        </tr>
        <tr class="odd">
        <td>Description</td>
        <td><code>                 Request Log                </code></td>
        <td>The <strong>Description</strong> field provides the name that appears for the Log mediator icon in the design view.</td>
        </tr>
        </tbody>
        </table>

        ![](attachments/119132413/119132422.png){width="660"
        height="944"}

    3.  Click **OK** to save the Log mediator configuration.

    4.  Configure the [Send
        Mediator](https://docs.wso2.com/display/EI650/Send+Mediator) to
        send the request message to the
        `             HealthcareService            ` endpoint.

        1.  From the **Mediators** palette, click and drag a **Send**
            mediator to the In sequence adjoining the Log mediator you
            added above.

        2.  From the **Defined EndPoints** palette, click and drag the
            **QueryDoctorEP** endpoint, which we created, right next to
            the empty space of the Send mediator.

        ![](attachments/119132413/119132421.png){width="971"
        height="652"}

8.  Configure the Out sequence to ensure that we send the response from
    the Healthcare service endpoint, back to the client. For this, we
    use a Send mediator with no output endpoint defined, which defaults
    to sending the response back to the requesting client  
    From the **Mediators** palette, click and drag a **Send** mediator
    to the Out Sequence (the bottom part of the canvas).  
    ![](attachments/119132413/119132420.png){width="967" height="659"}

You have successfully created all the artifacts that are required to
send a request through the ESB to the back-end service. We will now
package these artifacts and deploy them in the ESB profile of WSO2 EI.

### Packaging the artifacts

You need to Package the `         QueryDoctorEP        ` endpoint and
the `         HealthcareAP        ` I resource into the [Composite
Application (C-App)
project](https://docs.wso2.com/display/EI600/Key+Concepts#KeyConcepts-CompositeApplicationproject)
named `         SampleServicesCompositeApplication        ` .

!!! note

The `         SampleCApp        ` Composite Application project is
generated and is listed under SampleApp project in the Project Explorer.
The **Composite Application Project POM Editor** can be accessed by
selecting the `         pom.xml        ` file listed under
SampleServicesCompositeApplication project.


Follow the steps given below to create the CAR file using one of the
following options:

-   [**C-App project**](#d315a56d2d1a4fb09994e4b7feae2edb)
-   [**pom.xml file**](#06b881fb4c044882b615cbab73367095)

1.  Right-click the C-App project and select **Export Composite
    Application Project** from the pop-up menu.  
    Example:  
    ![](attachments/119132413/119132419.png){width="500" height="332"}
2.  Define the location you want to generate the CAR file by clicking
    the Browse next to **Export Destination** and click **Next** .
3.  Select the artifact that needs to be included into the CAR file and
    click **Finish** .

        !!! tip
    
        **Tip** : When you create a CAR file with artifacts, ensure that
        each artifact name is the same as the relevant artifact file name.
    

Now you see the `             .car            ` file created in the
defined location.

1.  Open the `              pom.xml             ` file of the C-App
    project in the Composite Application Project POM Editor.
2.  Select the artifact that needs to be included into the CAR file.
3.  Click ![](attachments/119132413/119132433.png){width="30"} and
    define the location you want to create the CAR file.

Now you see the .car file created in the defined location.

You have now exported all your project's artifacts into a single CAR
file. Next, you need to deploy the Composite Application in the server.

!!! note

Note

-   In a CAR file, if a particular artifact name is different from the
    relevant artifact file name, re-deploying the CAR file fails with an
    error.
-   If a CAR file has one or more artifacts that are named differently
    from the relevant artifact file name, you are unable to remove those
    artifacts from memory when you delete the CAR file.


### Starting the ESB profile and deploying the artifacts

Follow the steps given below to create a server and start the server
using WSO2 Integration Studio.

1.  In **WSO2 Integration Studio** , click **Miscellaneous → Add New
    Server** to add a new server..
2.  In the **Define a New Server** dialog box, expand the **WSO2**
    folder, and select the respective product and version.
3.  Click **Next** , click **Browse** that is next to **CARBON\_HOME** ,
    and select the WSO2 EI distribution directory, to define the server
    runtime environment and click **Next** .

        !!! warning
    
        Warning!
    
        If you are running on a **linux-based system** (MacOS, Ubunto, or
        CentOS), be sure that your WSO2 EI distribution is a **binary**
        distribution downloaded from the website. If you use a product
        distribution that was set up using the product installer, you will
        require special permissions to start the product through WSO2
        Integration Studio.
    

4.  Review the default port details for the ESB profile of WSO2 EI.
    Typically, you can leave these unchanged, but if you are already
    running another server on these ports, specify unused ports here.
    See [Default Ports of WSO2
    Products](https://docs.wso2.com/display/ADMIN44x/Default+Ports+of+WSO2+Products)
    for more information. Click **Next** .
5.  To deploy the CApp project in the ESB server that we just added,
    select **SampleServicesCompositeApplication** from the list, click
    **Add** to move it into the Configured list, and then click
    **Finish** .  
    The ESB server of WSO2 EI is now added inside Eclipse tooling.

6.  On the Servers tab, you see that the server is currently stopped.
    Click ![](attachments/119132413/119132432.png){width="20"} on the
    Servers tab's toolbar to start the server.  
    ![](attachments/119132413/119132417.png){width="1074"
    height="129"}  
    If you are prompted to save changes to any of the artifact files,
    click **Yes** .  
    As the server starts, the Console tab appear. You should see
    messages indicating that the C-App was successfully deployed. The
    C-App will now be available in the ESB profile's management console
    ( **Manage -\> Carbon Applications -\> List** ).

        !!! info
    
        You can also deploy the artifacts to the ESB server using a
        [Composite Application Archive (CAR)
        file](https://docs.wso2.com/display/ADMIN44x/Packaging+Artifacts+into+Composite+Applications#PackagingArtifactsintoCompositeApplications-createCARCreatingaCompositeApplicationArchive(CAR)file)
        .
    

### Starting the MSF4J profile

To be able to send requests to the back-end service (which is an MSF4J
service deployed in MSF4J profile), you need to first start the MSF4J
runtime:

1.  Open a terminal and navigate to the
    `          <EI_HOME>/wso2/msf4j/bin         ` directory.
2.  To start the runtime, execute the MSF4J startup script as shown
    below.

    -   [**On MacOS/Linux/CentOS**](#3b736db85a004459bc63277f27bfcca9)
    -   [**On Windows**](#93db0c971b9d429db5952d3cc21c62de)

    ``` java
        sh carbon.sh
    ```

    ``` java
            carbon.bat
    ```

The Healthcare service is now active and you can start sending requests
to the service.

### Sending requests to the ESB

Let's send a request to our REST API (HealthcareAPI), which is now
deployed in the ESB profile of WSO2 EI. You will need a REST client like
curl for this.

1.  In your Web browser, navigate to the ESB profile's management
    console using the following URL:
    `                     https://localhost:9443/carbon                    /         `
    .
2.  Log in to the management console using the following credentials:
    -   Username: admin
    -   Password: admin
3.  In the left navigation pane, click **APIs** under **Service Bus**
    .  
    You see that the REST API that we created earlier (HealthcareAPI)
    listed here. This API is now ready to receive requests and to send
    them to the back-end service. You can also view the API Invocation
    URL that is used in to send the request to the service.

    ![](attachments/119132413/119132497.png){width="800" height="270"}

4.  Open a command line terminal and enter the following request:

        curl -v http://localhost:8280/healthcare/querydoctor/surgery 

        !!! info
    
        The above request is formed as per the [URI-Template
        defined](#SendingaSimpleMessagetoaService-endpoint-uri) when
        creating the endpoint.
    
        `                       http://            :            /healthcare/{uri.var.category                      }          `
    
        The `           {uri.var.category}          ` of the above URI is
        derived from the [URI-Template
        defined](#SendingaSimpleMessagetoaService-uriTemplate) when creating
        the API resource.
    
        `           http://<host>:<port>/querydoctor/{category}          `
    
        Other categories you can try sending in the request are:
    
        -   `             cardiology            `
    
        -   `             gynaecology            `
    
        -   `             ent            `
    
        -   `             paediatric            `
    

5.  You will see the response message from the HealthcareService with a
    list of available doctors and the relevant details.

    ``` xml
        [{"name":"thomas collins",
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
    ```

6.  Now, check the ESB profile's server console in Eclipse, and you will
    see the following message:
    `                      INFO - LogMediator message = "Welcome to HealthcareService"          `

    This is the message printed by the Log mediator when the message
    from the client is received in the In sequence of the API resource.

You have now created and deployed an API resource in the ESB profile,
which receives requests, logs a message using the Log mediator, sends
the request to a back-end service using the Send mediator, and returns a
response to the requesting client.
