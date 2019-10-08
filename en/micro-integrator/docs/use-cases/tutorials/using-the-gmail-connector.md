# Using the Gmail Connector

## What you'll build

When you integrate the systems in your organizaion, it is also necessary to integrate with third-party systems and its capabilities to enhance your services. WSO2 Micro Integrator uses **Connectors** for the purpose of referring the APIs of third-party systems.

**In this tutorial**, when a client sends an appointment reservation request to the Micro Integrator, the client should receive an email confirming the appointment reservation details. To build this use case, you can add a GMail connector to the mediation flow of the REST resource that you defined in the [previous tutorial](storing-and-forwarding-messages.md).

## Let's get started!

### Step 1: Set up the workspace

To set up the tools:

-   Select the relevant [WSO2 Integration Studio](https://wso2.com/integration/tooling/) based on your operating system and extract the
    ZIP file.  The path to this folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.
-   Download the [CLI Tool](https://wso2.com/integration/micro-integrator/install/) for monitoring artifact deployments.

If you did not try the [Exposing Several Services as a Single Service](exposing-several-services-as-a-single-service.md) tutorial yet, open WSO2 Integration Studio, click **File** , and then click **Import** . Next, select **Existing WSO2 Projects into workspace** under the **WSO2** category, click **Next** and upload the [pre-packaged project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/StoreAndForwardTutorial.zip).

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
2. Select **Add Connector** and click **Next**.
3. Select **Connector Store location** and click **Connect** to connect to [WSO2 Connector store](https://store.wso2.com).
4. Scroll down and select **Gmail** from the list of connectors.  
    ![](../../assets/img/tutorials/119132294/import-gmail-connector.png)

5. Click **Finish**.
   The connector is now downloaded into your WSO2 Integration Studio and the connector operations will be available in the Gmail Connector palette.  
    ![](../../assets/img/tutorials/119132294/select-connector-dialog.png)

Let's use these connector operations in the configuration.

#### Update the message flow

The connector operations are used in the **PaymentRequestProcessingSequence**. Select this sequence and do the following updates:

1.  Add a Property Mediator just before the Call mediator to retrieve and store the patient's email address.

    ![](../../assets/img/tutorials/119132294/119132299.png)  

2.  With the Property mediator selected, access the **Property** tab of the mediator and fill in the information in the following table:

    | Property          | Value                      |
    |-------------------|----------------------------|
    | Property Name     | Select **New Property**    |
    | New Property Name | email_id                   |
    | Property Action   | Select **Set**             |
    | Value Type        | Select **Expression**      |
    | Value Expression  | json-eval($.patient.email) |
    | Description       | Get Email ID               |

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
    <thead>
    <tr class="header">
    <th>Property</th>
    <th>Value/Expression</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Parameter Editor Type</td>
    <td>Select <strong>inline</strong></td>
    <td><br />
    </td>
    </tr>
    <tr class="even">
    <td>to</td>
    <td><code>               {$ctx:email_id}              </code></td>
    <td>Retrieves the patient email address that was stored in the relevant Property mediator.</td>
    </tr>
    <tr class="odd">
    <td>subject</td>
    <td>Payment Status</td>
    <td>The subject line in the email that is sent out.</td>
    </tr>
    <tr class="even">
    <td>messageBody</td>
    <td><code>               {$ctx:payment_response}              </code></td>
    <td>Retrieves the payment response that was stored in the relevant Property mediator.</td>
    </tr>
    </tbody>
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

1. Download the JAR file of the back-end service from [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0-EI7.jar).
2. Open a terminal, navigate to the location where your saved the [back-end service](#step-1-set-up-the-workspace).
3. Execute the following command to start the service:

    ```bash
    java -jar Hospital-Service-2.0.0-EI7.jar
    ```

#### Start the Message Broker runtime

To set up WSO2 Message Broker:

1. Download WSO2 Message Broker. The path to this folder is referred to as `MB_HOME` throughout this tutorial.
2. Add the following JAR files from the `MB_HOME/wso2/broker/client-lib/` directory to the `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/lib/` directory.
3. Open the `deployment.toml` file from `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/conf/` or 
`MI_TOOLING_HOME/runtime/microesb/conf/` ( for linux ) directory and add 
the configurations given below. This is required for enabling the broker to store messages.

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
    QueueConnectionFactory = "amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'"

    [transport.jndi.queue]
    PaymentRequestJMSMessageStore="PaymentRequestJMSMessageStore"
    ```
To start WSO2 Message Broker:

1.  Open a terminal and navigate to the `MI_HOME/wso2/broker/bin` directory.
2.  Execute the following command to run the in message broker. 
    
    -   On **MacOS/Linux/CentOS**:

        ```bash
        sh wso2server.sh
        ```

    -   On **Windows**:

        ```bash
        wso2server.bat
        ```

    See the [WSO2 EI 6.5.0 documentation](https://docs.wso2.com/display/EI650/Running+the+Product) for more information on how to run the WSO2 MB.

#### Restart the Micro Integrator

Start the Micro Integrator on WSO2 Integration Studio.

#### Send the client request

Let's use the **CLI Tool** to find the URL of the REST API that is deployed in the Micro Integrator:

1.  Open a terminal and navigate to the `CLI_HOME/bin` directory.
2.  Execute the following command to start the tool:
    `./mi`
3.  Execute the following command to find the APIs deployed in the server:
    `mi api show`
    
    Note: Please remember to login to CLI using `mi remote login` command giving `admin` as both username and 
        password before executing the above command.

Let's send a request to the API resource.

1.  Create a JSON file names `request.json` with the following request payload. Make sure you provide a valid email address so that you can test the email being sent to the patient.

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

2.  Open a command line terminal and execute the following command from the location where `           request.json          ` file you created is saved:

    ```bash
    curl -v -X POST --data @request.json http://localhost:8290/healthcare/categories/surgery/reserve --header 
    "Content-Type:application/json"
    ```

    !!! Info
        This is derived from the **URI-Template** defined when creating the API resource: `http://<host>:<port>/categories/{category}/reserve`
    
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
