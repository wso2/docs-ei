# Connecting Web APIs/Cloud Services

## What you'll build

When you integrate the systems in your organizaion, it is also necessary to integrate with third-party systems and its capabilities to enhance your services. WSO2 Micro Integrator uses **Connectors** for the purpose of referring the APIs of third-party systems.

**In this tutorial**, when a client sends an appointment reservation request to the Micro Integrator, the client should receive an email confirming the appointment reservation details. To build this use case, you can add a GMail connector to the mediation flow of the REST resource that you defined in the [previous tutorial](storing-and-forwarding-messages.md).

## Let's get started!

### Step 1: Set up the workspace

Set up WSO2 Integration Studio as follows:

1.  Download the relevant [WSO2 Integration Studio](https://wso2.com/integration/tooling/) based on your operating system. The path to the extracted/installed folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.
2.   If you did not try the [asynchronous messaging](storing-and-forwarding-messages.md) tutorial yet:
    1.  Open WSO2 Integration Studio and go to **File -> Import**. 
    2.  Select **Existing WSO2 Projects into workspace** under the **WSO2** category, click **Next**, and then upload the [pre-packaged project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/StoreAndForwardTutorial.zip).

Optionally, you can set up the **CLI tool** for artifact monitoring. This will later help you get details of the artifacts that you deploy in your Micro Integrator.

1.  Go to the [WSO2 Micro Integrator website](https://wso2.com/integration/#). 
2.  Click **Download -> Other Resources** and click **CLI Tooling** to download the tool. 
3.  Extract the downloaded ZIP file. This will be your `MI_CLI_HOME` directory. 
4.  Export the `MI_CLI_HOME/bin` directory path as an environment variable. This allows you to run the tool from any location on your computer using the `mi` command. Read more about the [CLI tool](../../../administer-and-observe/using-the-command-line-interface).

### Step 2: Develop the integration artifacts

#### Creating a Client ID and Client Secret

Follow the steps below if you want to use your own email address for sending the emails.

1.  As the email sender, navigate to the [URL](https://console.developers.google.com/projectselector/apis/credentials) and log in to your google account.
2.  If you do not already have a project, create a new project.
3.  Now, navigate again to the [URL](https://console.developers.google.com/projectselector/apis/credentials) and click
 **Google APIs -> Credentials -> Create Credential -> OAuth client ID**.

    !!! Note
        At this point, if the consent screen name is not provided, you will be prompted to do so.

4.  Select **Web Application** and create a client.
5.  Provide <https://developers.google.com/oauthplayground> as the redirect URL under **Authorized redirect URIs** and click **Create**. The client ID and client secret will then be displayed.

    !!! Info
        See [Gmail API documentation](https://developers.google.com/gmail/api/auth/web-server) for details on creating the Client ID and Client Secret.
    
6.  Click on the Library on the side menu and select **Gmail API**.
7.  Click **Enable**.

#### Obtaining the Access Token and Refresh Token

Follow these steps to automatically refresh the expired token when connecting to Google API:

1. Navigate to the <https://developers.google.com/oauthplayground> URL, click ![](/assets/img/tutorials/119132294/oauth-credential-icon.png) at the top right corner of the screen and select **Use your own OAuth credentials**.

    ![](../../assets/img/tutorials/using-the-gmail-connector/OAuth2.0Configuration.png)
    
2. Provide the client ID and client secret you [previously created](#creating-a-client-id-and-client-secret) and click **Close**.
3. Now under Step 1, select **Gmail API v1** from the list of APIs, select all the scopes listed under it, and click **Authorize APIs**.

    ![](../../assets/img/tutorials/using-the-gmail-connector/GmailApiV1.png)
   
4. You will then be prompted to allow permission, click **Allow**.
5. Now , click **Exchange authorization code for tokens** to generate and display the access token and refresh token.

#### Importing the Gmail Connector into WSO2 Integration Studio

1. Right click on **Sample Services** in the Project Explorer and select **Add or Remove Connector**.
2. Select **Add Connector** and click **Next**. You are now connected to the [WSO2 Connector store](https://store.wso2.com).
3. Find **Gmail** from the list of connectors and click the **Download** button (for the Gmail connector). 
    ![](../../assets/img/tutorials/119132294/import-gmail-connector.png)

4. Click **Finish**.
   The connector is now downloaded to your workspace in WSO2 Integration Studio and the connector operations are available in the Gmail Connector palette.  
    ![](../../assets/img/tutorials/119132294/select-connector-dialog.png)

Let's use these connector operations in the configuration.

#### Update the message flow

The connector operations are used in the sequence named **PaymentRequestProcessingSequence**. Select this sequence and do the following updates:

1.  Add a Property Mediator just before the Call mediator to retrieve and store the patient's email address.

    ![](../../assets/img/tutorials/119132294/119132299.png)  

2.  With the Property mediator selected, access the **Property** tab of the mediator and fill in the information in the following table:

    <table>
        <tr>
            <th>Property</th>
            <th>Description</th>
        </tr>
      <tr class="odd">
         <td>Property Name</td>
         <td>Enter <code>               New Property...              </code>.</td>
      </tr>
      <tr class="even">
         <td>New Property Name</td>
         <td>Enter <code>email_id</code>.</td>
      </tr>
      <tr class="odd">
         <td>Property Action</td>
         <td>Enter <code>               set              </code>.</td>
      </tr>
      <tr class="even">
         <td>Value Type</td>
         <td>Enter <code>               EXPRESSION              </code>.</td>
      </tr>
      <tr class="even">
         <td>Value Expression</td>
         <td>
            <div class="content-wrapper">
              <p>Follow the steps given below to specify the expression:</p>
            <ol>
                <li>Click the text box for the <strong>Value Expression</strong> field. This opens the <b>Expression Selector</b> dialog box.</li>
               <li>Select <strong>Expression</strong> from the list.
                </li>
               <li>Enter <code>json-eval($.patient.email)</code> to overwrite the default expression.</li>
               <li>Click <strong>OK</strong>.<br />
               </li>
            </ol>
            </div>
         </td>
      </tr>
      <tr>
          <td>Description</td>
          <td>Get Email ID</td>
      </tr>
    </table>

3.  Add another Property mediator just after the Log mediator to retrieve and store the response sent from SettlePaymentEP. This will be used within the body of the email.

    ![](../../assets/img/tutorials/119132294/119132298.png)

4.  With the Property mediator selected, access the **Property** tab and specify the details given below.

    | Property            | Value                   |
    |-------------------|-------------------------|
    | Property Name     | Select **New Property** |
    | New Property Name | payment_response        |
    | Property Action   | Select **Set**          |
    | Value Type        | Select **Expression**   |
    | Value Expression  | json-eval($.)           |
    | Description       | Get Payment Response    |

5.  Drag and drop the init method from the **Gmail Connector** palette adjoining the Property mediator you added in the previous step.

    ![](../../assets/img/tutorials/119132294/119132297.png)

6.  With the init method selected, access the Property tab and specify the details given below.

    | Property               | Value                                                                                                   |
    |-----------------------|---------------------------------------------------------------------------------------------------------|
    | Parameter Editor Type | Select **inline**                                                                                       |
    | userId                | The sender's email address (use the email address that you used to configure the client ID and secret.) |
    | accessToken           | The **access token** you obtained.                           |
    | apiUrl                | <https://www.googleapis.com/gmail>                                                                      |
    | clientId              | The **client ID** you created.                                               |
    | clientSecret          | The **client secret** you created.                             |
    | refreshToken          | The **refresh token** you obtained.                          |

7.  Add the sendMail method from the Gmail Connector palette and access the **Property** tab and specify the following details;

    <table>
        <tr>
            <th>Property</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>to</td>
            <td>
              Enter `{$ctx:email_id}` as the value. This retrieves the patient email address that was stored in the relevant Property mediator.  
            </td>
        </tr>
        <tr>
            <td>subject</td>
            <td>
                Enter `Payment Status` as the value. This is the subject line in the email that is sent out.
            </td>
        </tr>
        <tr>
            <td>messageBody</td>
            <td>
                Enter `{$ctx:payment_response}` as the value. This retrieves the payment response that was stored in the relevant Property mediator.
            </td>
        </tr>
    </table>

    The updated **PaymentRequestProcessingSequence** should now look like this:  

    ![](../../assets/img/tutorials/119132294/119132296.png)

8.  Save the updated sequence configuration.
9.  Right click on **SampleServicesConnectorExporter** and navigate to **New →  Other → Add/Remove Connectors** and select **Add connector** and click on **Next** . Select **Workspace** to list down the connectors that were added.  

    ![](../../assets/img/tutorials/119132294/add-remove-connectors.png)

    ![](../../assets/img/tutorials/119132294/connector-select-dialog.png)

10. Select the Gmail connector from the list and click **OK** and then **Finish**.

### Step 3: Package the artifacts

Package the artifacts in your composite application project (SampleServicesCompositeApplication project), the registry resource project (SampleRegistryResource project), and the Connector project (SampleServicesConnectorExporter project) to be able to deploy the artifacts in the server.

1.  Open the `          pom.xml         ` file in the composite application project POM editor.
2.  Ensure that the following projects and artifacts are selected in the POM file.

    -   SampleServicesCompositeApplicationProject
        -   `HealthcareAPI`
        -   `ClemencyEP`
        -   `GrandOakEP`
        -   `PineValleyEP`
        -   `ChannelingFeeEP`
        -   `SettlePaymentEP`
        -   `PaymentRequestMessageStore`
        -   `PaymentRequestProcessingSequence`
        -   `PaymentRequestProcessor`
    -   SampleServicesRegistryProject
    -   SampleServicesConnectorExporter

3.  Save the project.

### Step 4: Build and run the artifacts

To test the artifacts, deploy the [packaged artifacts](#step-3-package-the-artifacts) in the embedded Micro Integrator:

1.  Right-click the composite application project and click **Export Project Artifacts and Run**.
2.  In the dialog that opens, select the composite application project that you want to deploy.  
4.  Click **Finish**. The artifacts will be deployed in the embedded Micro Integrator and the server will start. See the startup log in the **Console** tab.

!!! Warning
    Stop the Micro Integrator before proceeding to test. This is because you need to start the broker profile before starting the Micro Integrator.

### Step 5: Test the use case

Let's test the use case by sending a simple client request that invokes the service.

#### Start the backend service

1. Download the JAR file of the back-end service from [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-JDK11-2.0.0.jar).
2. Open a terminal, navigate to the location where your saved the [back-end service](#step-1-set-up-the-workspace).
3. Execute the following command to start the service:

    ```bash
    java -jar Hospital-Service-2.0.0-JDK11.jar
    ```

#### Start the Message Broker runtime

Set up the Message Broker profile of WSO2 EI 6.6.0. This is required because the Message Broker profile is used for guaranteed message delivery in this use case.

1. Download [WSO2 EI 6.6.0](https://wso2.com/enterprise-integrator/6.6.0), which includes the Message Broker profile. The path to this folder is referred to as `EI_6.6.0_HOME` throughout this tutorial.

2. Add the following JAR files from the `EI_6.6.0_HOME/wso2/broker/client-lib/` directory to the 
`MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/lib/` (in MacOS) or 
`MI_TOOLING_HOME/runtime/microesb/lib` (in Windows/Linux) directory.
    -   andes-client-*.jar
    -   geronimo-jms_1.1_spec-*.jar
    -   org.wso2.securevault-*.jar
3. Open the `deployment.toml` file from the `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/conf/` (in MacOS) or 
`MI_TOOLING_HOME/runtime/microesb/conf/` (in Windows/Linux) directory and add the configurations given below. This is required for enabling the broker to store messages.

    ```toml
    [[transport.jms.listener]]
    name = "myQueueListener"
    parameter.initial_naming_factory = "org.wso2.andes.jndi.PropertiesFileInitialContextFactory"
    parameter.broker_name = "wso2mb"
    parameter.provider_url = "conf/jndi.properties"
    parameter.connection_factory_name = "QueueConnectionFactory"
    parameter.connection_factory_type = "queue"
    parameter.cache_level = "consumer"

    [[transport.jms.sender]]
    name = "myQueueSender"
    parameter.initial_naming_factory = "org.wso2.andes.jndi.PropertiesFileInitialContextFactory"
    parameter.broker_name = "wso2mb"
    parameter.provider_url = "conf/jndi.properties"
    parameter.connection_factory_name = "QueueConnectionFactory"
    parameter.connection_factory_type = "queue"
    parameter.cache_level = "producer"

    [transport.jndi.connection_factories]
    'connectionfactory.QueueConnectionFactory' = "amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'"

    [transport.jndi.queue]
    PaymentRequestJMSMessageStore="PaymentRequestJMSMessageStore"
    ```
    
To start the Message Broker:

1.  Open a terminal and navigate to the `EI_6.6.0_HOME/wso2/broker/bin` directory.
2.  Execute the following command to run the message broker. 
    
    -   On **MacOS/Linux/CentOS**:

        ```bash
        sh wso2server.sh
        ```

    -   On **Windows**:

        ```bash
        wso2server.bat
        ```

    See the [WSO2 EI 6.6.0 documentation](https://docs.wso2.com/display/EI660/Running+the+Product) for more information on how to run the Message Broker profile.

#### Restart the Micro Integrator

Let's restart the Micro Integrator with the deployed artifacts:

Right-click the composite application project and click **Export Project Artifacts and Run** as shown below.

<img src="../../../assets/img/tutorials/restart_server.png" width="400">

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

Similarly, you can get details of Connectors as well as other artifacts deployed in the server. Read more about [using the CLI tool](../../../administer-and-observe/using-the-command-line-interface).

#### Send the client request

Let's send a request to the API resource. You can use the embedded <b>HTTP Client</b> of WSO2 Integration Studio as follows:

1. Open the <b>HTTP Client</b> of WSO2 Integration Studio.

    !!! Tip
        If you don't see the <b>HTTP Client</b> pane, go to <b>Window -> Show View - Other</b> and select <b>HTTP Client</b> to enable the client pane.

    <img src="../../../assets/img/tutorials/common/http4e-client-empty.png" width="800">
    
2. Enter the request information as given below and click the <b>Send</b> icon (<img src="../../../assets/img/tutorials/common/play-head-icon.png" width="20">).
    
    <table>
        <tr>
            <th>Method</th>
            <td>
               <code>POST</code> 
            </td>
        </tr>
        <tr>
            <th>Headers</th>
            <td>
              <code>Content-Type=application/json</code>
            </td>
        </tr>
        <tr>
            <th>URL</th>
            <td><code>http://localhost:8290/healthcare/categories/surgery/reserve</code></br></br>
              <ul>
                <li>
                  The URI-Template format that is used in this URL was defined when creating the API resource:
          <code>http://<host>:<port>/categories/{category}/reserve</code>.
                </li>
              </ul>
            </td>
        </tr>
        <tr>
            <th>Body</th>
            <td>
            <div>
              <code>
                {
                 "name": "John Doe",
                 "dob": "1940-03-19",
                 "ssn": "234-23-525",
                 "address": "California",
                 "phone": "8770586755",
                 "email": "johndoe@gmail.com",
                 "doctor": "thomas collins",
                 "hospital": "grand oak community hospital",
                 "cardNo": "7844481124110331",
                 "appointment_date": "2025-04-02"
                }
              </code>
            </div></br>
            <ul>
              <li>
                This JSON payload contains details of the appointment reservation, which includes patient details, doctor, hospital, and data of appointment.
              </li>
            </ul>
        </tr>
     </table>
     
     <img src="../../../assets/img/tutorials/119132294/http-client-request-config.png" width="800">

If you want to send the client request from your terminal:

1.  Install and set up [cURL](https://curl.haxx.se/) as your REST client.

2.  Create a JSON file names `request.json` with the following request payload. Make sure you provide a valid email address so that you can test the email being sent to the patient.

    ```json
    {
    "name": "John Doe",
    "dob": "1940-03-19",
    "ssn": "234-23-525",
    "address": "California",
    "phone": "8770586755",
    "email": "johndoe@gmail.com",
    "doctor": "thomas collins",
    "hospital": "grand oak community hospital",
    "cardNo": "7844481124110331",
    "appointment_date": "2025-04-02"
    }
    ```

3.  Open a command line terminal and execute the following command from the location where the `request.json` file you created is saved:

    ```bash
    curl -v -X POST --data @request.json http://localhost:8290/healthcare/categories/surgery/reserve --header 
    "Content-Type:application/json"
    ```
   
#### Analyze the response

You will see the response as follows:

```
{"message":"Payment request successfully submitted. Payment confirmation will be sent via email."}
```

An email will be sent to the provided patient email address with the following details:

Note: If you haven't received the mail yet, there are possibilities that your access token might have expired.
Follow the steps in [obtaining-the-access-token-and-refresh-token](#obtaining-the-access-token-and-refresh-token) to 
obtain a new access token and update the gmail init operation.

```bash
Subject: Payment Status
             
Message:
    {"appointmentNo":2,"doctorName":"thomas collins","patient":"John
    Doe","actualFee":7000.0,"discount":20,"discounted":5600.0,"paymentID":"8458c75a-c8e0-4d49-8da4-5e56043b1a20","status":"Settled"}
```

You have now explored how to import the Gmail connector to the Micro Integrator and then use the connector operations to send emails.
