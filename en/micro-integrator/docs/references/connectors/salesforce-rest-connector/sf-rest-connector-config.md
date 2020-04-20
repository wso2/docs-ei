# Salesforce REST Connector Configuration

The following operations allow you to work with the Salesforce REST Connector. Click an operation name to see parameter details and samples on how to use it.

---

Salesforce REST API uses the OAuth protocol to allow application users to securely access data without having to reveal their user credentials. For more information on how authentication is done in Salesforce, see [Understanding Authentication](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm).

To use the Salesforce REST connector, add the `<salesforcerest.init>` element in your configuration before carrying out any other Salesforce REST operations. 

??? note "salesforcerest.init"
    The salesforcerest.init operation initializes the connector to interact with the Salesforce REST API. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>apiVersion</td>
            <td>The version of the Salesforce API.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>accessToken</td>
            <td>The access token to authenticate your API calls.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>apiUrl</td>
            <td>The instance URL for your organization.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>hostName</td>
            <td>SalesforceOAuth endpoint when issuing authentication requests in your application.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>refreshToken</td>
            <td>The refresh token that you received to refresh the API access token.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>clientId</td>
            <td>The consumer key of the connected application that you created.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>clientSecret</td>
            <td>The consumer secret of the connected application that you created.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>intervalTime</td>
            <td>The time interval in milliseconds, after which you need to check the validity of the access token.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>registryPath</td>
            <td>The registry path of the connector. You must specify the registry path as follows: registryPath = “connectors/salesforcerest”.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>blocking</td>
            <td>Indicates whether the connector needs to perform blocking invocations to Salesforce.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.init>
        <accessToken>{$ctx:accessToken}</accessToken>
        <apiUrl>{$ctx:apiUrl}</apiUrl>
        <clientId>{$ctx:clientId}</clientId>
        <clientSecret>{$ctx:clientSecret}</clientSecret>
        <refreshToken>{$ctx:refreshToken}</refreshToken>
        <hostName>{$ctx:hostName}</hostName>
        <apiVersion>{$ctx:apiVersion}</apiVersion>
        <registryPath>{$ctx:registryPath}</registryPath>
        <intervalTime>{$ctx:intervalTime}</intervalTime>
        <blocking>{$ctx:blocking}</blocking>
    </salesforcerest.init>
    ```

    **Sample request**

    ```json
    {
        "clientId": "3MVG9ZL0ppGP5UrBztM9gSLYyUe7VwAVhD9.yQnZX2mmCu_48Uwc._doxrBTgY4jqmOSDhxRAiUBf8gCr2mk7",
        "refreshToken": "5Aep861TSESvWeug_ztpnAk6BGQxRdovMLhHso81iyYKO6hTm6kHoL4.YfwIi9cHLwga.pPTsTuJlmKjo05x.o.",
        "clientSecret": "1187341468789253319",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "registryPath":"connectors/salesforcerest",
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "intervalTime" : "100000",
        "apiUrl":"https://ap2.salesforce.com",
        "blocking" : "false"
    }
    ```

??? note "salesforcerest.init for username/password flow"
    The salesforcerest.init operation initializes the connector to interact with the Salesforce REST API using a username/password flow. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_username_password_oauth_flow.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>apiVersion</td>
            <td>The version of the Salesforce API.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>apiUrl</td>
            <td>The instance URL for your organization.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>hostName</td>
            <td>SalesforceOAuth endpoint when issuing authentication requests in your application.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>clientId</td>
            <td>The consumer key of the connected application that you created.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>clientSecret</td>
            <td>The consumer secret of the connected application that you created.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>username</td>
            <td>The username for Salesforce.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>password</td>
            <td>The password for Salesforce (need to append the password with security key).</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>blocking</td>
            <td>Indicates whether the connector needs to perform blocking invocations to Salesforce.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.init>
        <apiUrl>{$ctx:apiUrl}</apiUrl>
        <clientId>{$ctx:clientId}</clientId>
        <clientSecret>{$ctx:clientSecret}</clientSecret>
        <hostName>{$ctx:hostName}</hostName>
        <apiVersion>{$ctx:apiVersion}</apiVersion>
        <username>{$ctx:username}</username>
        <password>{$ctx:password}</password>
        <blocking>{$ctx:blocking}</blocking>
    </salesforcerest.init>
    ```

    **Sample request**
    
    ```json
    {
        "clientId": "3MVG9ZL0ppGP5UrBztM9gSLYyUe7VwAVhD9.yQnZX2mmCu_48Uwc._doxrBTgY4jqmOSDhxRAiUBf8gCr2mk7",
        "clientSecret": "xxxxxxxxxxxxxxxxxxxxxxxx",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "username": "tharis63@outlook.com",
        "password": "xxxxxxxxxxxxxxxxxxxxxx",
        "apiUrl":"https://ap2.salesforce.com",
        "blocking" : "false"
    }
    ```

---

### Working with AppMenu

??? note "listItemsInMenu"
    To retrieve the list of items in either the Salesforce app drop-down menu or the Salesforce1 navigation menu, use salesforcerest.listItemsInMenu and specify the following property. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/resources_appmenu.htm?search_text=menu) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>menuType</td>
            <td>The type of the menu, either AppSwitcher or Salesforce.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.listItemsInMenu>
        <menuType>{$ctx:menuType}</menuType>
    </salesforcerest.listItemsInMenu>
    ```
    
    **Sample request**

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "menuType": "AppSwitcher",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample response**

    ```json
        {"NetworkTabs":"/services/data/v32.0/appMenu/NetworkTabs","Salesforce1":"/services/data/v32.0/appMenu/Salesforce1","AppSwitcher":"/services/data/v32.0/appMenu/AppSwitcher"}  
    ```

??? note "tabs"
    To retrieve a list of all tabs, use salesforcerest.tabs. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/resources_tabs.htm?search_text=tabs) for more information.

    **Sample configuration**

    ```xml
    <salesforcerest.tabs/>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the tabs operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample response**

    Given below is a sample response for the tabs operation.

    ```json
    {"output":"[{\"colors\":[{\"color\":\"4dca76\",\"context\":\"primary\",\"theme\":\"theme4\"},{\"color\":\"319431\",\"context\":\"primary\",\"theme\":\"theme3\"}],\"custom\":true,\"iconUrl\":\"https://sampletest-dev-ed.my.salesforce.com/img/icon/form32.png\",..}
    ```

??? note "themes"
    To retrieve a list of icons and colors used by themes in the Salesforce application, use salesforcerest.themes. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/resources_themes.htm?search_text=themes) for more information.

    **Sample configuration**

    ```xml
    <salesforcerest.themes/>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the themes operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample response**

    Given below is a sample response for the themes operation.

    ```json
    {
        "themeItems":[
            {
                "name":"Account",
                "icons":[
                    {
                    "width":32,
                    "theme":"theme3",
                    "contentType":"image/png",
                    "url":"https://kesavan-dev-ed.my.salesforce.com/img/icon/accounts32.png",
                    "height":32
                    }
                ]
            }
        ]
    }
    ```

---

### Working with Approvals

??? note "listApprovals"
    To retrieve the list of approvals in Salesforce, use salesforcerest.listApprovals. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/dome_process_approvals.htm) for more information.

    **Sample configuration**

    ```xml
    <salesforcerest.listApprovals/>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the listApprovals operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample response**

    Given below is a sample response for the listApprovals operation.

    ```json
    {
        "approvals":{

        }
    }
    ```

