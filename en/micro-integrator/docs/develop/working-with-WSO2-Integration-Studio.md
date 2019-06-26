# Working with WSO2 Integration Studio

The following sections describe how you can use the development
experience provided by WSO2 Integration Studio to create and manage the
artifacts for your integration use case.

-   [Installing WSO2 Integration
    Studio](#WorkingwithWSO2IntegrationStudio-InstallingWSO2IntegrationStudio)
-   [Using Templates](#WorkingwithWSO2IntegrationStudio-UsingTemplates)
-   [Working with ESB
    artifacts](#WorkingwithWSO2IntegrationStudio-WorkingwithESBartifacts)
    -   [Creating ESB
        projects](#WorkingwithWSO2IntegrationStudio-CreatingESBprojects)
    -   [Importing ESB
        projects](#WorkingwithWSO2IntegrationStudio-ImportingESBprojects)
    -   [Creating ESB
        artifacts](#WorkingwithWSO2IntegrationStudio-CreatingESBartifacts)
    -   [Packaging ESB
        artifacts](#WorkingwithWSO2IntegrationStudio-PackagingESBartifacts)
        -   [Using an existing composite
            application](#WorkingwithWSO2IntegrationStudio-Usinganexistingcompositeapplication)
        -   [Creating a new composite
            application](#WorkingwithWSO2IntegrationStudio-Creatinganewcompositeapplication)
        -   [Generating Docker
            images](#WorkingwithWSO2IntegrationStudio-GeneratingDockerimages)
    -   [Exporting the ESB
        artifacts](#WorkingwithWSO2IntegrationStudio-ExportingtheESBartifacts)
    -   [Testing the ESB
        artifacts](#WorkingwithWSO2IntegrationStudio-TestingtheESBartifacts)
    -   [Deploying ESB
        artifacts](#WorkingwithWSO2IntegrationStudio-DeployingESBartifacts)
        -   [Using the ESB
            profile](#WorkingwithWSO2IntegrationStudio-UsingtheESBprofile)
        -   [Deploying EI solutions in Integration
            Cloud](#WorkingwithWSO2IntegrationStudio-DeployingEIsolutionsinIntegrationCloud)
-   [Working with business process
    artifacts](#WorkingwithWSO2IntegrationStudio-Workingwithbusinessprocessartifacts)
    -   [Creating business
        workflows](#WorkingwithWSO2IntegrationStudio-Creatingbusinessworkflows)
    -   [Packaging BPMN
        artifacts](#WorkingwithWSO2IntegrationStudio-PackagingBPMNartifacts)
    -   [Deploying BPMN
        artifacts](#WorkingwithWSO2IntegrationStudio-DeployingBPMNartifacts)
    -   [Packaging BPEL/Human Task
        artifacts](#WorkingwithWSO2IntegrationStudio-PackagingBPEL/HumanTaskartifacts)
    -   [Deploying BPEL/Human Task
        artifacts](#WorkingwithWSO2IntegrationStudio-DeployingBPEL/HumanTaskartifacts)

### Installing WSO2 Integration Studio

For instructions, see [Installing WSO2 Integration
Studio](_Installing_WSO2_Integration_Studio_) .

### Using Templates

WSO2 Integration Studio provides a set of artifact templates that will
help you get started with the most prominent integration use cases. C
lick the icon on the top-right of the WSO2 Integration Studio interface
to open the **Getting Started** view shown below and then select the
required template. There are ESB templates, data services templates, as
well as business process templates.

![](attachments/119133415/119134220.png){width="850"}  

### Working with ESB artifacts

See the topics given below for instructions on how to use WSO2
Integration Studio to build your integration use case.

#### Creating ESB projects

An ESB solution consists of one or several project directories. These
directories (listed below) store the various ESB artifacts that you
create for your integration sequence.

<table>
<tbody>
<tr class="odd">
<td><div class="content-wrapper">
<p><img src="attachments/119133415/119133510.png" /></p>
</div></td>
<td><div class="content-wrapper">
<p><strong>ESB Config Project</strong></p>
<p>This project directory stores the ESB artifacts that are used when defining a mediation flow. Use one of the following approaches to create an ESB config project:</p>
<div class="panel" style="background-color: #ffffff;border-color: #542989;border-width: 1px;">
<div class="panelContent" style="background-color: #ffffff;">
<div id="expander-946398338" class="expand-container">
<div id="expander-control-946398338" class="expand-control">
<img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> Creating an ESB Solution that includes an ESB Config project
</div>
<div id="expander-content-946398338" class="expand-content">
<ol>
<li><p>Open <strong>WSO2 Integration Studio</strong> and click <strong>ESB Project → Create New <strong></strong></strong> in the <strong>Getting Started</strong> view as shown below.</p>
<p><img src="attachments/119133415/119134178.png" width="600" height="279" /></p></li>
<li><p>In the <strong>New ESB Solution Project</strong> dialog that opens, enter a name for the ESB config project. Select the relevant check boxes if you want to create a <a href="#WorkingwithWSO2IntegrationStudio-RegistryResourceProject"><strong>Registry Resources</strong> project</a> , <a href="#WorkingwithWSO2IntegrationStudio-ConnectorExporterProject"><strong>Connector Exporter</strong> project</a> , and/or a <a href="#WorkingwithWSO2IntegrationStudio-CompositeApplicationProject"><strong>Composite Application</strong> project</a> along with the <strong>ESB Config</strong> project.<br />
<img src="attachments/119133415/119133501.png" width="500" height="518" /></p></li>
<li><p>Click <strong>Finish</strong> to save the projects. The ESB projects are listed in the project explorer as shown below.<br />
<img src="attachments/119133415/119133500.png" width="350" /></p></li>
</ol>
</div>
</div>
</div>
</div>
<div class="panel" style="background-color: #ffffff;border-color: #542989;border-width: 1px;">
<div class="panelContent" style="background-color: #ffffff;">
<div id="expander-1633646892" class="expand-container">
<div id="expander-control-1633646892" class="expand-control">
<img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> Creating an individual ESB Config project
</div>
<div id="expander-content-1633646892" class="expand-content">
<ol>
<li>Open <strong>WSO2 Integration Studio</strong> and click <strong>Miscellaneous → Create New Config Project <strong></strong></strong> in the <strong>Getting Started</strong> view as shown below.<br />
<img src="attachments/119133415/119134180.png" width="700" height="347" /></li>
<li>In the dialog that opens, select <strong>New ESB Config Project</strong> and click <strong>Next</strong> .<br />
<img src="attachments/119133415/119133493.png" width="550" height="291" /></li>
<li>Enter a name for the ESB config project.<br />
<img src="attachments/119133415/119133455.png" width="500" height="365" /></li>
<li>Click <strong>Finish</strong> and see that the project is now listed in the project explorer.<br />
<img src="attachments/119133415/119133454.png" width="250" height="108" /></li>
</ol>
</div>
</div>
</div>
</div>
<p>You can now start creating the <a href="#WorkingwithWSO2IntegrationStudio-CreatingESBartifacts">ESB config artifacts</a> in your ESB Config project.</p>
</div></td>
</tr>
<tr class="even">
<td><div class="content-wrapper">
<p><img src="attachments/119133415/119133509.png" /></p>
</div></td>
<td><div class="content-wrapper">
<p><strong>Registry Resource Project</strong></p>
<p>Create this project directory if you want to create registry resources for your mediation flow. You can later use these registry artifacts when you define your mediation sequences in the <a href="#WorkingwithWSO2IntegrationStudio-config_project">ESB config project</a> .</p>
<div class="panel" style="background-color: #ffffff;border-color: #542989;border-width: 1px;">
<div class="panelContent" style="background-color: #ffffff;">
<div id="expander-1902673336" class="expand-container">
<div id="expander-control-1902673336" class="expand-control">
<img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> Creating a Registry project
</div>
<div id="expander-content-1902673336" class="expand-content">
<ol>
<li>Open <strong>WSO2 Integration Studio</strong> and click <strong>Miscellaneous → Create New Registry Project <strong></strong></strong> in the <strong>Getting Started</strong> view as shown below.<br />
<img src="attachments/119133415/119134182.png" width="700" height="347" /></li>
<li>In the dialog that opens, enter a name for the registry project.<br />
<img src="attachments/119133415/119133448.png" width="600" height="428" /></li>
<li>Click <strong>Finish</strong> and see that the project is now listed in the project explorer.<br />
<img src="attachments/119133415/119133447.png" width="300" /></li>
</ol>
</div>
</div>
</div>
</div>
<p>See the instructions on <a href="https://docs.wso2.com/display/EI650/Working+with+Registry+Artifacts">creating and using registry artifacts</a> .</p>
</div></td>
</tr>
<tr class="odd">
<td><div class="content-wrapper">
<p><img src="attachments/119133415/119133506.png" /></p>
</div></td>
<td><div class="content-wrapper">
<p><strong>Mediator Project</strong></p>
<p>Create this project directory to start creating custom mediator artifacts. You can use these customer mediators when you define the mediation flow in your <a href="#WorkingwithWSO2IntegrationStudio-config_project">ESB config project</a> .</p>
<div class="panel" style="background-color: #ffffff;border-color: #542989;border-width: 1px;">
<div class="panelContent" style="background-color: #ffffff;">
<div id="expander-881464626" class="expand-container">
<div id="expander-control-881464626" class="expand-control">
<img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> Creating a Mediator project
</div>
<div id="expander-content-881464626" class="expand-content">
<ol>
<li>Open <strong>WSO2 Integration Studio</strong> and click <strong>Miscellaneous → Create Mediator Project <strong></strong></strong> in the <strong>Getting Started</strong> view as shown below.<br />
<strong><img src="attachments/119133415/119134184.png" width="800" height="410" /></strong></li>
<li>In the dialog that opens, select <strong>Create New Mediator</strong> and click <strong>Next</strong> .<br />
<img src="attachments/119133415/119133442.png" width="500" /></li>
<li>Enter a project name, package name, and class name.<br />
<img src="attachments/119133415/119133443.png" width="600" height="493" /></li>
<li>Click <strong>Finish</strong> and see that the project is now listed in the project explorer.<br />
<img src="attachments/119133415/119133444.png" width="741" height="250" /></li>
</ol>
</div>
</div>
</div>
</div>
<p>See the instructions on <a href="https://docs.wso2.com/display/EI650/Working+with+Mediators+via+Tooling">creating and using custom mediators</a> .</p>
</div></td>
</tr>
<tr class="even">
<td><div class="content-wrapper">
<p><img src="attachments/119133415/119133502.png" /></p>
</div></td>
<td><div class="content-wrapper">
<p><strong>Data Service Project</strong></p>
<p>Create this project directory to start creating data services (.dbs files) for exposing various datasources as a service.</p>
<div class="panel" style="background-color: #ffffff;border-color: #542989;border-width: 1px;">
<div class="panelContent" style="background-color: #ffffff;">
<div id="expander-2144093255" class="expand-container">
<div id="expander-control-2144093255" class="expand-control">
<img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> Creating a Data Service project
</div>
<div id="expander-content-2144093255" class="expand-content">
<ol>
<li>Open <strong>WSO2 Integration Studio</strong> and click <strong>DS Project → Create New Data Service</strong> in the <strong>Getting Started</strong> view as shown below.<br />
<img src="attachments/119133415/119134218.png" width="800" height="386" /></li>
<li>In the dialog that opens, enter a project name and click <strong>Next</strong> .<br />
<img src="attachments/119133415/119133440.png" width="500" height="427" /></li>
<li>Click <strong>Finish</strong> and see that the project is now listed in the project explorer.<br />
<img src="attachments/119133415/119133439.png" width="300" height="112" /></li>
</ol>
</div>
</div>
</div>
</div>
<p>See instructions on <a href="https://docs.wso2.com/display/EI650/Managing+Data+Integration+Artifacts+via+Tooling">managing data service artifacts using WSO2 Integration Studio</a> .</p>
</div></td>
</tr>
<tr class="odd">
<td><div class="content-wrapper">
<p><img src="attachments/119133415/119133508.png" /></p>
</div></td>
<td><div class="content-wrapper">
<p><strong>Connector Exporter Project</strong></p>
<p>Create this project directory if you have used <a href="https://docs.wso2.com/display/EI650/Working+with+Connectors">ESB connectors</a> in your medition sequence (defined in the <a href="#WorkingwithWSO2IntegrationStudio-config_project">ESB config project</a> ). All connector artifacts need to be stored in a connector exporter project <a href="#WorkingwithWSO2IntegrationStudio-PackagingESBartifacts">before packaging</a> . See the instructions on <a href="https://docs.wso2.com/display/EI650/Working+with+Connectors+via+Tooling">creating and using connectors</a> .</p>
</div></td>
</tr>
<tr class="even">
<td><div class="content-wrapper">
<p><img src="attachments/119133415/119133507.png" /></p>
</div></td>
<td><div class="content-wrapper">
<p><strong>Composite Application Project</strong></p>
<p>This poject directory allows you to package all the artifacts (stored in other ESB projects) into one composite application (C-APP). This C-APP can then be deployed in the ESB server. See the instructions on <a href="#WorkingwithWSO2IntegrationStudio-PackagingESBartifacts">packaging ESB artifacts</a> .</p>
</div></td>
</tr>
</tbody>
</table>

You can use the above ESB projects and other various projects as
follows:

1.  Right-click the **Project Explorer** and click **New → Project** as
    shown below.  
    ![](attachments/119133415/119133438.png){width="354" height="250"}
2.  In the **New Project** dialog that opens, select the required
    project.  
    ![](attachments/119133415/119133437.png){width="450"}

#### Importing ESB projects

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

#### Creating ESB artifacts

Once you have created the ESB projects described above, you can create
the artifacts under those projects.

-   [**ESB Config Artifacts**](#ebf4d4190e8241e1a747de0825da559f)
-   [**Registry Artifacts**](#219d22ddee384c8387b8a9b7ee808c9d)
-   [**Connectors**](#4feb6aedb74648eebaa581ed3a845f27)
-   [**Data Service**](#d57115e9b2ae4ee8bdbfd26db7c4ec02)
-   [**Custom Mediators**](#dbf83f7aa0e1467fb34cb8b77d5cccb3)

After creating the [ESB Config
project](#WorkingwithWSO2IntegrationStudio-config_project) , you can
define the mediation flow by using the required integration artifacts. R
ight-click the ESB config project, click **New** , and select the
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

![](attachments/119133415/119133499.png){width="600" height="471"}  

Registry artifacts are resources (such as images, WSDLs, XSLTs), which
are stored in a central repository. To create such artifacts,
right-click the [Registry Resource
project](#WorkingwithWSO2IntegrationStudio-registry_project) , click
**New** , and select **Registry Resource** . See the instructions on
[creating and using registry
artifacts](https://docs.wso2.com/display/EI650/Working+with+Registry+Artifacts)
.

![](attachments/119133415/119133498.png){width="600" height="381"}  

After creating the [Connector Exporter
project](#WorkingwithWSO2IntegrationStudio-connector_project) ,
right-click the project, click **New** , and select **Add/Remove
Connectors** to start adding connector artifacts to your project. See
the instructions on [working with
connectors](https://docs.wso2.com/display/EI650/Working+with+Connectors+via+Tooling)
.

![](attachments/119133415/119133497.png){width="600" height="389"}

Data service artifacts are used for exposing data as a service. After
creating the [Data Service
project](#WorkingwithWSO2IntegrationStudio-data_service_project) ,
right-click the project, click **New** , and select required artifact
types. See the instructions on [creating and using data
services](https://docs.wso2.com/display/EI650/Managing+Data+Integration+Artifacts+via+Tooling)
.

![](attachments/119133415/119133496.png){width="600" height="420"}  

After creating the [Mediator
project](#WorkingwithWSO2IntegrationStudio-mediator_project) ,
right-click the project, click **New** , and select required artifact
types. See the instructions on [creating and using custom
mediators](https://docs.wso2.com/display/EI650/Working+with+Mediators+via+Tooling)
.

![](attachments/119133415/119133495.png){width="621" height="250"}

#### Packaging ESB artifacts

To package the ESB artifacts, you need to create a **[Composite
Application Project](#WorkingwithWSO2IntegrationStudio-capp_project)** .
Use one of the following methods:

##### Using an existing composite application

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

##### Creating a new composite application

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

##### Generating Docker images

To generate Docker images, follow the steps below:

!!! tip

Before you begin:

1.  Install Docker from the [Docker Site](https://docs.docker.com/) .
2.  Create a Docker Account at [Docker Hub](https://hub.docker.com) and
    log in.

3.  Start the Docker server.


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

#### Exporting the ESB artifacts

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

#### Testing the ESB artifacts

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

#### Deploying ESB artifacts

WSO2 EI includes an ESB profile and WSO2 Integration Studio. The
light-weight Micro Integrator is already included in your WSO2
Integration Studio package, which allows you to deploy and run the
artifacts instantly. Alternatively, you can add the ESB profile server
to your evironment and then deploy and run the artifacts in the ESB. See
the instructions given below.

##### Using the ESB profile

To deploy the [packaged
artifacts](#WorkingwithWSO2IntegrationStudio-PackagingESBartifacts) in
the ESB profile of WSO2 EI, you need to first add the ESB server to the
tool. Follow the steps given below.

1.  Open the **Getting Started** view and click **Miscellaneous →Add New
    Server** to open the **New Server** dialog.

    ![](attachments/119133415/119134215.png){width="700" height="344"}  

2.  In the **New Server** dialog that opens, expand the WSO2 folder and
    select the version of your server.  
    ![](attachments/119133415/119133518.png){width="500" height="544"}
3.  Click **Next** . In the CARBON\_HOME field, provide the path to your
    product's home directory and then click **Next** again.
4.  Review the default port details for your server and click **Next**
    .  
    Typically, you can leave these unchanged but if you are already
    running another server on these ports, give unused ports.

        !!! tip
    
        See [Default Ports of WSO2
        Products](https://docs.wso2.com/display/ADMIN44x/Default+Ports+of+WSO2+Products)
        for more information.
    

    ![](attachments/119133415/119133528.png){width="500"}

5.  To deploy the C-App project to your server, select the composite
    application from the list, click **Add** to move it into the
    configured list, and then click **Finish** .  

6.  On the **Servers** tab, note that the server is currently stopped.
    Click the 'play' icon on the tool bar. If prompted to save changes
    to any of the artifact files you created earlier, click **Yes** .

    ![](attachments/119133415/119133513.png){width="600" height="88"}

7.  As the server starts, the **Console** tab appears. Note messages
    indicating that the composite app was successfully deployed.

8.  You can also deploy/redeploy or remove C-Apps from a running server:

-   -   To deploy/remove C-Apps, right-click the server, click **Add and
        Remove** and follow the instructions on the wizard.  
        ![](attachments/119133415/119133452.png){width="500"}
    -   If you want to redeploy a C-App after modifying the included
        artifacts, select the already deployed C-App, right-click and
        click **Redeploy** .

##### Deploying EI solutions in Integration Cloud

Once you have developed an EI solution, you can host it on the
Integration Cloud to make it available for multiple users. To understand
how to host a solution on Integration Cloud, follow the steps below:

!!! tip

Before you begin:

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

        !!! tip
    
        The purpose of this step is to change the default values. You can
        skip it if required.
    
        !!! info
    
        You can skip this step if required.
    

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

### Working with business process artifacts

The following topics explain the steps involved in building BPMN/BPEL
artifacts and Human Tasks for business workflows, and for deploying them
in the Business Process Server profile of WSO2 EI.

#### Creating business workflows

<table>
<tbody>
<tr class="odd">
<td><div class="content-wrapper">
<p><img src="attachments/119133415/119133490.png" /></p>
</div></td>
<td><div class="content-wrapper">
<p><strong>BPMN workflows</strong></p>
<p>If you are using BPMN to define a business workflow, the BPMN artifacts should be stored in the BPMN project. After creating a BPMN project, add a BPMN diagram to that project. See the instructions given below.</p>
<div class="panel" style="background-color: #ffffff;border-color: #542989;border-width: 1px;">
<div class="panelContent" style="background-color: #ffffff;">
<div id="expander-710187260" class="expand-container">
<div id="expander-control-710187260" class="expand-control">
<img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> 1. Creating a BPMN project
</div>
<div id="expander-content-710187260" class="expand-content">
<ol>
<li>Open the <strong>Getting Started</strong> view and click <strong>BP Project → Create New BPMN Project</strong> .<br />
<img src="attachments/119133415/119134200.png" width="700" height="313" /><br />
</li>
<li>In the <strong>Create an Activiti Project</strong> dialog, e nter a name for the project and click <strong>Next</strong> .<br />
<img src="attachments/119133415/119133488.png" width="450" height="357" /></li>
<li>In the next step, select any referenced projects and click <strong>Finish</strong> . <strong></strong> <strong><br />
</strong></li>
<li><p>Click <strong>Open Perspective</strong> in the below message.</p>
<p>!!! tip</p>
<p>You will not get this message if you are already in the <strong>Activiti</strong> perspective. You can access the current perspective from the project explorer.</p>
</p>
<p><img src="attachments/119133415/119133487.png" width="450" height="132" /></p></li>
<li><p>When you click <strong>Finish</strong> , the project will be listed in the project explorer (Activity explorer).</p>
<p><img src="attachments/119133415/119133486.png" width="442" height="250" /></p></li>
</ol>
</div>
</div>
</div>
</div>
<div class="panel" style="background-color: #ffffff;border-color: #542989;border-width: 1px;">
<div class="panelContent" style="background-color: #ffffff;">
<div id="expander-2075405504" class="expand-container">
<div id="expander-control-2075405504" class="expand-control">
<img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> 2. Creating BPMN artifacts
</div>
<div id="expander-content-2075405504" class="expand-content">
<ol>
<li>Right-click your BPMN project and go to <strong>New → Other</strong> .<br />
</li>
<li>Select <strong>BPMN Diagram</strong> and click <strong>Next</strong> .<br />
<img src="attachments/119133415/119133545.png" width="564" height="534" /></li>
<li>Select the <strong>diagrams</strong> folder in your BPMN project, give a file name for your diagram and click <strong>Next</strong> .<br />
<img src="attachments/119133415/119133485.png" width="500" height="509" /><br />
</li>
<li>Select a template diagram or choose to create an empty diagram and click <strong>Finish</strong> .<br />
<img src="attachments/119133415/119133543.png" width="529" height="578" /></li>
<li>You will see that the BPMN diagram has been added under the project you specified and a new empty diagram will open up along with a palette. You can drag and drop notations from the <strong>Pallete</strong> to create the desired diagram.<br />
<img src="attachments/119133415/119133483.png" width="800" height="289" /></li>
</ol>
</div>
</div>
</div>
</div>
<p>See the <a href="https://docs.wso2.com/display/EI650/BPMN+Tutorials">BPMN tutorials</a> for step-by-step instructions on how to create a BPMN workflow.</p>
</div></td>
</tr>
<tr class="even">
<td><div class="content-wrapper">
<p><img src="attachments/119133415/119133491.png" /></p>
</div></td>
<td><div class="content-wrapper">
<p><strong>BPEL workflows</strong></p>
<p>If you are using BPEL to define a business workflow, you need to create a BPEL workflow . See the instructions given below.</p>
<div class="panel" style="background-color: #ffffff;border-color: #542989;border-width: 1px;">
<div class="panelContent" style="background-color: #ffffff;">
<div id="expander-1679650885" class="expand-container">
<div id="expander-control-1679650885" class="expand-control">
<img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> Creating a BPEL workflow
</div>
<div id="expander-content-1679650885" class="expand-content">
<ol>
<li>Open the <strong>Getting Started</strong> view and click <strong>BP Project → Create New BPEL Workflow</strong> .<br />
<img src="attachments/119133415/119134202.png" width="700" height="313" /></li>
<li>In the <strong>New BPEL Project</strong> dialog that opens, select <strong>Create New BPEL Workflow</strong> and click <strong>Next</strong> .<br />
<img src="attachments/119133415/119133482.png" width="500" height="206" /><br />
</li>
<li>Enter the project name, process name, process namespace, and template accordingly, and click <strong>Finish</strong> .<br />
<img src="attachments/119133415/119133549.png" width="622" height="632" /><br />
</li>
<li><p>Click <strong>Open Perspective</strong> in the below message.</p>
<p>!!! tip</p>
<p>You will not get this message if you are already in the <strong>BPEL</strong> perspective. You can access the current perspective from the project explorer.</p>
</p>
<p><img src="attachments/119133415/119133481.png" width="550" /></p></li>
<li><p>A n ew BPEL diagram is added to the project explorer under the BPEL project. You can drag and drop artifact symbols from the pallette to create the desired diagram.</p>
<p><img src="attachments/119133415/119133480.png" width="1000" height="301" /></p></li>
</ol>
</div>
</div>
</div>
</div>
<p>See the <a href="https://docs.wso2.com/display/EI650/BPMN+Tutorials">BPEL tutorials</a> for step-by-step instructions on how to create a BPEL workflow.</p>
</div></td>
</tr>
<tr class="odd">
<td><div class="content-wrapper">
<p><img src="attachments/119133415/119133489.png" /></p>
</div></td>
<td><div class="content-wrapper">
<p><strong>Human Tasks</strong></p>
<p>If you want to integrate a 'human task' into a business workflow, you need to defined a Human Task artifact as explained below.</p>
<div class="panel" style="background-color: #ffffff;border-color: #542989;border-width: 1px;">
<div class="panelContent" style="background-color: #ffffff;">
<div id="expander-2058582196" class="expand-container">
<div id="expander-control-2058582196" class="expand-control">
<img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> Creating human task artifacts
</div>
<div id="expander-content-2058582196" class="expand-content">
<ol>
<li>Open the <strong>Getting Started</strong> view and click <strong>BP Project → Create New Human Task</strong> as shown below.<br />
<img src="attachments/119133415/119134203.png" width="700" height="314" /><br />
</li>
<li><p>In the <strong>Human Task Project Wizard</strong> that opens, enter a project name, process name, process namespace, and click <strong>Finish</strong> .</p>
<p><img src="attachments/119133415/119133479.png" width="500" height="308" /></p>
<p>The Human Task project and artifact files are added to the project explorer as shown below. The 'newfile.ht' file in the project lists the various properties of the human task artifact that should be defined.</p>
<p><img src="attachments/119133415/119133478.png" width="800" height="344" /><br />
</p></li>
<li><p>Click on each of the topics ( <strong>Task Properties</strong> , <strong>Task Input</strong> , <strong>Task Output</strong> , <strong>Presentation Elements</strong> , <strong>People Assignment</strong> ) to expand the view as shown below. See the tutorial on <a href="https://docs.wso2.com/display/EI640/Simulating+a+Simple+Order+Approval+Process">simulating a simple order approval process</a> for instructions defining these properties.</p>
<div class="localtabs-macro">
<div class="aui-tabs horizontal-tabs" data-aui-responsive="true" role="application">
<ul>
<li><a href="#3dcf8ed77f7247f0bc2782925aeb1f5d"><strong>Task Properties</strong></a></li>
<li><a href="#29ce2d5391aa49f1becc6b0219ad69a5"><strong>Task Input</strong></a></li>
<li><a href="#5f25955731264bdeb85bdb6fbe40edbd"><strong>Task Output</strong></a></li>
<li><a href="#3ac8d3d5f6f241219fd9062b34896162"><strong>Presentation Elements</strong></a></li>
<li><a href="#05891a46078d489ba53a8c45f5c07c9a"><strong>People Assignments</strong></a></li>
</ul>
<div id="3dcf8ed77f7247f0bc2782925aeb1f5d" class="tabs-pane active-pane" name="Task Properties">
<p><img src="attachments/119133415/119133477.png" width="650" height="287" /></p>
</div>
<div id="29ce2d5391aa49f1becc6b0219ad69a5" class="tabs-pane" name="Task Input">
<p><img src="attachments/119133415/119133476.png" width="658" height="250" /></p>
</div>
<div id="5f25955731264bdeb85bdb6fbe40edbd" class="tabs-pane" name="Task Output">
<p><img src="attachments/119133415/119133475.png" width="655" height="250" /></p>
</div>
<div id="3ac8d3d5f6f241219fd9062b34896162" class="tabs-pane" name="Presentation Elements">
<p><img src="attachments/119133415/119133474.png" width="700" /></p>
</div>
<div id="05891a46078d489ba53a8c45f5c07c9a" class="tabs-pane" name="People Assignments">
<p><img src="attachments/119133415/119133473.png" width="1000" /></p>
</div>
</div>
</div></li>
</ol>
</div>
</div>
</div>
</div>
<p>See the <a href="https://docs.wso2.com/display/EI650/BPMN+Tutorials">Human Task tutorials</a> for step-by-step instructions on integrating human tasks into a business workflow.</p>
</div></td>
</tr>
</tbody>
</table>

#### Packaging BPMN artifacts

Follow the steps given below.

1.  Select **Window** **-\>** **Show View** **-\>** **Other** in the top
    menu of your screen.  
    ![](attachments/119133415/119133469.png){width="408" height="250"}
2.  Search for **Package Explorer** and click **Open** .  
    ![](attachments/119133415/119133470.png){width="300"}
3.  In the **Package Explorer** , right-click the BPMN project and click
    **Create deployment artifacts** .  
    ![](attachments/119133415/119133468.png){width="500" height="530"}
4.  The BPMN artifacts will be pacakged into a .bar file and stored in
    the / `          deployment         ` folder as shown below.  
    ![](attachments/119133415/119133467.png){width="337" height="250"}  

#### Deploying BPMN artifacts

You can deploy the .bar file of the BPMN process using the management
console of the Business Process profile in WSO2 EI.

1.  [Install WSO2 Enterprise
    Integrator](https://docs.wso2.com/display/EI650/Installing+the+Product)
    and start the Business Process profile by executing one of the given
    commands. See the [installation
    guide](https://docs.wso2.com/display/EI650/Installation+Guide) for
    more information on setting up and running WSO2 EI.

    -   [**On MacOS/Linux/CentOS**](#6d78323dba654fc08afbf167cd3b698a)
    -   [**On Windows**](#e23b7d31958e470890cdd090fd5b0e24)

    Open a terminal and execute the following command:

    ``` java
        wso2ei-6.5.0-business-process
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 **Business Process** .** This will open a terminal and start
    the business process profile.

2.  Open the management console from
    `                     https://localhost:9445/carbon/                   `
    .
3.  Log in by using admin as the username and password.
4.  Go to **Main -\> Manage -\> Add -\>BPMN** and upload the .bar
    file.  
    ![](attachments/119133415/119133515.png){width="526" height="193"}

#### Packaging BPEL/Human Task artifacts

BPEL artifacts and Human Task artifacts can be packaged into separate
.zip files. Follow the steps given below.

1.  Select one of the projects (BPEL or Human Task) from the **Project
    Explorer** .  
    ![](attachments/119133415/119133466.png){width="300"}
2.  Right-click the project and select **Export Project as a Deployable
    Archive** .  
    ![](attachments/119133415/119133465.png){width="250" height="447"}
3.  When the **Project Export** dialog opens, provide the location where
    you want to save the artifact and click **Finish** .  
    ![](attachments/119133415/119133464.png){width="450" height="211"}

This will generate a .zip archive that can be deployed directly in the
Business Process profile of WSO2 EI.

#### Deploying BPEL/Human Task artifacts

Once you have packaged your BPEL or Human Task artifacts, deploy them in
the Business Process profile as follows:

1.  [Install WSO2 Enterprise
    Integrator](https://docs.wso2.com/display/EI650/Installing+the+Product)
    and start the Business Process profile by executing one of the given
    commands. See the [installation
    guide](https://docs.wso2.com/display/EI650/Installation+Guide) for
    more information on setting up and running WSO2 EI.

    -   [**On MacOS/Linux/CentOS**](#83a67b9580a9469baf4614726db6400a)
    -   [**On Windows**](#98a43b75b43d467dab0c8b29a1e74f13)

    Open a terminal and execute the following command:

    ``` java
            wso2ei-6.5.0-business-process
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 **Business Process** .** This will open a terminal and start
    the business process profile.

2.  Open the management console from
    `                     https://localhost:9445/carbon/                   `
    .
3.  Log in by using admin as the username and password.
4.  Go to the **Main** tab and upload the relevant .zip files.  
    -   Click **Add -\>BPEL** and upload the .zip file with your BPEL
        artifacts.  
        ![](attachments/119133415/119133461.png){width="700"
        height="191"}
    -   Click **Human Tasks →Add** and upload the .zip file with your
        Human Task artifacts.  
        ![](attachments/119133415/119133460.png){width="676"
        height="250"}
