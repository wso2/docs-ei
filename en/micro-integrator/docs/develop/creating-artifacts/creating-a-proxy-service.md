# Creating a Proxy Service

Follow the instructions given below to create a new Proxy Service artifact in WSO2 Integration Studio.

## Instructions

1.  If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right-click the project in the navigator and go to **New → Proxy Service** to open the **New Proxy Service** dialog. 
2.  Select **Create New Proxy Service** and click **Next**.
3.  Type a unique name for the proxy service and select a proxy service template from the list shown below. These templates will automatically generate the mediation flow that is required for each use case.

    <table>
    <tr class="header">
    <th>Template Type</th>
    <th>Description</th>
    </tr>
    <tbody>
    <tr class="odd">
    <td>Pass-Through proxy</td>
    <td>This template creates a proxy service that forwards messages to the endpoint without performing any processing.</td>
    </tr>
    <tr class="even">
    <td>Transformer proxy</td>
    <td>This template creates a proxy service that transforms all the incoming requests using XSLT and then forwards them to a given endpoint. It can also transform responses from the backend service.</td>
    </tr>
    <tr class="odd">
    <td>Log Forward proxy</td>
    <td>This template creates a proxy service that first logs all the incoming requests and passes them to a given endpoint. It can also log responses from the backend service before routing them to the client. You can specify the log level for requests and responses.</td>
    </tr>
    <tr class="even">
    <td>WSDL-Based proxy</td>
    <td>This template generates a proxy service from the remotely hosted WSDL of an existing web service. The endpoint information is extracted from the WSDL. 
    </td>
    </tr>
    <tr class="odd">
    <td>Secure proxy</td>
    <td>This template creates a proxy service that uses WS-Security to process incoming requests and forward them to an unsecured backend service. You simply need to provide the policy file that should be used.</td>
    </tr>
    <tr class="even">
    <td>Custom proxy</td>
    <td>This template creates an empty proxy service file, where you can manually create the mediation flow by adding all the sequences, endpoints, transports, and other QoS settings.</td>
    </tr>
    </tbody>
    </table>

4. Do one of the following:  
    -   To save the endpoint in an existing ESB Config project in your workspace, click **Browse** and select that project.
    -   To save the endpoint in a new ESB Config project, click **Create new Project** and create the new project.  
5. Click **Finish**. The proxy service is created in the `src/main/synapse-config/proxy-services` folder under the project you specified.
6. To update the properties of the proxy service:
    1. Open the proxy service artifact from the project explorer and go to the **Design View** of the artifact.
    2. Click the proxy service artifact on the canvas to enable the **Properties** tab.
    3. You can now update the [service-level properties and parameters](../../references/synapse-properties/proxy-service-properties.md).

## Examples
..

## Guides
...
