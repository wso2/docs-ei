# Creating an Endpoint
Follow the instructions given below to create a new Endpoint artifact in WSO2 Integration Studio.

## Instructions

1.  If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right-click the project and go to **New → Endpoint** to open the **New Endpoint Artifact** dialog.
2.  Select **Create a New Endpoint** and click **Next**.
3.  Type a unique name for the endpoint, and then select the type of endpoint you are creating.
4.  Specify values for the [required parameter](../../references/synapse-properties/endpoint-properties.md) for the selected endpoint type.
5.  Do one of the following:  
    -   To save the endpoint in an existing ESB Config project in your workspace, click **Browse** and select that project.
    -   To save the endpoint in a new ESB Config project, click **Create new Project** and create the new project.
    -   To save the endpoint in a [registry resource project](../../creating-projects/#registry-resource-project), click **Make this a Dynamic Endpoint**, click **Browse** next to **Save endpoint in**, and then select the registry (Configuration or Governance) where you want to save the endpoint. Lastly, type the endpoint name in the **Registry Path** box.
6.  Click **Finish**. The endpoint is created in the `src/main/synapse-config/endpoints` folder under the ESB Config project or [registry resource project](../../creating-projects/#registry-resource-project) you specified.
7.  Open the new artifact from the project explorer, and update any [optional endpoint properties](../../references/synapse-properties/endpoint-properties.md).

## Examples
..

## Guides

