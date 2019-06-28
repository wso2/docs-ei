# Working with Registry Artifacts

This page describes how to create artifacts for Registry. It contains
the following sections:

-   [Creating a Registry Resource
    project](#WorkingwithRegistryArtifacts-CreatingaRegistryResourceproject)
-   [Creating registry
    resources](#WorkingwithRegistryArtifacts-Creatingregistryresources)
    -   [From existing
        template](#WorkingwithRegistryArtifacts-Fromexistingtemplate)
    -   [Import from file
        system](#WorkingwithRegistryArtifacts-Importfromfilesystem)
    -   [Import Registry dump file from file
        system](#WorkingwithRegistryArtifacts-ImportRegistrydumpfilefromfilesystem)
    -   [Check-out from
        registry](#WorkingwithRegistryArtifacts-Check-outfromregistry)
-   [Editing a registry
    resource](#WorkingwithRegistryArtifacts-Editingaregistryresource)
-   [Creating a Smooks configuration
    artifact](#WorkingwithRegistryArtifacts-SmooksConfigArtifactCreatingaSmooksconfigurationartifact)

### Creating a Registry Resource project

!!! warning Note that registry resources created in WSO2 Integration
Studio (and deployed via Composite Applications) will only support the
registry aspect and not the governance aspect when used with Registry.

You can save resources such as images, WSDLs, XSLTs in a central
repository as registry resources. All these registry resources need to
be saved in a separate project called a Registry Resources project. To
create a Registry Resources project, follow the steps below.

1.  Open **WSO2 Integration Studio** and click **Miscelleneous → Create
    New Registry **Project**** in the **Getting Started** tab as shown
    below.

    ![](attachments/119131643/119134750.png){width="800" height="396"}

2.  Enter a name for the project and click **Next** .
3.  Enter the Maven information about the project and click **Finish** .
4.  The new project will be listed in the project explorer.

### Creating registry resources

Initially, your registry resources project will contain only a
`         pom        ` file. You can create any number of registry
resources inside that project. To create a registry resource:

1.  Right-click the registry resource project in the left navigation
    panel, click **New** , and then click **Registry Resource** . This
    will open the **New Registry Resource** window.
2.  Select the **From existing template** option as shown below and
    click **Next** .  
    ![](attachments/119131643/119131645.png){width="500" height="539"}

-   [From existing
    template](#WorkingwithRegistryArtifacts-Fromexistingtemplate)
-   [Import from file
    system](#WorkingwithRegistryArtifacts-Importfromfilesystem)
-   [Import Registry dump file from file
    system](#WorkingwithRegistryArtifacts-ImportRegistrydumpfilefromfilesystem)
-   [Check-out from
    registry](#WorkingwithRegistryArtifacts-Check-outfromregistry)

#### From existing template

Use the **From existing template** option if you want to select a
template from which to create a registry resource.

1.  Select **From existing template** and click **Next** .
2.  In the **Template** field, select a template from the list. In this
    example, a WSDL file template is used.
3.  Specify a name for the `          WSDL         ` file.
4.  In the **Registry Path** field, define where you want to save the
    resource in the registry.
5.  In the **Save Resource in** field, select an existing Registry
    Resource project in which you want to save the resource.
    Alternatively, you can create a new Registry Resource project.  
    ![](attachments/119131643/119131644.png){width="500" height="540"}  
      
6.  Click **Finish** .
7.  Now you will see the `          WSDL         ` file generated with
    the specified name and opened in the embedded WSDL editor as shown
    below.  
    ![](attachments/119131643/119131646.png){width="700"}  

#### Import from file system

Use the **Import from file system** option to import a file or a folder
containing registry resources. This helps you import a resource and
collection from the same registry instance or a different registry
instance that you have added. Similarly, you can export a resource or
collection to the same registry instance or a different registry
instance.

1.  Click the **Import from file system** option and click **Next** .
2.  Click **Browse file** or **Browse folder** and browse to the
    relevant file or folder.
3.  If you browsed to a folder, the **Copy content only** check box will
    be enabled. Select the check box if you want to copy only the
    content of the folder and not the folder itself to the location you
    specify below.
4.  In the **Registry Path to deploy** field, specify where the registry
    resource should be checked-in at the time of deployment.
5.  In the **Save Resource in** field, select an existing Registry
    Resource project in which you want to save the resource.
    Alternatively, you can create a new Registry Resource project.
6.  Click **Finish** .  
      
    ![](attachments/119131643/119131677.png)

#### Import Registry dump file from file system

Use this option to browse to a Registry Dump file which you can use to
sync a registry.

1.  Click the **Import Registry dump file from file system** option and
    click **Next** .
2.  Click **Browse** and browse to the relevant file.
3.  In the **Registry Path to deploy** field, specify where the registry
    resource should be checked-in at the time of deployment.
4.  In the **Save Resource in** field, select an existing Registry
    Resource project in which you want to save the resource.
    Alternatively, you can create a new Registry Resource project.
5.  Click **Finish** .  
    ![](attachments/119131643/119131676.png)

#### Check-out from registry

1.  Click **Check-out from registry** and click **Next** .
2.  Specify the path and artifact name.
3.  In the **Registry Path to deploy** field, specify where the registry
    resource should be checked-in at the time of deployment.
4.  In the **Save Resource in** field, select an existing Registry
    Resource project in which you want to save the resource.
    Alternatively, you can create a new Registry Resource project.
5.  Click **Finish** .

### Editing a registry resource

You may need to change the details you entered for a registry resource,
for example, the registry path. You can edit such information using
the Registry Resource Editor. To open the Registry Resource Editor,
right-click on the Registry Resources project and click **Registry
Resource Editor** .

This editor lists all the registry resources that you have defined in
that project and it will list the **Registry Path to Deploy**
information per resource.

![](attachments/119131643/119131675.png)

### Creating a Smooks configuration artifact

[Smooks](http://www.smooks.org/) is an extensible framework for building
applications that process data, such as binding data objects and
transforming data. To create a Smooks configuration artifact, you must
first create a registry resource as described in [Creating Registry
Resources](https://docs.wso2.com/display/EI6xx/Working+with+Registry+Artifacts#WorkingwithRegistryArtifacts-Creatingregistryresources)
. When creating the registry resource, select the **From Existing
Template** option and select **Smooks Configuration** as the template.  
  
![](attachments/119131643/119131653.png)

The `         smooksconfig.xml        ` file is created. Double-click it
in the Project Explorer to open it in the embedded JBoss Smooks
editor.  

![](attachments/119131643/119131654.png)  
  
Click **Input Task** , create the data mapping, and save the
configuration file.

Before you can run the Smooks configuration, you must add libraries from
the Smooks framework to your registry resources project.

1.  Right-click the registry resources project in the Project Explorer
    and click **Properties** .
2.  In the list on the left, click **Java Build Path** , and then click
    the **Libraries** tab.  
      
    ![](attachments/119131643/119131655.png)
3.  Click **Add Library** , click **WSO2 Classpath Libraries** , and
    then click **Next** .
4.  In the **WSO2 Classpath Libraries** dialog box, click the **Smooks**
    tab, click **Select All** , and click **Finish** .
5.  In the **Properties** dialog box, click **Apply and Close** .

All the Smooks-related libraries have been added to the project
classpath. You can now run the Smooks configuration file by
right-clicking the file and choosing **Run As \> Smooks Run
Configuration** . If your Smooks configuration is correct, the console
displays the results according to the input model and output model you
specified.

You can now add the Smooks configuration artifact to a proxy service or
sequence to use it in the EI. To do so, create a [proxy
service](https://docs.wso2.com/display/EI650/Working+with+Proxy+Services)
. Drag and drop a Log mediator and a Smooks mediator to the
**InSequence** . Double-click on the **Smooks** mediator to see the
**Property** view. Click the button at the right hand corner of the
**Configuration Key** field.

![](attachments/119131643/119131652.png){width="900"}

The **Resource Key Editor** dialog box appears. Specify from where you
should select the resource file.

![](attachments/119131643/119131651.png){height="400"}

Select **Workspace** option since we have the created **Smooks
Configuration** in our workspace. Browse for the **Smooks
Configuration** file that we have created and click **OK** .

![](attachments/119131643/119131650.png){width="452"}

Now you have successfully referred to the Smooks configuration within
your proxy service.