---

### Working with Event Monitoring

??? note "describeEventMonitoring"
    To retrieve the description of the event monitoring log, use salesforcerest.describeEventMonitoring. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/dome_event_log_file_describe.htm) for more information.

    **Sample configuration**

    ```xml
    <salesforcerest.describeEventMonitoring/>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the describeEventMonitoring operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample response**

    Given below is a sample response for the describeEventMonitoring operation.

    ```json
    {
        "updateable":false,
        "activateable":false,
        "childRelationships":[

         ],
        "recordTypeInfos":[

        ],
        "deprecatedAndHidden":false,
        "searchLayoutable":false,
        "deletable":false,
        "replicateable":false,
        "actionOverrides":[

        ],
        .
        .
        ],
        "labelPlural":"Event Log Files",
        "triggerable":false
    }
    ```

??? note "queryEventMonitoringData"
    To retrieve the field values from a record, use salesforcerest.queryEventMonitoringData and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/dome_event_log_file_query.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>queryStringForEventMonitoringData</td>
            <td>The query string to use to get the field values from the log.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.queryEventMonitoringData>
        <queryStringForEventMonitoringData>{$ctx:queryStringForEventMonitoringData}</queryStringForEventMonitoringData>
    </salesforcerest.queryEventMonitoringData>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the queryEventMonitoringData operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBztM9gSLYyUe7VwAVhD9.yQnZX2mmCu_48Uwc._doxrBTgY4jqmOSDhxRAiUBf8gCr2mk7",
        "refreshToken": "5Aep861TSESvWeug_ztpnAk6BGQxRdovMLhHso81iyYKO6hTm45JVxz3FLewCKgI4BbUp19OzGfqG2TdCfqa2ZU",
        "clientSecret": "1187341468789253319",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v34.0",
        "intervalTime" : "100000",
        "queryStringForEventMonitoringData": "SELECT+Id+,+EventType+,+LogFile+,+LogDate+,+LogFileLength+FROM+EventLogFile+WHERE+LogDate+>+Yesterday+AND+EventType+=+'API'",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample response**

    Given below is a sample response for the queryEventMonitoringData operation.

    ```json
    {
        "totalSize" : 4,
        "done" : true,
        "records" : [ {
            "attributes" : {
            "type" : "EventLogFile",
            "url" : "/services/data/v32.0/sobjects/EventLogFile/0ATD000000001bROAQ"     }
            "Id" : "0ATD000000001bROAQ",
            "EventType" : "API",
            "LogFile" : "/services/data/v32.0/sobjects/EventLogFile/0ATD000000001bROAQ/LogFile",
            "LogDate" : "2014-03-14T00:00:00.000+0000",
            "LogFileLength" : 2692.0
            }, 
            .
        ]
    }
    ```

---

### Working with Invocable Actions

??? note "getListOfAction"
    To retrieve the list of general action types for the current organization, use salesforcerest.getListOfAction and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_actions_invocable.htm?search_text=action) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>actionType</td>
            <td>The type of the invocable action.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.getListOfAction/>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the getListOfAction operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample response**

    Given below is a sample response for the getListOfAction operation.

    ```json
    {
        "standard":"/services/data/v32.0/actions/standard",
        "custom":"/services/data/v32.0/actions/custom"
    }
    ```

??? note "getSpecificListOfAction"
    To retrieve an attribute of a single action, use salesforcerest.getAttributeOfSpecificAction and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_actions_invocable_standard.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>actionType</td>
            <td>The type of the invocable action.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.getSpecificListOfAction>
        <actionType>{$ctx:actionType}</actionType>
    </salesforcerest.getSpecificListOfAction>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the getSpecificListOfAction operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "actionType": "standard",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample response**

    Given below is a sample response for the getSpecificListOfAction operation.

    ```json
    {
        "standard":"/services/data/v32.0/actions/standard",
        "custom":"/services/data/v32.0/actions/custom"
    }
    ```

??? note "getAttributeOfSpecificAction"
    To retrieve an attribute of a single action, use salesforcerest.getAttributeOfSpecificAction and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_actions_invocable_standard.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>actionType</td>
            <td>The type of the invocable action.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>attribute</td>
            <td>The attribute whose details you want to retrieve.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.getAttributeOfSpecificAction>
        <actionType>{$ctx:actionType}</actionType>
        <attribute>{$ctx:attribute}</attribute>
    </salesforcerest.getAttributeOfSpecificAction>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the getAttributeOfSpecificAction operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "actionType": "standard",
        "attribute": "emailSimple",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample response**

    Given below is a sample response for the getAttributeOfSpecificAction operation.

    ```json
    {
        "actions":[
        {
            "name":"chatterPost",
            "label":"Post to Chatter",
            "type":"CHATTERPOST"
        },
        {
            "name":"emailSimple",
            "label":"Send Email",
            "type":"EMAILSIMPLE"
        }
        .
        ]
    }
    ```

### Working with Layouts

??? note "sObjectLayouts"
    To retrieve a list of layouts and descriptions (including for actions) for a specific object, use salesforcerest.sObjectLayouts and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/resources_sobject_layouts.htm?search_text=layouts) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object whose layouts and descriptions you want to retrieve.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.sObjectLayouts>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
    </salesforcerest.sObjectLayouts>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the sObjectLayouts operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "sObjectName": "Account",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the sObjectLayouts operation.

    ```json
    "layouts":[
        {
         "detailLayoutSections":[
            {
               "heading":"Account Information",
               "columns":2,
               "tabOrder":"TopToBottom",
               "useCollapsibleSection":false,
               "rows":8,
               "useHeading":false,
               "layoutRows":[
                  {
                     "layoutItems":[
                        {
                           "editableForUpdate":false,
                           "editableForNew":false,
                           "layoutComponents":[
                              {
                                 "tabOrder":1,
                                 "details":{
                                    "defaultValue":null,
                                    "precision":0,
                                    "nameField":false,
                                    "type":"reference",
                                    "restrictedDelete":false,
                                    "relationshipName":"Owner",
                                    "calculatedFormula":null,
                                    "controllerName":null,
                                    "namePointing":false,
                                    "defaultValueFormula":null,
                                    "calculated":false,
                                    "writeRequiresMasterRead":false,
                                    "inlineHelpText":null,
                                    "picklistValues":[

                                    ]
                               }
                        }
                    ]
                 }
        .
        }
    ```

??? note "globalSObjectLayouts"
    To retrieve descriptions of global publisher layouts, use salesforcerest.globalSObjectLayouts. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/resources_sobject_layouts.htm?search_text=layouts) for more information.
    
    **Sample configuration**

    ```xml
    <salesforcerest.globalSObjectLayouts/>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the globalSObjectLayouts operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the globalSObjectLayouts operation.

    ```json
    {
        "layouts":[
        {
            "detailLayoutSections":[

            ],
            "relatedContent":null,
            "editLayoutSections":[

            ],
            "relatedLists":[

            ],
            "buttonLayoutSection":null,
            "id":"00h28000001hExeAAE",
            "offlineLinks":[

            ],
            .
            .
        }
    }
    ```

??? note "compactLayouts"
    To retrieve a list of compact layouts for multiple objects, use salesforcerest.compactLayouts and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/resources_compact_layouts.htm?search_text=layouts) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectNameList</td>
            <td>A comma-separated list of the objects whose compact layouts you want to retrieve.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.globalSObjectLayouts/>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the compactLayouts operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "sObjectNameList":"Account,User",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the compactLayouts operation.

    ```json
    {
        "Account":{
        "name":"SYSTEM",
        "id":null,
        "label":"System Default",
        "actions":[
            {
                "showsStatus":false,
                "custom":false,
                "label":"Call",
                "overridden":false,
                "encoding":null,
                "icons":[
                {
                    "width":0,
                    "theme":"theme4",
                    "contentType":"image/svg+xml",
                    "url":"https://kesavan-dev-ed.my.salesforce.com/img/icon/t4v32/action/call.svg",
                    "height":0
                },
                ],
                "windowPosition":null,
                "colors":[
                {
                    "color":"F2CF5B",
                    "context":"primary",
                    "theme":"theme4"
                }
                ],
        .
        .
        ],
        "objectType":"User"
        }
    }
    ```

??? note "sObjectApprovalLayouts"
    To retrieve a list of approval layouts for a specified object, use salesforcerest.sObjectApprovalLayouts and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/resources_sobject_approvallayouts.htm?search_text=layouts) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object whose layouts you want to retrieve.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.sObjectApprovalLayouts>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
    </salesforcerest.sObjectApprovalLayouts>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the sObjectApprovalLayouts operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "sObjectName":"Account",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the sObjectApprovalLayouts operation.

    ```json
    {"approvalLayouts":[]}
    ```

??? note "sObjectCompactLayouts"
    To retrieve a list of compact layouts for a specific object, use salesforcerest.sObjectCompactLayouts and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/resources_sobject_compactlayouts.htm?search_text=layouts) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object whose layouts you want to retrieve.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.sObjectCompactLayouts>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
    </salesforcerest.sObjectCompactLayouts>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the sObjectCompactLayouts operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "sObjectName":"Account",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the sObjectCompactLayouts operation.

    ```json
    {
        "compactLayouts":[
        {
            "name":"SYSTEM",
            "id":null,
            "label":"System Default",
            "actions":[
                {
                "showsStatus":false,
                "custom":false,
                "label":"Call",
                "overridden":false,
                "encoding":null,
                "icons":[
                    {
                        "width":0,
                        "theme":"theme4",
                        "contentType":"image/svg+xml",
                        "url":"https://kesavan-dev-ed.my.salesforce.com/img/icon/t4v32/action/call.svg",
                        "height":0
                    }
                ],
                "defaultCompactLayoutId":null
        .
        ]
    }
    ```

??? note "sObjectNamedLayouts"
    To retrieve information about alternative named layouts for a specific object, use salesforcerest.sObjectNamedLayouts and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/resources_sobject_named_layouts.htm?search_text=layouts) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object whose layouts you want to retrieve.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>layoutName</td>
            <td>The type of layout.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.sObjectNamedLayouts>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
        <layoutName>{$ctx:layoutName}</layoutName>
    </salesforcerest.sObjectNamedLayouts>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the sObjectCompactLayouts operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "sObjectName":"Account",
        "layoutName": "UserAlt",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the sObjectCompactLayouts operation.

    ```json
    {
        "layouts":[
        {
            "detailLayoutSections":[
                {
                "heading":"About",
                "columns":2,
                "tabOrder":"LeftToRight",
                "useCollapsibleSection":false,
                "rows":2,
                "useHeading":false,
                "layoutRows":[
                    {
                        "layoutItems":[
                            {
                            "editableForUpdate":false,
                            "editableForNew":false,
                            "layoutComponents":[
                                {
                                    "components":[
                                        {
                                        "tabOrder":2,
                                        "details":{
                                            "defaultValue":null,
                                            "precision":0,
                                            "nameField":false,
                                            "type":"string",
                                            "restrictedDelete":false,
                                            "relationshipName":null,
                                            "calculatedFormula":null,
                                            "controllerName":null,
                                            "namePointing":false,
                                            "defaultValueFormula":null,
                                            "calculated":false,
                                            "writeRequiresMasterRead":false,
                                            "inlineHelpText":null,
                                            "picklistValues":[

                                            ]
                                        }
                                }     
                            ]
                        }
    .
    }
    ```

### Working with List Views

??? note "listViews"
    To retrieve a list of list views for the specific sObject, use salesforcerest.listViews and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_listviews.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object whose list views you want to retrieve.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.listViews>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
    </salesforcerest.listViews>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the listViews operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "sObjectName": "Account",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the listViews operation.

    ```json
    {
        "nextRecordsUrl":null,
        "size":7,
        "listviews":[
        {
            "resultsUrl":"/services/data/v32.0/sobjects/Account/listviews/00B280000032AihEAE/results",
            "soqlCompatible":true,
            "id":"00B280000032AihEAE",
            "label":"New This Week",
            "describeUrl":"/services/data/v32.0/sobjects/Account/listviews/00B280000032AihEAE/describe",
            "developerName":"NewThisWeek",
            "url":"/services/data/v32.0/sobjects/Account/listviews/00B280000032AihEAE"
        },
        .
        .
        ],
        "done":true,
        "sobjectType":"Account"
    }
    ```

??? note "listViewById"
    To retrieve the basic information about one list view for the specific sObject, use salesforcerest.listViewById and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_listviews.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object whose list of list views you want to retrieve.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>listViewId</td>
            <td>The ID of the specific list view whose information you want to return.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.listViewById>
        <listViewID>{$ctx:listViewID}</listViewID>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
    </salesforcerest.listViewById>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the listViewById operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "sObjectName": "Account",
        "intervalTime" : "100000",
        "listViewID":"00B28000002yqeVEAQ",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the listViewById operation.

    ```json
    {
        "resultsUrl":"/services/data/v32.0/sobjects/Account/listviews/00B280000032AihEAE/results",
        "soqlCompatible":true,
        "id":"00B280000032AihEAE",
        "label":"New This Week",
        "describeUrl":"/services/data/v32.0/sobjects/Account/listviews/00B280000032AihEAE/describe",
        "developerName":"NewThisWeek",
        "url":"/services/data/v32.0/sobjects/Account/listviews/00B280000032AihEAE"
    }
    ```

??? note "recentListViews"
    To retrieve the list of recently used list views for the given sObject type, use salesforcerest.recentListViews and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_recentlistviews.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object whose recently used list views you want to return.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.recentListViews>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
    </salesforcerest.recentListViews>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the recentListViews operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "sObjectName": "Account",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the recentListViews operation.

    ```json
    {
        "nextRecordsUrl":null,
        "size":2,
        "listviews":[
        {
            "resultsUrl":"/services/data/v32.0/sobjects/Account/listviews/00B280000032AihEAE/results",
            "soqlCompatible":true,
            "id":"00B280000032AihEAE",
            "label":"New This Week",
            "describeUrl":"/services/data/v32.0/sobjects/Account/listviews/00B280000032AihEAE/describe",
            "developerName":"NewThisWeek",
            "url":"/services/data/v32.0/sobjects/Account/listviews/00B280000032AihEAE"
        }
        .
        .
        ],
        "done":true,
        "sobjectType":"Account"
    }
    ```

??? note "describeListViewById"
    To retrieve detailed information (ID, columns, and SOQL query) about a specific list view for the given sObject type, use salesforcerest.describeListViewById and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_listviewdescribe.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object to which the list view applies.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>listViewID</td>
            <td>The ID of the list view.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.describeListViewById>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
        <listViewID>{$ctx:listViewID}</listViewID>
    </salesforcerest.describeListViewById>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the describeListViewById operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "sObjectName": "Account",
        "listViewID":"00B28000002yqeVEAQ",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the describeListViewById operation.

    ```json
    {
        "whereCondition":{
        "field":"CreatedDate",
        "values":[
            "THIS_WEEK"
        ],
        "operator":"equals"
        },
        "columns":[
        {
            "fieldNameOrPath":"Name",
            "sortDirection":"ascending",
            "hidden":false,
            "sortIndex":0,
            "ascendingLabel":"Z-A",
            "label":"Account Name",
            "sortable":true,
            "type":"string",
            "descendingLabel":"A-Z",
            "selectListItem":"Name"
        },
        .
        .
        ],
        "query":"SELECT Name, Site, BillingState, Phone, toLabel(Type), Owner.Alias, Id, CreatedDate, LastModifiedDate, SystemModstamp FROM Account WHERE CreatedDate = THIS_WEEK ORDER BY Name ASC NULLS FIRST, Id ASC NULLS FIRST",
        "scope":null,
        "orderBy":[
        {
            "fieldNameOrPath":"Name",
            "sortDirection":"ascending",
            "nullsPosition":"first"
        },
        {
            "fieldNameOrPath":"Id",
            "sortDirection":"ascending",
            "nullsPosition":"first"
        }
        ],
        "id":"00B280000032Aih",
        "sobjectType":"Account"
    }
    ```

??? note "listViewResults"
    To execute the SOQL query for the list view and return the resulting data and presentation information, use salesforcerest.listViewResults and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/resources_listviewresults.htm?search_text=list%20view) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object to which the list view applies.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>listViewID</td>
            <td>The ID of the list view.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.listViewResults>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
        <listViewID>{$ctx:listViewID}</listViewID>
    </salesforcerest.listViewResults>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the listViewResults operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "sObjectName": "Account",
        "intervalTime" : "100000",
        "listViewID":"00B28000002yqeVEAQ",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the listViewResults operation.

    ```json
    {
        "size":0,
        "records":[

        ],
        "columns":[
            {
                "fieldNameOrPath":"Name",
                "sortDirection":"ascending",
                "hidden":false,
                "sortIndex":0,
                "ascendingLabel":"Z-A",
                "label":"Account Name",
                "sortable":true,
                "type":"string",
                "descendingLabel":"A-Z",
                "selectListItem":"Name"
            },
            .
            .
        ],
        "id":"00B280000032Aih",
        "label":"New This Week",
        "developerName":"NewThisWeek",
        "done":true
    }
    ```

### Working with Process Rules

??? note "listProcessRules"
    To retrieve the list of process rules in the organization, use salesforcerest.listProcessRules. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/dome_process_rules.htm) for more information.
    
    **Sample configuration**

    ```xml
    <salesforcerest.listProcessRules/>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the listProcessRules operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the listProcessRules operation.

    ```json
    {
        "rules":{

        }
    }
    ```

??? note "getSpecificProcessRule"
    To retrieve the metadata for a specific sObject process rule, use salesforcerest.getSpecificProcessRule and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/dome_process_rules_particular.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The object whose process rule you want to retrieve.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>workflowRuleId</td>
            <td>The ID of the process rule.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.getSpecificProcessRule>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
        <workflowRuleId>{$ctx:workflowRuleId}</workflowRuleId>
    </salesforcerest.getSpecificProcessRule>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the getSpecificProcessRule operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "sObjectName": "Account",
        "workflowRuleId": "01QD0000000APli",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the getSpecificProcessRule operation.

    ```json
    {
        "actions" : [ {
            "id" : "01VD0000000D2w7",
            "name" : "ApprovalProcessTask",
            "type" : "Task"
            } ],
            "description" : null,
            "id" : "01QD0000000APli",
            "name" : "My Rule",
            "namespacePrefix" : null,
            "object" : "Account"
    }
    ```

### Working with Queries

??? note "query"
    To retrieve data from an object, use salesforcerest.query and specify the following properties. If you want your results to include deleted records in the Recycle Bin, use salesforcerest.queryAll in place of salesforcerest.query. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_query.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>queryString</td>
            <td>The SQL query to use to search for records.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    query:

    ```xml
    <salesforcerest.query>
        <queryString>{$ctx:queryString}</queryString>
    </salesforcerest.query>
    ```

    queryAll:

    ```xml
    <salesforcerest.queryAll>
        <queryString>{$ctx:queryString}</queryString>
    </salesforcerest.queryAll>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the query operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "queryString": "select id, name from Account",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the query operation.

    ```json
    {
        "done" : false,
        "totalSize" : 2014,
        "nextRecordsUrl" : "/services/data/v20.0/query/01gD0000002HU6KIAW-2000",
        "records" : 
        [ 
            {  
                "attributes" : 
                {    
                    "type" : "Account",    
                    "url" : "/services/data/v20.0/sobjects/Account/001D000000IRFmaIAH"  
                },  
                "Name" : "Test 1"
            }, 
            {  
                "attributes" : 
                {    
                    "type" : "Account",    
                    "url" : "/services/data/v20.0/sobjects/Account/001D000000IomazIAB"  
                },  
                "Name" : "Test 2"
            }, 

            ...

        ]
    }
    ```

