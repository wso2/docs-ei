# Creating Endpoint Templates

Follow the instructions given below to create a new Endpoint Template in WSO2 Integration Studio.

## Instructions
Follow these steps to create a new Endpoint Template.

1.  If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right click the project and go to **New → Template** to open the **New Template Artifact** dialog.
2.  Select **Create a New Template** and click **Next**.
3.  Type a unique name for the template and select one of the following **Endpoint Template** types: <b>Address Endpoint Template</b>, <b>Default Endpoint Template</b>, <b>HTTP Endpoint Template</b>, or <b>WSDL Endpoint Template</b>.
4. Specify values for the [required parameter](../../../references/synapse-properties/template-properties/#endpoint-template-properties) for the selected endpoint type.
5.  Do one of the following:  
    -   To save the endpoint in an existing ESB Config project in your workspace, click **Browse** and select that project.
    -   To save the endpoint in a new ESB Config project, click **Create new Project** and create the new project.
6.  Click **Finish** . The template is created in the `src/main/synapse-config/templates` folder under the ESB Config project you specified.
7.  Open the new artifact from the project explorer, and update any [optional endpoint template properties](../../../references/synapse-properties/template-properties/#endpoint-template-properties).
