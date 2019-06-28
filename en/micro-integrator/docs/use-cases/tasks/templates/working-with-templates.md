# Working with Templates via Tooling

You can create a new template or import an existing template from the
file system using WSO2 Integration Studio.

!!! tip

You need to have WSO2 Integration Studio installed to create a new
template or to import an existing template. For instructions, see
[Installing WSO2 Integration
Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio)
.


#### Creating a template

Follow these steps to create a new scheduled task. Alternatively, you
can [import an existing
template](#WorkingwithTemplatesviaTooling-import) .

1.  Open **WSO2 Integration Studio,** and click **Miscellaneous → Create
    New Config Project ** in the **Getting Started** tab.

2.  Enter a project name and click **Finish** .

3.  The new project will be listed in the project explorer.
4.  Right click the project and go to **New → Template** to open the
    **New Template Artifact** dialog.
5.  Type a unique name for the template and specify the type of template
    you are creating.

    Currently the following three types are supported (See their
    description following the appropriate links):

    -   Address Endpoint Template:
        The Address Endpoint Template has the following options:
        **Template Name** - The unique name for the [endpoint template](_Endpoint_Template_) .
       **Name** - A name for the inline endpoint.
        **Address** - The URL of the template endpoint. This can be a parametrized value such as `          $uri         ` .
    -   Default Endpoint Template
        The **Default Endpoint Template** has the following options:
        **Template Name** - The unique name for the [endpoint template](_Endpoint_Template_) .
        **Name** - A name for the inline endpoint.
    -   The **WSDL Endpoint Template** has the following options:
        **Template Name** - The unique name for the [endpoint template](_Endpoint_Template_) .
        **Name** - A name for the inline endpoint.
        **Specify As** - The method to specify the WSDL. The available values are:  
            **In-lined WSDL** - Paste the WSDL in the text box that appears
        when this option is selected.
           **URI** - Activates the WSDL URI field.
        **WSDL URI** - The URI of the WSDL.
        **Service** - The service selected from the available services for
    the WSDL.
        **Port** - The port selected for the service specified in the above
    field. In a WSDL an endpoint is bound to each port inside each
    service.
    -   HTTP Endpoint Template
        With the **HTTP Endpoint Template** you can define a URI template based REST service endpoint.
        The HTTP Endpoint Template has the following options:
        **Template Name** - The unique name for the [endpoint template](_Endpoint_Template_) .
        **Name** - A name for the inline endpoint.
        **URI Template** - The URI template of the endpoint. Insert `          uri.var.         ` before each variable. Click **Test** to test the URI.

6.  Do one of the following:  
    -   To save the template in an existing ESB Config project in your
        workspace, click **Browse** and select that project.
    -   To save the template in a new ESB Config project, click **Create
        new Project** and create the new project.
7.  If you specified an address or WSDL endpoint as the template type,
    enter the URL for the address or the WSDL URI and connection
    information in the **Advanced Configuration** fields.
8.  Click **Finish** . The template is created in the
    `          src/main/synapse-config/templates         ` folder under
    the ESB Config project you specified. When prompted, you can open
    the file in the editor, or you can right-click the template in the
    project explorer and click **Open With \> ESB Editor** . Click its
    icon in the editor to view its properties.

#### Importing a template

Follow these steps to import an existing template into an ESB Config
project. Alternatively, you can [create a new
template](#WorkingwithTemplatesviaTooling-CreateTemplate) .

1.  Open **WSO2 Integration Studio,** and click **Miscellaneous → Create
    New Config Project ** in the **Getting Started** tab.  
    ![](attachments/119131537/119133619.png){width="800" height="397"}

2.  Enter a project name and click **Finish** .

3.  The new project will be listed in the project explorer.
4.  Right click the project and go to **New → Template** to open the
    **New Template Artifact** dialog.
5.  Select **Import a Template** and click **Next** .
6.  Specify the XML file that defines the template by typing its full
    path name or clicking **Browse** and navigating to the file.
7.  In the **Save Template In** field, specify an existing ESB Config
    project in your workspace where you want to save the template, or
    click **Create new Project** to create a new ESB Config project and
    save the template configuration there.
8.  If there are multiple template definitions in the file, click
    **Create ESB Artifacts** , and then select the templates you want to
    import.
9.  Click **Finish** . The templates you selected are created in the
    subfolders of the
    `          src/main/synapse-config/templates         ` folder under
    the ESB Config project you specified, and the first template appears
    in the editor.
