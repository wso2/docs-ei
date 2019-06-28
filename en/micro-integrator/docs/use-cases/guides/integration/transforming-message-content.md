# Transforming Message Content

Message transformation is necessary when the message format sent by the
client is different from the message format expected by the back-end
service. The [Message
Translator](https://docs.wso2.com/display/EIP/Message+Translator)
architectural pattern in the ESB of WSO2 Enterprise Integrator (WSO2 EI)
describes how to translate from one data format to another.

**In this tutorial** , you send a request message to a back-end service
where the payload is in a different format when compared to the request
payload expected by the back-end service. The [Data Mapper
mediator](https://docs.wso2.com/display/EI650/Data+Mapper+Mediator) in
the ESB is used to transform the request message payload to the format
expected by the back-end service.

!!! note

See the following topics for a description of the **concepts** that you
need to know when creating ESB artifacts:

-   [REST
    API](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-RESTAPIs)
-   [Endpoints](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Messageexitpoints)
-   [Sequences  
    ](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Sequences)

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


4.  If you did not try the [Routing Requests Based on Message
    Content](_Routing_Requests_Based_on_Message_Content_) tutorial yet,
    open WSO2 Integration Studio, click **File** , and then click
    **Import** . Next, select **Existing WSO2 Projects into workspace**
    under the **WSO2** category, click **Next** and upload the
    [pre-packaged
    project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/RequestRoutingTutorial.zip)
    . This contains the configurations of the [Routing Requests Based on
    Message Content](_Routing_Requests_Based_on_Message_Content_)
    tutorial so that you do not have to repeat those steps.
5.  Download the MSF4J service from
    [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0.jar)
    and copy the JAR file to
    `          <EI_HOME>/wso2/msf4j/deployment/microservices         `
    folder. The back-end service is now deployed in the MSF4J profile of
    WSO2 EI.


Let’s assume this is the format of the request sent by the client:

``` xml
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
      "appointment_date": "2017-04-02"
    }
```

However, the format of the message must be as follows to be compatible
with the backend service.

``` xml
    {
      "patient": {
        "name": "John Doe",
        "dob": "1990-03-19",
        "ssn": "234-23-525",
        "address": "California",
        "phone": "8770586755",
        "email": "johndoe@gmail.com"
        "cardNo": "7844481124110331"
      },
      "doctor": "thomas collins",
      "hospital": "grand oak community hospital"
      "appointment_date": "2017-04-02"
    }
```

The client message format must be transformed to the back-end service
message format within the In sequence.

Let's get started!

This tutorial contains the following sections:

-   [Creating the deployable
    artifacts](#TransformingMessageContent-Creatingthedeployableartifacts)
-   [Packaging the
    artifacts](#TransformingMessageContent-Packagingtheartifacts)
-   [Starting the ESB profile and deploying the
    artifacts](#TransformingMessageContent-StartingtheESBprofileanddeployingtheartifacts)
-   [Starting the MSF4J
    profile](#TransformingMessageContent-StartingtheMSF4Jprofile)
-   [Sending requests to the
    ESB](#TransformingMessageContent-SendingrequeststotheESB)

### Creating the deployable artifacts

The Data Mapper mediator transforms the message within the In sequences.
The Data Mapper mediator is a data mapping solution that can be
integrated into a mediation sequence. It converts and transforms one
data format to another, or changes the structure of the data in a
message.

1.  In WSO2 Integration Studio, add a Data Mapper mediator just after
    the Property mediator in the In Sequence of the API resource.  
    ![](attachments/119132196/119132205.png){width="800" height="486"}

2.  Double-click the Data Mapper mediator icon and provide the following
    name for the data mapping configuration file that will be created.

    -   **Configuration Name** : RequestMapping

    ![](attachments/119132196/119132224.png){width="500"}

        !!! note
    
        The **SampleServicesRegistry** project is created at the time of
        creating the ESB Solution project and will get auto-picked here.
    

    Click **OK** . You view the data mapping editor.  
    ![](attachments/119132196/119132204.png){width="800" height="542"}

3.  Create a JSON file (e.g., `           input.json          ` ) by
    copying the following sample content of the request message sent to
    the API Resource, and save it in your local file system.

    ``` xml
        { "name": "John Doe",
          "dob": "1990-03-19",
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

        !!! info
    
        You can [create a JSON schema
        manually](https://docs.wso2.com/display/EI600/Creating+a+JSON+Schema+Manually)
        for input and output using the Data Mapper Diagram editor.
    

4.  Right-click on the top title bar of the **Input** box and click
    **Load Input** as shown below.

    ![](attachments/119132196/119132200.png){width="400"}

5.  Select **JSON** as the **Resource Type** as shown below.

    ![](attachments/119132196/119132203.png){width="400"}

6.  Click the **file system** link in **Select resource from** , select
    the JSON file (i.e., `           input.json          ` ) you saved
    in your local file system in step 5, and click **Open** .  
    You view the input format loaded in the **Input** box in the editor
    as shown below.

    ![the input
    format](attachments/119132196/119132211.png "the input format")

7.  Create another JSON file (e.g., `           out          `
    `           put.json          ` ) by copying the following sample
    content of the request message expected by the backend service, and
    save it in your local file system.

    ``` xml
        {
          "patient": {
            "name": "John Doe",
            "dob": "1990-03-19",
            "ssn": "234-23-525",
            "address": "California",
            "phone": "8770586755",
            "email": "johndoe@gmail.com"
          },
          "doctor": "thomas collins",
          "hospital": "grand oak community hospital",
          "appointment_date": "2025-04-02"
        }
    ```

8.  Right-click on the top title bar of the **Output** box and click
    **Load Output** as shown below.  
    ![](attachments/119132196/119132202.png){width="300"}
9.  Select **JSON** as the resource type.
10. Click the **file system** link in **Select resource from** , select
    the JSON file you saved in your local file system in [step
    7](#TransformingMessageContent-step7) , and click **Open** .  
    You view the input format loaded in the **Output** box in the editor
    as shown below.  
    ![](attachments/119132196/119132201.png){height="250"}

        !!! info
    
        Check the **Input** and **Output** boxes with the sample messages,
        to see if the element types (i.e. (Arrays, Objects and Primitive
        values) are correctly identified or not. Following signs will help
        you to identify them correctly.
    
        -   {} - represents object elements
        -   \[\] - represents array elements
        -   \<\> - represents primitive field values
        -   A - represents XML attribute value
    

11. Do the mapping by dragging arrows from field values in the input box
    to the relevant field values in the output box. The final mapping is
    as follows:  
    ![](attachments/119132196/119132199.png){height="250"}
12. Save and close the configuration.

13. Go back to the **Design View** of the API Resource and select the
    Data Mapper Mediator and edit the following in the **Properties**
    tab:

    -   **Input Type** : JSON
    -   **Output Type** : JSONInput and output can be tested as below.  
        ![](attachments/119132196/119132197.png){width="700"}

14. Save the REST API configuration.

    ![](attachments/119132196/119132198.png){width="900" height="660"}

We are now ready to package and deploy the C-App and send the request.

### Packaging the artifacts

Since we created a new Registry Resource project, this will need to be
packaged into our existing C-App.

[Package](https://docs.wso2.com/display/ADMIN44x/Packaging+Artifacts+into+Composite+Applications)
the C-App names SampleServicesCompositeApplication project with the
artifacts created.

!!! note

Ensure the following artifact check-boxes are selected in the
**Composite Application Project POM Editor** .

-   SampleServices
    -   HealthcareAPI
    -   ClemencyEP
    -   GrandOakEP
    -   PineValleyEP
-   SampleServicesRegistry


### Starting the ESB profile and deploying the artifacts

Assuming you have already added a server for the ESB profile in Eclipse,
on the **Servers** tab, expand the WSO2 Carbon server, right-click
**SampleServicesCompositeApplication** , and choose **Redeploy** . The
Console window of the ESB profile will indicate that the CApp has been
deployed successfully.

![](attachments/119132196/119132206.png){width="533" height="250"}

!!! info

-   If you do not have a server added in Eclipse, refer [this
    tutorial](Sending-a-Simple-Message-to-a-Service_119132413.html#SendingaSimpleMessagetoaService-StartingtheESBprofileanddeployingtheartifacts)
    .
-   You can also deploy the artifacts to the ESB server using a
    [Composite Application Archive (CAR)
    file](https://docs.wso2.com/display/ADMIN44x/Packaging+Artifacts+into+Composite+Applications#PackagingArtifactsintoCompositeApplications-createCARCreatingaCompositeApplicationArchive(CAR)file)
    .


### Starting the MSF4J profile

To be able to send requests to the back-end service (which is an MSF4J
service deployed in the MSF4J profile), you need to first start the
MSF4J runtime:

1.  Open a terminal and navigate to the
    `          <EI_HOME>/wso2/msf4j/bin         ` directory.
2.  Start the runtime by executing the MSF4J startup script as shown
    below.

    -   [**On MacOS/Linux/CentOS**](#4007d24486ba4f34a3e56dfa71270953)
    -   [**On Windows**](#c0a07286a13645039a26c292ab919be1)

    ``` java
        sh carbon.sh
    ```

    ``` java
            carbon.bat
    ```

The Healthcare service is now active and you can start sending requests
to the service.

### Sending requests to the ESB

1.  Create a JSON file names `           request.json          ` with
    the following request payload.

    ``` xml
            {
            "name": "John Doe",
            "dob": "1990-03-19",
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
    the location where `          request.json         ` file you
    created is saved:  

    `           curl -v -X POST --data @request.json                       http://localhost:8280/healthcare/categories/surgery/reserve                      --header "Content-Type:application/json"          `

        !!! info
    
        This is derived from the [URI-Template
        defined](https://docs.wso2.com/display/EI600/Routing+Requests+Based+on+Message+Content#RoutingRequestsBasedonMessageContent-uriTemplate)
        when creating the API resource.
    
        `           http://<host>:<port>/categories/{category}/reserve          `
    

    You will see the response as follows:

    ``` xml
        {"appointmentNumber":1,
          "doctor":
               {"name":"thomas collins",
                "hospital":"grand oak community hospital",
                "category":"surgery","availability":"9.00 a.m - 11.00 a.m",
                "fee":7000.0},
          "patient":
              {"name":"John Doe",
               "dob":"1990-03-19",
               "ssn":"234-23-525",
               "address":"California",
               "phone":"8770586755",
               "email":"johndoe@gmail.com"},
          "fee":7000.0,
          "confirmed":false}
    ```

You have now explored how the ESB profile of WSO2 EI can receive a
message in one format and transform it into the format expected by the
backend service using the Data Mapper mediator.
