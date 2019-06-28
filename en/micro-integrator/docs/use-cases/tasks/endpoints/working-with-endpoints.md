# Working with Endpoints via Tooling

You can create a new endpoint or import an existing endpoint from an XML
file, such as a Synapse configuration file using WSO2 Integration
Studio.

!!! tip

You need to have WSO2 Integration Studio installed to create a new
endpoint or to import an existing endpoint. For instructions, see
[Installing WSO2 Integration
Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio)
.


When you create an endpoint, you need to give it a name, at which point
it appears in the **Defined Endpoint** section of the tool palette in
WSO2 Integration Studio. To use a defined endpoint, simply drag and drop
the endpoint from the palette to the sequence, proxy service, or other
artifact where you want to reference it.

#### Creating a new endpoint

Follow these steps to create a new endpoint. Alternatively, you can
[import an existing endpoint](#WorkingwithEndpointsviaTooling-import) .

1.  Open **WSO2 Integration Studio,** and click **Miscellaneous → Create
    New Config Project ** in the **Getting Started** tab.  
    ![](attachments/119131929/119133621.png){width="800" height="397"}

2.  Enter a project name and click **Finish** .

3.  The new project will be listed in the project explorer.
4.  Right click the project and go to **New → Endpoint** to open the
    **New Endpoint Artifact** dialog.
5.  Select **Create a New Endpoint** and click **Next** .
6.  Type a unique name for the endpoint, and then specify the [type of
    endpoint](_ESB_Endpoints_) you are creating.
    -   **[Address Endpoint](_Address_Endpoint_)** - Defines the direct
        URL of the service.
    -   **[Default Endpoint](_Default_Endpoint_)** - Defines additional
        configuration for the default target.
    -   **[Configuring Failover
        Endpoints](_Configuring_Failover_Endpoints_)** - Defines the
        endpoints that the service will try to connect to in case of a
        failure. This will take place in a round robin manner.
    -   **[HTTP Endpoint](_HTTP_Endpoint_)** - Defines a URI template
        based REST service endpoint.
    -   **[Load Balance Endpoint](_Load-balance_Group_)** - Defines
        groups of endpoints for replicated services. The incoming
        requests will be directed to these endpoints in a round robin
        manner. These endpoints automatically handle the fail-over cases
        as well.
    -   **[Recipient List Group](_Recipient_List_Endpoint_) -** Defines
        the list of endpoints a message will be routed to.
    -   **[Template Endpoint](_Template_Endpoint_)** - Allows to create
        a custom Endpoint template.
    -   **[WSDL Endpoint](_WSDL_Endpoint_)** - Defines the WSDL, service
        and port.
7.  Do one of the following:  
    -   To save the endpoint in an existing ESB Config project in your
        workspace, click **Browse** and select that project.
    -   To save the endpoint in a new ESB Config project, click **Create
        new Project** and create the new project.
    -   To save the endpoint in a Registry Resource Project, click
        **Make this a Dynamic Endpoint** , click the **Browse** button
        next to **Save endpoint in** and select the registry, and then
        select the registry (Configuration or Governance) where you want
        to save the endpoint. Lastly, type the endpoint name in the
        **Registry Path** box.

                !!! tip
        
                For instructions on creating a Registry Resource Project, see
                [Working with Registry
                Artifacts](https://docs.wso2.com/display/EI620/Working+with+Registry+Artifacts)
                .
        

8.  Click **Add Property** to open the **Properties** tab and add the
    required properties.

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
    <p>For more information about these scopes, see <a href="https://docs.wso2.com/display/EI6xx/Accessing+Properties+with+XPath#AccessingPropertieswithXPath-XPathExtensionFunctions">XPath Extension Functions</a> .</p></td>
    </tr>
    <tr class="even">
    <td><strong>Action</strong></td>
    <td>This parameter allows you to delete a property.</td>
    </tr>
    </tbody>
    </table>

9.  Open the Endpoint artifact from the project explorer, go to the
    **Form** view, and update the additional configurations (which
    changes based on the type of endpoint you are creating).

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

10. Click **Finish** . The endpoint is created in the
    `          endpoints         ` folder under the ESB Config or
    Registry Resource project you specified, and the endpoint opens in
    the editor.

#### Importing an endpoint

Follow these steps to import an existing endpoint from an XML file such
as a Synapse configuration file, into an ESB Config project.
Alternatively, you can [create a new
endpoint](#WorkingwithEndpointsviaTooling-create) .

1.  Open **WSO2 Integration Studio,** and click **Miscellaneous → Create
    New Config Project ** in the **Getting Started** tab.  
    ![](attachments/119131929/119133621.png){width="800" height="397"}

2.  Enter a project name and click **Finish** .

3.  The new project will be listed in the project explorer.
4.  Right click the project and go to **New → Endpoint** to open the
    **New Endpoint Artifact** dialog.
5.  Select **Import Endpoint** and click **Next** .
6.  Specify the endpoint file by typing its full path name or clicking
    **Browse** and navigating to the file.
7.  In the **Save Endpoint In** field, specify an existing ESB Config
    project in your workspace where you want to save the endpoint, or
    click **Create new Project** to create a new ESB Config project and
    save the endpoint there.
8.  In the **Advanced Configuration** section, select the endpoints you
    want to import.
9.  Click **Finish** . The endpoints you selected are created in the
    `          endpoints         ` folder under the ESB Config project
    you specified, and the first endpoint opens in the editor.

!!! info

You also have the option of using the management console to create
endpoints: Click the **Main** tab on the management console, go to
**Service Bus** -\> **Endpoints** **,** and then click the **Add
Endpoint** tab. Select the type of endpoint you want to create and
follow the instructions on the screen.