??? note "queryMore"
    If the results from the query or queryAll operations are too large, the first batch of results is returned along with an ID that you can use with salesforcerest.queryMore to get additional results. If you want your results to include deleted records in the Recycle Bin, use salesforcerest.queryAllMore in place of salesforcerest.queryMore. See the [related API documentation for queryMore](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_query.htm) and [queryAllMore](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_query.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>nextRecordsUrl</td>
            <td>The query identifier for retrieving additional results.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    queryMore:

    ```xml
    <salesforcerest.queryMore>
        <nextRecordsUrl>{$ctx:nextRecordsUrl}</nextRecordsUrl>
    </salesforcerest.queryMore>
    ```

    queryAllMore:

    ```xml
    <salesforcerest.queryAllMore>
        <nextRecordsUrl>{$ctx:nextRecordsUrl}</nextRecordsUrl>
    </salesforcerest.queryAllMore>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the queryMore operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "nextRecordsUrl": "QWE45HUJ39D9UISD00",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the queryMore operation.

    ```json
    {
        "done" : true,
        "totalSize" : 3214,
        "records" : [...]
    }
    ```

??? note "queryPerformanceFeedback"
    To get feedback on how Salesforce will execute your query, use the salesforcerest.queryPerformanceFeedback operation. It uses the Query resource along with the explain parameter to get feedback. Salesforce analyzes each query to find the optimal approach to obtain the query results. Depending on the query and query filters, an index or internal optimization might be used. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_query_explain.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>queryString</td>
            <td>The SQL query to use to get feedback for a query.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.queryPerformanceFeedback>
        <queryString>{$ctx:queryString}</queryString>
    </salesforcerest.queryPerformanceFeedback>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the queryPerformanceFeedback operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "queryString": "select id, name from Account",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the queryPerformanceFeedback operation.

    ```json
    {
        "plans":[
        {
            "leadingOperationType":"TableScan",
            "relativeCost":2.8324836601307193,
            "sobjectCardinality":2549,
            "fields":[

            ],
            "cardinality":2549,
            "sobjectType":"Account"
        }
        ]
    }
    ```

??? note "listviewQueryPerformanceFeedback"
    For retrieving query performance feedback on a report or list view, use salesforcerest.listviewQueryPerformanceFeedback and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_query_explain.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>listViewID</td>
            <td>The ID of the report or list view to get feedback for a query.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.listviewQueryPerformanceFeedback>
        <listViewID>{$ctx:listViewID}</listViewID>
    </salesforcerest.listviewQueryPerformanceFeedback>
    ```
    
    **Sample request**

    The following is a sample request that can be handled by the listviewQueryPerformanceFeedback operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "listViewID":"00B28000002yqeVEAQ",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the listviewQueryPerformanceFeedback operation.

    ```json
    {
        "plans":[
        {
            "leadingOperationType":"Index",
            "relativeCost":0,
            "sobjectCardinality":2549,
            "fields":[
                "CreatedDate"
            ],
            "cardinality":0,
            "sobjectType":"Account"
        },
        .
        .
        ]
    }
    ```

### Working with Quick Actions

??? note "quickActions"
    To retrieve a list of global actions, use salesforcerest.quickActions. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_quickactions.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>queryString</td>
            <td>The SQL query to use to search for records.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.quickActions/>
    ```

    **Sample request**

    The following is a sample request that can be handled by the quickActions operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the quickActions operation.

    ```json
    {
        "output":"[
        {\"label\":\"Log a Call\",
        \"name\":\"LogACall\",
        \"type\":\"LogACall\",
        \"urls\":{\"defaultValues\":\"/services/data/v32.0/quickActions/LogACall/defaultValues\",\"quickAction\":\"/services/data/v32.0/quickActions/LogACall\",\"describe\":\"/services/data/v32.0/quickActions/LogACall/describe\",\"defaultValuesTemplate\":\"/services/data/v32.0/quickActions/LogACall/defaultValues/{ID}\"}},
        .
        .
        ]"
    }
    ```

??? note "sObjectAction"
    To retrieve a list of object-specific actions, use salesforcerest.sObjectAction and specify the following property. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_sobject_quickactions.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object for which you want to retrieve a list of quick actions.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.sObjectAction>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
    </salesforcerest.sObjectAction>
    ```

    **Sample request**

    The following is a sample request that can be handled by the sObjectAction operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "sObjectName": "Account",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the sObjectAction operation.

    ```json
    {
        "output":"[
        {\"label\":\"Log a Call\",
        \"name\":\"LogACall\",\"type\":\"LogACall\",
        \"urls\":{\"defaultValues\":\"/services/data/v32.0/quickActions/LogACall/defaultValues\",
        \"quickAction\":\"/services/data/v32.0/quickActions/LogACall\",
        \"describe\":\"/services/data/v32.0/quickActions/LogACall/describe\",
        \"defaultValuesTemplate\":\"/services/data/v32.0/quickActions/LogACall/defaultValues/{ID}\"}},
        .
        .
        ]"
    }
    ```

??? note "getSpecificAction"
    To retrieve a specific action, use salesforcerest.getSpecificAction and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_sobject_quickactions.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>actionName</td>
            <td>The action to return.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.getSpecificAction>
        <actionName>{$ctx:actionName}</actionName>
    </salesforcerest.getSpecificAction>
    ```

    **Sample request**

    The following is a sample request that can be handled by the getSpecificAction operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "actionName":"hariprasath__LogACall",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the getSpecificAction operation.

    ```json
    {
        "iconName":null,
        "targetRecordTypeId":null,
        "targetSobjectType":"Task",
        "canvasApplicationName":null,
        "label":"Log a Call",
        "accessLevelRequired":null,
        "icons":[
            {
                "width":0,
                "theme":"theme4",
                "contentType":"image/svg+xml",
                "url":"https://kesavan-dev-ed.my.salesforce.com/img/icon/t4v32/action/log_a_call.svg",
                "height":0
            },
        .
        . 
        ],
        "targetParentField":null,
        "iconUrl":"https://kesavan-dev-ed.my.salesforce.com/img/icon/log_a_call_32.png",
        "height":null
    }
    ```

??? note "getDescribeSpecificAction"
    To retrieve the description of a specific action, use salesforcerest.getDescribeSpecificAction and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/resources_sobject_quickactions.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>actionName</td>
            <td>The action whose description you want to return.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.getDescribeSpecificAction>
        <actionName>{$ctx:actionName}</actionName>
    </salesforcerest.getDescribeSpecificAction>
    ```

    **Sample request**

    The following is a sample request that can be handled by the getDescribeSpecificAction operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "sObjectName":"Account",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the getDescribeSpecificAction operation.

    ```json
    {
        "iconName":null,
        "targetRecordTypeId":null,
        "targetSobjectType":"Task",
        "canvasApplicationName":null,
        "label":"Log a Call",
        "accessLevelRequired":null,
        "icons":[
            {
                "width":0,
                "theme":"theme4",
                "contentType":"image/svg+xml",
                "url":"https://kesavan-dev-ed.my.salesforce.com/img/icon/t4v32/action/log_a_call.svg",
                "height":0
            }
        ],
        .
        .
        "targetParentField":null,
        "iconUrl":"https://kesavan-dev-ed.my.salesforce.com/img/icon/log_a_call_32.png",
        "height":null
    }
    ```

??? note "getDefaultValueOfAction"
    To return a specific action’s default values, including default field values, use salesforcerest.getDefaultValueOfAction and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/resources_sobject_quickactions.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>actionName</td>
            <td>The specific action.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.getDefaultValueOfAction>
        <actionName>{$ctx:actionName}</actionName>
    </salesforcerest.getDefaultValueOfAction>
    ```

    **Sample request**

    The following is a sample request that can be handled by the getDefaultValueOfAction operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "actionName":"hariprasath__LogACall",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the getDefaultValueOfAction operation.

    ```json
    {
        "WhoId":null,
        "Description":null,
        "WhatId":null,
        "attributes":{
            "type":"Task"
        },
        "Subject":"Call"
    }
    ```

### Working with Records

??? note "create"
    To create a record, use salesforcerest.create and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_sobject_create.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object for which you will create a record.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>fieldAndValue</td>
            <td>The .json format property used to create the record. Include all mandatory fields according to the requirements for the specified sObject.</td>
            <td>Yes</td>
        </tr>
    </table>

    > **Note**: For example, if you are creating a record for the Account sObject, "name" is a mandatory parameter, and you might want to include the optional description, so the fieldAndValue property would look like this:
    > ```json
    > {
    >   "name":"wso2",
    >   "description":"This account belongs to WSO2"
    > }
    > ```

    **Sample configuration**

    ```xml
    <salesforcerest.create>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
        <fieldAndValue>{$ctx:fieldAndValue}</fieldAndValue>
    </salesforcerest.create>
    ```

    **Sample request**

    The following is a sample request that can be handled by the create operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "intervalTime" : "100000",
        "apiVersion": "v32.0",
        "sObjectName":"Account",
        "registryPath": "connectors/SalesforceRest",
        "fieldAndValue": {
            "name": "wso2",
            "description":"This Account belongs to WSO2"
        }
    }
    ```

    **Sample Response**

    Given below is a sample response for the create operation.

    ```json
    {
        "success":true,
        "id":"0010K00001uiAn8QAE",
        "errors":[

        ]
    }
    ```

??? note "createMultipleRecords"
    To create multiple records of the same sObject type, use salesforcerest.createMultipleRecords and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/dome_composite_sobject_tree_flat.htm#topic-title) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object for which you will create a record.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>fieldAndValue</td>
            <td>The .json format property, which specifies each record as an entry within the records array. Include all mandatory fields according to the requirements for the specified sObject.</td>
            <td>Yes</td>
        </tr>
    </table>

    > **Note**: For example, if you are creating a record for the Account sObject, "name" is a mandatory parameter, and you might want to include the optional description, so the fieldAndValue property would look like this:
    > ```json
    > {
    >   "records": [
    >   {
    >        "attributes": {"type": "Account", "referenceId": "ref1"},
    >        "name": "wso2",
    >        "phone": "1111111",
    >        "website": "www.salesforce1.com"
    >    },
    >    {
    >        "attributes": {"type": "Account", "referenceId": "ref2"},
    >        "name": "slwso2",
    >        "phone": "22222222",
    >        "website": "www.salesforce2.com"
    >    }]
    > }
    > ```

    **Sample configuration**

    ```xml
    <salesforcerest.createMultipleRecords>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
        <fieldAndValue>{$ctx:fieldAndValue}</fieldAndValue>
    </salesforcerest.createMultipleRecords>
    ```

    **Sample request**

    The following is a sample request that can be handled by the createMultipleRecords operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "sObjectName":"Account",
        "registryPath": "connectors/SalesforceRest",
        "fieldAndValue": {
        "records": [
            {
            "attributes": {"type": "Account", "referenceId": "ref1"},
            "name": "wso2",
            "phone": "1111111",
            "website": "www.salesforce1.com"
            },
            {
            "attributes": {"type": "Account", "referenceId": "ref2"},
            "name": "slwso2",
            "phone": "22222222",
            "website": "www.salesforce2.com"
            }]
        }
    }
    ```

    **Sample Response**

    Given below is a sample response for the createMultipleRecords operation.

    ```json
    {
        "hasErrors" : false,
        "results" : [{
        "referenceId" : "ref1",
        "id" : "001D000000K1YFjIAN"
        },{
        "referenceId" : "ref2",
        "id" : "001D000000K1YFkIAN"
        },{
        "referenceId" : "ref3",
        "id" : "001D000000K1YFlIAN"
        },{
        "referenceId" : "ref4",
        "id" : "001D000000K1YFmIAN"     
        }]
    }
    ```

??? note "createNestedRecords"
    To create nested records for a specific sObject, use salesforcerest.createNestedRecords and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/dome_composite_sobject_tree_create.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object for which you will create a record.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>fieldAndValue</td>
            <td>The .json format property, which specifies each record as an entry within the records array. Include all mandatory fields according to the requirements for the specified sobject.</td>
            <td>Yes</td>
        </tr>
    </table>

    > **Note**: For example, if you are creating records for the Account sObject, "name" is a mandatory parameter, and you might want to include additional optional values for each record, so the fieldAndValue property might look like this:
    > ```json
    > {
    >   "records" :[{
    >   "attributes" : {"type" : "Account", "referenceId" : "ref1"},
    >   "name" : "SampleAccount1",
    >   "phone" : "1234567890",
    >   "website" : "www.salesforce.com",
    >   "numberOfEmployees" : "100",
    >   "type" : "Analyst",
    >   "industry" : "Banking",
    >   "Contacts" : {
    >     "records" : [{
    >       "attributes" : {"type" : "Contact", "referenceId" : "ref2"},
    >       "lastname" : "Smith",
    >       "Title" : "President",
    >       "email" : "sample@salesforce.com"
    >     },{
    >       "attributes" : {"type" : "Contact", "referenceId" : "ref3"},
    >       "lastname" : "Evans",
    >       "title" : "Vice President",
    >       "email" : "sample@salesforce.com"
    >     }]
    >   }
    >   },{
    >   "attributes" : {"type" : "Account", "referenceId" : "ref4"},
    >   "name" : "SampleAccount2",
    >   "phone" : "1234567890",
    >   "website" : "www.salesforce.com",
    >   "numberOfEmployees" : "52000",
    >   "type" : "Analyst",
    >   "industry" : "Banking",
    >   "childAccounts" : {
    >     "records" : [{
    >       "attributes" : {"type" : "Account", "referenceId" : "ref5"},
    >       "name" : "SampleChildAccount1",
    >       "phone" : "1234567890",
    >       "website" : "www.salesforce.com",
    >       "numberOfEmployees" : "100",
    >       "type" : "Analyst",
    >       "industry" : "Banking"
    >     }]
    >   },
    >   "Contacts" : {
    >     "records" : [{
    >       "attributes" : {"type" : "Contact", "referenceId" : "ref6"},
    >       "lastname" : "Jones",
    >       "title" : "President",
    >       "email" : "sample@salesforce.com"
    >     }]
    >   }
    >   }]
    > }
    > ```

    **Sample configuration**

    ```xml
    <salesforcerest.createNestedRecords>
        <sObjectName>{$ctx:sObjectName}</sobject>
        <fieldAndValue>{$ctx:fieldAndValue}</fieldAndValue>
    </salesforcerest.createNestedRecords>
    ```

    **Sample request**

    The following is a sample request that can be handled by the createNestedRecords operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "intervalTime" : "100000",
        "apiVersion": "v32.0",
        "sObjectName":"Account",
        "registryPath": "connectors/SalesforceRest",
        "fieldAndValue":
        {
        "records" :[{
            "attributes" : {"type" : "Account", "referenceId" : "ref1"},
            "name" : "SampleAccount1",
            "phone" : "1234567890",
            "website" : "www.salesforce.com",
            "numberOfEmployees" : "100",
            "type" : "Analyst",
            "industry" : "Banking",
            "Contacts" : {
              "records" : [{
                "attributes" : {"type" : "Contact", "referenceId" : "ref2"},
                "lastname" : "Smith",
                "Title" : "President",
                "email" : "sample@salesforce.com"
              },{
                "attributes" : {"type" : "Account", "referenceId" : "ref3"},
                "lastname" : "Evans",
                "title" : "Vice President",
                "email" : "sample@salesforce.com"
            }]
            }
        },{
            "attributes" : {"type" : "Account", "referenceId" : "ref4"},
            "name" : "SampleAccount2",
            "phone" : "1234567890",
            "website" : "www.salesforce.com",
            "numberOfEmployees" : "52000",
            "type" : "Analyst",
            "industry" : "Banking",
            "childAccounts" : {
            "records" : [{
                "attributes" : {"type" : "Account", "referenceId" : "ref5"},
                "name" : "SampleChildAccount1",
                "phone" : "1234567890",
                "website" : "www.salesforce.com",
                "numberOfEmployees" : "100",
                "type" : "Analyst",
                "industry" : "Banking"
            }]
            },
            "Contacts" : {
            "records" : [{
                "attributes" : {"type" : "Contact", "referenceId" : "ref6"},
                "lastname" : "Jones",
                "title" : "President",
                "email" : "sample@salesforce.com"
            }]
            }
        }]
        }
    }
    ```

    **Sample Response**

    Given below is a sample response for the createNestedRecords operation.

    ```json
    {
        "hasErrors" : false,
        "results" : [{
        "referenceId" : "ref1",
        "id" : "001D000000K0fXOIAZ"
        },{
        "referenceId" : "ref4",
        "id" : "001D000000K0fXPIAZ"
        },{
        "referenceId" : "ref2",
        "id" : "003D000000QV9n2IAD"
        },{
        "referenceId" : "ref3",
        "id" : "003D000000QV9n3IAD"
        },{
        "referenceId" : "ref5",
        "id" : "001D000000K0fXQIAZ"
        },{
        "referenceId" : "ref6",
        "id" : "003D000000QV9n4IAD"
        }]
    }
    ```

