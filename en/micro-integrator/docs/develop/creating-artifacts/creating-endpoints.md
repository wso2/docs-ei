# Creating an Endpoint
Follow the instructions given below to create a new REST API artifact in WSO2 Integration Studio.

## Instructions

1.  If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right-click the project and go to **New → Endpoint** to open the **New Endpoint Artifact** dialog.
2.  Select **Create a New Endpoint** and click **Next**.
3.  Type a unique name for the endpoint, and then specify the type of
    endpoint you are creating.

    <table>
        <tr>
            <th>Endpoint Type</th>
            <th>Description</th>
        </tr>
        <tr>
            <td><b>Address Endpoint</b></td>
            <td>Defines the direct URL of the service.</td>
        </tr>
        <tr>
            <td><b>Default Endpoint</b></td>
            <td>Defines additional configuration for the default target.</td>
        </tr>
        <tr>
            <td><b>Failover Endpoints</b></td>
            <td>Defines the endpoints that the service will try to connect to in case of a failure. This will take place in a round robin manner.</td>
        </tr>
        <tr>
            <td><b>HTTP Endpoint</b></td>
            <td>Defines a URI template based REST service endpoint.</td>
        </tr>
        <tr>
            <td><b>Load Balance Endpoint</b></td>
            <td>Defines groups of endpoints for replicated services. The incoming requests will be directed to these endpoints in a round robin manner. These endpoints automatically handle the fail-over cases as well.</td>
        </tr>
        <tr>
            <td><b>Recipient List Group</b></td>
            <td>Defines the list of endpoints a message will be routed to.</td>
        </tr>
        <tr>
            <td><b>Template Endpoint</b></td>
            <td>Allows to create a custom Endpoint template.</td>
        </tr>
        <tr>
            <td><b>WSDL Endpoint</b></td>
            <td>Defines the WSDL, service and port.</td>
        </tr>
    </table>

4.  Do one of the following:  
    -   To save the endpoint in an existing ESB Config project in your workspace, click **Browse** and select that project.
    -   To save the endpoint in a new ESB Config project, click **Create new Project** and create the new project.
    -   To save the endpoint in a Registry Resource Project, click **Make this a Dynamic Endpoint**, click the **Browse** button next to **Save endpoint in** and select the registry, and then select the registry (Configuration or Governance) where you want to save the endpoint. Lastly, type the endpoint name in the **Registry Path** box.

## Properties

Click **Add Property** to open the **Properties** tab and add the required properties.

<table>
    <thead>
    <tr class="header">
    <th>Parameter Name</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><strong>Name</strong></td>
    <td>The name of the endpoint property.</td>
    </tr>
    <tr class="even">
    <td><strong>Value</strong></td>
    <td>The value of the endpoint property.</td>
    </tr>
    <tr class="odd">
    <td><strong>Scope</strong></td>
    <td>The scope of the property. Possible values are as follows.
    <ul>
    <li><strong>Synapse</strong></li>
    <li><strong>Transport</strong></li>
    <li><strong>Axis2</strong></li>
    <li><strong>axis2-client</strong></li>
    </ul>
    <p>For more information about these scopes, see <a href="../references/mediators/accessing-Properties-with-XPath.md">XPath Extension Functions</a> .</p></td>
    </tr>
    <tr class="even">
    <td><strong>Action</strong></td>
    <td>This parameter allows you to delete a property.</td>
    </tr>
    </tbody>
</table>

Open the Endpoint artifact from the project explorer, go to the **Form** view, and update the additional configurations (which changes based on the type of endpoint you are creating).

<table>
    <thead>
    <tr class="header">
    <th><p>Field Name</p></th>
    <th><p>Description</p></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Suspend Error Codes</p></td>
    <td><p>This parameter allows you to select one or more error codes from the List of Values. If any of the selected errors is received from the endpoint, the endpoint will be suspended.</p></td>
    </tr>
    <tr class="even">
    <td><p>Initial Duration (in mili seconds))</p></td>
    <td><p>The time duration for which the endpoint will be suspended, when one or more suspend error codes are received from it for the first time.</p></td>
    </tr>
    <tr class="odd">
    <td><p>Max Duration (Millis)</p></td>
    <td><p>The maximum time duration for which the endpoint is suspended when suspend error codes are received from it.</p></td>
    </tr>
    <tr class="even">
    <td><p>Factor</p></td>
    <td><p>The duration to suspend can vary from the first time suspension to the subsequent time. The factor value decides the suspense duration variance between subsequent suspensions.</p></td>
    </tr>
    <tr class="odd">
    <td><p>On Timeout Error codes</p></td>
    <td><p>A list of error codes. If these error codes are received from the endpoint, the request will be subjected to a timeout.</p></td>
    </tr>
    <tr class="even">
    <td><p>Retry</p></td>
    <td><p>The number of re-tries in case of a timeout, caused by the above listed error codes.</p></td>
    </tr>
    <tr class="odd">
    <td><p>Retry Delay(in milliseconds)</p></td>
    <td><p>The delay between retries in milliseconds.</p></td>
    </tr>
    <tr class="even">
    <td><p>Timeout Action</p></td>
    <td><p>The action to be done at a timeout situation. You can select from:</p>
    <ul>
    <li><strong>Never Timeout</strong></li>
    <li><strong>Discard Message</strong></li>
    <li><strong>Execute Fault Sequence</strong></li>
    </ul></td>
    </tr>
    <tr class="odd">
    <td><p>Timeout Duration (in milliseconds)</p></td>
    <td><p>The duration in milliseconds before considering a request to be subjected to a time-out.</p></td>
    </tr>
    <tr class="even">
    <td><p>WS-Addressing</p></td>
    <td><p>Adds WS-Addressing headers to the endpoint.</p></td>
    </tr>
    <tr class="odd">
    <td><p>Separate Listener</p></td>
    <td><p>The listener to the response will be a separate transport stream from the caller.</p></td>
    </tr>
    <tr class="even">
    <td><p>WS-Security</p></td>
    <td><p>Adds WS-Security features as described in a policy key (referring to a registry location).</p></td>
    </tr>
    <tr class="odd">
    <td>Non Retry Error Codes</td>
    <td><p>When a child endpoint of a <a href="_Configuring_Failover_Endpoints_">failover endpoint</a> or <a href="_Load-balance_Group_">load-balance endpoint</a> fails for one of the error codes specified here, the child endpoint will be marked for suspension and the request will not be sent to the next endpoint in the group.</p></td>
    </tr>
    <tr class="even">
    <td>Retry Error Codes</td>
    <td>When adding a child endpoint to a <a href="_Configuring_Failover_Endpoints_">failover endpoint</a> or <a href="_Load-balance_Group_">load-balance endpoint</a> , you can specify the error codes that trigger this node to be <a href="Endpoint-Error-Handling_119131815.html#EndpointErrorHandling-retryConfig">retried</a> instead of suspended when that error is encountered. This is useful when you know that certain errors are transient and that the node will become available again shortly. Note that if you specify an error code as a Retry code on one node in the group but specify that same code as a Non Retry error code on another node in the group, it will be treated as a Non Retry error code for all nodes in the group.</td>
    </tr>
    </tbody>
</table>

Click **Finish** . The endpoint is created in the
    `          endpoints         ` folder under the ESB Config or
    Registry Resource project you specified, and the endpoint opens in
    the editor.

## Examples
..

## Guides

