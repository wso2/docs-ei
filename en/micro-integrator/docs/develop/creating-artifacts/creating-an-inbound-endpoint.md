# Creating an Inbound Endpoint

Follow the instructions given below to create a new Inbound Endpoint artifact in WSO2 Integration Studio.

## Instructions

Follow the steps below to create a new inbound endpoint.

1.  If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right-click the project in the navigator and go to **New → Inbound Endpoint** to open the **New Inbound Endpoint Artifact** dialog:

    ![](attachments/119130498/119130502.png)

2. Select **Create a New Inbound Endpoint** and click **Next** .
3. Enter a unique name for the inbound endpoint, and select an **Inbound Endpoint Creation Type** from the list shown below.

    ![](attachments/119130498/119130501.png)
    
4.  For certain protocols ( HL7, KAFKA, Custom, MQTT, RabbitMq, WSO2_MB, WS, and  WSS) the main sequence and error sequence are mandatory fields. You can select sequences that already exists in the workspace and add them to the **Sequence** and **Error sequence** fields (shown below). If you don't have any sequences incthe workspace, click **Generate Sequence and Error Sequence** to generate new sequences for the inbound endpoint.  
    ![](attachments/119130498/119130500.png)  
5.  Specify the **ESB Solution** project where Inbound Endpoint should be saved.
6.  Click **Finish** to generate the inbound endpoint. The artifact will
    now be saved in the ESB project.

!!! Note
    **Redeployment of listening inbound endpoints fail?**

    A [listening inbound endpoint](_Listening_Inbound_Endpoints_) opens the port for itself during deployment. Therefore, if you are **redeploying** a listening inbound endpoint artifact, the redeployment will not be successful until the port that was previously opened for the inbound endpoint is closed.
    
    By default, the system will wait for 10 seconds for the previously opened port to close down. If you want to increase this waiting time beyond 10 seconds, be sure to add the following system property in the `         carbon.properties        ` file, which is stored in the `MI_HOME/conf/        ` directory and restart the server before redeploying the artifacts.

    ``` java
    -Dsynapse.transport.portCloseVerifyTimeout=20
    ```

    Note that this setting may be required in Windows environments as the process of closing a port can sometimes take longer than 10 seconds.xss

## Properties

Right-click the inbound endpoint and click **Show Properties View** to go to the **Property** tab. You can update the parameters relevant to the type of inbound endpoint.

### Common inbound endpoint parameters

The following parameters are common to all inbound endpoints:

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
<th><p>Possible Values</p></th>
<th><p>Default Value</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>sequential</td>
<td>The behavior when executing the given sequence.<br />
When set as <code>             true            </code> , mediation will happen within the same thread. When set as <code>             false            </code> , the mediation engine will use the inbound thread pool. (The default thread pool values can be found in the <code>             &lt;EI_HOME&gt;/conf/synapse.properties            </code> file).</td>
<td>Yes</td>
<td>true or false</td>
<td>true</td>
</tr>
<tr class="even">
<td>suspend</td>
<td>When set to <code>             true            </code> , this makes the inbound endpoint inactive.</td>
<td>Yes</td>
<td>true or false</td>
<td>false</td>
</tr>
</tbody>
</table>

### Common polling inbound endpoint parameters

The following parameters are common to all polling inbound endpoints:

| Parameter Name | Description                                                                                                                                                                                                                                                                                                                                                       | Required | Possible Values    | Default Value |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|--------------------|---------------|
| interval       | The polling interval for the inbound endpoint to execute each cycle. This value is set in milliseconds.                                                                                                                                                                                                                                                           | No       | A positive integer | \-            |
| coordination   | This parameter only applicable in a cluster environment. In a cluster environment an inbound endpoint will only be executed in worker nodes. If set to `             true            ` in a cluster setup, this will run the inbound only in a single worker node. Once the running worker is down the inbound starts on another available worker in the cluster. | No       | true or false      | true          |

### Common event-based endpoint parameters

The following parameter is common to all event-based inbound endpoints:

<table>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
<th><p>Possible Values</p></th>
<th><p>Default Value</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>coordination</td>
<td>This parameter is only applicable in a cluster environment.<br />
In a cluster environment an inbound endpoint will only be executed in worker nodes. If this parameter is set to <code>             true            </code> in a cluster environment, the inbound will only be executed in a single worker node. Once the running worker node is down the inbound will start on another available worker node in the cluster.</td>
<td>No</td>
<td>true or false</td>
<td>true</td>
</tr>
</tbody>
</table>

## Examples
..

## Guides
...