# Creating a Message Processor

You can create a new message processor or import an existing message
processor from the file system using WSO2 Integration Studio.

-   [Before you begin](#CreatingaMessageProcessor-Beforeyoubegin)
-   [Creating a message
    processor](#CreatingaMessageProcessor-createCreatingamessageprocessor)
-   [Importing a message
    processor](#CreatingaMessageProcessor-importImportingamessageprocessor)

### Before you begin

!!! tip

-   You need to have WSO2 Integration Studio installed to create a new
    message processor or to import an existing message processor. For
    instructions, see [Installing WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio)
    .
-   Be sure to create the message store before creating the message
    processor, as you will need to specify the message store that this
    processor applies to.


### Creating a message processor

Follow these steps to create a new message processor. Alternatively, you
can [import an existing message
processor](#CreatingaMessageProcessor-import) .

1.  Open **WSO2 Integration Studio** and click **Miscellaneous → Create
    New Config Project ** in the **Getting Started** tab.  
    ![](attachments/119131517/119133610.png){width="800" height="397"}

2.  Enter a project name and click **Finish** .

3.  The new project will be listed in the project explorer.
4.  Right click the project and go to **New → Message Processor** to
    open the **New Message Processor Artifact** dialog.
5.  Leave the **Create a new message-processor artifact** option
    selected and click **Next** .
6.  Type a unique name for this message processor, specify the type of
    processor you're creating, specify the message store this processor
    applies to, and then specify values for the other fields required to
    create the processor type you selected.
7.  Do one of the following:  
    -   To save the message processor in an existing EI Config project
        in your workspace, click **Browse** and select that project.
    -   To save the message processor in a new EI Config project, click
        **Create a new EI Project** and create the new project.
8.  Click **Finish** . The message processor is created in the
    `          src/main/synapse-config/message-processors         `
    folder under the EI Config project you specified and appears in the
    editor. You can click its icon in the editor to view its properties.

### Importing a message processor

Follow these steps to import an existing message processor into an EI
Config project. Alternatively, you can [create a new message
processor](#CreatingaMessageProcessor-create) .

1.  Open **WSO2 Integration Studio** and click **Miscellaneous → Create
    New Config Project ** in the **Getting Started** tab.  
    ![](attachments/119131517/119133610.png){width="800" height="397"}

2.  Enter a project name and click **Finish** .

3.  The new project will be listed in the project explorer.
4.  Right click the project and go to **New → Message Processor** to
    open the **New Message Processor Artifact** dialog.
5.  Select **Import a Message Processor** and click **Next** .
6.  Specify the XML file that defines the message processor by typing
    its full path name or clicking **Browse** and navigating to the
    file.
7.  In the **Save In** field, specify an existing EI Config project in
    your workspace where you want to save the message processor, or
    click **Create new Project** to create a new EI Config project and
    save the message processor configuration there.
8.  Click **Finish** . The message processor is created in the
    `          src/main/synapse-config/message-processors         `
    folder under the EI Config project you specified and appears in the
    editor.