??? note "update"
    To update a record, use salesforcerest.update and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_update_fields.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The type of object for which you will create a record.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>fieldAndValue</td>
            <td>The json format property with the new definition for the record.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>Id</td>
            <td>The ID of the record you are updating.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.update>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
        <fieldAndValue>{$ctx:fieldAndValue}</fieldAndValue>
        <Id>{$ctx:Id}</Id>
    </salesforcerest.update>
    ```

    **Sample request**

    The following is a sample request that can be handled by the update operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "intervalTime" : "100000",
        "apiVersion": "v32.0",
        "sObjectName":"Account",
        "Id":"00128000002OOhD",
        "registryPath": "connectors/SalesforceRest",
        "fieldAndValue": {
            "name": "wso2",
            "description":"This Account belongs to WSO2"
        }
    }
    ```

??? note "delete"
    To delete a record, use salesforcerest.delete and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_delete_record.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The object type of the record.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>Id</td>
            <td>The ID of the record you are deleting.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.update>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
        <fieldAndValue>{$ctx:fieldAndValue}</fieldAndValue>
        <Id>{$ctx:Id}</Id>
    </salesforcerest.update>
    ```

    **Sample request**

    The following is a sample request that can be handled by the delete operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "intervalTime" : "100000",
        "apiVersion": "v32.0",
        "sObjectName":"Account",
        "idToDelete":"00128000002OOhD",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

??? note "recentlyViewedItem"
    To retrieve the recently viewed items, use salesforcerest.recentlyViewedItem and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_recent_items.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>limit</td>
            <td>The maximum number of records to be returned.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.recentlyViewedItem>
        <limit>{$ctx:limit}</limit>
    </salesforcerest.recentlyViewedItem>
    ```

    **Sample request**

    The following is a sample request that can be handled by the recentlyViewedItem operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "intervalTime" : "100000",
        "apiVersion": "v32.0",
        "limit":"5",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the recentlyViewedItem operation.

    ```json
    {
        "output":"[{\"attributes\":
        {\"type\":\"User\",
        \"url\":\"/services/data/v32.0/sobjects/User/00528000000ToIrAAK\"},
        \"Id\":\"00528000000ToIrAAK\",
        \"Name\":\"kesan yoga\"},
        .
        .
        ]"
    }
    ```

