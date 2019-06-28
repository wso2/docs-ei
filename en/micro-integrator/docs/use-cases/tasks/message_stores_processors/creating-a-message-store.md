# Creating a Message Store

You can create a new message store or import an existing message store
from the file system using WSO2 Integration Studio.

-   [Before you begin](#CreatingaMessageStore-Beforeyoubegin)
-   [Creating a message
    store](#CreatingaMessageStore-createCreatingamessagestore)
-   [Importing a message
    store](#CreatingaMessageStore-importImportingamessagestore)

### Before you begin

!!! tip

You need to have WSO2 Integration Studio installed to create a new
message store or to import an existing message store. For instructions,
see [Installing WSO2 Integration
Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio)
.


### Creating a message store

Follow these steps to create a new message store. Alternatively, you can
[import an existing message store](#CreatingaMessageStore-import) .

First, create an **ESB Solution Project** in WSO2 Integration Studio. We
will use this project to store the REST API file.

1.  Open **WSO2 Integration Studio** and click **Miscellaneous → Create
    New Config Project ** in the **Getting Started** tab.  
    ![](attachments/119131505/119133606.png){width="800" height="397"}

2.  Enter a project name and click **Finish** .

3.  The new project will be listed in the project explorer.
4.  Right click the project and go to **New → Message Store** to open
    the **New Message Store Artifact** dialog.
5.  Leave the **Create a new message-store artifact** option selected
    and click **Next** .
6.  Type a unique name for this message store, specify the type of store
    you are creating, and then specify values for the other fields
    required to create the store type you you selected.
7.  Do one of the following:  
    -   To save the message store in an existing EI Config project in
        your workspace, click **Browse** and select that project.
    -   To save the message store in a new EI Config project, click
        **Create a new EI Project** and create the new project.
8.  Click **Finish** . The message store is created in the
    `          src/main/synapse-config/message-stores         ` folder
    under the EI Config project you specified and appears in the editor.
    You can click its icon in the editor to view its properties.

### Importing a message store

Follow these steps to import an existing message store into an EI Config
project. Alternatively, you can [create a new message
store](#CreatingaMessageStore-create) .

1.  Open **WSO2 Integration Studio** and click **Miscellaneous Project
    → Create New Config Project ** in the **Getting Started** tab.  
    ![](attachments/119131505/119133606.png){width="800" height="397"}
2.  Enter a project name and click **Finish** .

3.  The new project will be listed in the project explorer.
4.  Right click the project and go to **New → Message Store** to open
    the **New Message Store Artifact** dialog.
5.  Select **Import a Message Store** and click **Next** .
6.  Specify the XML file that defines the message store by typing its
    full path name or click **Browse** and navigating to the file.
7.  In the **Save In** field, specify an existing EI Config project in
    your workspace where you want to save the message store, or click
    **Create new Project** to create a new EI Config project and save
    the message store configuration there.
8.  Click **Finish** . The message store is created in the
    `          src/main/synapse-config/message-stores         ` folder
    under the EI Config project you specified and appears in the editor.
