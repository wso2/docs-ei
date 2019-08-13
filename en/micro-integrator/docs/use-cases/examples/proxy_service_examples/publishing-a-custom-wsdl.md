# Publishing a Custom WSDL

When you create a proxy service, a default WSDL is automatically
generated. You can access this WSDL by suffixing the service URL
with ?wsdl. See the example given below, where the proxy service name is
'sample\_service' and IP is localhost:

[http://localhost:8280/services/sample\_service?wsdl](http://localhost:8280/services/Logging?wsdl)

However, this default WSDL only shows the `         mediate        `
operation. This can be a limitation because your proxy service may be
exposing a back-end service that expects additional information such as
the message format. Therefore, the proxy service should be able to
publish a custom WSDL based on the back-end service's WSDL or a modified
version of that WSDL. For example, if the back-end service expects a
message that includes the name, department, and permission level, and
you want the proxy service to inject the permission level as it
processes the message, you could publish a WSDL that includes just the
name and department without the permission level parameter.

See the following topics try a custom WSDL for a proxy service:

-   [Prerequisites](#PublishingaCustomWSDL-Prerequisites)
-   [Adding a custom WSDL to the proxy
    service](#PublishingaCustomWSDL-AddingacustomWSDLtotheproxyservice)
-   [Testing the custom
    WSDL](#PublishingaCustomWSDL-TestingthecustomWSDL)

### Prerequisites

Let's set up a sample proxy service in WSO2 Integration Studio.

-   Install WSO2 Integration Studio. For instructions, see [Installing
    WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio)
    .
-   Click [this
    link](https://docs.wso2.com/download/attachments/85371060/StockQuoteProxy.xml?version=2&modificationDate=1526364863000&api=v2)
    to download the sample proxy service ( **StockQuoteProxy.xml** ).

### Adding a custom WSDL to the proxy service

Follow the steps given below to add a custom WSDL to your proxy service.

1.  Open the proxy service from your tooling project.
2.  In the **Properties** tab, the WSDL type is set to **NONE** by
    default.  
    ![](attachments/119130987/119130990.png){width="750"}
3.  To publish a custom WSDL for this proxy service, select one of the
    WSDL types.

    <table>
    <thead>
    <tr class="header">
    <th>WSDL Type</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>INLINE</td>
    <td>Enter the WSDL definition in the <strong>WSDL XML</strong> field.</td>
    </tr>
    <tr class="even">
    <td>SOURCE_URL</td>
    <td><p>Enter the URI of the WSDL in the text box, and then click <strong>Test URI</strong> to ensure it is available. A URI consists of a URL and URN. The URL defines the host address of the network resource (can be omitted if resources are not network homed), and the URN defines the resource name in local "namespaces."</p>
    <p>For example URI = <code>                                 ftp://ftp.dlink.ru/pub/ADSL                                , w               </code> here URL = <code>                                 ftp://ftp.dlink.ru                               </code> and URN = <code>                pub/ADSL               </code></p></td>
    </tr>
    <tr class="odd">
    <td>REGISTRY_KEY</td>
    <td><div class="content-wrapper">
    <p>If the WSDL is saved as a registry entry, select this option and choose the reference key of that registry entry from the governance Registry or configuration Registry.</p>
    </div></td>
    </tr>
    <tr class="even">
    <td>ENDPOINT</td>
    <td>-</td>
    </tr>
    </tbody>
    </table>

4.  If your WSDL has dependencies with other resources (schemas or other
    WSDL documents), you can link them using the **Wsdl Resources**
    property shown below. Click the **browse** icon and enter the
    registry key and the location of the dependent resource: The
    location is available in the WSDL. When you have the location, you
    can find registry key corresponding to the location from the
    registry.

    ![](attachments/119130987/119130989.png){width="800" height="510"}

    In the following example, the WSDL imports a metadata schema from
    the metadata.xsd file. Therefore, the location is metadata.xsd.

    ``` java
        <xsd:import namespace=http://www.wso2.org/test/10 schemaLocation="metadata.xsd" />
    ```

    In the following example, the WSDL is retrieved from the registry
    using the key `           my.wsdl          ` . This WSDL imports
    another WSDL from
    `                                    http://www.standards.org/standard.wsdl                                 `
    . This dependent WSDL is retrieved from the registry using the
    `           standard.wsdl          ` registry key.

    ``` java
            <publishWSDL key="my.wsdl">
               <resource location="http://www.standards.org/standard.wsdl" key="standard.wsdl"/>
             </publishWSDL>
    ```

5.  Go to the **Service Parameters** section in the **Properties**
    tab.  
    ![](attachments/119130987/119130988.png?effects=drop-shadow){width="700"}

    Following are additional service parameters you can use to configure
    the service WSDL.

    <table>
    <thead>
    <tr class="header">
    <th><p>Parameter</p></th>
    <th><p>Description</p></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>useOriginalwsdl</p></td>
    <td><div class="content-wrapper">
    <p>If this parameter is set to <code>                 true                </code> , the original WSDL published via the <code>                 publishWSDL                </code> parameter is used instead of the custom WSDL.</p>
    </div></td>
    </tr>
    <tr class="even">
    <td><p>modifyUserWSDLPortAddress</p></td>
    <td><p>If true (default), the port addresses will be modified to the current host. Effective only with <code>                useOriginalwsdl=true               </code> .</p></td>
    </tr>
    <tr class="odd">
    <td>ApplicationXMLBuilder.allowDTD</td>
    <td>If this parameter is set to true, it enables data type definition processing for the proxy service.<br />
    Data type definition processing is disabled in the Axis2 engine due to security vulnerability. This parameter enables it for individual proxy services.</td>
    </tr>
    <tr class="even">
    <td><p>enablePublishWSDLSafeMode</p></td>
    <td><div class="content-wrapper">
    <p>If this parameter is set to <code>                 true                </code> when deploying a proxy service, even though the WSDL is not available, you can prevent the proxy service from being faulty. However, the deployed proxy service will be inaccessible since the WSDL is not available.</p>
        !!! note
        <p>This is only applicable when you publish the WSDL (i.e., via the <code>                 publishWSDL                </code> property) either as a URI or as an endpoint.</p>

    </div></td>
    </tr>
    <tr class="odd">
    <td>showAbsoluteSchemaURL</td>
    <td>If this parameter is set to <code>               true              </code> , the absolute path of the referred schemas of the WSDL is shown instead of the relative paths.</td>
    </tr>
    <tr class="even">
    <td>showProxySchemaURL</td>
    <td>If this parameter is set to <code>               true              </code> , the full proxy URL will be set as the prefix to the schema location of the imports in proxy WSDL.</td>
    </tr>
    </tbody>
    </table>

### Testing the custom WSDL

To test the custom WSDL:

1.  Deploy the proxy service in the ESB profile.
2.  Copy the URL shown below to your browser:

    [http://localhost:8280/services/](http://localhost:8280/services/Logging?wsdl)
    [StockQuoteProxy?wsdl](http://wso2s-macbook-air-59.local:8280/services/StockQuoteProxy?wsdl)
