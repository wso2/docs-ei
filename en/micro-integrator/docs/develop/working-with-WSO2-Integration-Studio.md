# Working with WSO2 Integration Studio

The following sections describe how you can use the development
experience provided by WSO2 Integration Studio to create and manage the
artifacts for your integration use case.

## Installing WSO2 Integration Studio

For instructions, see [Installing WSO2 Integration
Studio](_Installing_WSO2_Integration_Studio_) .

## Using Templates

WSO2 Integration Studio provides a set of artifact templates that will
help you get started with the most prominent integration use cases. C
lick the icon on the top-right of the WSO2 Integration Studio interface
to open the **Getting Started** view shown below and then select the
required template. There are ESB templates, data services templates, as
well as business process templates.

![Using Templates](../../assets/img/using_templates.png)

## Creating ESB projects

An ESB solution consists of one or several project directories. These
directories (listed below) store the various ESB artifacts that you
create for your integration sequence.

### ESB Solution

This project directory stores the ESB artifacts that are used when defining a mediation flow. Use one of the following approaches to create an ESB config project:

1. Open WSO2 Integration Studio and click ESB Project → Create New  in the Getting Started view as shown below.
2. In the New ESB Solution Project dialog that opens, enter a name for the ESB config project. Select the relevant check boxes if you want to create a Registry Resources project, Connector Exporter project, and/or a Composite Application project along with the ESB Config project.
3. Click Finish to save the projects. The ESB projects are listed in the project explorer as shown below.


### ESB Config project

1. Open WSO2 Integration Studio and click Miscellaneous → Create New Config Project  in the Getting Started view as shown below.
2. In the dialog that opens, select New ESB Config Project and click Next.
3. Enter a name for the ESB config project.
4. Click Finish and see that the project is now listed in the project explorer.

You can now start creating the ESB config artifacts in your ESB Config project.

### Registry Resource project

Create this project directory if you want to create registry resources for your mediation flow. You can later use these registry artifacts when you define your mediation sequences in the ESB config project.

1. Open WSO2 Integration Studio and click Miscellaneous → Create New Registry Project  in the Getting Started view as shown below.
2. In the dialog that opens, enter a name for the registry project.
3. Click Finish and see that the project is now listed in the project explorer.

See the instructions on creating and using registry artifacts.

### Mediator project

Create this project directory to start creating custom mediator artifacts. You can use these customer mediators when you define the mediation flow in your ESB config project.

1. Open WSO2 Integration Studio and click Miscellaneous → Create Mediator Project  in the Getting Started view as shown below.
2. In the dialog that opens, select Create New Mediator and click Next.
3. Enter a project name, package name, and class name.
4. Click Finish and see that the project is now listed in the project explorer.

See the instructions on creating and using custom mediators.

### Data Services project

Create this project directory to start creating data services (.dbs files) for exposing various datasources as a service.

1. Open WSO2 Integration Studio and click DS Project → Create New Data Service in the Getting Started view as shown below.
2. In the dialog that opens, enter a project name and click Next.
3. Click Finish and see that the project is now listed in the project explorer.

See instructions on managing data service artifacts using WSO2 Integration Studio.

### Connector Exporter project

Create this project directory if you have used ESB connectors in your medition sequence (defined in the ESB config project). All connector artifacts need to be stored in a connector exporter project before packaging. See the instructions on creating and using connectors.

### Composite Application project

This poject directory allows you to package all the artifacts (stored in other ESB projects) into one composite application (C-APP). This C-APP can then be deployed in the ESB server. See the instructions on packaging ESB artifacts.

You can use the above ESB projects and other various projects as
follows:

1.  Right-click the **Project Explorer** and click **New → Project** as
    shown below.  
    ![](attachments/119133415/119133438.png){width="354" height="250"}
2.  In the **New Project** dialog that opens, select the required
    project.  
    ![](attachments/119133415/119133437.png){width="450"}

### Importing ESB projects

If you have an already created ESB project file, you can import it to
your WSO2 Integration Studio workspace.

1.  Open WSO2 Integration Studio, navigate to **File -\> Import** ,
    select **Existing WSO2 Projects into workspace,** and click **Next**
    :  
    ![](attachments/119133415/119137201.png){width="400" height="417"}