??? note "retrieveFieldValues"
    To retrieve specific field values for a specific sObject, use salesforcerest.retrieveFieldValues and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_get_field_values.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The object type whose metadata you want to retrieve.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>rowId</td>
            <td>The ID of the record whose values you want to retrieve.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>fields</td>
            <td>A comma-separated list of fields whose values you want to retrieve.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.recentlyViewedItem>
        <limit>{$ctx:limit}</limit>
    </salesforcerest.recentlyViewedItem>
    ```

    **Sample request**

    The following is a sample request that can be handled by the retrieveFieldValues operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "intervalTime" : "100000",
        "apiVersion": "v32.0",
        "sObjectName": "Account",
        "rowId":"00128000005YjDnAAK",
        "fields":"AccountNumber,BillingPostalCode",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the retrieveFieldValues operation.

    ```json
    {
        "AccountNumber" : "CD656092",
        "BillingPostalCode" : "27215",
    }
    ```

??? note "upsert"
    To create or update (upsert) a record using an external ID, use salesforcerest.upsert and specify the following properties. This method is used to create records or update existing records based on the value of a specified external ID field. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_upsert.htm) for more information.
    ```
    * If the specified value does not exist, a new record is created.
    * If a record does exist with that value, the field values specified in the request body are updated.
    ```
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The object type whose value you want to upsert.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>externalIDField</td>
            <td>The external Id Field of the subject.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>Id</td>
            <td>The value of the customExtIdField.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>fieldAndValue</td>
            <td>The json format property/payload used to create the record.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.upsert>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
        <externalIDField>{$ctx:externalIDField}</externalIDField>
        <Id>{$ctx:Id}</Id>
        <fieldAndValue>{$ctx:fieldAndValue}</fieldAndValue>
    </salesforcerest.upsert>
    ```

    **Sample request**

    The following is a sample request that can be handled by the upsert operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQMMZWoN9MQZcXLW475YYoIdJFUICTjbGh67jEfAeV7Q57Ac2Ov.0ZuM_2Zx6SnrOmwpml8Qf.XclstTQiXtCYSGRBcEv",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_ztpnAk6BGQxRdovMLhHso81iyYKO6hTm68KfebpK7UYtEzF0ku8JCz7CNto8b3YMRmZrhy",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "sObjectName":"Account",
        "registryPath": "connectors/Salesforcerest",
        "intervalTime" : "2400000",
        "externalIDField":"sample__c",
        "Id":"15222",
        "fieldAndValue":
        {
            "Name":"john"
        }
    }
    ```

    **Sample Response**

    Given below is a sample response for the upsert operation.

    ```json
    {
        "id" : "00190000001pPvHAAU",
        "errors" : [ ],
        "success" : true
    }
    ```

### Working with sObjects

??? note "describeGlobal"
    To retrieve a list of the objects that are available in the system, use salesforcerest.describeGlobal. You can then get metadata for an object or objects as described in the next sections. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_describeGlobal.htm) for more information.

    **Sample configuration**

    ```xml
    <salesforcerest.describeGlobal/>
    ```

    **Sample request**

    The following is a sample request that can be handled by the describeGlobal operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the describeGlobal operation.

    ```json
    {
        "maxBatchSize":200,
        "sobjects":[
            {
                "updateable":false,
                "activateable":false,
                "deprecatedAndHidden":false,
                "layoutable":false,
                "custom":false,
                "deletable":false,
                "replicateable":false,
                "undeletable":false,
                "label":"Accepted Event Relation",
                "keyPrefix":null,
                "searchable":false,
                "queryable":true,
                "mergeable":false,
                "urls":{
                    "rowTemplate":"/services/data/v32.0/sobjects/AcceptedEventRelation/{ID}",
                    "describe":"/services/data/v32.0/sobjects/AcceptedEventRelation/describe",
                    "sobject":"/services/data/v32.0/sobjects/AcceptedEventRelation"
                },
                "createable":false,
                "feedEnabled":false,
                "retrieveable":true,
                "name":"AcceptedEventRelation",
                "customSetting":false,
                "labelPlural":"Accepted Event Relations",
                "triggerable":false
            },
            .
            .
        ],
        "encoding":"UTF-8"
    }
    ```

??? note "describeSObject"
    To get metadata (such as name, label, and fields, including the field properties) for a specific object type, use salesforcerest.describeSObject and specify the following properties. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_sobject_describe.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The object type whose metadata you want to retrieve.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.describeSObject>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
    </salesforcerest.describeSObject>
    ```

    **Sample request**

    The following is a sample request that can be handled by the describeSObject operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "sObjectName":"Account",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the describeSObject operation.

    ```json
    {
        "updateable":true,
        "activateable":false,
        "childRelationships":[
            {
                "relationshipName":"ChildAccounts",
                "field":"ParentId",
                "deprecatedAndHidden":false,
                "childSObject":"Account",
                "cascadeDelete":false,
                "restrictedDelete":false
            },
            {
                "relationshipName":"AccountCleanInfos",
                "field":"AccountId",
                "deprecatedAndHidden":false,
                "childSObject":"AccountCleanInfo",
                "cascadeDelete":true,
                "restrictedDelete":false
            },
            .
        ]
    }
    ```

