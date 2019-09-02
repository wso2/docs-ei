# Creating a Message Store

Follow the instructions given below to create a new Message Store artifact in WSO2 Integration Studio.

## Instructions

1. If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right-click the project and go to **New → Message Store** to open the **New Message Store Artifact** dialog.
2. Select the **Create a new message-store artifact** option and click **Next**.
3. Type a unique name for the message store, and then select the type of message store you are creating.
4. Specify values for the [required properties](../../references/synapse-properties/message-store-properties.md) for the selected message store.
5. Do one of the following:  
	-   To save the endpoint in an existing ESB Config project in your workspace, click **Browse** and select that project.
	-   To save the endpoint in a new ESB Config project, click **Create new Project** and create the new project.
6. Click **Finish**. The message store is created in the `src/main/synapse-config/message-stores` folder under the ESB Config project you specified.
7. Open the new artifact from the project explorer, and update any [optional message store properties](../../references/synapse-properties/message-store-properties.md).

## Examples
..

## Guides
..
