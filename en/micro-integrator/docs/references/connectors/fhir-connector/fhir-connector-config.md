# FHIR Connector Configuration

The FHIR Connector allows you to work with resources in [FHIR](http://www.hl7.org/fhir/index.html), which are the modular components of FHIR. The connector uses the [FHIR RESTFul API](http://www.hl7.org/fhir/http.html)  to interact with FHIR.

## Initializing the connector

Before you start performing various operations with the connector, make sure to import the FHIR certificate to your ESB client keystore.

To use the FHIR connector, add the  <fhir.init>  element in your configuration before carrying out any other FHIR operations.

For more information on authentication/security of the FHIR REST API, see http://www.hl7.org/implement/standards/fhir/security.html.

??? note "fhir.init"
    The fhir.init operation initializes the connector to interact with the FHIR REST API.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <th>base</th>
            <th>The service root URL.</th>
            <th>Yes.</th>
        </tr>
    </table>

    **Sample configurations**

    ```xml
    <fhir.init>
        <base>{$ctx:base}</base>
    <fhir.init>
    ```

---

### Conformance

??? note "getConformance"
    The conformance interaction retrieves the server's conformance statement that defines how it supports resources. The conformance interaction retrieves the server's conformance statement that defines how it supports resources, For that use fhir.getConformance and specify the following properties. For more information, see [FHIR API documentation](http://www.hl7.org/implement/standards/fhir/http.html#conformance).
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <th>base</th>
            <th>The <a href="http://www.hl7.org/implement/standards/fhir/http.html#root">service root URL</a>.</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>format</th>
            <th>The <a href="http://www.hl7.org/implement/standards/fhir/http.html#mime-type">Mime Type</a>.</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>id, content, lastUpdated, profile, query, security, tag, text, filter</th>
            <th>These are the optional parameters and are common search parameters for all resources.</th>
            <th>Optional.</th>
        </tr>
    </table>

    **Sample configurations**

    ```xml
    <fhir.getConformance>
        <base>{$ctx:base}</base>
        <format>{$ctx:format}</format>
        <id>{$ctx:id}</id>
        <content>{$ctx:content}</content>
        <lastUpdated>{$ctx:lastUpdated}</lastUpdated>
        <profile>{$ctx:profile}</profile>
        <query>{$ctx:query}</query>
        <security>{$ctx:security}</security>
        <tag>{$ctx:tag}</tag>
        <text>{$ctx:text}</text>
        <filter>{$ctx:filter}</filter>
    </fhir.getConformance>
    ```

    **Sample request**

    ```json
    {
        "base": "https://open-api.fhir.me",
        "format": "json",
        "id":"%s(id)",
        "content":"%s(content)",
        "lastUpdated":"%s(lastUpdated)",
        "profile":"%s(profile)",
        "query":"%s(query)",
        "security":"%s(security)",
        "tag":"%s(tag)",
        "text":"%s(text)",
        "filter":"%s(filter)"
    }
    ```

### History

??? note "history"
    To retrieves the history of a particular resource supported by the system , use fhir.history and specify the following properties. For more information, see related [FHIR API documentation](http://www.hl7.org/implement/standards/fhir/http.html#history).
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <th>base</th>
            <th>The <a href="http://www.hl7.org/implement/standards/fhir/http.html#root">service root URL</a>.</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>type</th>
            <th>The name of a resource type (e.g., "Patient").</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>idForHistory</th>
            <th>The id of the history that needs to be retrieved.</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>format</th>
            <th>The <a href="http://www.hl7.org/implement/standards/fhir/http.html#mime-type">Mime Type</a>.</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>id, content, lastUpdated, profile, query, security, tag, text, filter</th>
            <th>These are the optional parameters and are common search parameters for all resources.</th>
            <th>Optional.</th>
        </tr>
    </table>

    **Sample configurations**

    ```xml
    <fhir.history>
        <base>{$ctx:base}</base>
        <type>{$ctx:type}</type>
        <idForHistory>{$ctx:idForHistory}</idForHistory>
        <format>{$ctx:format}</format>
        <id>{$ctx:id}</id>
        <content>{$ctx:content}</content>
        <lastUpdated>{$ctx:lastUpdated}</lastUpdated>
        <profile>{$ctx:profile}</profile>
        <query>{$ctx:query}</query>
        <security>{$ctx:security}</security>
        <tag>{$ctx:tag}</tag>
        <text>{$ctx:text}</text>
        <filter>{$ctx:filter}</filter>
    </fhir.history>
    ```

    **Sample request**

    ```json
    {
        "base": "https://open-api.fhir.me",
        "type": "Patient",
        "idForHistory":1032702",
        "format": "json",
        "id":"%s(id)",
        "content":"%s(content)",
        "lastUpdated":"%s(lastUpdated)",
        "profile":"%s(profile)",
        "query":"%s(query)",
        "security":"%s(security)",
        "tag":"%s(tag)",
        "text":"%s(text)",
        "filter":"%s(filter)"
    }
    ```

??? note "historyAll"
    To retrieves the history of all resources supported by the system , use fhir.historyAll and specify the following properties. For more information, see related [FHIR API documentation](http://www.hl7.org/implement/standards/fhir/http.html#history).
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <th>base</th>
            <th>The <a href="http://www.hl7.org/implement/standards/fhir/http.html#root">service root URL</a>.</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>format</th>
            <th>The <a href="http://www.hl7.org/implement/standards/fhir/http.html#mime-type">Mime Type</a>.</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>id, content, lastUpdated, profile, query, security, tag, text, filter</th>
            <th>These are the optional parameters and are common search parameters for all resources.</th>
            <th>Optional.</th>
        </tr>
    </table>

    **Sample configurations**

    ```xml
    <fhir.historyAll>
        <base>{$ctx:base}</base>
        <format>{$ctx:format}</format>
        <id>{$ctx:id}</id>
        <content>{$ctx:content}</content>
        <lastUpdated>{$ctx:lastUpdated}</lastUpdated>
        <profile>{$ctx:profile}</profile>
        <query>{$ctx:query}</query>
        <security>{$ctx:security}</security>
        <tag>{$ctx:tag}</tag>
        <text>{$ctx:text}</text>
        <filter>{$ctx:filter}</filter>
    </fhir.historyAll>
    ```

    **Sample request**

    ```json
    {
        "base": "https://open-api.fhir.me",
        "format": "json",
        "id":"%s(id)",
        "content":"%s(content)",
        "lastUpdated":"%s(lastUpdated)",
        "profile":"%s(profile)",
        "query":"%s(query)",
        "security":"%s(security)",
        "tag":"%s(tag)",
        "text":"%s(text)",
        "filter":"%s(filter)"
    }
    ```

??? note "historyType"
    To retrieves the history of all resources of a given type supported by the system , use fhir.historyType and specify the following properties. For more information, see related [FHIR API documentation](http://www.hl7.org/implement/standards/fhir/http.html#history).
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <th>base</th>
            <th>The <a href="http://www.hl7.org/implement/standards/fhir/http.html#root">service root URL</a>.</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>type</th>
            <th>The name of a resource type (e.g., "Patient").</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>format</th>
            <th>The <a href="http://www.hl7.org/implement/standards/fhir/http.html#mime-type">Mime Type</a>.</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>id, content, lastUpdated, profile, query, security, tag, text, filter</th>
            <th>These are the optional parameters and are common search parameters for all resources.</th>
            <th>Optional.</th>
        </tr>
    </table>

    **Sample configurations**

    ```xml
    <fhir.historyType>
        <base>{$ctx:base}</base>
        <type>{$ctx:type}</type>
        <format>{$ctx:format}</format>
        <id>{$ctx:id}</id>
        <content>{$ctx:content}</content>
        <lastUpdated>{$ctx:lastUpdated}</lastUpdated>
        <profile>{$ctx:profile}</profile>
        <query>{$ctx:query}</query>
        <security>{$ctx:security}</security>
        <tag>{$ctx:tag}</tag>
        <text>{$ctx:text}</text>
        <filter>{$ctx:filter}</filter>
    </fhir.historyType>
    ```

    **Sample request**

    ```json
    {
        "base": "https://open-api.fhir.me",
        "type":"Patient"
        "format": "json",
        "id":"%s(id)",
        "content":"%s(content)",
        "lastUpdated":"%s(lastUpdated)",
        "profile":"%s(profile)",
        "query":"%s(query)",
        "security":"%s(security)",
        "tag":"%s(tag)",
        "text":"%s(text)",
        "filter":"%s(filter)"
    }
    ```

### Search

??? note "search"
    To search from a particular resource supported by the system , use fhir.search and specify the following properties. For more information, see related [FHIR API documentation on search operation](http://www.hl7.org/implement/standards/fhir/http.html#search) and [filter operation](http://www.hl7.org/implement/standards/fhir/search.html#filter).
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <th>base</th>
            <th>The <a href="http://www.hl7.org/implement/standards/fhir/http.html#root">service root URL</a>.</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>type</th>
            <th>The name of a resource type (e.g., "Patient").</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>format</th>
            <th>The <a href="http://www.hl7.org/implement/standards/fhir/http.html#mime-type">Mime Type</a>.</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>id, content, lastUpdated, profile, query, security, tag, text, filter</th>
            <th>These are the optional parameters and are common search parameters for all resources.</th>
            <th>Optional.</th>
        </tr>
    </table>

    **Sample configurations**

    ```xml
    <fhir.search>
        <base>{$ctx:base}</base>
        <type>{$ctx:type}</type>
        <format>{$ctx:format}</format>
        <id>{$ctx:id}</id>
        <content>{$ctx:content}</content>
        <lastUpdated>{$ctx:lastUpdated}</lastUpdated>
        <profile>{$ctx:profile}</profile>
        <query>{$ctx:query}</query>
        <security>{$ctx:security}</security>
        <tag>{$ctx:tag}</tag>
        <text>{$ctx:text}</text>
        <filter>{$ctx:filter}</filter>
    </fhir.search>
    ```

    **Sample request**

    ```json
    {
        "base": "https://open-api.fhir.me",
        "type": "Patient",
        "format": "json",
        "id":"%s(id)",
        "content":"%s(content)",
        "lastUpdated":"%s(lastUpdated)",
        "profile":"%s(profile)",
        "query":"%s(query)",
        "security":"%s(security)",
        "tag":"%s(tag)",
        "text":"%s(text)",
        "filter":"%s(filter)"
    }
    ```

??? note "searchPost"
    To search from a particular resource supported by the system , use fhir.searchPost and specify the following properties. For more information, see related [FHIR API documentation on the search operation](http://www.hl7.org/implement/standards/fhir/http.html#search).
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <th>base</th>
            <th>The <a href="http://www.hl7.org/implement/standards/fhir/http.html#root">service root URL</a>.</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>type</th>
            <th>The name of a resource type (e.g., "Patient").</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>format</th>
            <th>The <a href="http://www.hl7.org/implement/standards/fhir/http.html#mime-type">Mime Type</a>.</th>
            <th>Yes.</th>
        </tr>
        <tr>
            <th>id, content, lastUpdated, profile, query, security, tag, text, filter</th>
            <th>These are the optional parameters and are common search parameters for all resources.</th>
            <th>Optional.</th>
        </tr>
    </table>

    **Sample configurations**

    ```xml
    <fhir.historyAll>
        <base>{$ctx:base}</base>
        <format>{$ctx:format}</format>
        <id>{$ctx:id}</id>
        <content>{$ctx:content}</content>
        <lastUpdated>{$ctx:lastUpdated}</lastUpdated>
        <profile>{$ctx:profile}</profile>
        <query>{$ctx:query}</query>
        <security>{$ctx:security}</security>
        <tag>{$ctx:tag}</tag>
        <text>{$ctx:text}</text>
        <filter>{$ctx:filter}</filter>
    </fhir.historyAll>
    ```

    **Sample request**

    ```json
    {
        "base": "https://open-api.fhir.me",
        "type": "Patient",
        "format": "json",
        "id":"%s(id)",
        "content":"%s(content)",
        "lastUpdated":"%s(lastUpdated)",
        "profile":"%s(profile)",
        "query":"%s(query)",
        "security":"%s(security)",
        "tag":"%s(tag)",
        "text":"%s(text)",
        "filter":"%s(filter)"
    }
    ```