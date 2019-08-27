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

5.  Click **Finish** . The endpoint is created in the `          endpoints         ` folder under the ESB Config or Registry Resource project you specified, and the endpoint opens in the editor.

## Examples
..

## Guides

