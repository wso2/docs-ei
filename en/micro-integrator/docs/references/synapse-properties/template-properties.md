# Template Properties

## Sequence Template Properties

The parameters available to configure the Sequence Template are as follows.

| Parameter Name      | Description                                                       |
|---------------------|-------------------------------------------------------------------|
| Name                | The name of the Sequence Template                                 |
| onError             | Select the error sequence that needs to be invoked.               |
| Trace Enabled       | Whether or not trace is to be enabled for the sequence.           |
| Statistics Enabled  | Whether or not statistics is to be enabled for the sequence.      |
| Template Parameters | The input parameter that are supported by this Sequence Template. |


## Endpoint Template Properties

The basic parameters available to configure the Endpoint template are as follows.

<table>
  <tr>
    <th>Parameter</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Name</td>
    <td>The name of the Endpoint template. This name will be referred by Template Endpoint to access the parameters defined within the Endpoint template.</td>
  </tr>
  <tr>
    <td>Parameter Value</td>
    <td>Parameters added to the endpoint template. As the endpoint defined within the endpoint template is parameterized using these parameters. Therefore, these can be accessed from Template Endpoints.</td>
  </tr>
</table>

### Endpoint Properties (Required)

An Endpoint Template contains an **Endpoint** artifact. The following Endpoint properties are required depending on the template type:

<table>
        <tr>
            <th>Template Type</th>
            <th>Description</th>
        </tr>
        <tr>
            <td><b>Address Endpoint Template</b></td>
            <td>
                The following information needs to be specified:
                <ul>
                    <li><b>Template Name</b>: The unique name for the endpoint template.</li>
                    <li><b>Name</b>: A name for the inline endpoint.</li>
                    <li><b>Address</b>: The URL of the template endpoint. This can be a parametrized value such as <code>$uri</code>.</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><b>Default Endpoint Template</b></td>
            <td>
                The following information needs to be specified:
                <ul>
                    <li><b>Template Name</b>: The unique name for the endpoint template.</li>
                    <li><b>Name</b>: A name for the inline endpoint.</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><b>WSDL Endpoint Template</b></td>
            <td>
                The following information needs to be specified:
                <ul>
                    <li><b>Template Name</b>: The unique name for the template.</li>
                    <li><b>Name</b>: A name for the inline endpoint.</li>
                    <li><b>Specify As</b>: The method to specify the WSDL. The available values are as follows:
                        <ul>
                            <li><b>In-lined WSDL</b>: Paste the WSDL in the text box that appears when this option is selected.
                            </li>
                            <li><b>URI</b>: Activates the WSDL URI field.</li>
                        </ul>
                    </li>
                    <li><b>WSDL URI</b>: The URI of the WSDL.</li>
                    <li><b>Service</b>: The service selected from the available services for the WSDL.</li>
                    <li><b>Port</b>: The port selected for the service specified in the above
                        field. In a WSDL an endpoint is bound to each port inside each service.
                    </li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><b>HTTP Endpoint Template</b></td>
            <td>
                You can define a URI template based REST service endpoint. The Endpoint Template has the following options:
                <ul>
                    <li><b>Template Name</b>: The unique name for the endpoint template.</li>
                    <li><b>Name</b>: A name for the inline endpoint.</li>
                    <li><b>URI Template</b>: The URI template of the endpoint. Insert <code>uri.var.</code> before each variable.</li>
                </ul>
            </td>
        </tr>
    </table>

### Endpoint Properties (Optional)

To configure the other **Endpoint** properties of the inline Endpoint, see [Endpoint Properties](endpoint-properties.md).