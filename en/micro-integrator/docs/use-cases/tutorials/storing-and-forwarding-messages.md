# Asynchronous Messaging

## What you'll build

Store and forward messaging is used for serving traffic to back-end services that can accept request messages only at a given rate. This is also used for guaranteed delivery to ensure that request received never gets lost since they are stored in the message store and also available for future reference.

**In this tutorial**, instead of sending the request directly to the back-end service, you store the request message in WSO2 Message Broker. You will then use a **Message
Processor** to retrieve the message from the store before delivering it to the back-end service.

## Let's get started!

### Step 1: Set up the workspace

To set up the tools:

-   Download the relevant [WSO2 Integration Studio](https://wso2.com/integration/tooling/) based on your operating system. The path to the extracted/installed folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.
-  Download the [CLI Tool](https://wso2.com/integration/micro-integrator/install/) for monitoring artifact deployments.

If you did not try the [Exposing Several Services as a Single Service](exposing-several-services-as-a-single-service.md) tutorial yet, open WSO2 Integration Studio, click **File** , and then click **Import** . Next, select **Existing WSO2 Projects into workspace** under the **WSO2** category, click **Next** and upload the [pre-packaged project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/ExposingSeveralServicesTutorial.zip).

### Step 2: Develop the integration artifacts

#### Create the Message Store

Now, let's create a Message Store artifact to represent the broker.

1.  Right-click on **SampleServices** in the Project Explorer and navigate to **New->Message Store**.
2.  Select **Create a new message-store artifact** and specify the following details:

    <table>
    <tr>
        <th>Property</th>
        <th>Value</th>
        <th>Description</th>
    </tr>
    <tr>
    <td>Message Store Name</td>
    <td><code>PaymentRequestMessageStore</code></td>
    <td>The name of the message store.</td>
    </tr>
    <tr>
    <td>Message Store Type</td>
    <td>WSO2 MB Message Store</td>
    <td>An instance of WSO2 Message Broker will be used as the broker.</td>
    </tr>
    <tr>
    <td>Queue Connection Factory</td>
    <td><code>amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'</code>
    </td>
    <td>
        The connection factory URL for the WSO2 MB broker. Be sure to change the port to 5675.
    </td>
    </tr>
    <tr>
    <td>JNDI Queue Name</td>
    <td>PaymentRequestJMSMessageStore</td>
    <td>The name of the queue that will be created in WSO2 MB.</td>
    </tr>
    </table>

    ![](../../assets/img/tutorials/119132268/119132276.png)

3.  Click **Finish**.

#### Create the Response Sequence

Let's create a Sequence that uses the message in the message store to send the request to SettlePaymentEP.

1.  Right click the **SampleServices** project in the Project Explorer and navigate to **New -> Sequence**. 
2.  Select **Create New Sequence** and provide the name **PaymentRequestProcessingSequence**.

    ![](../../assets/img/tutorials/119132268/119132273.png)  

3.  Click **Finish**.

4.  In the sequence you have created in the previous step, drag and drop a Call mediator from the **Mediators** palette 
and add SettlePaymentEP from **Defined 
Endpoints** palette to the empty box adjoining the Call mediator. This sends the request message from the store to SettlePaymentEP.

    ![](../../assets/img/tutorials/119132268/119132272.png)

4.  Drag and drop a Log mediator from the **Mediators** palette to log the response from SettlePaymentEP. Access the **Property** tab and specify the following details:

    | Field        | Value           |
    |--------------|-----------------|
    | Log Category | INFO |
    | Log Level    | FULL |

4.  Add a Drop mediator from the **Mediators** palette. You should now have a completed sequence configuration that looks like this:

    ![](../../assets/img/tutorials/119132268/119132271.png)

5.  Save the updated REST API configuration.


#### Create the Message Processor

Let's create a **Message Sampling Processor** to dispatch the request message from the **Message Store** to the **PaymentRequestProcessingSequence**.

!!! Info
    You can also use the **Scheduled Message Forwarding Processor** here and define the endpoint within the processor. The Message Sampling Processor is used because you need to perform mediation on the request message in the next tutorial.

1.  Right-click the **SampleServices** project in the Project Explorer and navigate to **New -> Message Processor**. Select **create a new message-processor artifact** and specify the details shown below:
    <table>
        <tr class="header">
        <th>Property</th>
        <th>Value</th>
        <th>Description</th>
        </tr>
        <tr class="odd">
        <td>Message Processor Type</td>
        <td>Message Sampling Processor</td>
        <td><p>This processor takes the message from the store and puts it into a sequence.</p></td>
        </tr>
        <tr class="even">
        <td>Message Processor Name</td>
        <td>PaymentRequestProcessor</td>
        <td>The name of the scheduled message forwarding processor.</td>
        </tr>
        <tr class="odd">
        <td>Message Store</td>
        <td>PaymentRequestMessageStore</td>
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
        <li>Select <strong>PaymentRequestProcessingSequence</strong> and click <strong>OK</strong>.</li>
        </ol>
        </div></td>
        <td>The name of the sequence to which the message from the store needs to be sent.</td>
        </tr>
    </table>

    ![](../../assets/img/tutorials/119132268/119132269.png)

2.  Click **Finish**.

#### Update the mediation flow

Let's update the REST API so that the messages sent to SettlePaymentEP is forwarded to the message store we created above.

1.  Drag and add a **Store** mediator from the mediators palette just after the PayloadFactory mediator.

    ![](../../assets/img/tutorials/119132268/119132275.png)

2.  With the Store mediator selected, access the **Property** tab and specify the following details:

    | Field                   | Description                                                       |
    |-------------------------|-------------------------------------------------------------------|
    | Available Message Store | Select **PaymentRequestMessageStore**                             |
    | Message Store           | Double click to populate the value **PaymentRequestMessageStore** |
    | Description             | Payment Store                                                     |

3.  Let's use a **PayloadFactory** mediator to send a customized response
    message to the client.
    
    Delete the Call mediator by right-clicking on the mediator and selecting **Delete from Model**. Replace this with a PayloadFactory mediator from the Mediators palette to configure the response to be sent to the client. 

4.  With the PayloadFactory mediator selected, access the Property tab and fill in the information in the following table to define a customized message to be returned to the client.

    !!! Tip
        To avoid getting an error message, first select **Media Type** before selecting **Payload**.

    | Field          | Value                                                                                                  |
    |----------------|--------------------------------------------------------------------------------------------------------|
    | Media Type     | json                                                                                                   |
    | Payload Format | Inline                                                                                                 |
    | Payload        | `{"message":" Payment request successfully submitted. Payment confirmation will be sent via email ."}` |

    
You should now have a completed configuration that looks like this:  

![](../../assets/img/tutorials/119132268/119132274.png)

We have now finished creating all the required artifacts.

### Step 3: Package the artifacts

Package the artifacts in your composite application project (SampleServicesCompositeApplication project) and the registry resource project (SampleRegistryResource project) to be able to deploy the artifacts in the server.

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

2. Add the following JAR files from the `MB_HOME/wso2/broker/client-lib/` directory to the 
`MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/lib/` (in MacOS) or 
`MI_TOOLING_HOME/runtime/microesb/lib` (in Windows) directory.
    -   andes-client-*.jar
    -   geronimo-jms_1.1_spec-*.jar
    -   org.wso2.securevault-*.jar
3. Open the `deployment.toml` file from the `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/conf/` (in MacOS) or 
`MI_TOOLING_HOME/runtime/microesb/conf/` (in Windows) directory and add the configurations given below. This is required for enabling the broker to store messages.

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

Let's send a request to the API resource.

1.  Create a JSON file names `request.json` with the following request payload.

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
    "cardNo": "7844481124110331"
    }
    ```
2.  Open a command line terminal and execute the following command from the location where `request.json` file you created is saved:

    ```bash
    curl -v -X POST --data @request.json http://localhost:8290/healthcare/categories/surgery/reserve --header "Content-Type:application/json"
    ```

    !!! Info
        This URL format is from the **URI-Template** defined when creating the API resource QueryDoctorAPI: `http://<host>:<port>/categories/{category}/reserve`
    
#### Analyze the response

You will see the response as follows:

```json
{"message":"Payment request successfully submitted. Payment confirmation will be sent via email."}
```

Check the WSO2 Integration Studio's **Console** tab and you will see that the response from SettlePaymentEP is logged as follows:

```bash
[2017-04-30 14:33:48,578] [EI-Core]  INFO - LogMediator message = Routing to grand oak community hospital
        
[2017-04-30 14:33:48,598] [EI-Core]  INFO - TimeoutHandler This engine will expire all callbacks after GLOBAL_TIMEOUT: 120 seconds, irrespective of the timeout action, after the specified or optional timeout
        
2017-04-30 14:33:53,464] [EI-Core]  INFO - LogMediator To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:a2cf1fd2-7a89-44b6-9571-990bbdfbd289, Direction: request, Payload: {"appointmentNo":1,"doctorName":"thomas collins","patient":"John Doe","actualFee":7000.0,"discount":20,"discounted":5600.0,"paymentID":"a77038e9-3e42-46f7-ac97-11e1b3a50018","status":"Settled"}
```

You have now explored how the Micro Integratorr can be used to implement store and forward messaging using a **Message Store**, **Message Processors**, and the **Store Mediator**.
