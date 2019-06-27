# Using the Gmail Connector

**In this tutorial,** you use the Gmail connector to send an email
containing the response received from SettlePaymentEP in the [previous
tutorial](_Storing_and_Forwarding_Messages_) .

!!! note

See the following topics for a description of the **concepts** that you
need to know when creating ESB artifacts:

-   [REST
    APIs](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-RESTAPIs)
-   [Endpoints](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Messageexitpoints)
-   [Sequences](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Sequences)
-   [Connectors](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Connectors)
-   [Store and
    forward](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Messagestoringandforwarding)

!!! tip

**Before you begin** ,

1.  Install Oracle Java SE Development Kit (JDK) version 1.8.\* and set
    the JAVA\_HOME environment variable.
2.  Go to the [product page](https://wso2.com/integration/) of **WSO2
    Enterprise Integrator** , select **Other Installation Options** ,
    and download the **Binary** distribution. Extract the ZIP file of
    the binary. This will be your `          <EI_HOME>         `
    directory.
3.  Select and download the relevant WSO2 Integration Studio ZIP file
    based on your operating system from
    [here](https://wso2.com/integration/tooling/) and then extract the
    ZIP file.  
    The path to this folder will be referred to as
    `          <EI_TOOLING>         ` throughout this tutorial.  

      

    !!! info

    -   For more detailed installation instructions, see [Installing
        WSO2 Integration
        Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio)
        .
    -   Getting an error message? See the troubleshooting tips given
        under [Installing WSO2 Integration
        Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio#InstallingWSO2IntegrationStudio-Troubleshooting)
        .


4.  If you did not try the [Storing and Forwarding
    Messages](_Storing_and_Forwarding_Messages_) tutorial yet, o pen
    WSO2 Integration Studio, click **File** , and then click **Import**
    . Next, select **Existing WSO2 Projects into workspace** under the
    **WSO2** category, click **Next** and upload the [pre-packaged
    project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/StoreAndForwardTutorial.zip)
    . This contains the configurations of the [Storing and Forwarding
    Messages](_Storing_and_Forwarding_Messages_) tutorial so that you do
    not have to repeat those steps.
5.  Download the MSF4J service from
    [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0.jar)
    and copy the JAR file to
    `          <EI_HOME>/wso2/msf4j/deployment/microservices         `
    folder. The back-end service is now deployed in the MSF4J profile of
    WSO2 EI.


Let's get started!

-   [Setting up the message broker
    profile](#UsingtheGmailConnector-Settingupthemessagebrokerprofile)
-   [Creating the credentials for using the Gmail
    Connector](#UsingtheGmailConnector-CreatingthecredentialsforusingtheGmailConnector)
-   [Importing the Gmail Connector into WSO2 Integration
    Studio](#UsingtheGmailConnector-ImportingtheGmailConnectorintoWSO2IntegrationStudio)
-   [Creating the Deployable
    Artifacts](#UsingtheGmailConnector-CreatingtheDeployableArtifacts)
-   [Packaging the
    artifacts](#UsingtheGmailConnector-Packagingtheartifacts)
-   [Starting the Message Broker
    runtime](#UsingtheGmailConnector-StartingtheMessageBrokerruntime)
-   [Starting the MSF4J
    profile](#UsingtheGmailConnector-StartingtheMSF4Jprofile)
-   [Starting the ESB profile and deploying the
    artifacts](#UsingtheGmailConnector-StartingtheESBprofileanddeployingtheartifacts)
-   [Sending requests to the
    ESB](#UsingtheGmailConnector-SendingrequeststotheESB)

### Setting up the message broker profile

Open the
`         <EI_HOME>/conf/jndi                   .properties         `
file and add the following line after the
`         queue.MyQueue = example.MyQueue        ` line:

``` java
    queue.PaymentRequestJMSMessageStore=PaymentRequestJMSMessageStore
```

### Creating the credentials for using the Gmail Connector

#### Creating a Client ID and Client Secret

Follow the steps below if you want to use your own email address for
sending the emails.

1.  As the email sender, navigate to the URL
    <https://console.developers.google.com/projectselector/apis/credentials>
    and log in to your google account.
2.  If you do not already have a project, create a new project.

3.  Click **Google APIs -\> Credentials -\> Create Credential -\> OAuth
    client ID** .

        !!! note
    
        At this point, if the consent screen name is not provided, you will
        be prompted to do so.
    

4.  Select **Web Application** and create a client.  

5.  Provide <https://developers.google.com/oauthplayground> as the
    redirect URL under **Authorized redirect URIs** and click **Create**
    .  
    The client ID and client secret will then be displayed.  

        !!! info
    
        See [Gmail API
        documentation](https://developers.google.com/gmail/api/auth/web-server)
        for details on creating the Client ID and Client Secret.
    

6.  Click on the Library on the side menu, and select **Gmail API** .

7.  Click Enable.

#### Obtaining the Access Token and Refresh Token

Follow these steps to automatically refresh the expired token when
connecting to Google API:

1.  Navigate to the <https://developers.google.com/oauthplayground>
    URL, click ![](attachments/119132294/119132302.png){width="50"
    height="22"} at the top right corner of the screen, and select **Use
    your own OAuth credentials** .
2.  Provide the client ID and client secret you [previously
    created](#UsingtheGmailConnector-CreateCredentials) and click
    **Close** .
3.  Now under Step 1, select **Gmail API v1** from the list of APIs,
    select all the scopes listed under it, and click **Authorize APIs**
    .
4.  You will then be prompted to allow permission, click **Allow** .
5.  Under Step 2, click **Exchange authorization code for tokens** to
    generate and display the access token and refresh token.

### Importing the Gmail Connector into WSO2 Integration Studio

1.  Right click on **Sample Services** in the Project Explorer and
    Select **Add or Remove Connector** .
2.  Select **Add Connector** and click **Next** .
3.  Select **Connector Store location** and click **Connect** to connect
    to [WSO2 Connector store](https://store.wso2.com) .
4.  Scroll down and select **Gmail** from the list of connectors.  

    ![](attachments/119132294/119132300.png){width="500" height="546"}

5.  Click **Finish** .  
    The connector is now downloaded into your WSO2 Integration Studio
    and the connector operations will be available in the Gmail
    Connector palette.  

    ![](attachments/119132294/119132314.png){width="198" height="400"}

Let's use these connector operations in the configuration.

### Creating the Deployable Artifacts

The connector operations are used in the
**PaymentRequestProcessingSequence** . Select this sequence, and do the
following updates:

1.  Add a Property Mediator just before the Call mediator to retrieve
    and store the patient's email address.

    ![](attachments/119132294/119132299.png){width="800" height="281"}  

    With the Property mediator selected, access the **Property** tab of
    the mediator and fill in the information in the following table:

    | Field             | Value                      |
    |-------------------|----------------------------|
    | Property Name     | Select **New Property**    |
    | New Property Name | email\_id                  |
    | Property Action   | Select **Set**             |
    | Value Type        | Select **Expression**      |
    | Value Expression  | json-eval($.patient.email) |
    | Description       | Get Email ID               |

2.  Add another Property mediator just after the Log mediator to
    retrieve and store the response sent from SettlePaymentEP. This will
    be used within the body of the email.

    ![](attachments/119132294/119132298.png){width="800" height="341"}

    With the Property mediator selected, access the **Property** tab and
    fill in the information in the following table:

    | Field             | Value                   |
    |-------------------|-------------------------|
    | Property Name     | Select **New Property** |
    | New Property Name | payment\_response       |
    | Property Action   | Select **Set**          |
    | Value Type        | Select **Expression**   |
    | Value Expression  | json-eval($.)           |
    | Description       | Get Payment Response    |

3.  Drag and drop the init method from the **Gmail Connector** palette
    adjoining the Property mediator you added in the previous step.

    ![](attachments/119132294/119132297.png){width="800" height="504"}

    With the init method selected, access the Property tab and fill in
    the information in the following table:

    | Field                 | Value                                                                                                   |
    |-----------------------|---------------------------------------------------------------------------------------------------------|
    | Parameter Editor Type | Select **inline**                                                                                       |
    | userId                | The sender's email address (use the email address that you used to configure the client ID and secret.) |
    | accessToken           | The [access token](#UsingtheGmailConnector-AccessRefreshTokens) you obtained.                           |
    | apiUrl                | <https://www.googleapis.com/gmail>                                                                      |
    | clientId              | The [client ID](_Using_the_Gmail_Connector_) you created.                                               |
    | clientSecret          | The [client secret](#UsingtheGmailConnector-CreateCredentials) you created.                             |
    | refreshToken          | The [refresh token](#UsingtheGmailConnector-AccessRefreshTokens) you obtained.                          |

4.  Add the sendMail method from the Gmail Connector palette and access
    the **Property** tab and fill in the information in the following
    table:

    <table>
    <thead>
    <tr class="header">
    <th>Field</th>
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

    The updated **PaymentRequestProcessingSequence** should now look
    like this:  
    ![](attachments/119132294/119132296.png){width="800"}

5.  Save the updated sequence configuration.
6.  Right click on **SampleServicesConnectorExporter** and navigate to
    **New →  Other → Add/Remove Connectors** and select **Add
    connector** and click on **Next** . Select **Workspace** to list
    down the connectors that were added.  

    ![](attachments/119132294/119132310.png){width="500" height="418"}

    ![](attachments/119132294/119132295.png){width="500" height="467"}

      
    Select the Gmail connector from the list and click **OK** and then
    **Finish** .

### Packaging the artifacts

Since you added a connector to the Connector Exporter Project, this will
need to be packaged into our existing C-App.

[Package](https://docs.wso2.com/display/ADMIN44x/Packaging+Artifacts+into+Composite+Applications)
the C-App names SampleServicesCompositeApplication project with the
artifacts created.

!!! note

Ensure the following artifact check boxes are selected in the
**Composite Application Project POM Editor** .

-   SampleServices
    -   HealthcareAPI
    -   ClemencyEP
    -   GrandOakEP
    -   PineValleyEP
    -   ChannelingFeeEP
    -   SettlePaymentEP
-   SampleServicesRegistry
-   SampleServicesConnectorExporter


### Starting the Message Broker runtime

Start the message broker profile as follows:

1.  Open a terminal and navigate to the
    `          <EI_HOME>/wso2/broker/bin         ` directory.
2.  Start the runtime by executing the message broker startup script as
    shown below.

    -   [**On MacOS/Linux/CentOS**](#3073d27ea0d14344a182d76d9109d720)
    -   [**On Windows**](#06e33815543641b4ade207126eb746f4)

    ``` java
        sh wso2server.sh
    ```

    ``` java
            wso2server.bat
    ```

The message broker profile is now ready to receive messages from the ESB
profile.

### Starting the MSF4J profile

To be able to send requests to the back-end service (which is an MSF4J
service deployed in MSF4J profile), you need to first start the MSF4J
runtime:

1.  Open a terminal and navigate to the
    `          <EI_HOME>/wso2/msf4j/bin         ` directory.
2.  Start the runtime by executing the MSF4J startup script as shown
    below.

    -   [**On MacOS/Linux/CentOS**](#7ca8cb547cb44e91af2d582dd6a15120)
    -   [**On Windows**](#5e42add95fb549069c337bd05babc9ba)

    ``` java
            sh carbon.sh
    ```

    ``` java
            carbon.bat
    ```

The Healthcare service is now active and you can start sending requests
to the service.

### Starting the ESB profile and deploying the artifacts

Assuming you have already added a server for the ESB profile in Eclipse,
on the **Servers** tab, expand the WSO2 Carbon server, right-click
**SampleServicesCompositeApplication** , and choose **Redeploy** . The
Console window of the ESB profile will indicate that the CApp has been
deployed successfully.

![](attachments/119132294/119132301.png){width="533" height="250"}

!!! info

-   If you do not have a server added in Eclipse, refer [this
    tutorial](Sending-a-Simple-Message-to-a-Service_119132413.html#SendingaSimpleMessagetoaService-StartingtheESBprofileanddeployingtheartifacts)
    .
-   You can also deploy the artifacts to the ESB server using a
    [Composite Application Archive (CAR)
    file](https://docs.wso2.com/display/ADMIN44x/Packaging+Artifacts+into+Composite+Applications#PackagingArtifactsintoCompositeApplications-createCARCreatingaCompositeApplicationArchive(CAR)file)
    .


### Sending requests to the ESB

1.  Create a JSON file names `           request.json          ` with
    the following request payload. Make sure you provide a valid email
    address so that you can test the email being sent to the patient.

    ``` java
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

2.  Open a command line terminal and execute the following command from
    the location where `           request.json          ` file you
    created is saved:

    ``` java
            curl -v -X POST --data @request.json http://localhost:8280/healthcare/categories/surgery/reserve --header "Content-Type:application/json"
    ```

        !!! info
    
        This is derived from the [URI-Template
        defined](https://docs.wso2.com/display/EI600/Routing+Requests+Based+on+Message+Content#RoutingRequestsBasedonMessageContent-uriTemplate)
        when creating the API resource.
    
        `           http://<host>:<port>/categories/{category}/reserve          `
    

    You will see the response as follows:

    ``` java
        {"message":"Payment request successfully submitted. Payment confirmation will be sent via email."}
    ```

    An email will be sent to the provided patient email address with the
    following details:

    ``` java
            Subject: Payment Status
             
            Message:
            {"appointmentNo":2,"doctorName":"thomas collins","patient":"John
            Doe","actualFee":7000.0,"discount":20,"discounted":5600.0,"paymentID":"8458c75a-c8e0-4d49-8da4-5e56043b1a20","status":"Settled"}
    ```

You have now explored how to import the Gmail connector to the ESB
profile of WSO2 EI and then use the connector operation to send emails.
