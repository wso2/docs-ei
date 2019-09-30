# REST API Configurations

See the topics given below for the list of properties that can be configured when [creating a REST API artifact](../../develop/creating-artifacts/creating-an-api.md).

## REST API Properties (Required)

The following properties are required in order to [create the REST API artifact](../../develop/creating-artifacts/creating-an-api.md).

<table>
    <tr>
        <th>Property</th>
        <th>Description</th>
    </tr>
    <tr>
         <td>Name</td>
         <td>A unique name for the new API.</td>
      </tr>
      <tr>
         <td>Context URL</td>
         <td>
            An API in WSO2 Micro Integrator is analogous to a web application deployed in the Micro Integrator. Each API is anchored at a user-defined URL context, much like how a web application deployed in a servlet container is anchored at a fixed URL context. An API only processes requests that fall under its URL context. For example, if a particular API is anchored at the context <code> /test</code>, the API only handles HTTP requests with a URL path that starts with <code>/test</code>.
         </td>
      </tr>
</table>

## REST API Properties (Optional)

The following properties are optional properties you can configure when [creating a REST API artifact](../../develop/creating-artifacts/creating-an-api.md).

<table>
    <tr>
        <th>Property</th>
        <th>Description</th>
    </tr>
      <tr>
         <td>Hostname</td>
         <td>The Host at which the API is anchored. If you do not enter a value, <code>localhost</code> is considered the default hostname. If required, you can bind a given API to a user-defined hostname.</td>
      </tr>
      <tr>
         <td>Port</td>
         <td>The Port of the REST API. If you do not enter a value, <code>8280</code> is considered the default hostname. If required, you can bind a given API to a user-defined port number.</td>
      </tr>
      <tr>
         <td>Version</td>
         <td>The Micro Integrator identifies each API by its unique context name. If you introduce a version in the API context (e.g., /Service 1.0.0), you can update it when you upgrade the same API (e.g., /Service 1.0.1). Version your APIs as early as possible in the development cycle.</td>
      </tr>
</table>

## REST API Resource Properties

When you [creating a REST API artifact](../../develop/creating-artifacts/creating-an-api.md), you need to configure the API resource. Listed below are the properties you can configure when [defining an API resource](../../../develop/creating-artifacts/creating-an-api/#step-3-update-api-resource-properties).

<table>
    <tr>
        <th>Property</th>
        <th>Description</th>
    </tr>
    <tr>
        <td id="uri-style">URI Style</td>
        <td>
            This allows us to restrict the type of HTTP requests processed by a particular resource. A resource can be associated with a user-defined <a href="#url-mapping">URL mapping</a> or a <a href="#uri-template">URI template</a>.
        </td>
    </tr>
    <tr>
        <td id="url-mapping">URI Template</td>
        <td>
            This property is enabled only if <b>URI Template</b> is specified as the <a href="#uri-style">URI Style</a>.</br>
            A URI template represents a class of URIs using patterns and variables. When a resource is associated with a URI template, all requests that match the template will be processed by the resource. Some examples of valid URI templates are as follows:</br>
            <code>/order/{orderId}/dictionary/{char}/{word}</code></br></br>
            The identifiers within curly braces are considered variables. For example, a URL that matches the template “/order/{orderId}” is as follows:</br>
            <code>/order/A0001</code></br></br>
            In the above URL instance, the variable "orderId" has been assigned the value “A0001”. Similarly, the following URL adheres to the template “/dictionary/{char}/{word}”:</br>
            <code>/dictionary/c/cat</code></br></br>
            In this case, the variable “char” has the value “c” and the variable “word” is given the value “cat”.
            It is possible to retrieve the exact values of the two variables using the "get-property" XPath extension of the Micro Integrator and prefixing the variable with "uri.var".
        </td>
    </tr>
    <tr>
        <td id="uri-template">URL Mapping</td>
        <td>
            This property is enabled only if <b>URL Mapping</b> is specified as the <a href="#uri-style">URI Style</a>.</br>
            When a resource is defined with a URL mapping, only those requests that match the given pattern will be processed by the resource. There are three types of URL mappings:</br>
            <ul>
                <li>Path mappings (<code>/test/</code>, <code>/foo/bar/</code>)</li>
                <li>Extension mappings (<code>.jsp </code>, <code>.do</code>)</li>
                <li>Exact mappings (<code>/test</code>, <code>/test/foo</code>)</li>
            </ul>
            For example, if the URL mapping is “/foo/” and the HTTP method is “GET”, the resource will only process GET requests with a URL path that matches the pattern “/foo/”. Therefore, the following example requests will be processed by the resource:</br>
            <code>GET /test/foo/bar</code>
            <code>GET /test/foo/a?arg1=hello</code></br>
            The following example requests would not be handled by this resource:</br>
            <code>GET /test/food/bar</code> (URL pattern does not match)
            <code>POST /test/foo/bar</code> (HTTP verb does not match)
        </td>
    </tr>
    <tr>
        <td>Protocol</td>
        <td>
            Specify the HTTP methods that the resource should handle. This provides additional control over what requests are handled by a given resource.
        </td>
    </tr>
</table>