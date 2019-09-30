# Creating an Inbound Endpoint

Follow the instructions given below to create a new Inbound Endpoint artifact in WSO2 Integration Studio.

## Instructions

1. If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right-click the project and go to **New → Inbound Endpoint** to open the **New Inbound Endpoint Artifact**.
2. Select **Create a New Inbound Endpoint** and click **Next**.
3. Enter a unique name for the inbound endpoint, and select an **Inbound Endpoint Creation Type** from the list.
4. Specify values for the required parameter for the selected inbound endpoint type.

    !!! Info
        See the list of configurable properties for [listening inbound endpoints](../../references/synapse-properties/inbound-endpoints/listening-inbound-endpoint-properties.md), [polling inbound endpoints](../../references/synapse-properties/inbound-endpoints/polling-inbound-endpoint-properties.md), [event-based inbound endpoints](../../references/synapse-properties/inbound-endpoints/event-based-inbound-endpoint-properties.md), and [custom inbound endpoints](../../references/synapse-properties/inbound-endpoints/custom-inbound-endpoint-properties.md).

	!!! Note
		For certain protocols ( HL7, KAFKA, Custom, MQTT, RabbitMq, WSO2_MB, WS, and  WSS) the **main sequence** and **error sequence** are mandatory fields. You can select sequences that already exist in the workspace and add them to the **Sequence** and **Error sequence** fields. If you don't have any sequences in the workspace, click **Generate Sequence and Error Sequence** to generate new sequences for the inbound endpoint.  
        
5.	Do one of the following:  
    -   To save the endpoint in an existing ESB Config project in your workspace, click **Browse** and select that project.
    -   To save the endpoint in a new ESB Config project, click **Create new Project** and create the new project.
5.  Click **Finish**. The inbound endpoint is created in the `src/main/synapse-config/inbound-endpoint` folder under the ESB Config project you specified.
6.  Open the new artifact from the project explorer, and update any optional inbound endpoint properties.

    !!! Info
        See the list of configurable properties for [listening inbound endpoints](../../references/synapse-properties/inbound-endpoints/listening-inbound-endpoint-properties.md), [polling inbound endpoints](../../references/synapse-properties/inbound-endpoints/polling-inbound-endpoint-properties.md), [event-based inbound endpoints](../../references/synapse-properties/inbound-endpoints/event-based-inbound-endpoint-properties.md), and [custom inbound endpoints](../../references/synapse-properties/inbound-endpoints/custom-inbound-endpoint-properties.md).

!!! Note
    **Redeployment of listening inbound endpoints fail?**

    A **listening inbound endpoint** opens the port for itself during deployment. Therefore, if you are **redeploying** a listening inbound endpoint artifact, the redeployment will not be successful until the port that was previously opened for the inbound endpoint is closed.
    
    By default, the system will wait for 10 seconds for the previously opened port to close down. If you want to increase this waiting time beyond 10 seconds, be sure to add the following system property in the `         carbon.properties        ` file, which is stored in the `MI_HOME/conf/        ` directory and restart the server before redeploying the artifacts.

    ```xml
    -Dsynapse.transport.portCloseVerifyTimeout=20
    ```

    Note that this setting may be required in Windows environments as the process of closing a port can sometimes take longer than 10 seconds.xss

## Examples
..

## Guides
...