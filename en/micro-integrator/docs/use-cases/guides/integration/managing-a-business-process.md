# Managing a Business Process

**In this tutorial,** you will use a BPMN process defined in the
[Business Process
profile](https://docs.wso2.com/display/EI650/Key+Concepts) of WSO2 EI to
manage the process of canceling a doctor's appointment. The appointment
cancellation process should be designed according to the following
requirement:

-   The patient should be allowed to cancel the appointment
    (unconditionally) if the appointment date is at least two days away.
-   The patient should not be allowed to cancel the appointment if the
    appointment date is less than two days away.
-   If the appointment date falls on the second day from the current
    date, the patient can request for appointment cancellation, which
    should be manually approved by an administrator.
-   The patient should receive email notifications informing if the
    appointment cancellation is accepted or rejected.

!!! note

See the [Business
processes](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Businessprocess)
topic for a description of the **concepts** that you need to know when
creating the artifacts:

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
4.  The path to this folder will be referred to as
    `           <EI_TOOLING>          ` throughout this tutorial.

    !!! info

    -   For more detailed installation instructions, see the [Installing
        WSO2 Integration
        Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio)
        .
    -   Getting an error message? See the troubleshooting tips given
        under [Installing WSO2 Integration
        Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio#InstallingWSO2IntegrationStudio-Troubleshooting)
        .


5.  You need to try out the [Using the Gmail
    Connector](_Using_the_Gmail_Connector_) tutorial before trying out
    this tutorial as the Gmail connector that is used in this tutorial
    is configured using your client ID and secret.
6.  Download the MSF4J service from
    [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0.jar)
    and copy the JAR file to
    `          <EI_HOME>/wso2/msf4j/deployment/microservices         `
    folder. The back-end service is now deployed in the MSF4J profile of
    WSO2 EI.
7.  Create STMP an email account to send out emails to users that want
    to cancel an appointment using WSO2 Business Process Server (e.g.,
    <no-reply@foo.com> ).


Let's get started!

-   [Configuring the Integration
    artifacts](#ManagingaBusinessProcess-ConfiguringtheIntegrationartifacts)
-   [Creating the appointment cancellation process using
    BPMN](#ManagingaBusinessProcess-AppointmentCancelBarCreatingtheappointmentcancellationprocessusingBPMN)
-   [Deploying the BPMN process in the Business Process
    profile](#ManagingaBusinessProcess-BPMN_artifactsDeployingtheBPMNprocessintheBusinessProcessprofile)
-   [Starting the Message Broker
    profile](#ManagingaBusinessProcess-start_MBStartingtheMessageBrokerprofile)
-   [Deploying the ESB artifacts in the ESB
    profile](#ManagingaBusinessProcess-start_integrationDeployingtheESBartifactsintheESBprofile)
-   [Starting the back-end
    service](#ManagingaBusinessProcess-start_msf4jStartingtheback-endservice)
-   [Sending requests to the
    ESB](#ManagingaBusinessProcess-sending_requestsSendingrequeststotheESB)
-   [Canceling appointments using the BPMN
    explorer](#ManagingaBusinessProcess-canceling_appointmentCancelingappointmentsusingtheBPMNexplorer)

### Configuring the Integration artifacts

1.  Open the ESB Config project that you used when trying out the [Using
    the Gmail Connector](_Using_the_Gmail_Connector_) tutorial.
2.  Open the `          PaymentRequestProcessingSequence.xml         `
    file that is inside the
    `          SampleServices > src > main > synapse-config > sequences         `
    directory.
3.  Add an **Enrich mediator** next to the **call mediator** from the
    Mediators pallet to add the appointment cancellation link to the
    email that is being sent to the patient.
4.  Select the Enrich mediator, go to the Properties tab, and update the
    following:

    <table>
    <thead>
    <tr class="header">
    <th>Field</th>
    <th>Value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Source Type</td>
    <td>Select <strong>inline</strong> from the drop-down.</td>
    </tr>
    <tr class="even">
    <td>Source XML</td>
    <td><div class="content-wrapper">
    <p>Click the browse button, add the code given below, and click <strong>OK</strong> .</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;cancellation_link&gt;https:<span class="co">//localhost:9445/bpmn-explorer/&lt;/cancellation_link&gt;</span></span></code></pre></div>
    </div>
    </div>
    <p>You need to add this link so that it appears in the appointment confirmation email. You can cancel the appointment by clicking on this link and starting the appointment cancellation business process.</p>
    </div></td>
    </tr>
    <tr class="odd">
    <td>Target Action</td>
    <td>Select <strong>child</strong> from the drop-down.</td>
    </tr>
    <tr class="even">
    <td>Target XPath</td>
    <td>Click the browse button, add <code>               //jsonObject              </code> , and click <strong>OK</strong> .</td>
    </tr>
    </tbody>
    </table>

5.  Save the changes you made to the
    `          PaymentRequestProcessingSequence.xml         ` file.  

### Creating the appointment cancellation process using BPMN

1.  Open **WSO2 Integration Studio** .
2.  Go to **BP Project → **BPMN**** .  
    ![](attachments/119132344/119132346.png){width="800" height="311"}
3.  Create a new project named **AppointmentCancellation** .  
    ![](attachments/119132344/119132398.png){width="500"}
4.  Click **Finish.**
5.  Right click the project in the navigator and go to **New → Other**
    and select **BPMN diagram** .  
    ![](attachments/119132344/119132345.png){width="500" height="392"}
6.  Select the
    `          AppointmentCancellation/src/main/resources/diagrams         `
    directory as the **parent folder** , and enter
    `          AppointmentCancellation.bpmn         ` for the **File
    name** .  
    ![](attachments/119132344/119132375.png){width="524" height="535"}
7.  Click **Next,** select the option to create an empty diagram as
    shown below, and click **Finish** .  
    ![](attachments/119132344/119132396.png){width="500" height="508"}
8.  Let's start drawing the BPMN process diagram for the appointment
    cancellation process. The **AppointmentCancellation** canvas. The
    **Palette** on the right side of the **AppointmentCancellation**
    canvas contains all the activity symbols that will be used for
    drawing the process as shown below.  
    ![](attachments/119132344/119132395.png){width="600" height="324"}  
    Let's get started. You can easily drag and drop the activities from
    the **Palette** the canvas, and then update the properties and
    configurations of each activity as explained below.
    1.  Add a **StartEvent** from the **Start Event** palette.  
        ![](attachments/119132344/119132394.png)
    2.  Select the **Properties** tab, click **Form** , click **New** ,
        update the following and click **OK.**

        |          |                     |
        |----------|---------------------|
        | Id       | patientName         |
        | Name     | Name of the Patient |
        | Type     | string              |
        | Required | True                |

        ![](attachments/119132344/119132371.png)

    3.  Repeat clicking **New** and adding the following to add the
        other form properties one by one.

        | Id                                               | Name                    | Type   | Required |
        |--------------------------------------------------|-------------------------|--------|----------|
        | `                 appointmentNo                ` | Appointment Number      | long   | True     |
        | `                 doctor                `        | Name of the Doctor      | string | True     |
        | `                 reason                `        | Reason for cancellation | string | True     |
        | `                 email                `         | Email Address           | string | True     |

        ![](attachments/119132344/119132370.png){height="250"}

    4.  Add a **REST Task** from the **WSO2 Tasks** palette.  
        ![](attachments/119132344/119132392.png)  
        Select the **Properties** tab, click **General** and give the
        **Name** as `             Get Appointment            ` , then
        click **Main config** and update the following:
        `                         `

        |                          |                                                                                                             |
        |--------------------------|-------------------------------------------------------------------------------------------------------------|
        | Service URL              | http `                 ://localhost:9090/healthcare/appointments/validity/${appointmentNo}                ` |
        | HTTP method              | GET                                                                                                         |
        | Output Variable Mappings | `                 noOfDays:$.status                `                                                        |

          
        ![](attachments/119132344/119132380.png){width="700"}

    5.  Add an **Exclusive Gateway** from the **Gateway** palette.  
        ![](attachments/119132344/119132369.png)
    6.  Add a **User Task** from the **Task** palette and name it
        `            Appointment Control Task           ` .  
        ![](attachments/119132344/119132368.png)
    7.  Click **Properties** , click **Main config** and give
        `            admin           ` as the **Assignee** .  
        ![](attachments/119132344/119132354.png)  
    8.  In the **Properties** tab click **Form** , click **New** and add
        the following.

        <table>
        <thead>
        <tr class="header">
        <th>Id</th>
        <th>Name</th>
        <th>Type</th>
        <th>Readable</th>
        <th>Writable</th>
        <th>Required</th>
        </tr>
        </thead>
        <tbody>
        <tr class="odd">
        <td><code>                 approveCancellation                </code></td>
        <td>Do You Approve the<br />
        Appointment Cancellation Request?</td>
        <td>enum</td>
        <td>True</td>
        <td>True</td>
        <td>True</td>
        </tr>
        </tbody>
        </table>

        Click **New** next to **Form values** and add the following:

        | Id      | Name    |
        |---------|---------|
        | approve | Approve |
        | reject  | Reject  |

        ![](attachments/119132344/119132352.png){width="600"
        height="507"}

    9.  Repeat clicking **New** and adding the following to add the
        other form properties one by one.

        | Id                                                    | Name                        | Type   | Variable      | Readable | Writable | Required |
        |-------------------------------------------------------|-----------------------------|--------|---------------|----------|----------|----------|
        | `                 AppNo                `              | Appointment Number          | long   | appointmentNo | True     | False    | True     |
        | `                 cancellationReason                ` | Reason For The Cancellation | string | reason        | True     | False    | True     |
        | `                 doctorName                `         | Name of the Doctor          | string | doctor        | True     | False    | True     |

        ![](attachments/119132344/119132351.png){width="1010"
        height="250"}

    10. Add another **Exclusive Gateway** from the ****Gateway****
        palette.  
        ![](attachments/119132344/119132367.png)
    11. Add a **REST Task** from the ******WSO2 Tasks****** palette.  
        ![](attachments/119132344/119132366.png)  

        Select the **Properties** tab and click **Main config** and
        update the following: `                         `

        |                      |                                                                                                            |
        |----------------------|------------------------------------------------------------------------------------------------------------|
        | Service URL          | `                 http://localhost:9090/healthcare/appointments/validity/${appointmentNo}                ` |
        | HTTP method          | DELETE                                                                                                     |
        | Output Variable name | status                                                                                                     |

        ![](attachments/119132344/119132355.png)

    12. Add a **MailTask** from the **Task** palette below the second
        Parallel Gateway and name it
        `             Reject cancellation Task            ` .  
        ![](attachments/119132344/119132365.png)  
        Select the **Properties** tab and click **Main config** and
        update the following:

        <table>
        <tbody>
        <tr class="odd">
        <td>To</td>
        <td><code>                 ${email}                </code></td>
        </tr>
        <tr class="even">
        <td>From</td>
        <td>Enter a valid SMTP email address.</td>
        </tr>
        <tr class="odd">
        <td>Subject</td>
        <td>Appointment cancellation rejected</td>
        </tr>
        <tr class="even">
        <td>Html</td>
        <td>Appointment cancellation request with Appointment Number ${appointmentNo} has been rejected. Thank you.<br />
        Administration Department</td>
        </tr>
        </tbody>
        </table>

        ![](attachments/119132344/119132382.png){width="600"}

    13. Add another MailTask above the second Parallel Gateway and name
        it `             Approve Cancellation Task            ` .  
        ![](attachments/119132344/119132364.png){height="250"}  
          
        Select the **Properties** tab and click **Main config** and
        update the following:

        <table>
        <tbody>
        <tr class="odd">
        <td>To</td>
        <td><code>                 ${email}                </code></td>
        </tr>
        <tr class="even">
        <td>From</td>
        <td>Enter a valid SMTP email address.</td>
        </tr>
        <tr class="odd">
        <td>Subject</td>
        <td>Appointment cancellation successful</td>
        </tr>
        <tr class="even">
        <td>Html</td>
        <td>Appointment cancellation request with Appointment Number ${appointmentNo} has been approved. Thank you.<br />
        Administration Department</td>
        </tr>
        </tbody>
        </table>

        ![](attachments/119132344/119132383.png){width="600"}

    14. Link each activity by hovering over the element, selecting the
        arrow sign and dragging it to the connecting element as shown
        below.  
        ![](attachments/119132344/119132363.png){height="250"}

9.  Click on the arrow leading from the first **Exclusive Gateway** to
    the `          Approve Cancellation Task         ` , and in the
    **Main config** section of the **Properties** tab add the
    `          ${noOfDays >2}         ` condition.  
    ![](attachments/119132344/119132362.png)
10. Click on the arrow leading from the first **Exclusive Gateway** to
    the `          Reject Cancellation Task         ` , and in the
    **Main config** section of the **Properties** tab add the
    `          ${noOfDays<2}         ` condition.  
    ![](attachments/119132344/119132361.png)
11. Click on the arrow leading from the second **Exclusive Gateway** to
    the `          Rest Task         ` , and in the **Main config**
    section of the **Properties** tab add the
    `          ${approveCancellation == 'approve'}         `
    condition.  
    ![](attachments/119132344/119132360.png)
12. Click on the arrow leading from the second **Exclusive Gateway** to
    the `          Reject Cancellation Task         ` , and in the
    **Main config** section of the **Properties** tab add the
    `          ${approveCancellation == 'reject'}         ` condition.  
    ![](attachments/119132344/119132359.png)
13. Click the arrow leading to the **Appointment Control Task** , open
    the **General** tab that is under properties and note the **Id** .  
    Example:  
    ![](attachments/119132344/119132349.png){width="800" height="439"}
14. Click the first exclusive gateway, open the **General** tab that is
    under properties, and select the ID you noted in the above steps as
    the value for **Default flow.  
    ![](attachments/119132344/119132348.png){width="800"}  
    **
15. Save all the changes you have made.
16. Click **Window** **-\>** **Show View** **-\>** **Other** in the top
    menu of WSO2 Integration Studio.  
    ![](attachments/119132344/119132374.png){width="900"}
17. Type `          Package Explorer         ` in the search bar, select
    it and click **Open** .  
    ![](attachments/119132344/119132350.png){width="300"}
18. In the **Package Explorer** , right-click the
    `          AppointmentCancellation         ` project and select
    **Create deployment artifacts** .  
    ![](attachments/119132344/119132373.png){width="900"}  
    This will create a `          AppointmentCancellation.bar         `
    file in the `          deployment         ` folder:  
    ![](attachments/119132344/119132372.png)

### Deploying the BPMN process in the Business Process profile

!!! info

**Before you begin** , apply the following configurations to the
business process profile:

1.  Copy the following JARs to the `          <EI_HOME>/lib         `
    directory.
    -   `                           org.apache.commons:commons-email:jar:1.3                         `

    -   [javax.mail:mail:jar:1.4.7](http://central.maven.org/maven2/javax/mail/javax.mail-api/1.4.7/javax.mail-api-1.4.7.jar)

    -   `                           javax.activation:activation:jar:1.1                         `

2.  Open the `           activiti.xml          ` file in the
    `           <EI_HOME>/wso2/business-process/conf          `
    directory and add the following properties under
    `           <bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">          `
    as shown below to enable the Gmail configurations.

    -   Replace the `             mailServerHost            ` and
        `             mailServerPort            ` property values as
        follows:

        ``` java
                <property name="mailServerHost" value="smtp.gmail.com"/>
                <property name="mailServerPort" value="587"/>
        ```
        
            -   Add the following properties:
        
        ``` java
                    <property name="mailServerDefaultFrom" value="<ENTER_A_VALID_SMTP_EMAIL_ADDRESS>"/>
                    <property name="mailServerUseTLS" value="true"/>
                    <property name="mailServerUsername" value="<ENTER_A_VALID_SMTP_EMAIL_ADDRESS>"/>
                    <property name="mailServerPassword" value="<ENTER_THE_EMAIL_PASSWORD>"/>
        ```
            

You can now deploy the .bar file of the BPMN process using the
management console of the business process profile in WSO2 EI.

1.  Open a terminal and navigate to the
    `           <EI_HOME>/wso2/business-proces/bin          ` directory.

2.  Start the Business Process profile in WSO2 EI by executing one of
    the following commands:

    -   [**On MacOS/Linux/CentOS**](#f7865274cda04b5a93759a03b3de9b05)
    -   [**On Windows**](#374a40a0680d4a67a4e63843845302c3)

    ``` java
        sh wso2server.sh
    ```

    ``` java
            wso2server.bat
    ```

3.  Open the management console from
    `                     https://localhost:9445/carbon/                   `
    .
4.  Log in using admin as the username and password.
5.  Go to **Main -\> Manage -\> Add -\>BPMN** and upload the
    `          AppointmentCancellation.bar         ` file by browsing in
    the workspace directory of your WSO2 Integration Studio instance as
    shown below.  
    ![](attachments/119132344/119132377.png)

### Starting the Message Broker profile

Let's start the Message Broker profile:

1.  Open a terminal and navigate to the
    `           <EI_HOME>/wso2/broker/bin          ` directory.

2.  Start the Business Process profile in WSO2 EI by executing one of
    the following commands:

    -   [**On MacOS/Linux/CentOS**](#b141c3f528c840df9a65d0134c635953)
    -   [**On Windows**](#d8179ac3f84945ee8e5a5376c915416c)

    ``` java
            sh wso2server.sh
    ```

    ``` java
            wso2server.bat
    ```

### Deploying the ESB artifacts in the ESB profile

!!! info

**Before you start** the ESB profile, configure the profile so that it
can connect to the Message Broker profile for reliable messaging. To do
this, open the `         <EI_HOME>/conf/jndi.properties        ` file
and update the connection factories and queues at the beginning of the
file so that it now looks like this:

``` java
    # register some connection factories
    # connectionfactory.[jndiname] = [ConnectionURL]
    connectionfactory.QueueConnectionFactory = amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'
    connectionfactory.TopicConnectionFactory = amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'
    # register some queues in JNDI using the form
    # queue.[jndiName] = [physicalName]
    queue.MyQueue = example.MyQueue
    queue.PaymentRequestJMSMessageStore=PaymentRequestJMSMessageStore
```
    

Now, let's start the ESB profile in WSO2 EI and upload the ESB
artifacts:

Assuming you have already added a server for the ESB profile in Eclipse,
on the **Servers** tab, expand the WSO2 Carbon server, right-click
**SampleServicesCompositeApplication** , and choose **Redeploy** . The
Console window of the ESB profile will indicate that the CApp has been
deployed successfully.

![](attachments/119132344/119132347.png){width="533" height="250"}

!!! info

-   If you do not have a server added in Eclipse, refer [this
    tutorial](https://docs.wso2.com/display/EI6xx/Sending+a+Simple+Message+to+a+Service+Using+the+ESB+Profile#SendingaSimpleMessagetoaServiceUsingtheESBProfile-StartingtheESBprofileanddeployingtheartifacts)
    .
-   You can also deploy the artifacts to the ESB server using a
    [Composite Application Archive (CAR)
    file](https://docs.wso2.com/display/ADMIN44x/Packaging+Artifacts+into+Composite+Applications#PackagingArtifactsintoCompositeApplications-createCARCreatingaCompositeApplicationArchive(CAR)file)
    .


### Starting the back-end service

To be able to send requests to the back-end service (which is an MSF4J
service deployed in the MSF4J profile), you need to first start the
MSF4J runtime:

1.  Open a terminal and navigate to the
    `          <EI_HOME>/wso2/msf4j/bin         ` directory.
2.  Start the runtime by executing the MSF4J startup script as shown
    below.

    -   [**On MacOS/Linux/CentOS**](#e01d219c67b54cc6bdaf051bf3e6241f)
    -   [**On Windows**](#d6b9b427b6c44680b50f01e5b384adaa)

    ``` java
        sh carbon.sh
    ```

    ``` java
            carbon.bat
    ```

The system is now ready to receive appointment reservation requests.

### Sending requests to the ESB

To demonstrate how the BPMN process for appointment cancellation works,
let's send three separate requests to the ESB profile of WSO2 EI. These
requests will create three separate doctor's appointments on three
separate dates. You will receive confirmation emails for all of the
appointments.

1.  Create a JSON file named `           request.json          ` with a
    request payload as shown below. Note the following when you create
    the request payloads.

        !!! note
    
        Make sure you **provide a valid email address** so that you can test
        the email being sent to the patient.
    

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

2.  Open a command line terminal and navigate to the location where you
    have stored the above request payloads.
3.  Execute the following command, which will send the request1.json
    payload as a request.

    ``` java
            curl -v -X POST --data @request.json http://localhost:8280/healthcare/categories/surgery/reserve --header "Content-Type:application/json"
    ```

    You will see the response as follows:

    ``` java
            {"message":"Payment request successfully submitted. Payment confirmation will be sent via email."}
    ```

    An email will be sent to the provided patient email address with the
    following details:

    ``` java
            Subject: Payment Status
             
            Message: 
            {"appointmentNo":1,"doctorName":"thomas collins","patient":"John
            Doe","actualFee":7000.0,"discount":20,"discounted":5600.0,"paymentID":"8c6e153e-4034-4d5a-b011-391553b15a8f","status":"Settled","cancellation_link":"https://localhost:9445/bpmn-explorer/"}
    ```

!!! info

When configuring the artifacts, you set conditions to [reject the
cancellation request](#ManagingaBusinessProcess-reject) if it is less
than 2 days. Therefore, you can try setting the appointment date to less
than 2 days from the current date and schedule a request too. You won't
be able to cancel the request when trying out the steps under [Canceling
appointments using the BPMN
explorer](#ManagingaBusinessProcess-CancelingappointmentsusingtheBPMNexplorer)
.


### Canceling appointments using the BPMN explorer

Let's now look at how to cancel the doctor's appointments. If you
successfully, completed the previous steps, you will now have three
emails confirming three appointments. The cancellation link is provided
in each mail.

1.  Click the cancellation link in one of the mails, which will take you
    to the **BPMN explorer** .  
    ![](attachments/119132344/119132402.png){width="500"}
2.  Log in to the BPMN explorer using admin as the username and
    password.
3.  Click **Processes** and see that an appointment cancellation process
    is listed as shown below.  
    ![](attachments/119132344/119132401.png){height="250"}
4.  Click **Start** to begin the cancellation process and the **Start
    Process** form will open as shown below.  
    ![](attachments/119132344/119132400.png){width="601"}
5.  Let's cancel the appointment that was confirmed.  
    1.  Enter the appointment details of the appointment that was
        emailed to you and click **Start** . Be sure to enter the
        correct appointment number and email address.
    2.  Since the appointment is more than two days from the current
        date, the appointment cancellation will be approved
        automatically.
    3.  You will receive an email confirming that the appointment
        cancellation is approved.

!!! info

-   If you try to cancel an appointment that is less than two days away
    from the current date, you r eceive an email rejecting the
    cancellation of your appointment.
-   If the appointment date is exactly two days from the current date
    the appointment cancellation will neither be approved or rejected
    automatically. To cancel the request, follow the steps given below.

1.  1.  Click **TASKS** on the menu bar and see that a manual task has
        been created for the appointment cancellation.  
        ![](attachments/119132344/119132378.png){width="800"}
    2.  You can now assume the role of an administrator in the hospital
        service and determine if the appointment cancellation should be
        approved or rejected.
    3.  Let's approve the request: Select **Approve** and click
        **Complete Task** in the appointment control task shown above.
    4.  You will receive an email confirming that the appointment
        cancellation is approved.


**Congratulations, you have successfully completed the tutorial!**

## What's next?

Want to check out the statistics of the business processor? Try out the
[Analyzing Business Process
Statistics](_Analyzing_Business_Process_Statistics_) tutorial.