??? note "listAvailableApiVersion"
    To retrieve a list of summary information about each REST API version that is currently available, use salesforcerest.listAvailableApiVersion. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/dome_versions.htm) for more information.

    **Sample configuration**

    ```xml
    <salesforcerest.listAvailableApiVersion/>
    ```

    **Sample request**

    The following is a sample request that can be handled by the listAvailableApiVersion operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the listAvailableApiVersion operation.

    ```json
    {
        "output":"[
        {\"label\":\"Winter '11\",\"url\":\"/services/data/v20.0\",\"version\":\"20.0\"},
        .
        .
        ]"
    }
    ```

??? note "listOrganizationLimits"
    To retrieve the limit information for your organization, use salesforcerest.listOrganizationLimits. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.198.0.api_rest.meta/api_rest/dome_limits.htm) for more information.

    **Sample configuration**

    ```xml
    <salesforcerest.listOrganizationLimits/>
    ```

    **Sample request**

    The following is a sample request that can be handled by the listOrganizationLimits operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the listOrganizationLimits operation.

    ```json
    {
        "DailyApiRequests":{
            "Dataloader Bulk":{
                "Max":0,
                "Remaining":0
            },
            "test":{
                "Max":0,
                "Remaining":0
            },
            "Max":5000,
            "Salesforce Mobile Dashboards":{
                "Max":0,
                "Remaining":0
            },
        .
        .
    }
    ```

