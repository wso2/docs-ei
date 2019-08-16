# Template Properties

## Properties: Sequence Templates

The parameters available to configure the Sequence Template are as follows.

| Parameter Name      | Description                                                       |
|---------------------|-------------------------------------------------------------------|
| Name                | The name of the Sequence Template                                 |
| onError             | Select the error sequence that needs to be invoked.               |
| Trace Enabled       | Whether or not trace is to be enabled for the sequence.           |
| Statistics Enabled  | Whether or not statistics is to be enabled for the sequence.      |
| Template Parameters | The input parameter that are supported by this Sequence Template. |


## Properties: Endpoint Templates

The parameters available to configure the Endpoint template are as follows.

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