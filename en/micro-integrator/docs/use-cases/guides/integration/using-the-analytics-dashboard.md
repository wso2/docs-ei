# Using the Analytics Dashboard

The Analytics profile of WSO2 Enterprise Integrator (WSO2 EI) allows you
to analyze the statistics related to the message mediation that is
carried out in the ESB profile of WSO2 EI. Once the data from the
mediation artifacts in the ESB profile are published to the Analytics
profile, the dashboard will display the statistics.

**In this tutorial,** you will use the Analytics dashboard to view and
analyze the statistics that are carried out in [Exposing Several
Services as a Single Service
tutorial](_Exposing_Several_Services_as_a_Single_Service_) .

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
6.  If you are running on Windows, download the
    `          snappy-java_1.1.1.7.jar         ` from
    [here](http://mvnrepository.com/artifact/org.xerial.snappy/snappy-java/1.1.1.7)
    and copy the JAR file to `          <EI_HOME>\lib         `
    directory.


Let's get started!

-   [Setting up
    Analytics](#UsingtheAnalyticsDashboard-SettingupAnalytics)
-   [Starting the
    servers](#UsingtheAnalyticsDashboard-Startingtheservers)
-   [Enabling statistics and tracing for the
    artifacts](#UsingtheAnalyticsDashboard-Enablingstatisticsandtracingfortheartifacts)
-   [Sending a message](#UsingtheAnalyticsDashboard-Sendingamessage)
-   [Analyze the mediation
    statistics](#UsingtheAnalyticsDashboard-Analyzethemediationstatistics)

### Setting up Analytics

Set the following properties in the
`         <EI-HOME>/conf/synapse.properties        ` file to
`         true        ` so that the ESB profile can publish mediation
statistics:

  

``` java
    ...
    mediation.flow.statistics.enable=true
    mediation.flow.statistics.tracer.collect.payloads=true
    mediation.flow.statistics.tracer.collect.properties=true
    ...
```

### Starting the servers

Let's start the **Analytics** , **ESB** , and **MSF4J** profiles as
follows:

!!! note

Be sure to start the **Analytics** profile first and then start the
**ESB** profile.


1.  Start the Analytics profile:

    1.  Open a terminal and navigate to the
        `             <EI_HOME>/wso2/analytics/bin            `
        directory.

    2.  Start the runtime by executing the message broker startup script
        as shown below.

        -   [**On
            MacOS/Linux/CentOS**](#eaa64cd9ea774248b72564d1391bd6ce)
        -   [**On Windows**](#0c4b0a045ca24450b0cbea3069ee26c2)

        ``` java
                sh worker.sh
        ```

        ``` java
                    worker.bat
        ```

2.  Start the ESB runtime from within WSO2 Integration Studio. For
    instructions, see [Starting the Integrator runtime and deploying the
    artifacts](https://docs.wso2.com/display/EI650/Running+the+Product#RunningtheProduct-RunningtheESBprofileviaWSO2IntegrationStudio)
    .
3.  Start the back-end service (which is an MSF4J service):

    1.  Open a terminal and navigate to the
        `             <EI_HOME>/wso2/msf4j/bin            ` directory.

    2.  Start the runtime by executing the message broker startup script
        as shown below.

        -   [**On
            MacOS/Linux/CentOS**](#9aa868256fdf49a5b01b36689f9fb0cf)
        -   [**On Windows**](#37e261969beb4e3c86259a47a34c4165)

        ``` java
                    sh carbon.sh
        ```

        ``` java
                    carbon.bat
        ```

  

### Enabling statistics and tracing for the artifacts

  

Follow the steps below to enable statistics and tracing for the
REST API:

-   Go to the management console of the **ESB** profile:
    `           https://localhost:9443/carbon          `
-   On the **Main** tab of the management console, go to **Manage** -\>
    **Service Bus** and click **APIs** .  
    The **Deployed APIs** screen appears, and you will see the
    `           HealthcareAPI          ` listed as follows:  
    ![Enable statistics for the REST
    API](attachments/119132315/119132337.png "Enable statistics for the REST API"){width="869"
    height="250"}
-   To enable the collection of mediation statistics for the REST API,
    click **Enable Statistics** .
-   To enable mediation tracing for the REST API, click **Enable
    Tracing** .

        !!! note
    
        **Note** that it is not recommended to enable **tracing** in
        production environments as it generates a large number of events
        that reduces the performance of the analytics profile. Therefore,
        tracing should only be enabled in development environments.
    

Follow the steps below to enable statistics for the endpoints:

-   On the **Main** tab of the management console, go to **Manage -\>
    Service Bus** , and click **Endpoints** .  
    The **Manage Endpoints** screen appears, and you will see
    several endpoints listed.
-   To enable the collection of mediation statistics for the endpoints,
    click **Enable Statistics** for each endpoint.  
    ![Enable statistics for the endpoint
    artifacts](attachments/119132315/119132336.png "Enable statistics for the endpoint artifacts"){width="800"
    height="378"}

### Sending a message

Now that you have the artifacts deployed and configured for statistics
monitoring, let's send 8 requests to the ESB profile as explained below.

!!! tip

For the purpose of demonstrating how successful messages and message
failures are illustrated in the dashboard, let's send 2 of the requests
while the back-end service is not running. This should generate a
success rate of 75%.


1.  Create a JSON file called `            request.json           ` with
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

2.  Open a command line terminal and execute the following command (
    **six times** ) from the location where you save the
    request.json file:  

    `            curl -v -X POST --data @request.json                         http://localhost:8280/healthcare/categories/surgery/reserve                        --header "Content-Type:application/json"           `

    If the messages are sent successfully, you will receive the
    following response for each request.

    ``` java
            {"appointmentNo":1,
            "doctorName":"thomas collins",
            "patient":"John Doe",
            "actualFee":7000.0,
            "discount":20,
            "discounted":5600.0,
            "paymentID":"e1a72a33-31f2-46dc-ae7d-a14a486efc00",
            "status":"Settled"}
    ```

3.  Now, shut down the MSF4J server (to ensure that the back-end service
    is inactive) and send two more requests.

### Analyze the mediation statistics

Now, let's analyze the statistics generated from the message mediation:

1.  Start the Analytics dashboard:

    1.  Open a terminal and navigate to the
        `              <EI_HOME>/wso2/analytics/bin             `
        directory.

    2.  Start the runtime by executing the message broker startup script
        as shown below

        -   [**On
            MacOS/Linux/CentOS**](#de7cb79c1e4944f1b81821cf0ae8770d)
        -   [**On Windows**](#f92fa5bf6d254b8db996b7239e8b4506)

        ``` java
                    sh dashboard.sh
        ```

        ``` java
                    dashboard.bat
        ```

2.  In a new browser window or tab, open the Analytics dashboard using
    the following URL: <https://localhost:9643/portal> . Use
    `           admin          ` for both the username and password.
3.  Click the **Enterprise Integrator Analytics** icon shown below to
    open the dashboard.  
    ![Opening the Analytics dashboard for WSO2
    EI](attachments/119132315/119132335.png "Opening the Analytics dashboard for WSO2 EI")  
      
4.  View the statistics overview for all the ESB artifacts that have
    published statistics:  
    ![ESB total request
    count](attachments/119132315/119132316.png "ESB total request count"){height="250"}

5.  The number of transactions handled by the ESB per second is mapped
    on a graph as follows.

    ![ESB overall
    TPS](attachments/119132315/119132326.png "ESB overall TPS"){height="250"}  

6.  The success rate and the failure rate of the messages received by
    the ESB profile during the last hour are mapped in a graph as
    follows.  
    ![ESB overall message
    count](attachments/119132315/119132325.png "ESB overall message count"){height="250"}

7.  The `            HealthcareAPI           ` REST API is displayed
    under **TOP APIS BY REQUEST COUNT** as follows.  
    ![Top APIs by request
    count](attachments/119132315/119132324.png "Top APIs by request count"){height="250"}

8.  The three endpoints used for the message mediation are displayed
    under **Top Endpoints by Request Count** as shown below.  
    ![Top endpoints by request
    count](attachments/119132315/119132318.png "Top endpoints by request count"){width="500"}
9.  In the Top APIS BY Request COUNT gadget, click
    `           HealthcareAPI          ` to open the
    **OVERVIEW/API/HealthcareAPI** page. The following is displayed.
    1.  The **API Request Count** gadget shows the total number of
        requests handled by the `             StockQuoteAPI            `
        REST API during the last hour:  
        ![Total request per
        API](attachments/119132315/119132323.png "Total request per API"){height="250"}
    2.  The **API** **Message Count** gadget maps the number of
        successful messages as well as failed messages at different
        times within the last hour in a graph as shown below.  
        ![API message
        count](attachments/119132315/119132322.png "API message count"){height="250"}  
    3.  The **API** **Message Latency** gadget shows the speed with
        which the messages are processed by mapping the amount of time
        taken per message at different times within the last hour as
        shown below.  
        ![API message
        latency](attachments/119132315/119132321.png "API message latency"){height="250"}  
    4.  The **Messages** gadget lists all the the messages handled by
        the `             StockQuoteAPI            ` REST API during the
        last hour with the following property details as follows.  
        ![Message per
        API](attachments/119132315/119132320.png "Message per API"){height="250"}  
    5.  The **Message Flow** gadget illustrates the order in which the
        messages handled by the `             StockQuoteAPI            `
        REST API within the last hour passed through all the mediation
        sequences, mediators and endpoints that were included in the
        message flow as shown below.  
        ![Message flow per
        API](attachments/119132315/119132319.png "Message flow per API"){width="1000"}  
10. In the **Top Endpoints by Request Count** gadget, click one of the
    endpoints to view simillar statistics per endpoint.

    1.  `             ChannelingFeeEP                         `
    2.  `             SettlePaymentEP            `
        `                         `
    3.  `             GrandOaksEP            `

11. You can also navigate to any of the artifacts by using the top-left
    menu as shown below. For example, to view the statistics of a
    specific endpoint, click **Endpoint** and search for the required
    endpoint.  
    ![Dashboard navigation
    menu](attachments/119132315/119132317.png "Dashboard navigation menu"){width="1000"}