??? note "listResourcesByApiVersion"
    To retrieve the resources that are available in the specified API version, use salesforcerest.listResourcesByApiVersion. You can then get the details of those resources. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_discoveryresource.htm) for more information.

    **Sample configuration**

    ```xml
    <salesforcerest.listResourcesByApiVersion/>
    ```

    **Sample request**

    The following is a sample request that can be handled by the listResourcesByApiVersion operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the listResourcesByApiVersion operation.

    ```json
    {
        "tooling":"/services/data/v32.0/tooling",
        "folders":"/services/data/v32.0/folders",
        "eclair":"/services/data/v32.0/eclair",
        "prechatForms":"/services/data/v32.0/prechatForms",
        "chatter":"/services/data/v32.0/chatter",
        "tabs":"/services/data/v32.0/tabs",
        "appMenu":"/services/data/v32.0/appMenu",
        "quickActions":"/services/data/v32.0/quickActions",
        "queryAll":"/services/data/v32.0/queryAll",
        "commerce":"/services/data/v32.0/commerce",
        .
    }
    ```

??? note "sObjectBasicInfo"
    To retrieve the individual metadata for the specified object, use salesforcerest.sObjectBasicInfo. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_sobject_basic_info.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The object type whose metadata you want to retrieve.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.sObjectBasicInfo>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
    </salesforcerest.sObjectBasicInfo>
    ```

    **Sample request**

    The following is a sample request that can be handled by the sObjectBasicInfo operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "intervalTime" : "100000",
        "sObjectName":"Account",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the sObjectBasicInfo operation.

    ```json
    {
        "objectDescribe":{
            "updateable":true,
            "activateable":false,
            "deprecatedAndHidden":false,
            "layoutable":true,
            "custom":false,
            "deletable":true,
            "replicateable":true,
            "undeletable":true,
            "label":"Account",
            "keyPrefix":"001",
            "searchable":true,
            "queryable":true,
            "mergeable":true,
            "urls":{
                "compactLayouts":"/services/data/v32.0/sobjects/Account/describe/compactLayouts",
                "rowTemplate":"/services/data/v32.0/sobjects/Account/{ID}"
            },
            "createable":true,
            "feedEnabled":true,
            "retrieveable":true,
            "name":"Account",
            "customSetting":false,
            "labelPlural":"Accounts",
            "triggerable":true
        },
        .
    }
    ```

??? note "sObjectGetDeleted"
    To retrieve a list of individual records that have been deleted within the given timespan for the specified object, use salesforcerest.sObjectGetDeleted. The date and time should be provided in ISO 8601 format:YYYY-MM-DDThh:mm:ss+hh:mm. See the [related API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_sobject_basic_info.htm) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>sObjectName</td>
            <td>The object type whose metadata you want to retrieve.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>startTime</td>
            <td>Starting date/time (Coordinated Universal Time (UTC)—not local—timezone) of the timespan for which to retrieve the data.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>endTime</td>
            <td>Ending date/time (Coordinated Universal Time (UTC)—not local—timezone) of the timespan for which to retrieve the data.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <salesforcerest.sObjectGetDeleted>
        <sObjectName>{$ctx:sObjectName}</sObjectName>
        <startTime>{$ctx:startTime}</startTime>
        <endTime>{$ctx:endTime}</endTime>
    </salesforcerest.sObjectGetDeleted>
    ```

    **Sample request**

    The following is a sample request that can be handled by the sObjectGetDeleted operation.

    ```json
    {
        "accessToken":"00D280000017q6q!AQoAQOeXcp7zKo3gUdy6r064_LsJ5bYYrUn_qAZG9TtKFLPfUMRxiato.E162_2XAtCTZLFQTbNk2Rz6Zm_juSakFE_aaBPp",
        "apiUrl":"https://ap2.salesforce.com",
        "clientId": "3MVG9ZL0ppGP5UrBrnsanGUZRgHqc8gTV4t_6tfuef8Zz4LhFPipmlooU6GBszpplbTzVXXWjqkGHubhRip1s",
        "refreshToken": "5Aep861TSESvWeug_xvFHRBTTbf_YrTWgEyjBJo7Xr34yOQ7GCFUN5DnNPxzDIoGoWi4evqOl_lT1B9nE5dAtSb",
        "clientSecret": "9104967092887676680",
        "hostName": "https://login.salesforce.com",
        "apiVersion": "v32.0",
        "sObjectName":"Account",
        "intervalTime" : "100000",
        "startTime":"2015-10-05T12:30:30+05:30",
        "endTime":"2015-10-10T20:30:30+05:30",
        "registryPath": "connectors/SalesforceRest"
    }
    ```

    **Sample Response**

    Given below is a sample response for the sObjectGetDeleted operation.

    ```json
    {
        "objectDescribe":{
            "updateable":true,
            "activateable":false,
            "deprecatedAndHidden":false,
            "layoutable":true,
            "custom":false,
            "deletable":true,
            "replicateable":true,
            "undeletable":true,
            "label":"Account",
            "keyPrefix":"001",
            "searchable":true,
            "queryable":true,
            "mergeable":true,
            "urls":{
                "compactLayouts":"/services/data/v32.0/sobjects/Account/describe/compactLayouts",
                "rowTemplate":"/services/data/v32.0/sobjects/Account/{ID}"
            },
            "createable":true,
            "feedEnabled":true,
            "retrieveable":true,
            "name":"Account",
            "customSetting":false,
            "labelPlural":"Accounts",
            "triggerable":true
        },
        .
    }
    ```