2.  If you have a ZIP file of your project, browse for the **archive
    file** , or if you have an extracted project folder, browse for the
    **root directory** :  
    ![](attachments/119133415/119137202.png){width="400" height="463"}

        !!! tip
    
        Select **Copy projects into workspace** check box if you want to
        save the project in the workspace.
    

3.  Click **Finish** , and see that the project files are imported in
    the project explorer:  
    ![](attachments/119133415/119137203.png){width="300"}

## Creating ESB artifacts

Once you have created the ESB projects described above, you can create
the artifacts under those projects.

### ESB configurations

After creating the [ESB Config project](#WorkingwithWSO2IntegrationStudio-config_project), you can define the mediation flow by using the required integration artifacts. Right-click the ESB config project, click **New** , and select the
required ESB artifact. See the links given below for more information on
each of the artifacts:

[Proxy
Service](https://docs.wso2.com/display/EI650/Working+with+Proxy+Services)
\| [REST API](https://docs.wso2.com/display/EI650/Working+with+APIs) \|
[Inbound
Endpoint](https://docs.wso2.com/display/EI650/Working+with+Inbound+Endpoints)
\| [Scheduled
Task](https://docs.wso2.com/display/EI650/Scheduling+ESB+Tasks) \|
[Sequence](https://docs.wso2.com/display/EI650/Mediation+Sequences) \|
[Template](https://docs.wso2.com/display/EI650/Working+with+Templates)
\|
[Endpoint](https://docs.wso2.com/display/EI650/Working+with+Endpoints)
\| [Local
Entry](https://docs.wso2.com/display/EI650/Working+with+Local+Registry+Entries)
\| [Message
Processor](https://docs.wso2.com/display/EI650/Message+Processors) \|
[Message Store](https://docs.wso2.com/display/EI650/Message+Stores)

### Registry artifacts 

Registry artifacts are resources (such as images, WSDLs, XSLTs), which
are stored in a central repository. To create such artifacts,
right-click the [Registry Resource
project](#WorkingwithWSO2IntegrationStudio-registry_project) , click
**New** , and select **Registry Resource** . See the instructions on
[creating and using registry
artifacts](https://docs.wso2.com/display/EI650/Working+with+Registry+Artifacts).

### Connectors

After creating the [Connector Exporter
project](#WorkingwithWSO2IntegrationStudio-connector_project) ,
right-click the project, click **New** , and select **Add/Remove
Connectors** to start adding connector artifacts to your project. See
the instructions on [working with
connectors](https://docs.wso2.com/display/EI650/Working+with+Connectors+via+Tooling).

### Data Services

Data service artifacts are used for exposing data as a service. After
creating the [Data Service
project](#WorkingwithWSO2IntegrationStudio-data_service_project) ,
right-click the project, click **New** , and select required artifact
types. See the instructions on [creating and using data
services](https://docs.wso2.com/display/EI650/Managing+Data+Integration+Artifacts+via+Tooling). 

### Custom Mediators

After creating the [Mediator
project](#WorkingwithWSO2IntegrationStudio-mediator_project) ,
right-click the project, click **New** , and select required artifact
types. See the instructions on [creating and using custom
mediators](https://docs.wso2.com/display/EI650/Working+with+Mediators+via+Tooling).

## Packaging ESB artifacts

To package the ESB artifacts, you need to create a **[Composite
Application Project](#WorkingwithWSO2IntegrationStudio-capp_project)** .
Use one of the following methods:

### Using an existing composite application

If you have an already created composite appliction project, do the
following to package the [ESB
artifacts](#WorkingwithWSO2IntegrationStudio-CreatingESBartifacts) into
the composite application:

1.  Select the `          pom.xml         ` file that is under the
    composite application project in the project explorer.  
    ![](attachments/119133415/119133459.png){width="350" height="150"}
2.  In the **Dependencies** section, select the artifacts from each of
    the projects.

        !!! info
    
        **Note:** If you have created a custom mediator artifact, it should
        be packaged in the same composite application along with the other
        artifacts that uses the mediator.
    
    ![](attachments/119133415/119133458.png){width="600" height="197"}

3.  Save the artifacts.

### Creating a new composite application

If you have not previously created a composite application project, do
the following to package the artifacts in your ESB Config project.

1.  Open the **Getting Started** view and click **Miscellaneous → Create
    New Composite Application** .  
    ![](attachments/119133415/119134213.png){width="700" height="343"}  
2.  In the **New Composite Application Project** dialog that opens, s
    elect the artifacts from the relevant ESB projects and click
    **Finish** .  
    ![](attachments/119133415/119133453.png){width="500" height="506"}

Alternatively,

1.  Right-click the project explorer and click **New -\> Project** .  
    ![](attachments/119133415/119133533.png){width="900" height="379"}
2.  In the **New Project** dialog that opens, select **Composite
    Application Project** from the list and click **Next** .  
    ![](attachments/119133415/119133534.png){width="500"}
3.  Give a name for the **Composite Application** project and select the
    artifacts that you want to package.  
    ![](attachments/119133415/119133535.png){width="500" height="548"}
4.  In the **Composite Application Project POM Editor** that opens,
    under **Dependencies** , note the information for each of the
    projects you selected earlier.  
    ![](attachments/119133415/119133536.png){width="800" height="392"}

### Generating Docker images

To generate Docker images, follow the steps below:

> **Before you begin:**

> 1.  Install Docker from the [Docker Site](https://docs.docker.com/) .
> 2.  Create a Docker Account at [Docker Hub](https://hub.docker.com) and log in.
> 3.  Start the Docker server.


1.  Open the WSO2 Integration Studio interface.
2.  Open an existing project. Right-click on **Composite Project** and
    then click **Generate Docker Image** .  
    ![](attachments/119133415/119133431.png){width="350" height="698"}  
    The **WSO2 Platform Distribution - Generate Docker Image** wizard
    opens.
3.  Enter information in the wizard as follows:
    1.  In the **Generate Docker Image** page, enter the following
        details:  
        ![](attachments/119133415/119133432.png){width="732"
        height="290"}  
        -   **Name of the application** : The name of the composite
            application with the artifacts created for your EI project.
            The name of the EI projecty is displayed by default, but it
            can be changed if required.
        -   **Application version** : The version of the composite
            application.
        -   **Name of the Docker Image** : A name for the Docker image.
        -   **Docker Image Tag** : A tag for the Docker image to be used
            for reference.
        -   **Export Destination** : Browse for the preferred location
            in your machine to export the Docker image.

        Once you have entered the required details, click **Next** .
    2.  In the next page, select the EI projects that you want to
        include in the Docker image and click **Finish** .  
        ![](attachments/119133415/119133430.png){width="732"}  
          
        Once the Docker image is successfully created, a message similar
        to the following appears in your screen.  
        ![](attachments/119133415/119133429.png){width="300"}

## Testing the integration

You can test artifacts by deploying the [packaged
artifacts](_Working_with_WSO2_Integration_Studio_) in the built-in Micro
Integrator:

1.  Be sure to create a **composite application project** and include
    your artifacts.
2.  Right-click the composite application project and click **Export
    Project Artifacts and Run** .  
    ![](attachments/119133415/119133434.png){width="450" height="498"}
3.  In the dialog that opens, select the artifacts form the composite
    application project that you want to deploy.  
    ![](attachments/119133415/119133435.png){width="500" height="307"}
4.  Click **Finish** . The artifacts will be deployed in the WSO2 Micro
    Integrator and the server will start. See the startup log in the
    **Console** tab:  
    ![](attachments/119133415/119133433.png){width="656" height="250"}
5.  If you find errors in your mediation sequence, use the [debugging
    features](https://docs.wso2.com/display/EI650/Debugging+Mediation)
    to troubleshoot.

## Continuous deployment

### Deploying ESB artifacts in Micro Integrator

WSO2 EI includes an ESB profile and WSO2 Integration Studio. The
light-weight Micro Integrator is already included in your WSO2
Integration Studio package, which allows you to deploy and run the
artifacts instantly.

Once you have created a composite application of your artifacts, you can
export it into a CAR file (.car file):

1.  Select the composite application project in the project explorer,
    right-click, and click **Export Composite Application Project** .  
    ![](attachments/119133415/119134342.png){width="450" height="535"}
2.  In the dialog that opens, give a name for the CAR file, the
    destination where the file should be saved, and click **Next** .
3.  You can select the artifacts that should be packaged in the CAR
    file.
4.  Click **Finish** to generate the CAR file.


### Deploying EI solutions in Integration Cloud

Once you have developed an EI solution, you can host it on the
Integration Cloud to make it available for multiple users. To understand
how to host a solution on Integration Cloud, follow the steps below:

> **Before you begin:**
-   [Register as a user of the Integration
    Cloud](https://wso2.com/integration/cloud/) .
-   [Download WSO2 Integration
    Studio](https://wso2.com/integration/tooling/) .


1.  Create an EI application as follows:
    1.  Open WSO2 Integration Studio. In the **Getting Started** page,
        click the **Hello World Service** template to start creating a
        new EI application based on this template.  
        ![](attachments/119133415/119134204.png){width="800"
        height="388"}
    2.  In the **Create Project Using Hello World Service Template**
        dialog box, enter a name for the application. In this example,
        let's enter **HelloWorldApps** as the name.  
        ![](attachments/119133415/119133420.png){width="600"}  
        Click **Finish** to add the project for the application.
2.  The project currently has the configurations derived from the
    template. Let's modify them as follows:

     > The purpose of this step is to change the default values. You can
        skip it if required.
    
     > You can skip this step if required.
    

    1.  In the left navigator, open the
        `            HelloWorldApplication/src/main/synapse-config/proxy-services/HelloWorld.xml           `
        file. Then click on the **PayloadFactory** icon to open the
        **Payload Factory Mediator** configuration in the **Properties**
        tab.  
        ![](attachments/119133415/119133424.png){width="900"}
    2.  In the **Payload** field, replace the existing value  with
        `            {“data”: “HelloWorld”}           ` .

3.  Before deploying the composite application, you need to know the key
    of the organization to which you are deploying it. To get the
    organization ID, sign in to the Integration Cloud and access your
    organization as follows:

        !!! tip
    
        If you already know the key of the organization to which the
        application needs to be deployed, you can skip this step.
    

    1.  Sign in to the [Integration
        Cloud](https://wso2.com/integration/cloud/) with your
        credentials.
    2.  Click on the following icon tray in the right end of the top
        bar.  
        ![](attachments/119133415/119133425.png){width="52"}  
          
        Then click **Organizations** to open the **Manage
        Organizations** page.  
          
        ![](attachments/119133415/119133428.png){width="500"}  
          
        The keys of the available organizations are displayed as shown
        below.  
        ![](attachments/119133415/119133416.png){width="714"}

4.  Deploy the `          Hello World Application         ` that you
    created as follows:
    1.  In the WSO2 Integration Studio, open your workbench. Then
        right-click on **HelloWorldAppsCompositeApplication** , and then
        click **Deploy to Integration Cloud** . The **WSO2 Integration
        Cloud - Authentication** wizard opens as follows.  
          
        ![](attachments/119133415/119133421.png){width="700"}
    2.  Enter the following information in the wizard:  
        -   **Organization Key** : The key of the organization to which
            you want to deploy the EI application. The required
            organization key needs to be already registered under your
            Integration Cloud account.
        -   **Email** : The email address with which you are registered
            in the Integration Cloud.
        -   **Password** : The password with which you sign in to the
            Integration Cloud.
    3.  Click **Finish** . The **WSO2 Platform Distribution** wizard
        opens.
    4.  In the **WSO2 Platform Distribution** wizard, select the
        applications that you want to include in the CAR file that you
        are deploying to the Integration Cloud. For this example, select
        **HelloWorldApps** as shown below.  
        ![](attachments/119133415/119133422.png){width="700"}
    5.  Click **Next** , and then click **Finish** . A message appears
        to inform you that your application is being deployed to the
        cloud. Once the deployment is complete, the following message
        appears.  
          
        ![](attachments/119133415/119133427.png){width="400"}
5.  Access your organization on Integration Cloud as you did in step 3.
    The **HelloWorldAppsComposite Application** you deployed is
    displayed as follows.  
    ![](attachments/119133415/119133426.png){width="600"}
6.  To create a new version, repeat step 4, sub steps a-c. Then follow
    the steps below to create a new version.
    1.  In the page where you select deployable artifacts, select
        **HelloWorldApps** and click **Next** .
    2.  In the next page, select the **Create New Version** option
        and update the value displayed in the **Application Version**
        field.  
        ![](attachments/119133415/119133419.png){width="645"
        height="467"}
    3.  Click **Finish** .
    4.  Sign in to the [Integration
        Cloud](https://integration.cloud.wso2.com/appmgt/site/pages/index.jag)
        and click on the **HelloWorldApps** application.  
        ![](attachments/119133415/119133417.png){width="900"}  
        The application opens, and the updated version is displayed as
        shown below.  
        ![](attachments/119133415/119133418.png){width="900"}