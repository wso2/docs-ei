# Storing and Forwarding Messages

Store and forward messaging is used for serving traffic to back-end
services that can accept request messages only at a given rate. This is
also used for guaranteed delivery to ensure that request received never
gets lost since they are stored in the message store and also available
for future reference.

**In this tutorial,** instead of sending the request directly to the
back-end service, you store the request message in the [Message Broker
profile](https://docs.wso2.com/display/EI650/Message+Brokering) of WSO2
EI. You then use a [Message
Processor](https://docs.wso2.com/display/EI650/Message+Processors) to
retrieve the message from the store and then deliver the message to the
back-end service.

!!! note

See the following topics for a description of the **concepts** that you
need to know when creating ESB artifacts:

-   [REST
    APIs](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-RESTAPIs)
-   [Endpoints](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Messageexitpoints)
-   [Sequences](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Sequences)
-   [Composite Application Project
    (C-App)](https://docs.wso2.com/display/EI600/Key+Concepts#KeyConcepts-CompositeApplicationProject)
-   [Store and
    Forward](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Messagestoringandforwarding)

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
    `           <EI_TOOLING>          ` throughout this tutorial.

    !!! note

    Getting an error message? See the troubleshooting tips given under
    [Installing WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio#InstallingWSO2IntegrationStudio-Troubleshooting)
    .


4.  If you did not try the [Exposing Several Services as a Single
    Service](_Exposing_Several_Services_as_a_Single_Service_) tutorial
    yet, open WSO2 Integration Studio, click **File** , and then click
    **Import** . Next, select **Existing WSO2 Projects into workspace**
    under the **WSO2** category, click **Next** and upload the
    [pre-packaged
    project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/ExposingSeveralServicesTutorial.zip)
    . This contains the configurations of the [Exposing Several Services
    as a Single
    Service](_Exposing_Several_Services_as_a_Single_Service_)
    tutorial so that you do not have to repeat those steps.
5.  Download the MSF4J service from
    [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0.jar)
    and copy the JAR file to
    `          <EI_HOME>/wso2/msf4j/deployment/microservices         `
    folder. The back-end service is now deployed in the MSF4J profile of
    WSO2 EI.


Let's get started!

-   [Setting up the message broker
    profile](#StoringandForwardingMessages-Settingupthemessagebrokerprofile)
-   [Creating the Message
    Store](#StoringandForwardingMessages-CreatingtheMessageStore)
-   [Creating the Deployable
    Artifacts](#StoringandForwardingMessages-CreatingtheDeployableArtifacts)
-   [Creating the Response
    Sequence](#StoringandForwardingMessages-#PaymentRequestProcessingSequenceCreatingtheResponseSequence)
-   [Creating the Message
    Processor](#StoringandForwardingMessages-CreatingtheMessageProcessor)
-   [Packaging the
    artifacts](#StoringandForwardingMessages-Packagingtheartifacts)
-   [Starting the Message Broker
    runtime](#StoringandForwardingMessages-StartingtheMessageBrokerruntime)
-   [Starting the MSF4J
    profile](#StoringandForwardingMessages-StartingtheMSF4Jprofile)
-   [Starting the ESB profile and deploying the
    artifacts](#StoringandForwardingMessages-StartingtheESBprofileanddeployingtheartifacts)
-   [Sending requests to the
    ESB](#StoringandForwardingMessages-SendingrequeststotheESB)

### Setting up the message broker profile

The message broker profile (which is an instance of WSO2 Message Broker)
is shipped with the WSO2 EI product distribution. You need to enable the
following configurations to be able to store messages in the broker
profile.

Open the
`         <EI_HOME>/conf/jndi                   .properties         `
file and add the following line after the
`         queue.MyQueue = example.MyQueue        ` line:

``` java
    queue.PaymentRequestJMSMessageStore=PaymentRequestJMSMessageStore
```

### Creating the Message Store

Now, let's create a message store artifact in WSO2 Integration Studio.

1.  Right click on **SampleServices** in the Project Explorer and
    navigate to **New-\>Message Store** .
2.  Select **Create a new message-store artifact** and fill in the
    information in the following table and click **Finish.**

    <table>
    <tbody>
    <tr class="odd">
    <td>Message Store Name</td>
    <td><code>               PaymentRequestMessageStore              </code></td>
    </tr>
    <tr class="even">
    <td>Message Store Type</td>
    <td>Select <strong>WSO2 MB Message Store</strong></td>
    </tr>
    <tr class="odd">
    <td>Queue Connection Factory</td>
    <td><p><code>                amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'               </code></p>
        !!! tip
        <p>Change the port to 5675.</p>
    </td>
    </tr>
    <tr class="even">
    <td>JNDI Queue Name</td>
    <td><code>               PaymentRequestJMSMessageStore              </code></td>
    </tr>
    </tbody>
    </table>

    ![](attachments/119132268/119132276.png){width="500" height="652"}

    Click **Finish** .

Let's look at how we update the REST API with the Store mediator.

### Creating the Deployable Artifacts

Let's now update the REST API so that the messages sent to
SettlePaymentEP is forwarded to the message store we created above.

1.  Drag and add a Store Mediator from the mediators palette just after
    the PayloadFactory mediator.  
    ![](attachments/119132268/119132275.png){width="800" height="507"}
2.  With the Store mediator selected, access the **Property** tab and
    fill in the information as in the following table:

    | Field                   | Value                                                             |
    |-------------------------|-------------------------------------------------------------------|
    | Available Message Store | Select **PaymentRequestMessageStore**                             |
    | Message Store           | Double click to populate the value **PaymentRequestMessageStore** |
    | Description             | Payment Store                                                     |

    Let's use a PayloadFactory mediator to send a customized response
    message to the client.

3.  Delete the Call mediator by right clicking on the mediator and
    selecting **Delete from Model** . Replace this with a PayloadFactory
    mediator from the Mediators palette to configure the response to be
    sent to the client. With the PayloadFactory mediator selected,
    access the Property tab and fill in the information in the following
    table to define a customized message to be returned to the client.

    | Field          | Value                                                                                                                               |
    |----------------|-------------------------------------------------------------------------------------------------------------------------------------|
    | Media Type     | Select **json**                                                                                                                     |
    | Payload Format | Select **Inline**                                                                                                                   |
    | Payload        | `               {"message":" Payment request successfully submitted. Payment confirmation will be sent via email ."}              ` |

        !!! tip
    
        To avoid getting an error message, first select **Media Type**
        before selecting **Payload** .
    

    You should now have a completed configuration that looks like
    this:  
    ![](attachments/119132268/119132274.png){width="1000" height="394"}

### Creating the Response Sequence

Let's create a Sequence that uses the message in the message store to
send the request to SettlePaymentEP.

1.  Right click the **SampleServices** project in the Project Explorer
    and navigate to **New -\> Sequence** . Select **Create New
    Sequence** and provide the name **PaymentRequestProcessingSequence**
    .

    ![](attachments/119132268/119132273.png){width="500" height="492"}  
    Click **Finish** .

2.  Drag and drop a Call mediator from the **Mediators** palette and add
    SettlePaymentEP from **Defined Endpoints** palette to the empty box
    adjoining the Call mediator. This sends the request message from the
    store to SettlePaymentEP.

    ![](attachments/119132268/119132272.png){width="500" height="304"}

3.  Drag and drop a Log mediator from the **Mediators** palette to log
    the response from SettlePaymentEP. Access the **Property** tab and
    fill in the following information:

    | Field        | Value           |
    |--------------|-----------------|
    | Log Category | Select **INFO** |
    | Log Level    | Select **FULL** |

4.  Add a Drop mediator from the **M** **ediators** palette.  You should
    now have a completed sequence configuration that looks like this:

    ![](attachments/119132268/119132271.png){width="500" height="346"}

5.  Save the updated REST API configuration.

### Creating the Message Processor

Let's create a [Message Sampling
Processor](https://docs.wso2.com/display/EI650/Message+Sampling+Processor)
to dispatch the request message from the [message
store](#StoringandForwardingMessages-MessageStore) to the
[PaymentRequestProcessingSequence](#StoringandForwardingMessages-PaymentRequestProcessingSequence)
.

!!! info

You can also use the [Scheduled Message Forwarding
Processor](_Storing_and_Forwarding_Messages_) here and define the
endpoint within the processor. The Message Sampling Processor is used
because you need to perform mediation on the request message in the next
tutorial.


Right click the **SampleServices** project in the Project Explorer and
navigate to **New -\> Message Processor** . Select **create a new
message-processor artifact** and fill in the details as in the following
table:

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
<td>Message Processor Type</td>
<td>Select <strong>Message Sampling Processor</strong></td>
<td><p>This processor takes the message from the store and puts it into a sequence.</p></td>
</tr>
<tr class="even">
<td>Message Processor Name</td>
<td>PaymentRequestProcessor</td>
<td>The name of the scheduled message forwarding processor.</td>
</tr>
<tr class="odd">
<td>Message Store</td>
<td>Select <strong>PaymentRequestMessageStore</strong></td>
<td>The message store from which the scheduled message forwarding processor consumes messages.</td>
</tr>
<tr class="even">
<td>Processor State</td>
<td>Activate</td>
<td>Whether the processor needs to be activated or deactivated.</td>
</tr>
<tr class="odd">
<td>Sequence</td>
<td><div class="content-wrapper">
<p>Follow the steps given below:</p>
<ol>
<li>Click <strong>Browse.</strong></li>
<li>Click the <strong>workspace</strong> link.</li>
<li>Click <strong>Carbon Application Sequences &gt; SampleServices</strong> .</li>
<li>Select <strong>PaymentRequestProcessingSequence</strong> and click <strong>OK</strong> .<br />
<img src="attachments/119132268/119132270.png" width="500" height="735" /></li>
</ol>
</div></td>
<td>The name of the sequence the message from the store needs to be sent to.</td>
</tr>
</tbody>
</table>

![](attachments/119132268/119132269.png){width="500" height="733"}

Click **Finish.**

We have now finished creating all the required artifacts.

### Packaging the artifacts

Since you created a message store, sequence and a message processor,
 these will need to be packaged into our existing CApp.

[Package](https://docs.wso2.com/display/ADMIN44x/Packaging+Artifacts+into+Composite+Applications)
the C-App names SampleServicesCompositeApplication project with the
artifacts created. Ensure the following artifact check boxes are
selected in the **Composite Application Project POM Editor** .

-   SampleServices
    -   HealthcareAPI
    -   ClemencyEP
    -   GrandOakEP
    -   PineValleyEP
    -   ChannelingFeeEP
    -   SettlePaymentEP
    -   PaymentRequestMessageStore
    -   PaymentRequestProcessingSequence
    -   PaymentRequestProcessor
-   SampleServicesRegistry

### Starting the Message Broker runtime

Start the message broker profile as follows:

1.  Open a terminal and navigate to the
    `          <EI_HOME>/wso2/broker/bin         ` directory.
2.  Start the runtime by executing the message broker startup script as
    shown below.

    -   [**On MacOS/Linux/CentOS**](#82bdc81baf9f4cee84c22d406d52bc2e)
    -   [**On Windows**](#576b678681eb452cb1bd8180489d46f2)

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
service deployed in the MSF4J profile), you need to first start the
MSF4J runtime:

1.  Open a terminal and navigate to the
    `           <EI_HOME>/wso2/msf4j/bin          ` directory.

2.  Start the runtime by executing the MSF4J startup script as shown
    below.

    -   [**On MacOS/Linux/CentOS**](#4928ef97e3164c0281b9aee3f74753c8)
    -   [**On Windows**](#81ebdc71d10a4560914669121d5bead4)

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

![](attachments/119132268/119132277.png){width="533" height="250"}

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
    the following request payload.

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
        "cardNo": "7844481124110331"
        }
    ```

2.  Open a command line terminal and execute the following command from
    the location where `           request.json          ` file you
    created is saved:

    ``` java
            curl -v -X POST --data @request.json http://localhost:8280/healthcare/categories/surgery/reserve --header "Content-Type:application/json"
    ```

        !!! info
    
        This is from the [URI-Template
        defined](https://docs.wso2.com/display/EI600/Routing+Requests+Based+on+Message+Content#RoutingRequestsBasedonMessageContent-uriTemplate)
        when creating the API resource QueryDoctorAPI.
    
        `           http://<host>:<port>/categories/{category}/reserve          `
    

    You will see the response as follows:

    ``` java
        {"message":"Payment request successfully submitted. Payment confirmation will be sent via email."}
    ```

3.  Now check the ESB server Console in Eclipse and you will see that
    the response from SettlePaymentEP is logged as follows:

    ``` java
            [2017-04-30 14:33:48,578] [EI-Core]  INFO - LogMediator message = Routing to grand oak community hospital
        
            [2017-04-30 14:33:48,598] [EI-Core]  INFO - TimeoutHandler This engine will expire all callbacks after GLOBAL_TIMEOUT: 120 seconds, irrespective of the timeout action, after the specified or optional timeout
        
            [2017-04-30 14:33:53,464] [EI-Core]  INFO - LogMediator To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:a2cf1fd2-7a89-44b6-9571-990bbdfbd289, Direction: request, Payload: {"appointmentNo":1,"doctorName":"thomas collins","patient":"John Doe","actualFee":7000.0,"discount":20,"discounted":5600.0,"paymentID":"a77038e9-3e42-46f7-ac97-11e1b3a50018","status":"Settled"}
    ```

You have now explored how the ESB profile of WSO2 EI can be used to
implement store and forward messaging using Message Stores, Message
Processors, and the Store mediator.
