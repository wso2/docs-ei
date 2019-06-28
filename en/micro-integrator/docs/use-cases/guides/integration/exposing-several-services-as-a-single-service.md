# Exposing Several Services as a Single Service

In the ESB profile of WSO2 Enterprise Integrator (WSO2 EI), this is
commonly referred to as Service Chaining, where several services are
integrated based on some business logic and exposed as a
single, aggregated service.

**In this tutorial** , you send a message through the ESB profile to the
back-end service using the [Call
mediator](https://docs.wso2.com/display/EI650/Call+Mediator) , instead
of the Send mediator. Using the Call mediator, you can build a service
chaining scenario as it allows you to specify all service invocations
one after the other within a single sequence.

You then use the [PayloadFactory
mediator](https://docs.wso2.com/display/EI650/PayloadFactory+Mediator)
to take the response from one back-end service and change it to the
format that is accepted by the other back-end service.

!!! note

See the following topics for a description of the **concepts** that you
need to know when creating ESB artifacts:

-   [REST
    API](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-RESTAPIs)
-   [Endpoints](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Messageexitpoints)
-   [Sequences](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Sequences)
-   [Service Chaining  
    ](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Servicechaining)

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


4.  If you did not try the [Transforming Message
    Content](_Transforming_Message_Content_) tutorial yet, open WSO2
    Integration Studio, click **File** , and then click **Import** .
    Next, select **Existing WSO2 Projects into workspace** under the
    **WSO2** category, click **Next** and upload the [pre-packaged
    project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/TransformingContentTutorial.zip)
    . This contains the configurations of the [Transforming Message
    Content](_Transforming_Message_Content_) tutorial so that you do not
    have to repeat those steps.
5.  Download the MSF4J service from
    [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0.jar)
    and copy the JAR file to
    `          <EI_HOME>/wso2/msf4j/deployment/microservices         `
    folder. The back-end service is now deployed in the MSF4J profile of
    WSO2 EI. For more information on MSF4J, see the [WSO2 MSF4 GitHub
    Documentation](https://github.com/wso2/msf4j/blob/master/README.md)
    .


Let's get started!

-   [Connecting to the back-end
    services](#ExposingSeveralServicesasaSingleService-Connectingtotheback-endservices)
-   [Creating the deployable
    artifacts](#ExposingSeveralServicesasaSingleService-Creatingthedeployableartifacts)
-   [Packaging the
    artifacts](#ExposingSeveralServicesasaSingleService-Packagingtheartifacts)
-   [Starting the ESB profile and deploying the
    artifacts](#ExposingSeveralServicesasaSingleService-StartingtheESBprofileanddeployingtheartifacts)
-   [Starting the MSF4J
    profile](#ExposingSeveralServicesasaSingleService-StartingtheMSF4Jprofile)
-   [Sending requests to the
    ESB](#ExposingSeveralServicesasaSingleService-SendingrequeststotheESB)

### Connecting to the back-end services

Let's create HTTP endpoints to the back-end services that you need to
connect, in order to check the channelling fee and to settle the
payment.

1.  Right click **SampleServices** in the Project Explorer and navigate
    to **New -\> Endpoint.** Ensure **Create a New Endpoint** is
    selected and click **Next.**

2.  Fill in the information as in the following table:

    | Field            | Value                                                                                                                                    |
    |------------------|------------------------------------------------------------------------------------------------------------------------------------------|
    | Endpoint Name    | ChannelingFeeEP                                                                                                                          |
    | Endpoint Type    | HTTP Endpoint                                                                                                                            |
    | URI Template     | `               http://localhost:9090/{uri.var.hospital}/categories/appointments/{uri.var.appointment_id}/fee              `             |
    | Method           | GET                                                                                                                                      |
    | Static Endpoint  | (Select this option because we are going to use this endpoint in this ESB Config project only and will not re-use it in other projects.) |
    | Save Endpoint in | SampleServices                                                                                                                           |

    Click **Finish** .

    ![](attachments/119132228/119132240.png){width="500" height="462"}

3.  Create another endpoint for Settle Payment and fill in the
    information as in the following table:

    | Field            | Value                                                                                                                                    |
    |------------------|------------------------------------------------------------------------------------------------------------------------------------------|
    | Endpoint Name    | SettlePaymentEP                                                                                                                          |
    | Endpoint Type    | HTTP Endpoint                                                                                                                            |
    | URI Template     | `                http://localhost:9090/healthcare/payments               `                                                               |
    | Method           | POST                                                                                                                                     |
    | Static Endpoint  | (Select this option because we are going to use this endpoint in this ESB Config project only and will not re-use it in other projects.) |
    | Save Endpoint in | SampleServices                                                                                                                           |

    Click **Finish** .

    ![](attachments/119132228/119132239.png){width="500" height="462"}

You have now created the additional endpoints that are required for this
tutorial.

### Creating the deployable artifacts

1.  In WSO2 Integration Studio, add a Property mediator just after the
    **Get Hospital** Property mediator in the in-sequence of the API
    resource to retrieve and store the card number that is sent in the
    request payload.

    ![](attachments/119132228/119132238.png?effects=drop-shadow){width="800"}  

    With the Property mediator selected, access the Properties tab and
    fill in the information as in the following table:

    | Field             | Value                                              |
    |-------------------|----------------------------------------------------|
    | Property Name     | Select **New Property**                            |
    | New Property Name | `               card_number              `         |
    | Property Action   | Select **set**                                     |
    | Value Type        | Select **Expression**                              |
    | Value Expression  | `               json-eval($.cardNo)              ` |
    | Description       | Get Card Number                                    |

        !!! tip
    
        For detailed instructions on adding a Property mediator, see
        [Mediating requests to the back-end
        service](https://docs.wso2.com/display/EI600/Routing+Requests+Based+on+Message+Content#RoutingRequestsBasedonMessageContent-Mediatingrequeststotheback-endservice)
        .
    

2.  Go to the first case box of the Switch mediator. Add a Property
    mediator just after the Log mediator to store the value for
    `          uri.var.hospital         ` variable that will be used
    when sending requests to
    [ChannelingFeeEP](#ExposingSeveralServicesasaSingleService-ChannelingFeeEP)
    .  
    ![](attachments/119132228/119132237.png){width="800" height="322"}  

    With the Property mediator selected, access the Properties tab and
    fill in the information as in the following table:

    | Field              | Value                                            |
    |--------------------|--------------------------------------------------|
    | Property Name      | Select **New Property**                          |
    | New Property Name  | uri `               .var.hospital              ` |
    | Property Action    | Select **set**                                   |
    | Value Type         | Select **LITERAL**                               |
    | Property Data Type | Select **STRING**                                |
    | Value              | `               grandoaks              `         |
    | Description        | Set Hospital Variable                            |

3.  Similarly, add property mediators in the other two case boxes in the
    Switch mediator. Change only the **Value** field as follows:
    -   Case 2: `            clemency           `
    -   Case 3: `            pinevalley           `  
        ![](attachments/119132228/119132236.png){width="800"
        height="424"}
4.  Delete the Send mediator by right clicking on the mediator and
    selecting **Delete from Model** . Replace this with a Call mediator
    from the **Mediators** palette and add GrandOakEP from the **Defined
    Endpoints** palette to the empty box adjoining the Call mediator.  
      
    Replace the Send mediators in the following two case boxes as well
    and add ClemencyEP and PineValleyEP to the respective boxes
    adjoining the Call mediators.  
    ![](attachments/119132228/119132235.png){width="800" height="462"}

        !!! info
    
        Replacing with a Call mediator allows us to define other service
        invocations following this mediator.
    

    Let's use Property mediators to retrieve and store the values that
    you get from the response you receive from GrandOakEP, ClemencyEP or
    PineValleyEP.

5.  Next to the Switch mediator, add a Property mediator to retrieve and
    store the value sent as `           appointmentNumber          ` .

    ![](attachments/119132228/119132234.png){width="800" height="599"}  

    With the Property mediator selected, access the Properties tab and
    fill in the information as in the following table:

    <table>
    <thead>
    <tr class="header">
    <th>Field</th>
    <th>Value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Property Name</td>
    <td>Select <strong>New Property</strong></td>
    </tr>
    <tr class="even">
    <td>New Property Name</td>
    <td><code>               uri.var.appointment_id              </code><br />
    (This value is used when invoking <a href="#ExposingSeveralServicesasaSingleService-ChannelingFeeEP">ChannelingFeeEP</a> )</td>
    </tr>
    <tr class="odd">
    <td>Property Action</td>
    <td><p>Select <strong>set</strong></p></td>
    </tr>
    <tr class="even">
    <td>Value Type</td>
    <td>Select <strong>EXPRESSION</strong></td>
    </tr>
    <tr class="odd">
    <td>Value Expression</td>
    <td><code>               json-eval($.appointmentNumber)              </code></td>
    </tr>
    <tr class="even">
    <td>Description</td>
    <td>Get Appointment Number</td>
    </tr>
    </tbody>
    </table>

        !!! note
    
        You derive the **Value Expression** in the above table from the
        following response that is received from GrandOakEP, ClemencyEP or
        PineValleyEP:
    
    ``` java
            {"appointmentNumber":1,   "doctor":
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
        

6.  Similarly, add two more Property mediators as follows. They retrieve
    and store the `           doctor          ` details and
    `           patient          ` details respectively, from the
    response that is received from GrandOakEP, ClemencyEP or
    PineValleyEP.

    <table>
    <thead>
    <tr class="header">
    <th>Field</th>
    <th>Value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Property Name</td>
    <td>Select <strong>New Property</strong></td>
    </tr>
    <tr class="even">
    <td>New Property Name</td>
    <td><code>               doctor_details              </code></td>
    </tr>
    <tr class="odd">
    <td>Property Action</td>
    <td><p>Select <strong>set</strong></p></td>
    </tr>
    <tr class="even">
    <td>Value Type</td>
    <td>Select <strong>EXPRESSION</strong></td>
    </tr>
    <tr class="odd">
    <td>Value Expression</td>
    <td><code>               json-eval($.doctor)              </code></td>
    </tr>
    <tr class="even">
    <td>Description</td>
    <td>Get Doctor Details</td>
    </tr>
    <tr class="odd">
    <td><br />
    </td>
    <td><br />
    </td>
    </tr>
    <tr class="even">
    <td>Property Name</td>
    <td>Select <strong>New Property</strong></td>
    </tr>
    <tr class="odd">
    <td>New Property Name</td>
    <td><code>               patient_details              </code></td>
    </tr>
    <tr class="even">
    <td>Property Action</td>
    <td>Select <strong>set</strong></td>
    </tr>
    <tr class="odd">
    <td>Value Type</td>
    <td>Select <strong>EXPRESSION</strong></td>
    </tr>
    <tr class="even">
    <td>Value Expression</td>
    <td><code>               json-eval($.patient)              </code></td>
    </tr>
    <tr class="odd">
    <td>Description</td>
    <td>Get Patient Details</td>
    </tr>
    </tbody>
    </table>

    ![](attachments/119132228/119132233.png){width="800" height="535"}

7.  Add a Call mediator and add ChannelingFeeEP from **Defined
    Endpoints** palette to the empty box adjoining the Call mediator.
8.  Add a Property mediator adjoining the Call mediator box to retrieve
    and store the value sent as `           actualFee          `
    . Access the Property tab of the mediator and fill in the
    information as in the following table:

    | Field             | Value                                                                                                                                            |
    |-------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
    | Property Name     | Select **New Property**                                                                                                                          |
    | New Property Name | `               actual_fee              ` (This value is used when invoking [SettlePaymentEP](#ExposingSeveralServicesasaSingleService-Settle) ) |
    | Property Action   | Select **set**                                                                                                                                   |
    | Value Type        | Select **EXPRESSION**                                                                                                                            |
    | Value Expression  | `               json-eval($.actualFee)              `                                                                                            |
    | Description       | Get Actual Fee                                                                                                                                   |

    ![](attachments/119132228/119132232.png){width="800" height="397"}

        !!! note
    
        You derive the Value Expression in the above table from the
        following response that is received from ChannelingFeeEP:
    
    ``` java
            {"patientName":" John Doe ", 
             "doctorName":"thomas collins", 
             "actualFee":"7000.0"}
    ```
        

    Let's use the [PayloadFactory
    mediator](https://docs.wso2.com/display/EI650/PayloadFactory+Mediator)
    to construct the following message payload for the request sent to
    SettlePaymentEP.

    ``` java
        {"appointmentNumber":2,
            "doctor":{
                "name":"thomas collins",
                "hospital":"grand oak community hospital",
                "category":"surgery",
                "availability":"9.00 a.m - 11.00 a.m",
                "Fee":7000.0
            },
            "patient":{
                "name":"John Doe",
                "Dob":"1990-03-19",
                "ssn":"234-23-525",
                "address":"California",
                "phone":"8770586755",
                "email":"johndoe@gmail.com"
            },
            "fee":7000.0,
            "Confirmed":false,
            "card_number":"1234567890"
        }
    ```

9.  Add a PayloadFactory mediator next to the Property mediator, from
    the **mediators** palette to construct the above message payload.

    ![](attachments/119132228/119132229.png){width="800" height="299"}  

    With the Payloadfactory mediator selected, access the properties tab
    of the mediator and fill in the information as in the following
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
    <td><strong>Payload Format</strong></td>
    <td>Select <strong>Inline</strong></td>
    <td>-</td>
    </tr>
    <tr class="even">
    <td><strong>Media Type</strong></td>
    <td><p>Select <strong>json</strong></p></td>
    <td>-</td>
    </tr>
    <tr class="odd">
    <td><strong>Payload</strong></td>
    <td><div class="content-wrapper">
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>{</span>
    <span id="cb1-2"><a href="#cb1-2"></a><span class="st">&quot;appointmentNumber&quot;</span>:$<span class="dv">1</span>,</span>
    <span id="cb1-3"><a href="#cb1-3"></a><span class="st">&quot;doctor&quot;</span>:$<span class="dv">2</span>,</span>
    <span id="cb1-4"><a href="#cb1-4"></a><span class="st">&quot;patient&quot;</span>:$<span class="dv">3</span>,</span>
    <span id="cb1-5"><a href="#cb1-5"></a><span class="st">&quot;fee&quot;</span>:$<span class="dv">4</span>,</span>
    <span id="cb1-6"><a href="#cb1-6"></a><span class="st">&quot;confirmed&quot;</span>:<span class="st">&quot;false&quot;</span>,</span>
    <span id="cb1-7"><a href="#cb1-7"></a><span class="st">&quot;card_number&quot;</span>:<span class="st">&quot;$5&quot;</span></span>
    <span id="cb1-8"><a href="#cb1-8"></a>}</span></code></pre></div>
    </div>
    </div>
    </div></td>
    <td>The message payload to send with the request to SettlePaymentEP. In this payload, <code>               $1              </code> , <code>               $2              </code> , <code>               $3              </code> , <code>               $4              </code> , and <code>               $5              </code> indicate variables.</td>
    </tr>
    </tbody>
    </table>

    We will look at adding the value for the field **Args** in the
    following steps.

        !!! tip
    
        To avoid getting an error message, first select the **Media Type**
        before providing the **Payload.**
    

10. To add the **Args** field for the PayloadFactory mediator, click the
    **Add (+)** icon in the **Args** field and enter the following
    information as in the table below. It provides the argument that
    defines the actual value of the first variable used in the format
    definition in the previous step.

    | Field         | Value                                                        | Description                                                          |
    |---------------|--------------------------------------------------------------|----------------------------------------------------------------------|
    | **Type**      | Select **Expression**                                        | \-                                                                   |
    | **Value**     | `                $ctx:uri.var.appointment_id               ` | The value for the first variable ($1) in the message payload format. |
    | **Evaluator** | Select **xml**                                               | Indicates that the expression provided is in XML.                    |

        !!! info
    
        The `           $ctx          ` method is similar to using the
        `           get-property          ` method. This method checks in
        the message context. For more details on using this method, refer
        the
        [documentation](https://docs.wso2.com/display/EI600/Synapse+XPath+Variables#SynapseXPathVariables-$ctx)
        .
    

11. Similarly, click **Add** and add more arguments to define the other
    variables that are used in the message payload format definition.
    Use the following as the **Value** for each of them:

    `           $ctx:doctor_details          `  
    `           $ctx:patient_details          `  
    `           $ctx:actual_fee          `  
    `           $ctx:card_number          `  
    ![](attachments/119132228/119132231.png){width="600" height="163"}

12. Add a Call mediator and add SettlePaymentEP from Defined Endpoints
    palette to the empty box adjoining the Call mediator.
13. Add a
    [Respond](https://docs.wso2.com/display/EI650/Respond+Mediator)
    mediator to send the response to the client.  
    You should now have a completed configuration that looks like
    this:  
    ![](attachments/119132228/119132230.png){width="900" height="698"}
14. Save the updated REST API configuration.

### Packaging the artifacts

Since you created new endpoints, these will need to be packaged into our
existing C-App.

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


### Starting the ESB profile and deploying the artifacts

Assuming you have already added a server for the ESB profile in Eclipse,
on the **Servers** tab, expand the WSO2 Carbon server, right-click
**SampleServicesCompositeApplication** , and choose **Redeploy** . The
Console window of the ESB profile will indicate that the CApp has been
deployed successfully.

![](attachments/119132228/119132241.png){width="533" height="250"}

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

    -   [**On MacOS/Linux/CentOS**](#c9197970c7c44002a0b4873d66c5ee75)
    -   [**On Windows**](#1a13806c880e4bcab48fa00dbd155191)

    ``` java
        sh carbon.sh
    ```

    ``` java
            carbon.bat
    ```

The Healthcare service is now active and you can start sending requests
to the service.

### Sending requests to the ESB

1.  Create a JSON file named `           request.json          ` with
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
            "cardNo": "7844481124110331",
            "appointment_date": "2025-04-02"
            }
    ```

2.  Open a command line terminal and execute the following command from
    the location where the `           request.json          ` file was
    saved:

    ``` java
            curl -v -X POST --data @request.json  http://localhost:8280/healthcare/categories/surgery/reserve  --header "Content-Type:application/json"
    ```

        !!! info
    
        This is derived from the [URI-Template
        defined](https://docs.wso2.com/display/EI650/Routing+Requests+Based+on+Message+Content#RoutingRequestsBasedonMessageContent-uriTemplate)
        when creating the API resource.
    
        `           http://<host>:<port>/categories/{category}/reserve          `
    

    You will see the response as follows:

    ``` java
        {  
           "appointmentNo":1,
           "doctorName":"thomas collins",
           "patient":"John Doe",
           "actualFee":7000.0,
           "discount":20,
           "discounted":5600.0,
           "paymentID":"480fead2-e592-4791-941a-690ad1363802",
           "status":"Settled"
        }
    ```

You have now explored how the ESB profile of WSO2 EI can do service
chaining using the Call mediator and transform message payloads from one
format to another using the PayloadFactory mediator.
