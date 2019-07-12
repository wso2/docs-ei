# Template Endpoint

Template endpoints are created based on predefined [endpoint
templates](https://docs.wso2.com/display/EI650/Endpoint+Template) .
Parameters of the endpoint template used as the target are copied to the
endpoint configuration. However, you can make modifications by removing
some of the copied parameters and/or adding new parameters.

### Parameters

Enter the required values for the following parameters.

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
<td>This is a <a href="_Default_Endpoint_">Default Endpoint</a> specific option. It defines the unique name of an endpoint.</td>
</tr>
<tr class="even">
<td><strong>Address</strong></td>
<td>The address of the endpoint (e.g., <a href="http://wso2.com">http://wso2.com</a> ).</td>
</tr>
<tr class="odd">
<td><strong>Target Template</strong></td>
<td><div class="content-wrapper">
<p>The endpoint template which should be used to define the endpoint. This is specified by selecting one of the existing templates from the <strong>Available Templates</strong> list. The parameters to be included in the endpoint you are defining will be copied from the target template. The values for these parameters can be entered in the <strong>Template Endpoint Parameters</strong> section shown below. Click <strong>Delete Parameter</strong> to remove a parameter fetched from the target template. Click <strong>Add Parameter</strong> to add additional parameters.</p>
</div></td>
</tr>
<tr class="even">
<td><strong>Parameters</strong></td>
<td>The parameters of the endpoint. For example, you can add <code>             name            </code> to add the parameter name, and <code>             uri            </code> to add the address etc.</td>
</tr>
</tbody>
</table>

You can save the endpoint configuration int he Synapse configuration or
the
[Registry](https://docs.wso2.com/display/ADMIN44x/Working+with+the+Registry)
.
