# Routing Requests Based on Message Content

In the [Sending a Simple Message to a
Service](_Sending_a_Simple_Message_to_a_Service_) tutorial, we routed a
simple message to a single endpoint in the back-end service. In this
tutorial, we are building on the same sequence, by creating the
mediation artifacts that can route a message to the relevant endpoint,
depending on the content of the message payload.

When the client sends the appointment reservation request to the ESB,
the message payload of the request contains the name of the
hospital where the appointment needs to be confirmed. The HTTP request
method that is used for this is POST. Based on the hospital name sent in
the request message, the ESB should route the appointment reservation to
the relevant hospital's back-end service.

In this tutorial, you will use a [Switch
mediator](https://docs.wso2.com/display/EI650/Switch+Mediator) to route
messages based on the message content to the relevant HTTP Endpoint
defined in the ESB. The back-end service used in this example is a micro
service stored in the [MSF4J
profile](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-msf4j)
of WSO2 EI.

For more details on how routing of messages within the ESB is done based
on the message content, refer [Content-Based Router Enterprise
Integration
Pattern.](https://docs.wso2.com/display/EIP/Content-Based+Router)

!!! note

See the following topics for a description of the **concepts** that you
need to know when creating ESB artifacts:

-   [REST
    API](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-RESTAPIs)
-   [Endpoints  
    ](https://docs.wso2.com/display/EI650/Key+Concepts#KeyConcepts-Messageexitpoints)

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
    based on your operating system from
    [here](https://wso2.com/integration/tooling/) and then extract the
    ZIP file.  
    The path to this folder will be referred to as
    `           <EI_TOOLING>          ` throughout this tutorial.

    !!! info

    Getting an error message? See the troubleshooting tips given under
    [Installing WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio#InstallingWSO2IntegrationStudio-Troubleshooting)
    .


4.  If you did not try the [Sending a Simple Message to a
    Service](_Sending_a_Simple_Message_to_a_Service_) tutorial yet, open
    WSO2 Integration Studio, click **File** , and click **Import** .  
    Next, expand the **WSO2** category and select **Existing WSO2
    Projects into workspace** , click **Next** and upload the
    [pre-packaged
    project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/SimpleMessageToServiceTutorial.zip)
    .  
    This contains the configurations of the [Sending a Simple Message to
    a Service](_Sending_a_Simple_Message_to_a_Service_) tutorial so that
    you do not have to repeat those steps.
5.  Download the MSF4J service from
    [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0.jar)
    and copy the JAR file to the
    `          <EI_HOME>/wso2/msf4j/deployment/microservices         `
    directory. The back-end service is now deployed in the MSF4J profile
    of WSO2 EI.


Let's get started!

This tutorial contains the following sections:

-   [Connecting to the back-end
    service](#RoutingRequestsBasedonMessageContent-Connectingtotheback-endservice)
-   [Mediating requests to the back-end
    service](#RoutingRequestsBasedonMessageContent-Mediatingrequeststotheback-endservice)
-   [Packaging the
    artifacts](#RoutingRequestsBasedonMessageContent-Packagingtheartifacts)
-   [Starting the ESB profile and deploying the
    artifacts](#RoutingRequestsBasedonMessageContent-StartingtheESBprofileanddeployingtheartifacts)
-   [Starting the MSF4J
    profile](#RoutingRequestsBasedonMessageContent-StartingtheMSF4Jprofile)
-   [Sending requests to WSO2
    EI](#RoutingRequestsBasedonMessageContent-SendingrequeststoWSO2EI)

### Connecting to the back-end service

!!! info

In this tutorial we have three hospital backend services hosted in the
MSF4J profile of WSO2 EI:

-   Grand Oak Community Hospital:
    `           http://localhost:9090/grandoaks/          `

-   Clemency Medical Center:
    `           http://localhost:9090/clemency/          `

-   Pine Valley Community Hospital:
    `           http://localhost:9090/pinevalley/          `

The request method is POST and the format of the request URL expected by
the backend services is:
`         http://localhost:9090/grandoaks/categories/{category}/reserve        `


Let's create three different HTTP endpoints for the above services.

1.  Right-click **SampleServices** in the Project Explorer and navigate
    to **New -\> Endpoint** . Ensure **Create a New Endpoint** is
    selected and click **Next.**

2.  Fill in the information as in the following table:

    | Field            | Value                                                                                                                                    |
    |------------------|------------------------------------------------------------------------------------------------------------------------------------------|
    | Endpoint Name    | `               GrandOakEP              `                                                                                                |
    | Endpoint Type    | `               HTTP Endpoint              `                                                                                             |
    | URI Template     | `               http://localhost:9090/grandoaks/categories/{uri.var.category}/reserve                             `                      |
    | Method           | `               POST              `                                                                                                      |
    | Static Endpoint  | Select this option because we are going to use this endpoint only in this ESB Solution project and will not re-use it in other projects. |
    | Save Endpoint in | `               SampleServices              `                                                                                            |

    ![](attachments/119132155/119132166.png){width="500"}

3.  Click **Finish** .
4.  Similarly, create the HTTP endpoints for the other two hospital
    services using the URI Templates given below:
    -   ClemencyEP:
        `            http://localhost:9090/clemency/categories/{uri.var.category}/reserve           `
    -   PineValleyEP:
        `            http://localhost:9090/pinevalley/categories/{uri.var.category}/reserve           `

You have now created the three endpoints for the hospital backend
services that will be used to make appointment reservations.

!!! tip

You can also create a single endpoint where the differentiation of the
hospital name can be handled using a variable in the URI template. See
the following tutorial: [Exposing Several Services as a Single
Service](https://docs.wso2.com/display/EI600/Exposing+Several+Services+as+a+Single+Service#ExposingSeveralServicesasaSingleService-Connectingtotheback-endservices)
.

Using three different endpoints is advantageous when the back-end
services are very different from one another and/or when there is a
requirement to configure error handling differently for each of them.


### Mediating requests to the back-end service

To implement the routing scenario, let's update the REST API we created
in the previous section by adding a new API resource. We will then use
a Switch mediator to route the message to the relevant back-end service
based on the hospital name that is passed in the payload of the request
message.

Let’s update the REST API we created in the [previous
tutorial](https://docs.wso2.com/display/EI600/Sending+a+Simple+Message#SendingaSimpleMessage-Mediatingrequeststotheback-endservice)
using WSO2 Integration Studio.

In the REST API configuration, select API Resource in the API palette
and drag it onto the canvas just below the previous API resource that
was created.  
![](attachments/119132155/119132165.png){width="800"}

Click the API Resource you just added to access the **Properties** tab
and fill in the following details:

|                  |                                                                                                 |
|------------------|-------------------------------------------------------------------------------------------------|
| **Url Style**    | Click in the **Value** field, click the down arrow, and select **URI\_TEMPLATE** from the list. |
| **URI-Template** | Enter `               /categories/{category}/reserve              `                             |
| **Methods**      | Enable **Post**                                                                                 |

![](attachments/119132155/119132164.png){width="700"}

Drag a [Property
Mediator](https://docs.wso2.com/display/EI650/Property+Mediator) from
the **Mediators** palette to the In Sequence of the API resource and
name it **Get Hospital** .  
This is used to extract the hospital name that is sent in the request
payload.

With the Property mediator selected, access the **Properties** tab and
fill in the following details:

<table>
<tbody>
<tr class="odd">
<td><strong>Property Name</strong></td>
<td><code>               New Property...              </code></td>
</tr>
<tr class="even">
<td><strong>New Property Name</strong></td>
<td><code>               Hospital              </code></td>
</tr>
<tr class="odd">
<td><strong>Property Action</strong></td>
<td><code>               set              </code></td>
</tr>
<tr class="even">
<td><strong>Value Type</strong></td>
<td><code>               EXPRESSION              </code></td>
</tr>
<tr class="odd">
<td>Value Expression</td>
<td><div class="content-wrapper">
<p>Click <strong>Value</strong> field of Value Expression in the <strong>Property</strong> tab and add the following expression:<br />
<code>                 json-eval($.hospital)                </code></p>
!!! info
<p>This is the <a href="https://docs.wso2.com/display/EI600/JSON+Support#JSONSupport-AccessingcontentfromJSONpayloads">JSONPath expression</a> that will extract the hospital from the request payload.</p>

</div></td>
</tr>
</tbody>
</table>

Add a Switch mediator from the **Mediator** palette just after the
Property Mediator.

Right-click the Switch mediator you just added and select **Add/Remove
Case** to add the number of cases you want to specify.  
![](attachments/119132155/119132163.png){width="500"}  
In this scenario, we are assuming there are three different hospitals,
hence there are three cases.

Enter 3 for Number of branches and click **OK** .  
![](attachments/119132155/119132194.png){width="250"}

With the Switch mediator selected, go to the **Properties** tab, and
fill in the details given below:

<table>
<tbody>
<tr class="odd">
<td><strong>Source XPath</strong></td>
<td><div class="content-wrapper">
<p>The <strong>Source XPath</strong> field is where we specify the XPath expression, which obtains the value of Hospital that we stored in the Property mediator.</p>
<p>Follow the steps given below to specify the expression:</p>
<ol>
<li>Click in the <strong>Value</strong> field of the <strong>Source XPath</strong> property.</li>
<li>Click the <strong>browse (...)</strong> .</li>
<li>Enter <code>                  get-property('Hospital')                 </code> and overwrite the default expression.</li>
<li>Click <strong>OK.</strong> <strong><br />
</strong></li>
</ol>
<p><strong><img src="attachments/119132155/119132162.png" width="450" /><br />
</strong></p>
!!! info
<p>For more information on <code>                 get-property()                </code> , see <a href="https://docs.wso2.com/display/EI600/XPath+Extension+Functions#XPathExtensionFunctions-func">XPath Extension Functions</a> .</p>

</div></td>
</tr>
<tr class="even">
<td><strong>Case Branches</strong></td>
<td><div class="content-wrapper">
<p>Follow the steps given below to add the case branches:</p>
<ol>
<li>Click in the <strong>Value</strong> field of the <strong>Case Branches</strong> property.</li>
<li>Click the <strong>browse (...)</strong> .</li>
<li>Change the RegExp values as follows:
<ul>
<li>Case 1: grand oak community hospital</li>
<li>Case 2:  clemency medical center</li>
<li>Case 3:  pine valley community hospital</li>
</ul>
<img src="attachments/119132155/119132161.png" width="900" /></li>
<li>Click <strong>OK</strong> .</li>
</ol>
</div></td>
</tr>
</tbody>
</table>

Let's add a [Log
mediator](https://docs.wso2.com/display/EI650/Log+Mediator) to print a
message indicating to which hospital the request message is being
routed.  
Drag a **Log mediator** to the first Case box of the Switch mediator,
and name it **Grand Oak Log** .  

With the Log mediator selected, access the **Properties** tab and fill
in the information given in the table below:

Field

Value

Description

**Log Category**

`               INFO              `

Indicates that the log contains an informational message.

**Log Level**

`               CUSTOM              `

When `               Custom              ` is selected, only specified
properties will be logged by this mediator.  
For more information on the available log levels, see the [Log
Mediator](https://docs.wso2.com/display/EI6xx/Log+Mediator#LogMediator-log-level)
.

Log Separator

`               (blank)              `

Since there is only one property that is being logged, we do not require
a separator, so this field can be left blank.

Properties

Follow the steps given below to extract the stock symbol from the
request and print a welcome message in the log

1.  Click the **Value** field of the **Properties** property, and then
    click the **browse (...)** icon that appears.
2.  In the Log Mediator Configuration dialog, click **New** , and then
    add a property as follows:  
    -   **Name** : `                    message                   `
    -   **Type** : `                    EXPRESSION                   `  
        (We select `                    EXPRESSION                   `
        because the required properties for the log message must be
        extracted from the request, which we can do using an XPath
        expression.)
    -   **Value/Expression** : Click the **browse (...)** icon in the
        **Value/Expression** field and enter
        `                     fn:concat('Routing to ', get-property('Hospital'))                    `
        .

                !!! info
        
                This XPath expression value gets the value stored in the
                Property mediator and concatenates the two strings to display
                the log message
                `                     Routing to <hospital name>                    `
                .
        

    ![](attachments/119132155/119132160.png){width="600"}
3.  Click **OK** .

Add a **Send mediator** adjoining the Log mediator and add the
**GrandOakEP endpoint** from **Defined Endpoints** palette to the empty
box adjoining the Send mediator.  
![](attachments/119132155/119132159.png){height="250"}

Add **Log mediators** in the other two **Case boxes** in Switch mediator
and then enter the same properties as in [Step
10](#RoutingRequestsBasedonMessageContent-SwitchLogMediator) .  
Make sure to name the two Log mediators as
`           Clemency Log          ` and
`           Pine Valley Log          ` respectively.

Add **Send mediators** adjoining these log mediators and respectively
add the **ClemencyEP** and **PineValleyEP** endpoints from the **Defined
Endpoints** palette.

!!! info

You have now configured the Switch mediator to log the
`           Routing to <Hospital Name>          ` message when a request
is sent to this API resource. The request message will then be routed to
the relevant hospital backend service based on the hospital that is sent
in the request payload.


Add a **Log mediator** to the **Default** (the bottom box) of the Switch
mediator and configure it the same way you did for the [Log mediator
above](#RoutingRequestsBasedonMessageContent-SwitchLogMediator) .

!!! note

Make sure to name it **Fault Log** and changing its Value/Expression as
follows:
`           fn:concat('Invalid hospital - ', get-property('Hospital'))          `


The default case of the Switch mediator handles the invalid hospital
requests that are sent to the request payload.  This logs the message
`           Invalid hospital - <Hospital Name>          ` " for requests
that have the invalid hospital name.

Drag a **Respond mediator** next to the Log mediator you just added.  
This ensures that there is no further processing of the current message
and returns the request message back to the client.  
The In Sequence of the API resource configuration should now look like
this:  
![](attachments/119132155/119132158.png?effects=drop-shadow){width="700"
height="639"}

Drag a **Send mediator** to the **Out sequence** of the API resource, to
send the response back to the client.

Save the updated REST API configuration.

### Packaging the artifacts

Package the C-App names SampleServicesCompositeApplication project with
the artifacts created.

1.  Open the `          pom.xml         ` file of the C-App project in
    the Composite Application Project POM Editor.
2.  Select the artifact that needs to be included into the CAR file.

        !!! note
    
        Ensure the following artifact check-boxes are selected in the
        **Composite Application Project POM Editor** .
    
        -   `            HealthcareAPI           `
        -   `            ClemencyEP           `
        -   `            GrandOakEP           `
        -   `            PineValleyEP           `
    

    ![](attachments/119132155/119132157.png){height="250"}

3.  Click ![](attachments/119132155/119132167.png){width="30"} and
    define the location you want to create the CAR file.

### Starting the ESB profile and deploying the artifacts

Assuming you have already added a server for the ESB profile in Eclipse,
on the **Servers** tab, expand the WSO2 Carbon server, right-click
**SampleServicesCompositeApplication** , and choose **Redeploy** . The
Console window of the ESB profile will indicate that the CApp has been
deployed successfully.

![](attachments/119132155/119132156.png){height="250"}

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

    -   [**On MacOS/Linux/CentOS**](#492d9963724e476a93ed91cd1c38cc82)
    -   [**On Windows**](#017a64436fcc49d69e04c2d9acfafcb1)

    ``` java
        sh carbon.sh
    ```

    ``` java
            carbon.bat
    ```

The Healthcare service is now active and you can start sending requests
to the service.

### Sending requests to WSO2 EI

Let's send a request to the API resource to make a reservation.

1.  Create a JSON file names `           request.json          ` with
    the following request payload.

    ``` xml
            {
              "patient": {
                "name": "John Doe",
                "dob": "1940-03-19",
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

        !!! info
    
        You can also try using any of the following parameters in your
        request payload.
    
        For hospital:
    
        -   clemency medical center
    
        -   pine valley community hospital
    
        Doctor Names:
    
        -   thomas collins
    
        -   henry parker
    
        -   abner jones
    
        -   anne clement
    
        -   thomas kirk
    
        -   cailen cooper
    
        -   seth mears
    
        -   emeline fulton
    
        -   jared morris
    
        -   henry foster
    

2.  Navigate to the directory where you have saved the
    `           request.json          ` file and execute the following
    command.

    ``` java
        curl -v -X POST --data @request.json http://localhost:8280/healthcare/categories/surgery/reserve --header "Content-Type:application/json"
    ```

        !!! info
    
        The URI-Template format that is used in this command was defined
        when creating the API resource in [step 2 of Connecting to the
        backend service](#RoutingRequestsBasedonMessageContent-uriTemplate)
        :
        `           http://<host>:<port>/categories/{category}/reserve          `
    

    You get the following response:

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

3.  Now check the Console tab in Eclipse and you will see the following
    message:
    `           INFO - LogMediator message = Routing to grand oak community hospital                     `
    This is the message printed by the Log mediator when the message
    from the client is routed to the relevant endpoint in the Switch
    mediator.

You have successfully completed this tutorial and have seen how the
requests received by the ESB can be routed to the relevant endpoint
using the Switch mediator.

  
