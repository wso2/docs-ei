# Creating Sequence Templates

Follow the instructions given below to create a new Sequence Template in WSO2 Integration Studio.

## Instructions
Follow these steps to create a new scheduled task.

1.  If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right click the project and go to **New → Template** to open the **New Template Artifact** dialog.
2.  Select **Create a New Template** and click **Next**.
3.  Type a unique name for the template and specify **Sequence Template** as the type of template
    you are creating.
4.  Do one of the following:  
    -   To save the endpoint in an existing ESB Config project in your workspace, click **Browse** and select that project.
    -   To save the endpoint in a new ESB Config project, click **Create new Project** and create the new project.
5.  Click **Finish** . The template is created in the `src/main/synapse-config/templates` folder under the ESB Config project you specified.
6.  Open the new artifact from the project explorer, and update any [optional sequence template properties](../../../references/synapse-properties/template-properties/#sequence-template-properties).