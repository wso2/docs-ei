# Working with JSON Message Payloads

The ESB Profile of WSO2 Enterprise Integrator (WSO2 EI) provides support
for [JavaScript Object Notation (JSON)](http://www.json.org/) payloads
in messages. The following sections describe how to work with JSON via
the ESB Profile of WSO2 EI:

-   [Handling JSON to XML
    conversion](#WorkingwithJSONMessagePayloads-HandlingJSONtoXMLconversion)
    -   [Empty objects](#WorkingwithJSONMessagePayloads-Emptyobjects)
    -   [Empty strings](#WorkingwithJSONMessagePayloads-Emptystrings)
    -   [Empty array](#WorkingwithJSONMessagePayloads-Emptyarray)
    -   [Named arrays](#WorkingwithJSONMessagePayloads-Namedarrays)
    -   [Anonymous
        arrays](#WorkingwithJSONMessagePayloads-Anonymousarrays)
    -   [XML processing instructions
        (PIs)](#WorkingwithJSONMessagePayloads-XMLprocessinginstructions(PIs))
    -   [Special
        characters](#WorkingwithJSONMessagePayloads-Specialcharacters)
    -   [Converting
        spaces](#WorkingwithJSONMessagePayloads-Convertingspaces)
-   [Handling XML to JSON
    conversion](#WorkingwithJSONMessagePayloads-HandlingXMLtoJSONconversion)
    -   [Empty XML
        elements](#WorkingwithJSONMessagePayloads-EmptyXMLelements)
    -   [Empty XML elements with the 'nil'
        attribute](#WorkingwithJSONMessagePayloads-EmptyXMLelementswiththe'nil'attribute)
-   [Converting a payload between XML and
    JSON](#WorkingwithJSONMessagePayloads-ConvertingapayloadbetweenXMLandJSON)
-   [Accessing content from JSON
    payloads](#WorkingwithJSONMessagePayloads-AccessingcontentfromJSONpayloads)
    -   [JSON path
        syntax](#WorkingwithJSONMessagePayloads-JSONpathsyntax)
-   [Logging JSON
    payloads](#WorkingwithJSONMessagePayloads-LoggingJSONpayloads)
-   [Constructing and transforming JSON
    payloads](#WorkingwithJSONMessagePayloads-ConstructingandtransformingJSONpayloads)
    -   [PayloadFactory
        mediator](#WorkingwithJSONMessagePayloads-PayloadFactorymediator)
        -   [Configuring the payload
            format](#WorkingwithJSONMessagePayloads-Configuringthepayloadformat)
    -   [Script
        mediator](#WorkingwithJSONMessagePayloads-ScriptScriptmediator)
-   [XML to JSON transformation
    parameters](#WorkingwithJSONMessagePayloads-XMLtoJSONtransformationparameters)
-   [Validating JSON
    messages](#WorkingwithJSONMessagePayloads-ValidatingJSONmessages)
    -   [Validate
        mediator](#WorkingwithJSONMessagePayloads-Validatemediator)
-   [Troubleshooting, debugging, and
    logging](#WorkingwithJSONMessagePayloads-Troubleshooting,debugging,andlogging)

### Handling JSON to XML conversion

When building the XML tree, JSON builders attach the converted XML
infoset to a special XML element that acts as the root element of the
final XML tree. If the original JSON payload is of type
`         object        ` , the special element is
`         <jsonObject/>        ` . If it is an `         array        `
, the special element is `         <jsonArray/>        ` . Following are
examples of JSON and XML representations of various objects and arrays.

##### Empty objects

JSON:

``` javascript
    {"object":{}}
```

XML:

``` html/xml
    <jsonObject>
       <object></object>
    </jsonObject>
```

##### Empty strings

JSON:

``` javascript
    {"object":""}
```

XML:

``` html/xml
    <jsonObject>
       <object></object>
    </jsonObject>
```

##### Empty array

JSON:

``` javascript
    []
```

XML (JsonStreamBuilder):

``` html/xml
    <jsonArray></jsonArray>
```

XML (JsonBuilder):

``` html/xml
    <jsonArray>
       <?xml-multiple jsonElement?>
    </jsonArray>
```

##### Named arrays

JSON:

``` javascript
    {"array":[1,2]}
```

XML (JsonStreamBuilder):

``` html/xml
    <jsonObject>
       <array>1</array>
       <array>2</array>
    </jsonObject>
```

XML (JsonBuilder):

``` html/xml
    <jsonObject>
       <?xml-multiple array?>
       <array>1</array>
       <array>2</array>
    </jsonObject>
```

JSON:

``` javascript
    {"array":[]}
```

XML (JsonStreamBuilder):

``` html/xml
    <jsonObject></jsonObject>
```

XML (JsonBuilder):

``` html/xml
    <jsonObject>
       <?xml-multiple array?>
    </jsonObject>
```

##### Anonymous arrays

JSON:

``` javascript
    [1,2]
```

XML (JsonStreamBuilder):

``` html/xml
    <jsonArray>
       <jsonElement>1</jsonElement>
       <jsonElement>2</jsonElement>
    </jsonArray>
```

XML (JsonBuilder):

``` html/xml
    <jsonArray>
       <?xml-multiple jsonElement?>
       <jsonElement>1</jsonElement>
       <jsonElement>2</jsonElement>
    </jsonArray>
```

JSON:

``` javascript
    [1, []]
```

XML (JsonStreamBuilder):

``` html/xml
    <jsonArray>
       <jsonElement>1</jsonElement>
       <jsonElement>
           <jsonArray></jsonArray>
       </jsonElement>
    </jsonArray>
```

XML (JsonBuilder):

``` html/xml
    <jsonArray>
       <?xml-multiple jsonElement?>
       <jsonElement>1</jsonElement>
       <jsonElement>
           <jsonArray>
               <?xml-multiple jsonElement?>
           </jsonArray>
       </jsonElement>
    </jsonArray>
```

##### XML processing instructions (PIs)

Note the addition of `         xml-multiple        ` processing
instructions to the XML payloads whose JSON representations contain
arrays. `         JsonBuilder        ` (via StAXON) adds these
instructions to the XML payload that it builds during the JSON to XML
conversion so that during the XML to JSON conversion,
`         JsonFormatter        ` can reconstruct the arrays that are
present in the original JSON payload. `         JsonFormatter        `
interprets the elements immediately following a processing instruction
to construct an array.

##### Special characters

When building XML elements, the EI handles the ‘$’ character and digits
in a special manner when they appear as the first character of a JSON
key. Following are examples of two such occurrences. Note the addition
of the `         _JsonReader_PS_        ` and
`         _JsonReader_PD_        ` prefixes in place of the ‘$’ and
digit characters, respectively.

JSON:

``` javascript
    {"$key":1234}
```

XML:

``` html/xml
    <jsonObject>
       <_JsonReader_PS_key>1234</_JsonReader_PS_key>
    </jsonObject>
```

JSON:

``` javascript
    {"32X32":"image_32x32.png"}
```

XML:

``` html/xml
    <jsonObject>
       <_JsonReader_PD_32X32>image_32x32.png</_JsonReader_PD_32X32>
    </jsonObject>
```

##### Converting spaces

Although you can have spaces in JSON elements, [you cannot have them
when converted to XML](https://www.w3.org/TR/REC-xml/#sec-common-syn) .
Therefore, you can handle spaces when converting JSON message payloads
to XML, by adding the following property to the
`         <EI_HOME>/conf/synapse.properties        ` file:
`         synapse.commons.json.buildValidNCNames        `

For example, consider the following JSON message:

``` js
    {
      "abc def" : "this is a sample value"
    }
```

The output converted to XML is as follows:

``` xml
    <abc_jsonreader_32_def>this is a sample value</abc_jsonreader_32_def> 
```

!!! tip

The value 32 represents the standard char value of the space.

!!! info

This works other way around as well. When you need to convert XML to
JSON, with a JSON element that needs to have a space within it. Then,
you use " `         _JsonReader_32_        ` " in the XML element, to
get the space in the JSON output. For example, if you consider the
following XML payload;

`         <abc_jsonreader_32_def>this is a sample value</abc_jsonreader_32_def>        `

The JSON output will be as follows:

`         {        `

`        ` `         "abc def" : "this is a sample value"        `

`         }        `


### Handling XML to JSON conversion

When an XML element is converted to JSON, the following rules apply:

##### Empty XML elements

Consider the following empty XML elements:

-   [**Example 1**](#5b5644c9d0124675b64cfc47c0eb707e)
-   [**Example 2**](#827f4bada1954b7e8eb60e972bef6854)

``` html/xml
    <jsonObject>
        <object></object>
    </jsonObject>
```

``` html/xml
    <jsonObject>
        <object/>
    </jsonObject>
```

By default, empty XML elements convert to JSON as null objects as shown
below.

``` java
    {"object":null}
```

If you set the '
`         synapse.commons.enableXmlNullForEmptyElement=false'        `
property in the `         synapse.properties        ` file (stored in
the `         <EI_HOME>/conf/        ` directory), the JSON
representaion of empty XML elements will change as shown below.

``` javascript
    {"object":""}
```

##### Empty XML elements with the 'nil' attribute

Consider the following XML element that has the 'nil' attribute set to
true.

``` html/xml
    <jsonObject>
        <object nil=true></object>
    </jsonObject>
```

By default, the above XML element converts to JSON as shown below.

``` javascript
    {"object":{"@nil":"true"}}
```

If you set the
`         synapse.commons.enableXmlNilReadWrite=true        ` property
in the `         synapse.properties        ` file (stored in the
`         <EI_HOME>/conf/        ` directory), XML elements where the
'nil' attribue is set to true will be represented in JSON as null
objects as shown below.

``` javascript
    {"object":null}
```

### Converting a payload between XML and JSON

To convert an XML payload to JSON, set the
`         messageType        ` property to
`         application/json        ` in the axis2 scope before sending
message to an endpoint. Similarly, to convert a JSON payload to XML, set
the `         messageType        ` property to
`         application/xml        ` or `         text/xml        ` . For
example:

``` html/xml
    <proxy xmlns="http://ws.apache.org/ns/synapse"
          name="tojson"
          transports="https,http"
          statistics="disable"
          trace="disable"
          startOnLoad="true">
      <target>
         <inSequence>
            <property name="messageType" value="application/json" scope="axis2"/>
            <respond/>
         </inSequence>
      </target>
      <description/>
    </proxy>
```

Use the following command to invoke this proxy service:

``` bash
    curl -v -X POST -H "Content-Type:application/xml" -d@request1.xml "http://localhost:8280/services/tojson"
```

If the request payload is as follows:

``` html/xml
    <coordinates>
       <location>
           <name>Bermuda Triangle</name>
           <n>25.0000</n>
           <w>71.0000</w>
       </location>
       <location>
           <name>Eiffel Tower</name>
           <n>48.8582</n>
           <e>2.2945</e>
       </location>
    </coordinates>
```

The response payload will look like this:

``` javascript
    {
      "coordinates":{
         "location":[
            {
               "name":"Bermuda Triangle",
               "n":25.0000,
               "w":71.0000
            },
            {
               "name":"Eiffel Tower",
               "n":48.8582,
               "e":2.2945
            }
         ]
      }
    }
```

Note that we have used the [Property
mediator](https://docs.wso2.com/display/EI650/Property+Mediator) to mark
the outgoing payload to be formatted as JSON:

``` html/xml
    <property name="messageType" value="application/json" scope="axis2"/>
```

!!! note

JSON requests cannot be converted to XML if it contains invalid XML
characters.

!!! info

If you need to convert complex XML responses (e.g., XML with with
`         xsi:type        ` values), you will need to set the message
type using the [Property
mediator](https://docs.wso2.com/display/EI650/Property+Mediator) as
follows:

    <property name="messageType" value="application/json/badgerfish" scope="axis2" type="STRING"/>

You will also need to ensure you register the following message builder
and formatter as specified in [Message Builders and
Formatters](#WorkingwithJSONMessagePayloads-MessageBuildersandFormatters)
.

``` xml
    <messageBuilder contentType="text/javascript" 
                   class="org.apache.axis2.json.JSONBadgerfishOMBuilder"/>
    
    <messageFormatter contentType="text/javascript" 
                    class="org.apache.axis2.json.JSONBadgerfishMessageFormatter"/> 
```
    

### Accessing content from JSON payloads

There are two ways to access the content of a JSON payload within the
EI.

-   JSONPath expressions (with `           json-eval()          `
    method)

-   XPath expressions

JSONPath allows you to access fields of JSON payloads with faster
results and less processing overhead. Although it is possible to
evaluate XPath expressions on JSON payloads by assuming the XML
representation of the JSON payload, we recommend that you use JSONPath
to query JSON payloads. It is also possible to evaluate both JSONPath
and XPath expressions on a payload (XML/JSON) at the same time.

You can use JSON path expressions with following mediators:

<table>
<thead>
<tr class="header">
<th>Mediator</th>
<th>Usage</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="https://docs.wso2.com/display/EI650/Log+Mediator">Log</a></td>
<td><div class="content-wrapper">
<p>As a log property:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="html/xml" data-syntaxhighlighter-params="brush: html/xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: html/xml; gutter: false; theme: Confluence"><code>&lt;log&gt;
    &lt;property name=&quot;location&quot; 
              expression=&quot;json-eval($.coordinates.location[0].name)&quot;/&gt;
&lt;/log&gt;</code></pre>
</div>
</div>
</div></td>
</tr>
<tr class="even">
<td><a href="https://docs.wso2.com/display/EI650/Property+Mediator">Property</a></td>
<td><div class="content-wrapper">
<p>As a standalone property:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="html/xml" data-syntaxhighlighter-params="brush: html/xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: html/xml; gutter: false; theme: Confluence"><code>&lt;property name=&quot;location&quot; 
              expression=&quot;json-eval($.coordinates.location[0].name)&quot;/&gt;</code></pre>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td><a href="https://docs.wso2.com/display/EI650/PayloadFactory+Mediator">PayloadFactory</a></td>
<td><div class="content-wrapper">
<p>As the payload arguments:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="html/xml" data-syntaxhighlighter-params="brush: html/xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: html/xml; gutter: false; theme: Confluence"><code>&lt;payloadFactory media-type=&quot;json&quot;&gt;
    &lt;format&gt;{&quot;RESPONSE&quot;:&quot;$1&quot;}&lt;/format&gt;
    &lt;args&gt;
        &lt;arg evaluator=&quot;json&quot; expression=&quot;$.coordinates.location[0].name&quot;/&gt;
    &lt;/args&gt;
&lt;/payloadFactory&gt;</code></pre>
</div>
</div>
<p><strong>IMPORTANT</strong> : You MUST omit the <code>               json-eval()              </code> method within the payload arguments to evaluate JSON paths within the PayloadFactory mediator. Instead, you MUST select the correct expression evaluator ( <code>               xml              </code> or <code>               json              </code> ) for a given argument.</p>
</div></td>
</tr>
<tr class="even">
<td><a href="https://docs.wso2.com/display/EI650/Switch+Mediator">Switch</a></td>
<td><div class="content-wrapper">
<p>As the switch source:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="html/xml" data-syntaxhighlighter-params="brush: html/xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: html/xml; gutter: false; theme: Confluence"><code>&lt;switch source=&quot;json-eval($.coordinates.location[0].name)&quot;&gt;</code></pre>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td><a href="https://docs.wso2.com/display/EI650/Filter+Mediator">Filter</a></td>
<td><div class="content-wrapper">
<p>As the filter source:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="html/xml" data-syntaxhighlighter-params="brush: html/xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: html/xml; gutter: false; theme: Confluence"><code>&lt;filter source=&quot;json-eval($.coordinates.location[0].name)&quot; 
        regex=&quot;Eiffel.*&quot;&gt;</code></pre>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

#### JSON path syntax

Suppose we have the following payload:

``` javascript
    { 
      "id": 12345,
      "id_str": "12345",
      "array": [ 1, 2, [ [], [{"inner_id": 6789}] ] ],
      "name": null,
      "object": {},
      "$schema_location": "unknown",
      "12X12": "image12x12.png"
    }
```

The following table summarizes sample JSONPath expressions and their
outputs:

<table>
<thead>
<tr class="header">
<th>Expression</th>
<th>Result</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>$.</code></pre></td>
<td><pre><code>{ &quot;id&quot;:12345, &quot;id_str&quot;:&quot;12345&quot;, &quot;array&quot;:[1, 2, [[],[{&quot;inner_id&quot;:6789}]]], &quot;name&quot;:null, &quot;object&quot;:{}, &quot;$schema_location&quot;:&quot;unknown&quot;, &quot;12X12&quot;:&quot;image12x12.png&quot;}</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>$.id</code></pre></td>
<td><pre><code>12345</code></pre></td>
</tr>
<tr class="odd">
<td><pre><code>$.name</code></pre></td>
<td><pre><code>null</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>$.object</code></pre></td>
<td><pre><code>{}</code></pre></td>
</tr>
<tr class="odd">
<td><pre><code>$.[&#39;$schema_location&#39;]</code></pre></td>
<td><pre><code>unknown</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>$.12X12</code></pre></td>
<td><pre><code>image12x12.png</code></pre></td>
</tr>
<tr class="odd">
<td><pre><code>$.array</code></pre></td>
<td><pre><code>[1, 2, [[],[{&quot;inner_id&quot;:6789}]]]</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>$.array[2][1][0].inner_id</code></pre></td>
<td><pre><code>6789</code></pre></td>
</tr>
</tbody>
</table>

You can learn more about JSONPath syntax
[here](http://goessner.net/articles/JsonPath/) .

### Logging JSON payloads

To log JSON payloads as JSON, use the [Log
mediator](https://docs.wso2.com/display/EI650/Log+Mediator) as shown
below. The `         json-eval()        ` method returns the
`         java.lang.String        ` representation of the existing JSON
payload.

``` html/xml
    <log>
        <property name="JSON-Payload" expression="json-eval($.)"/>
    </log>
```

To log JSON payloads as XML, use the Log mediator as shown below:

``` html/xml
    <log level="full"/>
```

For more information on logging, see [Troubleshooting, debugging, and
logging](#WorkingwithJSONMessagePayloads-troubleshooting) below.

### Constructing and transforming JSON payloads

To construct and transform JSON payloads, you can use the PayloadFactory
mediator or Script mediator as described in the rest of this section.

#### PayloadFactory mediator

The [PayloadFactory
mediator](https://docs.wso2.com/display/EI650/PayloadFactory+Mediator)
provides the simplest way to work with JSON payloads. Suppose we have a
service that returns the following response for a search query:

``` javascript
    {
       "geometry":{
          "location":{
             "lat":-33.867260,
             "lng":151.1958130
          }
       },
       "icon":"bar-71.png",
       "id":"7eaf7",
       "name":"Biaggio Cafe",
       "opening_hours":{
          "open_now":true
       },
       "photos":[
          {
             "height":600,
             "html_attributions":[
             ],
             "photo_reference":"CoQBegAAAI",
             "width":900
          }
       ],
       "price_level":1,
       "reference":"CnRqAAAAtz",
       "types":[
          "bar",
          "restaurant",
          "food",
          "establishment"
       ],
       "vicinity":"48 Pirrama Road, Pyrmont"
    }
```

We can create a proxy service that consumes the above response and
creates a new response containing the location name and tags associated
with the location based on several fields from the above response.

``` html/xml
    <proxy xmlns="http://ws.apache.org/ns/synapse"
         name="singleresponse"
         transports="https,http"
         statistics="disable"
         trace="disable"
         startOnLoad="true">
         <target>
             <outSequence>
                 <payloadFactory media-type="json">
                     <format>{
                                 "location_response" : {
                                     "name" : "$1",
                                     "tags" : $2
                             }}
                     </format>
                     <args>
                         <arg evaluator="json" expression="$.name"/>
                         <arg evaluator="json" expression="$.types"/>
                     </args>
                 </payloadFactory>
                 <send/>
             </outSequence>
             <endpoint>
                 <address uri="http://localhost:8280/location"/>
             </endpoint>
         </target>
     <description/>
    </proxy>
```

Use the following command to invoke this service:

``` bash
    curl -v -X GET "http://localhost:8280/services/singleresponse"
```

The response payload would look like this:

``` javascript
    {
       "location_response":{
          "name":"Biaggio Cafe",
          "tags":["bar", "restaurant", "food", "establishment"]
       }
    }
```

Note the following aspects of the proxy service configuration:

-   We use the `          payloadFactory         ` mediator to construct
    the new JSON payload.
-   The `          media-type         ` attribute is set to
    `          json         ` .
-   Because JSONPath expressions are used in arguments, the
    `          json         ` evaluators are specified.

##### Configuring the payload format

The `         <format>        ` section of the proxy service
configuration defines the format of the response. Notice that in the
example above, the name and tags field values are enclosed by double
quotes ("), which creates a string value in the final response. If you
do not use quotes, the value that gets assigned uses the real type
evaluated by the expression (boolean, number, object, array, or null).

It is also possible to instruct the PayloadFactory mediator to load a
payload format definition from the registry. This approach is
particularly useful when using large/complex payload formats in the
definitions. To load a format from the registry, click **Pick From
Registry** instead of **Define inline** when defining the PayloadFactory
mediator.

For example, suppose we have saved the following text content in the
registry under the location
`         conf:/repository/EI/transform        ` . (The resource name is
“transform”.)

``` html/xml
    {
        "location_response" : {
            "name" : "$1",
            "tags" : $2
        }
    }
```

We can now modify the definition of the PayloadFactory mediator to use
this format text saved as a registry resource as the payload format. The
new configuration would look as follows (note that the
`         <format>        ` element now uses the key attribute to point
to the registry resource key):

``` html/xml
    <payloadFactory media-type="json">
      <format key="conf:/repository/EI/transform"/>
      ... 
    </payloadFactory>
```

!!! note

When saving format text for the PayloadFactory mediator as a registry
resource, be sure to save it as text content with the “text/plain” media
type.


#### Script mediator

The [Script
mediator](https://docs.wso2.com/display/EI650/Script+Mediator) in
JavaScript is useful when you need to create payloads that have
recurring structures such as arrays of objects. The Script mediator
defines the following important methods that can be used to manipulate
payloads in many different ways:

-   `          getPayloadJSON         `
-   `          setPayloadJSON         `
-   `          getPayloadXML         `
-   `          setPayloadXML         `

By combining any of the setters with a getter, we can handle almost any
type of content transformation within the EI. For example, by combining
`         getPayloadXML        ` and `         setPayloadJSON        ` ,
we can easily implement an XML to JSON transformation scenario. In
addition, we can perform various operations (such as deleting individual
keys, modifying selected values, and inserting new objects) on JSON
payloads to transform from one JSON format to another JSON format by
using the `         getPayloadJSON        ` and
`         setPayloadJSON        ` methods. Following is an example of a
JSON to JSON transformation performed by the Script mediator.

Suppose a second service returns the following response:

``` javascript
    {
       "results" : [
          {
             "geometry" : {
                "location" : {
                   "lat" : -33.867260,
                   "lng" : 151.1958130
                }
             },
             "icon" : "bar-71.png",
             "id" : "7eaf7",
             "name" : "Biaggio Cafe",
             "opening_hours" : {
                "open_now" : true
             },
             "photos" : [
                {
                   "height" : 600,
                   "html_attributions" : [],
                   "photo_reference" : "CoQBegAAAI",
                   "width" : 900
                }
             ],
             "price_level" : 1,
             "reference" : "CnRqAAAAtz",
             "types" : [ "bar", "restaurant", "food", "establishment" ],
             "vicinity" : "48 Pirrama Road, Pyrmont"
          },
          {
             "geometry" : {
                "location" : {
                   "lat" : -33.8668040,
                   "lng" : 151.1955790
                }
             },
             "icon" : "generic_business-71.png",
             "id" : "3ef98",
             "name" : "Doltone House",
             "photos" : [
                {
                   "height" : 600,
                   "html_attributions" : [],
                   "photo_reference" : "CqQBmgAAAL",
                   "width" : 900
                }
             ],
             "reference" : "CnRrAAAAV",
             "types" : [ "food", "establishment" ],
             "vicinity" : "48 Pirrama Road, Pyrmont"
          }
       ],
       "status" : "OK"
    }
```

The following proxy service shows how we can transform the above
response using JavaScript with the Script mediator.

``` html/xml
    <proxy xmlns="http://ws.apache.org/ns/synapse"
           name="locations"
           transports="https,http"
           statistics="disable"
           trace="disable"
           startOnLoad="true">
       <target>
          <outSequence>
             <script language="js"
                     key="conf:/repository/EI/transform.js"
                     function="transform"/>
             <send/>
          </outSequence>
          <endpoint>
             <address uri="http://localhost:8280/locations"/>
          </endpoint>
       </target>
       <description/>
    </proxy>
```

The registry resource `         transform.js        ` contains the
JavaScript function that performs the transformation:

``` javascript
    function transform(mc) {
        payload = mc.getPayloadJSON();
        results = payload.results;
        var response = new Array();
        for (i = 0; i < results.length; ++i) {
            location_object = results[i];
            l = new Object();
            l.name = location_object.name;
            l.tags = location_object.types;
            l.id = "ID:" + (location_object.id);
            response[i] = l;
        }
        mc.setPayloadJSON(response);
    }
```

`         mc.getPayloadJSON()        ` returns the current JSON payload
as a JavaScript object. This object can be manipulated as a normal
JavaScript variable within a script as shown in the above JavaScript
code. The `         mc.setPayloadJSON()        ` method can be used to
replace the existing payload with a new payload. In the above script, we
build a new array object by using the fields of the incoming JSON
payload and set that array object as the new payload (see the response
payload returned by the final proxy service below.)  
  
Use the following command to invoke the proxy service:

``` bash
    curl -v -X GET "http://ggrky:8280/services/locations"
```

The response payload would look like this:

``` javascript
    [
        {
            "id":"ID:7eaf7", 
            "tags":["bar", "restaurant", "food", "establishment"], 
            "name":"Biaggio Cafe"
        }, 
        {
           "id":"ID:3ef98", 
           "tags":["food", "establishment"], 
           "name":"Doltone House"
        }
    ]
```

If you want to get the response in XML instead of JSON, you would modify
the out sequence by adding the Property mediator as follows:

``` html/xml
    <outSequence>
        <script language="js" 
                key="conf:/repository/EI/transform.js" 
                function="transform"/>
        <property name="messageType" value="application/xml" scope="axis2"/>
        <send/>
    </outSequence>
```

The response will then look like this:

``` html/xml
    <jsonArray>
       <jsonElement>
          <id>ID:7eaf7</id>
          <tags>bar</tags>
          <tags>restaurant</tags>
          <tags>food</tags>
          <tags>establishment</tags>
          <name>Biaggio Cafe</name>
       </jsonElement>
       <jsonElement>
          <id>ID:3ef98</id>
          <tags>food</tags>
          <tags>establishment</tags>
          <name>Doltone House</name>
       </jsonElement>
    </jsonArray>
```

If you are not getting the results you want when the Script mediator
converts the JSON payload directly into XML, you can build the XML
payload iteratively with the Script mediator as shown in the following
script.

``` javascript
    function transformXML(mc) {
        payload = mc.getPayloadJSON();
        results = payload.results;
        var response = <locations/>;
        for (i = 0; i < results.length; ++i) {
            var elem = results[i];
            response.locations += <location>
                <id>{elem.id}</id>
                <name>{elem.name}</name>
                <tags>{elem.types}</tags>
            </location>
        }
        mc.setPayloadXML(response);
    }
```

The response would now look like this:

``` html/xml
    <locations>
       <location>
          <id>7eaf7</id>
          <name>Biaggio Cafe</name>
          <tags>bar,restaurant,food,establishment</tags>
       </location>
       <location>
          <id>3ef98</id>
          <name>Doltone House</name>
          <tags>food,establishment</tags>
       </location>
    </locations>
```

Finally, let's look at how you can perform delete, modify, and add field
operations on JSON payloads with the Script mediator in JavaScript.
Let's send the JSON message returned by the `         locations        `
proxy service as the request for the following proxy service,
`         transformjson        ` :

``` html/xml
    <proxy xmlns="http://ws.apache.org/ns/synapse"
           name="transformjson"
           transports="https,http"
           statistics="disable"
           trace="disable"
           startOnLoad="true">
       <target>
          <inSequence>
             <script language="js">
               payload = mc.getPayloadJSON();
               for (i = 0; i &lt; payload.length; ++i) {
                   payload[i].id_str = payload[i].id;
                   delete payload[i].id;
                   payload[i].tags[payload[i].tags.length] = "pub";
               }
               mc.setPayloadJSON(payload);
             </script>
             <log>
                <property name="JSON-Payload" expression="json-eval($.)"/>
             </log>
             <respond/>
          </inSequence>
       </target>
       <description/>
    </proxy>
```

The proxy service will convert the request into the following format:

``` javascript
    [
       {
          "name":"Biaggio Cafe",
          "tags":["bar", "restaurant", "food", "establishment", "pub"],
          "id_str":"ID:7eaf7"
       },
       {
          "name":"Doltone House",
          "tags":["food", "establishment", "pub"],
          "id_str":"ID:3ef98"
       }
    ]
```

Note that the transformation (line 9 through 17) has added a new field
`         id_str        ` and removed the old field
`         id        ` from the request, and it has added a new tag
`         pub        ` to the existing tags list of the payload.

For additional examples that demonstrate different ways to manipulate
JSON payloads within the EI mediation flow, see the following samples:

-   [Sample 440: Converting JSON to XML Using
    XSLT](https://docs.wso2.com/display/ESB500/Sample+440%3A+Converting+JSON+to+XML+Using+XSLT)
-   [Sample 441: Converting JSON to XML Using
    JavaScript](https://docs.wso2.com/display/ESB500/Sample+441%3A+Converting+JSON+to+XML+Using+JavaScript)

### XML to JSON transformation parameters

You can use XML to JSON transformation parameters when you need
to transform XML formatted data into the JSON format.

Following are the XML to JSON transformation parameters and their
descriptions:

<table>
<thead>
<tr class="header">
<th><p>Parameter</p></th>
<th><p>Description</p></th>
<th>Default Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>              synapse.commons.json.preserve.namespace             </code></p></td>
<td><p>Preserves the namespace declarations in the JSON output in XML to JSON transformations.</p></td>
<td><code>             false            </code></td>
</tr>
<tr class="even">
<td><p><code>              synapse.commons.json.buildValidNCNames             </code></p></td>
<td><p>Builds valid XML NCNames when building XML element names in XML to JSON transformations.</p></td>
<td><code>             false            </code></td>
</tr>
<tr class="odd">
<td><p><code>              synapse.commons.json.output.autoPrimitive             </code></p></td>
<td><p>Allows primitive types in the JSON output in XML to JSON transformations.</p></td>
<td><code>             true            </code></td>
</tr>
<tr class="even">
<td><p><code>              synapse.commons.json.output.namespaceSepChar             </code></p></td>
<td><p>The namespace prefix separation character for the JSON output in XML to JSON transformations.</p></td>
<td>The default separation character is <code>             -            </code></td>
</tr>
<tr class="odd">
<td><p><code>              synapse.commons.json.output.enableNSDeclarations             </code></p></td>
<td><p>Adds XML namespace declarations in the JSON output in XML to JSON transformations.</p></td>
<td><code>             false            </code></td>
</tr>
<tr class="even">
<td><p><code>              synapse.commons.json.output.disableAutoPrimitive.regex             </code></p></td>
<td><p>Disables auto primitive conversion in XML to JSON transformations.</p></td>
<td>null</td>
</tr>
<tr class="odd">
<td><p><code>              synapse.commons.json.output.jsonoutAutoArray             </code></p></td>
<td><p>Sets the JSON output to an array element in XML to JSON transformations.</p></td>
<td><code>             true            </code></td>
</tr>
<tr class="even">
<td><p><code>              synapse.commons.json.output.jsonoutMultiplePI             </code></p></td>
<td><p>Sets the JSON output to an xml multiple processing instruction in XML to JSON transformations.</p></td>
<td><code>             true            </code></td>
</tr>
<tr class="odd">
<td><p><code>              synapse.commons.json.output.xmloutAutoArray             </code></p></td>
<td><p>Sets the XML output to an array element in XML to JSON transformations.</p></td>
<td><code>             true            </code></td>
</tr>
<tr class="even">
<td><p><code>              synapse.commons.json.output.xmloutMultiplePI             </code></p></td>
<td><p>Sets the XML output to an xml multiple processing instruction in XML to JSON transformations.</p></td>
<td><code>             false            </code></td>
</tr>
<tr class="odd">
<td><code>             synapse.commons.enableXmlNilReadWrite            </code></td>
<td>Handles how <a href="#WorkingwithJSONMessagePayloads-EmptyXMLelementswiththe&#39;nil&#39;attribute">empty XML elements with the 'nil' attribute</a> are converted to JSON.</td>
<td><code>             false            </code></td>
</tr>
<tr class="even">
<td><pre><code>synapse.commons.enableXmlNullForEmptyElement</code></pre></td>
<td>Handles how <a href="#WorkingwithJSONMessagePayloads-EmptyXMLelements">empty XML elements</a> are converted to JSON.</td>
<td><code>             true            </code></td>
</tr>
</tbody>
</table>

### Validating JSON messages

You can use the [Validate
mediator](https://docs.wso2.com/display/EI650/Validate+Mediator#ValidateMediator-json)
to validate JSON messages against a specified JSON schema as described
in the rest of this section.

#### Validate mediator

The parameters available in this section are as follows.

| Parameter Name                                | Description                                                                                                                                                              |
|-----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Schema keys defined for Validate Mediator** | This section is used to specify the key to access the main schema based on which validation is carried out, as well as to specify the JSON, which needs to be validated. |
| **Source**                                    | The JSONPath expression to extract the JSON that needs to be validated. E.g: `             json-eval($.msg)"            `                                                |

Following example use the below sample schema
`         StockQuoteSchema.json        ` file. Add this sample schema
file (i.e. `         StockQuoteSchema.json        ` ) to the following
Registry path:
`                   conf:/schema/StockQuoteSchema                  .        `
json. For instructions on adding the schema file to the Registry path,
see [Adding a
Resource](https://docs.wso2.com/display/ESB500/Adding+a+Resource) .

!!! tip

When adding this sample schema file to the Registry, specify the **Media
Type** as application/json.


``` java
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "getQuote": {
          "type": "object",
          "properties": {
            "request": {
              "type": "object",
              "properties": {
                "symbol": {
                  "type": "string"
                }
              },
              "required": [
                "symbol"
              ]
            }
          },
          "required": [
            "request"
          ]
        }
      },
      "required": [
        "getQuote"
      ]
    }
     
```

In this example, the required schema for validating messages going
through the Validate mediator is given as a registry key (i.e.
`         schema\StockQuoteSchema.json        ` ). You do not have any
source attributes specified. Therefore, the schema will be used to
validate the complete JSON body. The mediation logic to follow if the
validation fails is defined within the on-fail element. In this example,
the [PayloadFactory
mediator](https://docs.wso2.com/display/EI650/PayloadFactory+Mediator)
creates a fault to be sent back to the party, which sends the message.

``` java
    <validate>
        <schema key="conf:/schema/StockQuoteSchema.json"/>
        <on-fail>
            <payloadFactory media-type="json">
                <format>{"Error":$1"}</format>
                <args>
                    <arg evaluator="xml" expression="$ctx:ERROR_MESSAGE"/>
                </args>
            </payloadFactory>
            <property name="HTTP_SC" value="500" scope="axis2"/>
            <respond/>
        </on-fail>
    </validate>
```

An example for a valid JSON payload request is given below.

``` java
    {
       "getQuote": {
          "request": {
             "symbol": "WSO2"
          }
       }
    }
```

### Troubleshooting, debugging, and logging

To assist with troubleshooting, you can enable debug logging at several
stages of the mediation of a JSON payload within the EI by adding one or
more of the following loggers to the
`         <EI_HOME>/conf/log4j.properties        ` file and restarting
the EI.

!!! info

Be sure to turn off these loggers when running the EI in a production
environment, as logging every message will significantly reduce
performance.


Following are the available loggers:

Message builders and formatters

-   `           log4j.logger.org.apache.synapse.commons.json.JsonStreamBuilder=DEBUG          `

-   `           log4j.logger.org.apache.synapse.commons.json.JsonStreamFormatter=DEBUG          `

-   `           log4j.logger.org.apache.synapse.commons.json.JsonBuilder=DEBUG          `

-   `           log4j.logger.org.apache.synapse.commons.json.JsonFormatter=DEBUG          `

JSON utility class

`         log4j.logger.org.apache.synapse.commons.json.JsonUtil=DEBUG        `

PayloadFactory mediator

`         log4j.logger.org.apache.synapse.mediators.transform.PayloadFactoryMediator=DEBUG        `

JSONPath evaluator

`         log4j.logger.org.apache.synapse.util.xpath.SynapseJsonPath=DEBUG        `
