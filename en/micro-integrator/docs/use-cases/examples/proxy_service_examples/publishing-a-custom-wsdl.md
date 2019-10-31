# Publishing a Custom WSDL
When you create a proxy service, a default WSDL is automatically
generated. You can access this WSDL by suffixing the service URL
with ?wsdl. See the example given below, where the proxy service name is
'sample_service' and IP is localhost:

[http://localhost:8290/services/sample_service?wsdl](http://localhost:8290/services/Logging?wsdl)

However, this default WSDL only shows the `mediate`
operation. This can be a limitation because your proxy service may be
exposing a back-end service that expects additional information such as
the message format. Therefore, the proxy service should be able to
publish a custom WSDL based on the back-end service's WSDL or a modified
version of that WSDL. For example, if the back-end service expects a
message that includes the name, department, and permission level, and
you want the proxy service to inject the permission level as it
processes the message, you could publish a WSDL that includes just the
name and department without the permission level parameter.

## Synapse configuration

Follow the steps given below to add a custom WSDL to your proxy service.

1.  Open the proxy service from your tooling project.
2.  In the **Properties** tab, the WSDL type is set to **NONE** by
    default.  
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

    In the following example, the WSDL imports a metadata schema from
    the metadata.xsd file. Therefore, the location is metadata.xsd.

    ```xml
    <xsd:import namespace=http://www.wso2.org/test/10 schemaLocation="metadata.xsd" />
    ```

    In the following example, the WSDL is retrieved from the registry
    using the key `           my.wsdl          ` . This WSDL imports
    another WSDL from
    `                                    http://www.standards.org/standard.wsdl                                 `
    . This dependent WSDL is retrieved from the registry using the
    `           standard.wsdl          ` registry key.

    ```xml 
    <publishWSDL key="my.wsdl">
        <resource location="http://www.standards.org/standard.wsdl" key="standard.wsdl"/>
    </publishWSDL>
    ```

5.  Go to the **Service Parameters** section in the **Properties** tab. Following are additional service parameters you can use to configure the service WSDL.

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
    
## Synapse configuration

```xml
<proxy name="StockQuoteProxy" startOnLoad="true" transports="http https" xmlns="http://ws.apache.org/ns/synapse">
    <target>
        <endpoint>
            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
        </endpoint>
        <outSequence>
            <send/>
        </outSequence>
    </target>
    <publishWSDL uri="file:/path/to/sample_proxy_1.wsdl"/>
</proxy>
```

## Build and run

The wsdl file `sample_proxy_1.wsdl` can be downloaded from  [sample_proxy_1.wsdl](https://github.com/wso2-docs/WSO2_EI/blob/master/samples-protocol-switching/sample_proxy_1.wsdl). 
The wsdl uri needs to be updated with the path to the sample_proxy_1.wsdl file

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. [Create the proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Set up the back-end service.
* Download the [stockquote_service.jar](
https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/stockquote_service.jar)

* Run the mock service using the following command
```
$ java -jar stockquote_service.jar
```

Invoke the proxy service.
```xml
POST http://localhost:8290/services/StockQuoteProxy.StockQuoteProxyHttpSoap11Endpoint HTTP/1.1
Accept-Encoding: gzip,deflate
Content-Type: text/xml;charset=UTF-8
SOAPAction: "urn:getQuote"
Content-Length: 492
Host: localhost:8290
Connection: Keep-Alive
User-Agent: Apache-HttpClient/4.1.1 (java 1.5)

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
   <soapenv:Header/>
   <soapenv:Body>
      <ser:getQuote xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
         <!--Optional:-->
         <ser:request>
            <!--Optional:-->
            <xsd:symbol>IBM</xsd:symbol>
         </ser:request>
      </ser:getQuote>
   </soapenv:Body>
</soapenv:Envelope>
```

Sample response would be:
```xml
HTTP/1.1 200 OK
server: ballerina
content-encoding: gzip
content-type: application/xml
Transfer-Encoding: chunked
Connection: Keep-Alive

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns="http://services.samples" xmlns:ax21="http://services.samples/xsd">
   <soapenv:Body>
      <ns:getQuoteResponse>
         <ax21:change>-2.86843917118114</ax21:change>
         <ax21:earnings>-8.540305401672558</ax21:earnings>
         <ax21:high>-176.67958828498735</ax21:high>
         <ax21:last>177.66987465262923</ax21:last>
         <ax21:low>-176.30898912339075</ax21:low>
         <ax21:marketCap>5.649557998178506E7</ax21:marketCap>
         <ax21:name>IBM Company</ax21:name>
         <ax21:open>185.62740369461244</ax21:open>
         <ax21:peRatio>24.341353665128693</ax21:peRatio>
         <ax21:percentageChange>-1.4930577008849097</ax21:percentageChange>
         <ax21:prevClose>192.11844053187397</ax21:prevClose>
         <ax21:symbol>IBM</ax21:symbol>
         <ax21:volume>7791</ax21:volume>
      </ns:getQuoteResponse>
   </soapenv:Body>
</soapenv:Envelope>
```