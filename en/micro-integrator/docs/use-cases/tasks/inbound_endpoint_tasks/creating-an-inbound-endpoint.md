# Creating an Inbound Endpoint

You can create a new inbound endpoint or import an existing inbound
endpoint from an XML file, such as a Synapse configuration file using
WSO2 Integration Studio.

-   [Prerequisites](#CreatinganInboundEndpoint-Prerequisites)
-   [Step 1: Creating an ESB Solution
    project](#CreatinganInboundEndpoint-Step1:CreatinganESBSolutionproject)
-   [Step 2: Creating a new
    inbound endpoint](#CreatinganInboundEndpoint-createStep2:Creatinganewinboundendpoint)
-   [Step 3: Deploying the inbound endpoint in the ESB
    server](#CreatinganInboundEndpoint-Step3:DeployingtheinboundendpointintheESBserver)

### Prerequisites

You need to have WSO2 Integration Studio installed to create a new
inbound endpoint or to import an existing inbound endpoint. For
instructions, see [Installing WSO2 Integration
Studio](https://docs.wso2.com/display/EI611/Installing+Enterprise+Integrator+Tooling)
.

### Step 1: Creating an ESB Solution project

First, create an **ESB Solution Project** in WSO2 Integration Studio. We
will use this project to store the Inbound Endpoint artifacts.

1.  Open **WSO2 Integration Studio** and click **ESB Project → Create
    New ** in the **Getting Started** tab as shown below.  
    ![](attachments/119130498/119130499.png){width="700" height="336"}

2.  Enter a project name and click **Finish** .

3.  The new project will be listed in the project explorer.

### Step 2: Creating a new inbound endpoint

Follow the steps below to create a new inbound endpoint.

1.  Right-click the project in the navigator and go to **New → Inbound
    Endpoint** to open the **New Inbound Endpoint Artifact** dialog:  
    ![](attachments/119130498/119130502.png){width="500"}

        !!! tip
    
        Importing an Inbound Endpoint?
    
        If you already have an inbound endpoint artifact created, you have
        the option of importing the XML configuration. Select **Import
        Inbound Endpoint** and follow the instructions on the UI. To create
        a new proxy service from scratch, continue with the following steps.
    

    Select **Create a New Inbound Endpoint** and click **Next** .

2.  Enter a unique name for the inbound endpoint, and select an
    **Inbound Endpoint Creation Type** from the list shown below.

    ![](attachments/119130498/119130501.png){width="500" height="363"}

        !!! tip
    
        Go to [WSO2 EI Inbound Endpoints](_WSO2_EI_Inbound_Endpoints_) to
        learn more about how each of the protocol types work.
    

3.  For certain protocols ( HL7, KAFKA, Custom, MQTT, RabbitMq,
    WSO2\_MB, WS, and  WSS) the main sequence and error sequence are
    mandatory fields. You can select sequences that already exists in
    the workspace and add them to the **Sequence** and **Error
    sequence** fields (shown below). If you don't have any sequences in
    the workspace, click **Generate Sequence and Error Sequence** to
    generate new sequences for the inbound endpoint.  
    ![](attachments/119130498/119130500.png){width="500" height="359"}  
4.  Specify the **ESB Solution** project where Inbound Endpoint should
    be saved.
5.  Click **Finish** to generate the inbound endpoint. The artifact will
    now be saved in the ESB project.
6.  In the **Save Inbound Endpoint** field, select the Inbound Endpoint
    from the project explorer and go to the **Design View.**
7.  Right-click the inbound endpoint and click **Show Properties View**
    to go to the **Property** tab. You can update the parameters
    relevant to the type of inbound endpoint.

        !!! tip
    
        Go to [WSO2 EI Inbound Endpoints](_WSO2_EI_Inbound_Endpoints_) for
        details of the parameters available for each inbound endpoint type.
    

8.  Add sequences to the main sequence and error sequence of the Inbound
    Endpoint. This can be done using the **Sequence** mediator from the
    **Mediators** pallet. Open the **Property** tab (right-click and
    click **Show Properties View** ) of the **Sequence** mediator and
    update the relevant parameters.

        !!! tip
    
        See the parameters available for the [Sequence
        Mediator](https://docs.wso2.com/display/EI650/Sequence+Mediator) .
    

### Step 3: Deploying the inbound endpoint in the ESB server

Once you have created your inbound endpoint as explained in the previous
topics, you need to create a **Composite Application** project with a
CAR file. You can then deploy the CAR file in the ESB server:

1.  Right-click the **Project Explorer** and click **New \> Project** .
2.  From the window that opens, click **Composite Application Project**
    .
3.  Give a name to the **Composite Application** project and select the
    projects that you need to group into your C-App from the list of
    available projects. You need to select the **ESB** project, which
    contains the proxy service and security policy file respectively.

4.  Next, [deploy the CAR file in the ESB
    server](https://docs.wso2.com/display/ADMIN44x/Deploying+Composite+Applications+in+the+Server)
    .

!!! note

Redeployment of listening inbound endpoints fail?

A [listening inbound endpoint](_Listening_Inbound_Endpoints_) opens the
port for itself during deployment. Therefore, if you are **redeploying**
a listening inbound endpoint artifact, the redeployment will not be
successful until the port that was previously opened for the inbound
endpoint is closed.

By default, the system will wait for 10 seconds for the previously
opened port to close down. If you want to increase this waiting time
beyond 10 seconds, be sure to add the following system property in the
`         carbon.properties        ` file, which is stored in the
`         <EI_HOME>/conf/        ` directory and restart the server
before redeploying the artifacts.

``` java
    -Dsynapse.transport.portCloseVerifyTimeout=20
```
    
    Note that this setting may be required in Windows environments as the
    process of closing a port can sometimes take longer than 10 seconds.
    
