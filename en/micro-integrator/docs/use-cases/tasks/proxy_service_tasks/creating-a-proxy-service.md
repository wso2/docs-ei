# Creating a Proxy Service

You can create a new proxy service or import an existing proxy service
from an XML file, such as a Synapse configuration file using WSO2
Integration Studio.

-   [Prerequisites](#CreatingaProxyService-Prerequisites)
-   [Step 1: Creating a new proxy
    service](#CreatingaProxyService-createStep1:Creatinganewproxyservice)
-   [Step 2: Update the mediation flow in the proxy
    service](#CreatingaProxyService-Step2:Updatethemediationflowintheproxyservice)
-   [Step 3: Deploying the proxy service in the ESB
    server](#CreatingaProxyService-Step3:DeployingtheproxyserviceintheESBserver)

### Prerequisites

You need to have WSO2 Integration Studio installed to create a new proxy
service or to import an existing proxy service. For instructions on
installing WSO2 Integration Studio, see [Installing WSO2 Integration
Studio](https://docs.wso2.com/display/EI611/Installing+Enterprise+Integrator+Tooling)
.

### Step 1: Creating a new proxy service

Following sections describe how you can create a new proxy service.

1.  Open **WSO2 Integration Studio** and click **ESB Project → Create
    New ** in the **Getting Started** tab as shown below.

    ![](attachments/119130913/119130914.png){width="700" height="336"}  

2.  Enter a project name and click **Finish** .

3.  Right-click the SampleServices project in the navigator and go to
    **New → Proxy Service** to open the **New Proxy Service** dialog.

        !!! tip
    
        Importing a proxy service?
    
        If you already have a proxy service created, you have the option of
        importing the XML configuration. Select **Import Proxy Service** and
        follow the instructions on the UI. To create a new proxy service
        from scratch, continue with the following steps.
    

4.  Select **Create New Proxy Service** and click **Next** .

5.  Type a unique name for the proxy service, and select a proxy service
    template from the list shown below. These templates will
    automatically generate the mediation flow that is required for each
    use case.

    The use cases covered by each template are explained below.

    <table>
    <colgroup>
    <col style="width: 12%" />
    <col style="width: 87%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Template Type</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Pass-Through proxy</td>
    <td>This template creates a proxy service that forwards messages to the endpoint without performing any processing.</td>
    </tr>
    <tr class="even">
    <td>Transformer proxy</td>
    <td>This template creates a proxy service that transforms all the incoming requests using XSLT and then forwards them to a given endpoint. It can also transform responses from the backend service. You need to simply specify the location of the XSLT you want to use.</td>
    </tr>
    <tr class="odd">
    <td>Log Forward proxy</td>
    <td>This template creates a proxy service that first logs all the incoming requests and passes them to a given endpoint. It can also log responses from the backend service before routing them to the client. You can specify the log level for requests and responses, where <strong>Simple</strong> logs <code>               To              </code> , <code>               From              </code> , <code>               WSAction              </code> , <code>               SOAPAction              </code> , <code>               ReplyTo              </code> , <code>               MessageID              </code> , and any properties, and <strong>Full</strong> logs all attributes of the message plus the SOAP envelope information.</td>
    </tr>
    <tr class="even">
    <td>WSDL-Based proxy</td>
    <td>This template generates a proxy service from the remotely hosted WSDL of an existing web service. The endpoint information is extracted from the WSDL. In the <strong>URI</strong> field, enter the URL and URN of the WSDL. The URL defines the host address of the network resource (can be omitted if resources are not network homed), and the URN defines the resource name in local namespaces. For example, if the URL is <code>                                                ftp://ftp.dlink.ru                                             </code> and the URN is <code>               /pub/ADSL/              </code> , you would enter <code>                                                ftp://ftp.dlink.ru/pub/ADSL/                                             </code> for the URI. To ensure that the URI is valid, click <strong>Test URI</strong> . You then enter the service name and port of the WSDL. Lastly, if you want to publish this WSDL, click <strong>Publish Same Service Contract</strong> .</td>
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

6.  In the **Save in** field, enter the project folder where the proxy
    service should be saved.

        !!! tip
    
        If you don't have an already created project, click **Create New ESB
        Project** to create one.
    

7.  Click **Finish** . The proxy service is created in the
    src/main/synapse-config/proxy-services folder under the project you
    specified, and the proxy service appears in the editor. Click its
    icon in the editor to view its properties.

!!! info

You also have the option of using the management console to create proxy
services: Click the **Main** tab on the management console, go to
**Manage** -\> **Services** -\> **Add,** and then click **Proxy
Service** . The **Create Proxy Service from Template** screen appears.


### Step 2: Update the mediation flow in the proxy service

Once you have created a proxy service file as explained above, you can
update the mediation flow by adding new mediation artifacts and changing
the existing artifacts (which were generated for the template you
selected).

1.  First, open the proxy service configuration file from the project
    explorer.
2.  You can use either the **design view** or the **source view** to
    update the mediation flow. Shown below is an example of a
    PassThrough proxy service:

    -   [**Design View**](#64f180d33eec49b49ba90427910ca586)
    -   [**Source View**](#e73439b24065458fa46821b09b7ff941)

    You can select any of the mediation artifacts from the design view
    shown below and update its parameters from the **Properties** tab in
    the bottom pane. You can also drag and drop new mediation artifacts
    to the design view from the artifact **Palette** to modify the
    mediation flow.

    ![](attachments/119130913/119130917.png){width="600" height="654"}

    If you have a sample proxy service configuration, you can simply
    copy it to the source view shown below.

    ![](attachments/119130913/119130916.png){width="600" height="404"}

### Step 3: Deploying the proxy service in the ESB server

Once you have added the security policy to your proxy service as
explained in the previous topics, you need to create a **Composite
Application** project with a CAR file. You can then deploy the CAR file
in the ESB server:

1.  Right-click the **Project Explorer** and click **New \> Project** .
2.  From the window that opens, click **Composite Application Project**
    .
3.  Give a name to the **Composite Application** project and select the
    projects that you need to group into your C-App from the list of
    available projects. You need to select the **ESB** project, which
    contains the proxy service and security policy file respectively.

4.  Next, [deploy the CAR file in the ESB
    server](https://docs.wso2.com/display/ADMIN44x/Deploying+Composite+Applications+in+the+Server)
    .
