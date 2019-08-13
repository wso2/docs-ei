# Creating an API

Follow the instructions given below to create a new REST API artifact in WSO2 Integration Studio.

## Instructions 

### Step 1: Create new API

1.  If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right-click the project in the navigator and go to **New → REST API** to open the **API Artifact Creation Options** dialog box.
2.  Leave the first option selected and click **Next**.
3.  Specify the API name, context, hostname, and port:

    <table>
    <tbody>
    <tr class="odd">
    <td>Name</td>
    <td>Enter a unique name for the new API.</td>
    </tr>
    <tr class="even">
    <td>Context URL</td>
    <td><div class="content-wrapper">
    <p>An API in WSO2 Micro Integrator is analogous to a web application deployed in the Micro Integrator. Each API is anchored at a user-defined URL context, much like how a web application deployed in a servlet container is anchored at a fixed URL context. An API only processes requests that fall under its URL context. For example, if a particular API is anchored at the context <code>                 /test                </code> , the API only handles HTTP requests with a URL path that starts with <code>/test</code> .</p>
    </div></td>
    </tr>
    <tr class="odd">
    <td>Hostname</td>
    <td>The Host at which the API is anchored. If you do not enter a value, <code>localhost</code> is considered the default hostname. If required, you can bind a given API to a user-defined hostname.</td>
    </tr>
    <tr class="even">
    <td>Port</td>
    <td>The Port of the REST API. If you do notenter a value, <code>8280</code> is considered the default hostname. If required, you can bind a given API to a user-defined port number.</td>
    </tr>
    <tr class="odd">
    <td>Version</td>
    <td>The Micro Integrator identifies each API by its unique context name. If you introduce a version in the API context (e.g., /Service 1.0.0), you can update it when you upgrade the same API (e.g., /Service 1.0.1). Version your APIs as early as possible in the development cycle.</td>
    </tr>
    </tbody>
    </table>

4.  Click **Finish** . The REST API is created inside the
    `          src/main/synapse-config/api         ` folder under the
    ESB Config project you specified.

### Step 2: Add new API Resources (Optional)

When you created the API, an API resource is created by default. If you want to add a new resource, click **API Resource** in the Tool pallet of the API section and simply drag and drop the resource to the REST API.

!!! Info
    **About the default API Resource**
    
    Each API can have at most one default resource. Any request received
        by the API but does not match any of the enclosed resource
        definitions will be dispatched to the default resource of the API.
        In case of API\_3, a DELETE request on the URL “/payments” will be
        dispatched to the default resource as none of the other resources in
        API\_3 are configured to handle DELETE requests. If you go to the
        Source view, the default resource will be as follows: 
    ``` java
    <api context="/healthcare" name="HealthcareAPI" xmlns="http://ws.apache.org/ns/synapse">
        <resource methods="GET">
            <inSequence/>
            <outSequence/>
            <faultSequence/>
        </resource>
    </api>
    ```    

## Properties

In the **Design** view of the API Resource, click the **Resource** icon to enable the **Properties** tab. You can now enter the configuration parameters as given below.

1.  Specify the HTTP methods that the resource should handle. This provides additional control over what requests are handled by a given resource.

2.  Select one of the values given below for the **URL style** property. A resource can be associated with a user-defined **URL mapping** or **URI template**. This allows us to restrict the type of HTTP requests processed by a particular resource.

    <table>
    <tbody>
    <tr class="odd">
    <td>URL Template</td>
    <td><div class="content-wrapper">
    <p>A URI template represents a class of URIs using patterns and variables. When a resource is associated with a URI template, all requests that match the template will be processed by the resource. Some examples of valid URI templates are as follows:<br />
    </p>
    <p><code>/order/{orderId}/dictionary/{char}/{word}</code></p>
    <p>The identifiers within curly braces are considered variables. For example, a URL that matches the template “/order/{orderId}” is as follows:</p>
    <p><code>/order/A0001</code></p>
    <p>In the above URL instance, the variable "orderId" has been assigned the value “A0001”. Similarly, the following URL adheres to the template “/dictionary/{char}/{word}”:</p>
    <p><code>/dictionary/c/cat</code><br />
    </p>
    <p>In this case, the variable “char” has the value “c” and the variable “word” is given the value “cat”.<br />
    </p>
    <p>The Micro Integrator provides access to the exact values of the template variables through message context properties. For example, if you have a resource configured with the URI template “/dictionary/{char}/{word}”, and a request “/dictionary/c/cat” is sent to the Micro Integrator, it will be dispatched to this resource, and we will be able to retrieve the exact values of the two variables using the "get-property" XPath extension of the Micro Integrator and prefixing the variable with "uri.var." as shown in the following example:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;log level=<span class="st">&quot;custom&quot;</span>&gt;</span>
    <span id="cb1-2"><a href="#cb1-2"></a>   &lt;property name=<span class="st">&quot;Character&quot;</span> expression=<span class="st">&quot;get-property(&#39;uri.var.char&#39;)&quot;</span>/&gt;</span>
    <span id="cb1-3"><a href="#cb1-3"></a>   &lt;property name=<span class="st">&quot;Word&quot;</span> expression=<span class="st">&quot;get-property(&#39;uri.var.word&#39;)&quot;</span>/&gt;</span>
    <span id="cb1-4"><a href="#cb1-4"></a>&lt;/log&gt;</span></code></pre></div>
    </div>
    </div>
    <p>This log mediator configuration would generate the following output for the request “/dictionary/c/cat”:</p>
    <p><code>LogMediator Character = c, Word = cat</code></p>
    </div></td>
    </tr>
    <tr class="even">
    <td>URL Mapping</td>
    <td><p>When a resource is defined with a URL mapping, only those requests that match the given pattern will be processed by the resource. There are three types of URL mappings:</p>
    <ul>
    <li><p>Path mappings ( <code>/test/*</code> , <code>/foo/bar/*</code> )</p></li>
    <li><p>Extension mappings ( <code>                  *.jsp                 </code> , <code>                  *.do                 </code> )</p></li>
    <li><p>Exact mappings ( <code>                  /test                 </code> , <code>                  /test/foo                 </code> )</p></li>
    </ul>
    <div>
    <p>For example, if the URL mapping is “/foo/*” and the HTTP method is “GET”, the resource will only process GET requests with a URL path that matches the pattern “/foo/*”. Therefore, the following example requests will be processed by the resource:</p>
    <p><code>                 GET /test/foo/bar                </code><br />
    <code>                 GET /test/foo/a?arg1=hello                </code><br />
    </p>
    <p>The following example requests would <strong>not</strong> be handled by this resource:</p>
    <p><code>                 GET /test/food/bar                </code> (URL pattern does not match)<br />
    <code>                 POST /test/foo/bar                </code> (HTTP verb does not match)</p>
    </div></td>
    </tr>
    </tbody>
    </table>

3.  Select the sequences: The resource also contains **sequences**
    (in-sequence/out-sequence/fault sequence), which contains the
    mediation logic to process the message.

## Examples

## Guides