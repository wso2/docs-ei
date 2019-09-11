# Creating Registry Resources

Initially, your registry resources project will contain only a `         pom        ` file. You can create any number of registry resources inside that project. To create a registry resource:

1.  If you have already created an [Registry Resource project](../../creating-projects/#registry-resource-project), right-click the project in the left navigation panel, click **New** , and then click **Registry Resource**. This will open the **New Registry Resource** window.
2.  Select the **From existing template** option as shown below and click **Next**.

#### From existing template

Use the **From existing template** option if you want to select a template from which to create a registry resource.

1.  Select **From existing template** and click **Next**.
2.  In the **Template** field, select a template from the list. In this example, a WSDL file template is used.
3.  Specify a name for the `          WSDL         ` file.
4.  In the **Registry Path** field, define where you want to save the resource in the registry.
5.  In the **Save Resource in** field, select an existing Registry Resource project in which you want to save the resource. Alternatively, you can create a new Registry Resource project.       
6.  Click **Finish** .
7.  Now you will see the `          WSDL         ` file generated with
    the specified name and opened in the embedded WSDL editor.

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

#### Check-out from registry

1.  Click **Check-out from registry** and click **Next**.
2.  Specify the path and artifact name.
3.  In the **Registry Path to deploy** field, specify where the registry
    resource should be checked-in at the time of deployment.
4.  In the **Save Resource in** field, select an existing Registry
    Resource project in which you want to save the resource.
    Alternatively, you can create a new Registry Resource project.
5.  Click **Finish** .

### Editing a registry resource

You may need to change the details you entered for a registry resource, for example, the registry path. You can edit such information using the Registry Resource Editor. To open the Registry Resource Editor, right-click on the Registry Resources project and click **Registry Resource Editor**.

This editor lists all the registry resources that you have defined in that project and it will list the **Registry Path to Deploy**
information per resource.