# Creating a Proxy Service

Follow the instructions given below to create a new proxy service artifact in WSO2 Integration Studio.

## Instructions

1.  If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right-click the SampleServices project in the navigator and go to **New → Proxy Service** to open the **New Proxy Service** dialog. 
2.  Select **Create New Proxy Service** and click **Next**.
3.  Type a unique name for the proxy service, and select a proxy service template from the list shown below. These templates will automatically generate the mediation flow that is required for each use case.

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
    
6.  Click **Finish** . The proxy service is created in the
    src/main/synapse-config/proxy-services folder under the project you
    specified, and the proxy service appears in the editor. 

## Properties
...

## Examples
..

## Guides
...
