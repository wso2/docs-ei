# Script Mediator with Nashorn Support

From WSO2 EI 6.2.0 onwards, the [Script Mediator](_Script_Mediator_) of
the ESB profile uses
[Nashorn](http://www.oracle.com/technetwork/articles/java/jf14-nashorn-2126515.html)
to execute JavaScripts in addition to its default Rhino engine. You can
perform all Script Mediator functionalities that the Rhino engine
provides, with the Nashorn engine as well.

!!! info

You need to install JDK 8 version update 112 or later to use Nashorn
support, If not, you get the following exception when you call the
`         setPayloadJSON()        ` method:
`         java.lang.ClassCastException:        `
`         jdk.nashorn.internal.runtime.Undefined cannot be cast to java.lang.String        `

For more information, see [the bug that causes
this](http://bugs.java.com/bugdatabase/view_bug.do?bug_id=8157160) ,
which is fixed in JDK 8 version update 112 or later.


The Rhino engine uses E4X XML objects to handle XML payloads. However,
[E4X is
deprecated](https://developer.mozilla.org/en-US/docs/Archive/Web/E4X/Processing_XML_with_E4X)
in the Nashorn engine. Instead, Nashorn supports the AXIOM XML parser to
parse XML streams. You can use the
`         getParsedOMElement(InputStream stream)        ` method to
parse an XML stream into an OMElement, and the
`         getXpathResult(String expression)        ` method to retrieve
the AXIOMXPath using an Xpath query expression to access and modify it.

The XPath equivalents for a few common XML navigation operations are as
follows.

<table>
<thead>
<tr class="header">
<th>Operation</th>
<th>XPath expression</th>
<th>E4X equivalent</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Select all the children of an element</td>
<td><code>             element/*            </code></td>
<td><code>             element.*            </code></td>
</tr>
<tr class="even">
<td>Select all the attributes of element</td>
<td><code>             element/@*            </code></td>
<td><code>             element.@*            </code></td>
</tr>
<tr class="odd">
<td>Select all the descendants (children, grandchildren, etc.) of an element</td>
<td><code>             element//descendent            </code></td>
<td><code>             element..descendent            </code></td>
</tr>
<tr class="even">
<td>Select the parent of an element</td>
<td><code>             .. or parent::element            </code></td>
<td><code>             element.parent()            </code></td>
</tr>
<tr class="odd">
<td>Select a specific child (e.g. foo:bar ) of an element, where 'foo' is the prefix of a declared namespace</td>
<td><p><code>              xmlns:foo="..." element/foo:bar             </code></p>
<div>
<code>                           </code>
</div></td>
<td><p><code>              var foo = new Namespace(...);             </code></p>
<p><code>              element.foo::bar             </code></p>
<div>
<code>                           </code>
</div></td>
</tr>
<tr class="even">
<td>Return the full name (including prefixes if available) of an element</td>
<td><code>             name(element)            </code></td>
<td><code>             element.name()            </code></td>
</tr>
<tr class="odd">
<td>Return the local name of an element</td>
<td><code>             local-name(element)            </code></td>
<td><code>             element.localName()            </code></td>
</tr>
<tr class="even">
<td>Return the namespace URI of an element (if available)</td>
<td><code>             namespace-uri(element)            </code></td>
<td><code>             element.namespace()            </code></td>
</tr>
<tr class="odd">
<td><p>Return the collection of namespaces.</p>
<ul>
<li>For E4X: returns as an Array of Namespace objects</li>
<li>For XPath: returns as a set of Namespaces nodes</li>
</ul></td>
<td><code>             element/namespace::*            </code></td>
<td><code>             element.inScopeNamespaces()            </code></td>
</tr>
<tr class="even">
<td>Return the processing instructions of the specified children of an element (returns all if omitted)</td>
<td><code>             element/processing-instructions(name)            </code></td>
<td><code>             element.processingInstructions(name)            </code></td>
</tr>
<tr class="odd">
<td>Return the concatenated text nodes and descendants of an element</td>
<td><code>             string(element)            </code></td>
<td><code>             stringValue(element);            </code></td>
</tr>
</tbody>
</table>

You can create a Script mediator with Nashorn support using one of the
following methods.

-   Store the JavaScript program statements in a separate file, and
    refer it via a [Local or Remote Registry
    entry](https://docs.wso2.com/display/EI620/Working+with+Local+Registry+Entries)
    .

-   Embed the JavaScript program statements inline within the Synapse
    configuration.

You can invoke a function within the corresponding script using the
Script Mediator. Hence, you can predefine the Synapse configuration in a
script variable named `         mc        ` , and access it via these
functions. The `         mc        ` variable represents an
implementation of the
`         NashornScriptMessageContext.java        ` MessageContext. It
is an implementation of the `         ScriptMessageContext        `
interface, which contains the following methods. You can access these
methods within the script (e.g., `         mc.methodName        ` ).

| Method name                                                   | Description                                                                                                                                                                                 | If a value is returned or not |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| `             getParsedOMElement(stream)            `         | Converts the input stream of an XML String or document into an OMElement. This is useful to traverse the XML document.                                                                      | Yes                           |
| `             getXpathResult(expression)            `         | Converts a String, which represents an Xpath expression into an AXIOM Xpath. This is useful to traverse the XML document.                                                                   | Yes                           |
| `             addHeader(mustUnderstand, content)            ` | Adds a new SOAP header to the message. Content can be XML, String, parsed w3c.dom.Document, or an OMElement returned by the `             getParsedOMElement(stream)            ` method.   | No                            |
| `             getEnvelopeXML()            `                   | Retrieves the XML representation of the complete SOAP envelope.                                                                                                                             | Yes                           |
| `             getPayloadJSON()            `                   | Retrieves the JSON representation of a SOAP Body payload.                                                                                                                                   | Yes                           |
| `             getPayloadXML()            `                    | Retrieves the XML representation of a SOAP Body payload. This method Returns a OMElement so the user can directly access elements using Xpath.                                              | Yes                           |
| `             getProperty(name)            `                  | Retrieves a specified property from the current message context.                                                                                                                            | Yes                           |
| `             setPayloadJSON(payload)            `            | Sets the JSON representation of a payload obtained via the `             getPayloadJSON()            ` method in the current message context.                                               | No                            |
| `             setPayloadXML(payload)            `             | Sets the SOAP body payload from the XML.  Payload can be XML, String, parsed w3c.dom.Document or an OMElement returned by the `             getParsedOMElement(stream)            ` method. | No                            |
| `             setProperty(key, value)            `            | Replaces the existing property values a property in the current message context.                                                                                                            | No                            |

The Script Mediator has the flexibility of a class Mediator, with access
to the Synapse Message Context and Synapse Environment APIs. For both
types of script mediator definitions, the Message Context passed into
the script has additional methods over the standard Synapse Message
Context, to enable working with natural XML to JavaScript.

!!! info

The Script mediator is a [content
aware](https://docs.wso2.com/display/ei620/Mediators#Mediators-Content-awareness)
mediator.


------------------------------------------------------------------------

[Syntax](#ScriptMediatorwithNashornSupport-Syntax) \|
[Configuration](#ScriptMediatorwithNashornSupport-Configuration) \|
[Examples](#ScriptMediatorwithNashornSupport-Examples)

------------------------------------------------------------------------

### Syntax

Click on the relevant tab to view the syntax for a script mediator using
an Inline script, or a script mediator using a script of the Registry.

-   [**Using an Inline script**](#0533b492911a476184414a924320a1c2)
-   [**Using a script in the
    Registry**](#397e230892394573af0c7d2a5cb4431a)

The following syntax applies when you create a Script mediator with the
script program statements embedded inline within the Synapse
configuration.

``` java
    <script language="nashornJs"><![CDATA[......Nashorn JavaScript source code…….. ]]></script>
```

The following syntax applies when you create a Script mediator with the
script program statements stored in a separate file, referenced via the
[Local or Remote Registry
entry](https://docs.wso2.com/display/EI620/Working+with+Local+Registry+Entries)
. This uses a key to refer to a script, which is already saved in the
Registry.

  

``` xml
    <script key="string" language="nashornJs" [function="script-function-name"]>
        <include key="string"/>
    </script>
```

### Configuration

Click on the relevant tab to view the required UI configuration
depending on the script type you have selected. The available script
types are as follows:

-   **Inline** : If this script type is selected, the script is
    specified inline.
-   **Registry Key** : If this script type is selected, a script which
    is already saved in the registry will be referred using a key.

-   [**Inline**](#3e5035aff19c4b09971f5601a41b38ca)
-   [**Using a script in the
    Registry**](#0a2006113c99443ba2e0ef5ac50b429d)

The parameters available to configure a Script mediator using an inline
script are as follows.

<table>
<thead>
<tr class="header">
<th>Parameter Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Language</strong></td>
<td><p>The scripting language for the Script mediator. You can select from the following available languages.</p>
<ul>
<li>JavaScript - This is represented as <code>                   js                  </code> in the source view.</li>
<li>Groovy - This is represented as <code>                   groovy                  </code> in the source view.</li>
<li>Ruby - This is represented as <code>                   rb                  </code> in the source view.</li>
</ul></td>
</tr>
<tr class="even">
<td><strong>Source</strong></td>
<td>Enter the source in this parameter.</td>
</tr>
</tbody>
</table>

The parameters available to configure a Script mediator using a script
saved in the registry are as follows.

<table>
<thead>
<tr class="header">
<th>Parameter Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Language</strong></td>
<td><p>The scripting language for the Script mediator. You can select from the following available languages.</p>
<ul>
<li>JavaScript - This is represented as <code>                   js                  </code> in the source view.</li>
<li>Groovy - This is represented as <code>                   groovy                  </code> in the source view.</li>
<li>Ruby - This is represented as <code>                   rb                  </code> in the source view.</li>
</ul></td>
</tr>
<tr class="even">
<td><strong>Function</strong></td>
<td>The function of the selected script language to be invoked. This is an optional parameter. If no value is specified, a default function named <code>                 mediate                </code> will be applied. This function considers the Synapse MessageContext as a single parameter. The function may return a boolean. If it does not, then the value <code>                 true                </code> is assumed and the Script mediator returns this value.</td>
</tr>
<tr class="odd">
<td><strong>Key Type</strong></td>
<td><p>You can select one of the following options.</p>
<ul>
<li><strong>Static Key</strong> : If this is selected, an existing key can be selected from the registry for the <strong>Key</strong> parameter.</li>
<li><strong>Dynamic Key</strong> : If this is selected, the key can be entered dynamically in the <strong>Key</strong> parameter.</li>
</ul></td>
</tr>
<tr class="even">
<td><strong>Key</strong></td>
<td>The Registry location of the source. You can click either <strong>Configuration Registry</strong> or the <strong>Governance Registry</strong> to select the source from the resource tree.</td>
</tr>
<tr class="odd">
<td><strong>Include keys</strong></td>
<td><div class="content-wrapper">
<p>This parameter allows you to include functions defined in two or more scripts your Script mediator configuration. After pointing to one script in the <strong>Key</strong> parameter, you can click <strong>Add Include</strong> <strong>Key</strong> to add the function in another script.</p>
<p>When you click <strong>Add Include</strong> <strong>Key</strong> , the following parameters will be displayed. Enter the script to be included in the <strong>Key</strong> parameter by clicking either <strong>Configuration Registry</strong> or the <strong>Governance Registry</strong> <strong></strong> and then selecting the relevant script from the resource tree.</p>
</div></td>
</tr>
</tbody>
</table>

### Examples

-   [Example 1 - Using an inline
    script](#ScriptMediatorwithNashornSupport-Example1-Usinganinlinescript)
-   [Example 2 - Using a script saved in the
    registry](#ScriptMediatorwithNashornSupport-Example2-Usingascriptsavedintheregistry)
-   [Examples for
    methods](#ScriptMediatorwithNashornSupport-Examplesformethods)

Assume the below SOAP envelope for the following examples:

``` xml
    <?xml version='1.0' encoding='utf-8'?>
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Header>
    <AuthenticationInfo xmlns="http://www.x" soapenv:mustUnderstand="0">
    <userName>”wso2”</userName>
    <password>”wso2”</password>
    </AuthenticationInfo>
    </soapenv:Header>
    <soapenv:Body>
    <company>
        <staff id="1001">
            <firstname>”yong”</firstname>
            <lastname>”mook kim”</lastname>
            <nickname>”mkyong”</nickname>
            <salary>100000</salary>
        </staff>
        <staff id="2001">
            <firstname> “low”</firstname>
            <lastname>”yin fong”</lastname>
            <nickname>”fong fong”</nickname>
            <salary>200000</salary>
        </staff>
    </company>
    </soapenv:Body>
    </soapenv:Envelope>
```

#### Example 1 - Using an inline script

The following configuration is an example of using a Nashorn JavaScript
script inline in the Script Mediator. In this script, you get the
message payload in XML and you use an Xpath query to access the elements
of it.

``` xml
    <script language="nashornJs"><![CDATA[var symbol = mc.getPayloadXML();
    var expression = "//firstname";
    var xpath = mc.getXpathResult(expression);
    var c1l = xpath.selectNodes(symbol);
    var first_name = c1l.get(0).getText();
    ]]></script>
```

#### Example 2 - Using a script saved in the registry

In the following example, the script is loaded from the Registry using
the key
`          repository/conf/sample/resources/script/test.js         ` .

``` xml
    <script language="nashornJs"
        key="repository/conf/sample/resources/script/test.js"
        function="testFunction"/>
```

The `          script language="nashornJs"         ` property indicates
that the invoked function should be in the Nashorn JavaScript language.
You need to save the function named `          testFunction         ` ,
which is invoked in the above example as a resource in the Registry.
Following is an example for the script of the function.

``` js
    function testFunction(mc) {
    var symbol = mc.getPayloadXML();
    var expression = "//firstname";
    var xpath = mc.getXpathResult(expression);
    var c1l = xpath.selectNodes(symbol);
    var first_name = c1l.get(0).getText();
    }
```

#### Examples for methods

The following Script Mediator configuration examples show how you can
include some of the commonly used methods in the invoked script.

<table>
<thead>
<tr class="header">
<th>Methods</th>
<th>SOAP envelope</th>
<th>Sample Script Mediator configuration</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><ul>
<li><p><code>                 setPayloadXML(payload)                </code></p></li>
<li><p><code>                 getPayloadXML(mustUnderstand,content)                </code></p></li>
<li><p><code>                 addHeader()                </code></p></li>
<li><p><code>                 getEnvelopeXML()                </code></p></li>
<li><p><code>                 getXpathResult(expression)                </code></p></li>
<li><p><code>                 getParsedOMElement(stream)                </code></p></li>
</ul></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">&lt;?xml</span> version=&#39;1.0&#39; encoding=&#39;utf-8&#39;<span class="kw">?&gt;</span></span>
<span id="cb1-2"><a href="#cb1-2"></a><span class="kw">&lt;soapenv:Envelope</span><span class="ot"> xmlns:soapenv=</span><span class="st">&quot;http://schemas.xmlsoap.org/soap/envelope/&quot;</span><span class="kw">&gt;</span></span>
<span id="cb1-3"><a href="#cb1-3"></a><span class="kw">&lt;soapenv:Header&gt;</span></span>
<span id="cb1-4"><a href="#cb1-4"></a><span class="kw">&lt;AuthenticationInfo</span><span class="ot"> xmlns=</span><span class="st">&quot;http://www.x&quot;</span><span class="ot"> soapenv:mustUnderstand=</span><span class="st">&quot;0&quot;</span><span class="kw">&gt;</span></span>
<span id="cb1-5"><a href="#cb1-5"></a><span class="kw">&lt;userName&gt;</span>”wso2”<span class="kw">&lt;/userName&gt;</span></span>
<span id="cb1-6"><a href="#cb1-6"></a><span class="kw">&lt;password&gt;</span>”wso2”<span class="kw">&lt;/password&gt;</span></span>
<span id="cb1-7"><a href="#cb1-7"></a><span class="kw">&lt;/AuthenticationInfo&gt;</span></span>
<span id="cb1-8"><a href="#cb1-8"></a><span class="kw">&lt;/soapenv:Header&gt;</span></span>
<span id="cb1-9"><a href="#cb1-9"></a><span class="kw">&lt;soapenv:Body&gt;</span></span>
<span id="cb1-10"><a href="#cb1-10"></a><span class="kw">&lt;company&gt;</span></span>
<span id="cb1-11"><a href="#cb1-11"></a>    <span class="kw">&lt;staff</span><span class="ot"> id=</span><span class="st">&quot;1001&quot;</span><span class="kw">&gt;</span></span>
<span id="cb1-12"><a href="#cb1-12"></a>        <span class="kw">&lt;firstname&gt;</span>”yong”<span class="kw">&lt;/firstname&gt;</span></span>
<span id="cb1-13"><a href="#cb1-13"></a>        <span class="kw">&lt;lastname&gt;</span>”mook kim”<span class="kw">&lt;/lastname&gt;</span></span>
<span id="cb1-14"><a href="#cb1-14"></a>        <span class="kw">&lt;nickname&gt;</span>”mkyong”<span class="kw">&lt;/nickname&gt;</span></span>
<span id="cb1-15"><a href="#cb1-15"></a>        <span class="kw">&lt;salary&gt;</span>100000<span class="kw">&lt;/salary&gt;</span></span>
<span id="cb1-16"><a href="#cb1-16"></a>    <span class="kw">&lt;/staff&gt;</span></span>
<span id="cb1-17"><a href="#cb1-17"></a>    <span class="kw">&lt;staff</span><span class="ot"> id=</span><span class="st">&quot;2001&quot;</span><span class="kw">&gt;</span></span>
<span id="cb1-18"><a href="#cb1-18"></a>        <span class="kw">&lt;firstname&gt;</span> “low”<span class="kw">&lt;/firstname&gt;</span></span>
<span id="cb1-19"><a href="#cb1-19"></a>        <span class="kw">&lt;lastname&gt;</span>”yin fong”<span class="kw">&lt;/lastname&gt;</span></span>
<span id="cb1-20"><a href="#cb1-20"></a>        <span class="kw">&lt;nickname&gt;</span>”fong fong”<span class="kw">&lt;/nickname&gt;</span></span>
<span id="cb1-21"><a href="#cb1-21"></a>        <span class="kw">&lt;salary&gt;</span>200000<span class="kw">&lt;/salary&gt;</span></span>
<span id="cb1-22"><a href="#cb1-22"></a>    <span class="kw">&lt;/staff&gt;</span></span>
<span id="cb1-23"><a href="#cb1-23"></a><span class="kw">&lt;/company&gt;</span></span>
<span id="cb1-24"><a href="#cb1-24"></a><span class="kw">&lt;/soapenv:Body&gt;</span></span>
<span id="cb1-25"><a href="#cb1-25"></a><span class="kw">&lt;/soapenv:Envelope&gt;</span></span></code></pre></div>
</div>
</div>
</div></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb2-1"><a href="#cb2-1"></a><span class="kw">&lt;script</span><span class="ot"> language=</span><span class="st">&quot;nashornJs&quot;</span><span class="kw">&gt;</span><span class="bn">&lt;![CDATA[</span></span>
<span id="cb2-2"><a href="#cb2-2"></a>var symbol = mc.getPayloadXML();</span>
<span id="cb2-3"><a href="#cb2-3"></a> </span>
<span id="cb2-4"><a href="#cb2-4"></a>var expression = &quot;//firstname&quot;;</span>
<span id="cb2-5"><a href="#cb2-5"></a>var xpath = mc.getXpathResult(expression);</span>
<span id="cb2-6"><a href="#cb2-6"></a>var nameList = xpath.selectNodes(symbol);</span>
<span id="cb2-7"><a href="#cb2-7"></a>var name = nameList.get(0).getText();</span>
<span id="cb2-8"><a href="#cb2-8"></a>mc.setPayloadXML(symbol); //here we are setting payload by a parsed xml document or this coud be done                      by passing xml string as well</span>
<span id="cb2-9"><a href="#cb2-9"></a>var password = &quot;wso2&quot;;</span>
<span id="cb2-10"><a href="#cb2-10"></a>var username = &quot;wso2&quot;;</span>
<span id="cb2-11"><a href="#cb2-11"></a>var headerXml =</span>
<span id="cb2-12"><a href="#cb2-12"></a> &quot;&lt;AuthenticationInfo xmlns=\&quot;http://www&quot;</span>
<span id="cb2-13"><a href="#cb2-13"></a>                    + &quot;.w3.org/1999/xhtml\&quot;&gt;&lt;userName&gt;&quot; + username + &quot;&lt;/userName&gt;&lt;password&gt;&quot;+     password + &quot;&lt;/password&quot;</span>
<span id="cb2-14"><a href="#cb2-14"></a>                    + &quot;&gt;&lt;/AuthenticationInfo&gt;&quot;;</span>
<span id="cb2-15"><a href="#cb2-15"></a>mc.addHeader(false, headerXml ); </span>
<span id="cb2-16"><a href="#cb2-16"></a>var envelope = mc.getEnvelopeXML();</span>
<span id="cb2-17"><a href="#cb2-17"></a> </span>
<span id="cb2-18"><a href="#cb2-18"></a>expression = &quot;//nickname&quot;;</span>
<span id="cb2-19"><a href="#cb2-19"></a>stream = new byteArrayStream(envelope.getBytes());</span>
<span id="cb2-20"><a href="#cb2-20"></a>var docEnvelope = mc.getParsedOMElement(stream);</span>
<span id="cb2-21"><a href="#cb2-21"></a>xpath = mc.getXpathResult(expression);</span>
<span id="cb2-22"><a href="#cb2-22"></a>var nickNameList = xpath.selectNodes(docEnvelope);</span>
<span id="cb2-23"><a href="#cb2-23"></a>var nickname = nickNameList.get(0).getText();</span>
<span id="cb2-24"><a href="#cb2-24"></a><span class="bn">]]&gt;</span><span class="kw">&lt;/script&gt;</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="even">
<td><ul>
<li><p><code>                 setProperty("key", value);                </code></p></li>
<li><p><code>                 getPayloadJSON()                </code></p></li>
<li><p><code>                 setPayloadJSON(payload)                </code></p></li>
</ul></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb3" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb3-1"><a href="#cb3-1"></a><span class="kw">&lt;?xml</span> version=&#39;1.0&#39; encoding=&#39;utf-8&#39;<span class="kw">?&gt;</span></span>
<span id="cb3-2"><a href="#cb3-2"></a><span class="kw">&lt;soapenv:Envelope</span><span class="ot"> xmlns:soapenv=</span><span class="st">&quot;http://schemas.xmlsoap.org/soap/envelope/&quot;</span><span class="kw">&gt;</span></span>
<span id="cb3-3"><a href="#cb3-3"></a><span class="kw">&lt;soapenv:Body&gt;</span></span>
<span id="cb3-4"><a href="#cb3-4"></a><span class="kw">&lt;jsonObject&gt;</span></span>
<span id="cb3-5"><a href="#cb3-5"></a><span class="kw">&lt;appointmentNo&gt;</span>55<span class="kw">&lt;/appointmentNo&gt;</span></span>
<span id="cb3-6"><a href="#cb3-6"></a><span class="kw">&lt;doctorName&gt;</span>thomascollins<span class="kw">&lt;/doctorName&gt;</span></span>
<span id="cb3-7"><a href="#cb3-7"></a><span class="kw">&lt;patient&gt;</span>JohnDoe<span class="kw">&lt;/patient&gt;</span></span>
<span id="cb3-8"><a href="#cb3-8"></a><span class="kw">&lt;actualFee&gt;</span>7000.0<span class="kw">&lt;/actualFee&gt;</span></span>
<span id="cb3-9"><a href="#cb3-9"></a><span class="kw">&lt;discount&gt;</span>0<span class="kw">&lt;/discount&gt;</span></span>
<span id="cb3-10"><a href="#cb3-10"></a><span class="kw">&lt;discounted&gt;</span>7000.0<span class="kw">&lt;/discounted&gt;</span></span>
<span id="cb3-11"><a href="#cb3-11"></a><span class="kw">&lt;paymentID&gt;</span>c3b7cab3-8e22-4319-a43f-b1bef3de2693<span class="kw">&lt;/paymentID&gt;</span></span>
<span id="cb3-12"><a href="#cb3-12"></a><span class="kw">&lt;status&gt;</span>Settled<span class="kw">&lt;/status&gt;</span></span>
<span id="cb3-13"><a href="#cb3-13"></a><span class="kw">&lt;/jsonObject&gt;</span></span>
<span id="cb3-14"><a href="#cb3-14"></a><span class="kw">&lt;/soapenv:Body&gt;</span></span>
<span id="cb3-15"><a href="#cb3-15"></a><span class="kw">&lt;/soapenv:Envelope&gt;</span></span></code></pre></div>
</div>
</div>
</div></td>
<td><div class="content-wrapper">
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb4" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb4-1"><a href="#cb4-1"></a>&lt;script language=<span class="st">&quot;nashornJs&quot;</span>&gt;&lt;![CDATA[</span>
<span id="cb4-2"><a href="#cb4-2"></a>var doctor = {</span>
<span id="cb4-3"><a href="#cb4-3"></a>        name : <span class="st">&quot;x&quot;</span>,</span>
<span id="cb4-4"><a href="#cb4-4"></a>        fee : <span class="dv">200</span></span>
<span id="cb4-5"><a href="#cb4-5"></a>      };</span>
<span id="cb4-6"><a href="#cb4-6"></a>mc.<span class="fu">setProperty</span>(<span class="st">&quot;scriptObject&quot;</span>, doctor);</span>
<span id="cb4-7"><a href="#cb4-7"></a> </span>
<span id="cb4-8"><a href="#cb4-8"></a>var payload = mc.<span class="fu">getPayloadJSON</span>();</span>
<span id="cb4-9"><a href="#cb4-9"></a>var results = payload.<span class="fu">actualFee</span>;</span>
<span id="cb4-10"><a href="#cb4-10"></a>doctor.<span class="fu">fee</span> = results;</span>
<span id="cb4-11"><a href="#cb4-11"></a>mc.<span class="fu">setPayloadJSON</span>(doctor);</span>
<span id="cb4-12"><a href="#cb4-12"></a>]]&gt;&lt;/script&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>
