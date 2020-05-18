!!! note
    **This page is still a work in progress!**

# Streaming Integrator REST API Guide

The following sections cover the different categories of REST API available for the Streaming Integrator the HTTP status codes thyat they can return.

### Siddhi application management APIs

#### Creating a Siddhi application

#### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Creates a new Siddhi Application.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/siddhi-apps</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>POST</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><strong>Request</strong> : text/plain<br />
<strong>Response</strong> : application/json</td>
</tr>
<tr class="odd">
<th>Authentication</th>
<td>Basic</td>
</tr>
<tr class="even">
<th>Username</th>
<td>admin</td>
</tr>
<tr class="odd">
<th>Password</th>
<td>admin</td>
</tr>
<tr class="even">
<th>Runtime</th>
<td>server/tooling</td>
</tr>
</tbody>
</table>

#### curl command syntax

``` java
    curl -X POST "https://localhost:9443/siddhi-apps" -H "accept: application/json" -H "Content-Type: text/plain" -d @TestSiddhiApp.siddhi -u admin:admin -k
```

#### Sample curl command

``` java
    curl -X POST "https://localhost:9443/siddhi-apps" -H "accept: application/json" -H "Content-Type: text/plain" -d @TestSiddhiApp.siddhi -u admin:admin -k
```

#### Sample output

The response for the sample curl command given above can be one of the
following.

-   If API request is valid and there is no existing Siddhi application
    with the given name, a response similar to the following is
    generated with response code 201. This response contains a location
    header with the path of the newly created file from product root
    home.

    ``` java
    ```

-   If the API request is valid, but a Siddhi application with the given
    name already exists,  a response similar to the following is
    generated with response code 409.

    ``` java
            {
              "type": "conflict",
              "message": "There is a Siddhi App already exists with same name"
            }
    ```

-   If the API request is invalid due to invalid content in the Siddhi
    queries you have included in the request body,  a response similar
    to the following is generated is generated with response code 400.

    ``` java
            {
              "code": 800101,
              "type": "validation error",
              "message": "You have an error in your SiddhiQL at line 8:8, missing INTO at 'BarStream'"
            }
    ```

-   If the API request is valid, but an exception occured during file
    processing or saving, the following response is generated with
    response code 500.

    ``` java
            {
              "code": 800102,
              "type": "file processing error",
              "message": <error-message>
            }
    ```

#### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 201, 409, 400, and 500.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

### Updating a Siddhi Application

#### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Updates a Siddhi Application.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>             /siddhi-apps            </code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>PUT</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><strong>Request</strong> : text/plain<br />
<strong>Response</strong> : application/json</td>
</tr>
<tr class="odd">
<th>Authentication</th>
<td>Basic</td>
</tr>
<tr class="even">
<th>Username</th>
<td>admin</td>
</tr>
<tr class="odd">
<th>Password</th>
<td>admin</td>
</tr>
<tr class="even">
<th>Runtime</th>
<td>server/tooling</td>
</tr>
</tbody>
</table>

#### curl command syntax

``` java
    curl -X PUT "http://localhost:9090/siddhi-apps" -H "accept: application/json" -H "Content-Type: text/plain" -d @<SIDDHI_APPLICATION_NAME>.siddhi -u admin:admin -k
```

#### Sample curl command

``` java
    curl -X PUT "https://localhost:9443/siddhi-apps" -H "accept: application/json" -H "Content-Type: text/plain" -d @TestSiddhiApp.siddhi -u admin:admin -k
```

#### Sample output

-   If the API request is invalid due to invalid content in the Siddhi
    query, a response similar to the following is returned with response
    code 400.

    ``` java
            {
              "code": 800101,
              "type": "validation error",
              "message": "You have an error in your SiddhiQL at line 8:8, missing INTO at 'BarStream'"
            }
    ```

-   If the API request is valid, but an exception occured when saving or
    processing files, a response similar to the following is returned
    with response code 500.

    ``` java
            {
              "code": 800102,
              "type": "file processing error",
              "message": <error-message>
            }
    ```

#### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200, 201, 400, and 500.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>



### Deleting a Siddhi application

#### Overview

<table>
    <tr>
        <th>Description</th>
        <td>Sends the name of a Siddhi application as a URL parameter.</td>
    </tr>
    <tr>
        <th>API Context</th>
        <td><code>/siddhi-apps/{appName}</code></td>
    </tr>
    <tr>
        <th>HTTP Method </th>
        <td><code>DELETE</code></td>
    </tr>
    <tr>
        <th>Request/Response format</th>
        <td>application/json</td>
    </tr>
    <tr>
        <th>Authentication</th>
        <td>Basic</td>
    </tr>
    <tr>
        <th>Username</th>
        <td>admin</td>
    </tr>
    <tr>
        <th>Password</th>
        <td>admin</td>
    </tr>
    <tr>
        <th>Runtime</th>
        <td>server/tooling</td>
    </tr>
</table>

##### Parameter Description

| Parameter | Description                                       |
|-----------|---------------------------------------------------|
|`{appName}`| The name of the Siddhi application to be deleted. |



#### curl command syntax

``` java
    curl -X DELETE "http://localhost:9090/siddhi-apps/{app-name}" -H "accept: application/json" -u admin:admin -k
```

#### Sample curl command

``` java
    curl -X DELETE "https://localhost:9443/siddhi-apps/TestSiddhiApp" -H "accept: application/json" -u admin:admin -k
```

#### Sample output

The response for the sample curl command given above can be one of the
following:

-   If the API request is valid and a Siddhi application with the given
    name exists, the following response is received with response
    code 200.

    ``` java
            http://localhost:9090/siddhi-apps/TestExecutionPlan1
    ```

-   If the API request is valid, but a Siddhi application with the given
    name is not deployed, the following response is received with
    response code 404.

    ``` java
            {
              "type": "not found",
              "message": "There is no Siddhi App exist with provided name : TestExecutionPlan1"
            }
    ```

-   If the API request is valid, but an exception occured when deleting
    the given Siddhi application, the following response is received
    with response code 500.

    ``` java
            {
              "code": 800102,
              "type": "file processing error",
              "message": <error-message>
            }
    ```

-   If the API request is valid, but there are restricted characters in
    the given Siddhi application name, the following response is
    received with response code 400.

    ``` java
            {
              "code": 800101,
              "type": "validation error",
              "message": "File name contains restricted path elements . : ../../siddhiApp2'"
            }
    ```

#### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>200, 404, 500 or 400.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

### Listing all active Siddhi applications

#### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td><p>Lists all the currently active Siddhi applications.</p>
<p>If the <code>isActive=true</code> parameter is set, all the active Siddhi Applications are listed. If not, all the inactive Siddhi applications are listed.</p></td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/siddhi-apps</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>GET</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><strong>Request content type</strong> : any<br />
<strong>Response content type</strong> : <code>application/json</code></td>
</tr>
<tr class="odd">
<th>Authentication</th>
<td>Basic</td>
</tr>
<tr class="even">
<th>Username</th>
<td>admin</td>
</tr>
<tr class="odd">
<th>Password</th>
<td>admin</td>
</tr>
<tr class="even">
<th>Runtime</th>
<td>server/tooling</td>
</tr>
</tbody>
</table>

#### curl command syntax

``` java
    curl -X GET "http://localhost:9090/siddhi-apps" -H "accept: application/json" -u admin:admin -k
```

#### Sample curl command

``` java
    curl -X GET "https://localhost:9443/siddhi-apps?isActive=true" -H "accept: application/json" -u admin:admin -k
```

#### Sample output

Possible responses are as follows:

-   If the API request is valid and there are Siddhi applications
    deployed in your SP setup, a response similar to the following is
    returned with response code 200.

    ``` java
            ["TestExecutionPlan3", "TestExecutionPlan4"]
    ```

-   If the API request is valid, there are Siddhi applications deployed
    in your SP setup, and a query parameter is defined in the request, a
    response similar to the following is returned with response
    code 200. This response only contains Siddhi applications that are
    active.

        !!! info
            If these conditions are met, but the `isActive`
            parameter is set to `false` , the response
            contains only inactive Siddhi applications.


    ``` java
        ["TestExecutionPlan3"]
    ```

-   If the API request is valid, but there are no Siddhi applications
    deployed in your SP setup, the following response is returned.

    ``` java
            []
    ```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

### Retrieving a specific Siddhi application

#### Overview

|                         |                                                   |
|-------------------------|---------------------------------------------------|
| Description             | Retrieves the given Siddhi application.           |
| API Context             | `             /siddhi-apps/{appName}            ` |
| HTTP Method             | GET                                               |
| Request/Response format | application/json                                  |
| Authentication          | Basic                                             |
| Username                | admin                                             |
| Password                | admin                                             |
| Runtime                 | server/tooling                                    |



##### Parameter Description

| Parameter                            | Description                                         |
|--------------------------------------|-----------------------------------------------------|
| `             {appName}            ` | The name of the Siddhi application to be retrieved. |



#### curl command syntax

``` java
    curl -X GET "http://localhost:9090/siddhi-apps/{app-name}" -H "accept: application/json" -u admin:admin -k
```

#### Sample curl command

``` java
    curl -X GET "https://localhost:9443/siddhi-apps/SiddhiTestApp" -H "accept: application/json" -u admin:admin -k
```

#### Sample output

The possible outputs are as follows:

-   If the API request is valid and a Siddhi application of the given
    name exists, a response similar to the following is returned with
    response code 200.

    ``` java
            {
              "content": "\n@Plan:name('TestExecutionPlan')\ndefine stream FooStream (symbol string, price float, volume long);\n\n@source(type='inMemory', topic='symbol', @map(type='passThrough'))Define stream BarStream (symbol string, price float, volume long);\n\nfrom FooStream\nselect symbol, price, volume\ninsert into BarStream;"
            }
    ```

-   If the API request is valid, but a Siddhi application of the given
    name is not deployed, a response similar to the following is
    returned with response code 404.

    ``` java
            {
              "type": "not found",
              "message": "There is no Siddhi App exist with provided name : TestExecutionPlan1"
            }
    ```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

### Fetching the status of a Siddhi Application

#### Overview

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 66%" />
</colgroup>
<tbody>
<tr class="odd">
<td>Description</td>
<td>This fetches the status of the specified Siddhi application</td>
</tr>
<tr class="even">
<td>API Context</td>
<td><code>             /siddhi-apps/{appName}/status            </code></td>
</tr>
<tr class="odd">
<td>HTTP Method</td>
<td>GET</td>
</tr>
<tr class="even">
<td>Request/Response format</td>
<td>application/json</td>
</tr>
<tr class="odd">
<td>Authentication</td>
<td>Basic</td>
</tr>
<tr class="even">
<td>Username</td>
<td>admin</td>
</tr>
<tr class="odd">
<td>Password</td>
<td>admin</td>
</tr>
<tr class="even">
<td>Runtime</td>
<td>server/tooling</td>
</tr>
</tbody>
</table>



##### Parameter Description

| Parameter                            | Description                                                                 |
|--------------------------------------|-----------------------------------------------------------------------------|
| `             {appName}            ` | The name of the Siddhi application of which the status needs to be fetched. |



#### curl command syntax

``` java
    curl -X GET "http://localhost:9090/siddhi-apps/{app-file-name}/status" -H "accept: application/json" -u admin:admin -k
```

#### Sample curl command

``` java
    curl -X GET "https://localhost:9443/siddhi-apps/TestSiddhiApp/status" -H "accept: application/json" -u admin:admin -k
```

#### Sample output

-   If the Siddhi application is active, the following is returned with
    response code 200.

    ``` java
            {"status":"active"}
    ```

-   If the Siddhi application is inactive, the following is returned
    with response code 200.

    ``` java
            {"status":"inactive"}
    ```

-   If the Siddhi application does not exist, but the REST API call is
    valid, the following is returned with the response code 404.

    ``` java
            {
              "type": "not found",
              "message": "There is no Siddhi App exist with provided name : TestExecutionPlan1"
            }
    ```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

### Taking a snapshot of a Siddhi Application

#### Overview

|                         |                                                           |
|-------------------------|-----------------------------------------------------------|
| Description             | This takes a snapshot of the specific Siddhi application. |
| API Context             | `             /siddhi-apps/{appName}/backup            `  |
| HTTP Method             | POST                                                      |
| Request/Response format | application/json                                          |
| Authentication          | Basic                                                     |
| Username                | admin                                                     |
| Password                | admin                                                     |
| Runtime                 | server/tooling                                           |



##### Parameter Description

| Parameter                            | Description                                                               |
|--------------------------------------|---------------------------------------------------------------------------|
| `             {appName}            ` | The name of the Siddhi application of which a snapshot needs to be taken. |



#### curl command syntax

``` java
    curl -X POST "http://localhost:9090/siddhi-apps/{appName}/backup" -H "accept: application/json" -u admin:admin -k
```

#### Sample curl command

``` java
    curl -X POST "https://localhost:9443/siddhi-apps/TestSiddhiApp/backup" -H "accept: application/json" -u admin:admin -k
```

#### Sample output

The output can be one of the following:

-   If the API request is valid and a Siddhi application exists with the
    given name, an output similar to the following (i.e., with the
    snapshot revision number) is returned with response code 201.

    ``` java
            {"revision": "89489242494242"}
    ```

-   If the API request is valid, but no Siddhi application with the
    given name is deployed, an output similar to the following is
    returned with response code 404.

    ``` java
            {
              "type": "not found",
              "message": "There is no Siddhi App exist with provided name : TestExecutionPlan1"
            }
    ```

-   If the API request is valid, but an exception has occured when
    backing up the state at Siddhi level, an output similar to the
    following is returned with response code 500.

    ``` java
            {
              "code": 800102,
              "type": "file processing error",
              "message": <error-message>
            }
    ```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>201, 404, or 500.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a>.</p></td>
</tr>
</tbody>
</table>

### Restoring a Siddhi Application via a snapshot

!!! info
    In order to call this API, you need tohave already taken a snapshot of
    the Siddhi application to be restored. For more information about the
    API via which the snapshow is taken, see [Taking a snapshot of a Siddhi application](#SiddhiApplicationManagementAPIs-Snapshot).


#### Overview

<table>
<tbody>
<tr class="odd">
<td><p>Description</p></td>
<td><p>This restores a Siddhi application using a snapshot of the same that you have previously taken.</p></td>
</tr>
<tr class="even">
<td><p>API Context</p></td>
<td><ul>
<li><strong>To restore without considering the version</strong> : <code>               /siddhi-apps/{appName}/restore              </code></li>
<li><strong>To restore a specific version</strong> : <code>               /siddhi-apps/{appName}/restore?version=              </code></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>HTTP Method</p></td>
<td>POST</td>
</tr>
<tr class="even">
<td><p>Request/Response format</p></td>
<td>application/json</td>
</tr>
<tr class="odd">
<td><p>Authentication</p></td>
<td>Basic</td>
</tr>
<tr class="even">
<td><p>Username</p></td>
<td>admin</td>
</tr>
<tr class="odd">
<td><p>Password</p></td>
<td>admin</td>
</tr>
<tr class="even">
<td><p>Runtime</p></td>
<td><p>server/tooling</p></td>
</tr>
</tbody>
</table>

##### Parameter Description

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>{appName}</code></pre></td>
<td>The name of the Siddhi application that needs to be restored.</td>
</tr>
</tbody>
</table>



#### curl command syntax

``` java
    curl -X POST "http://localhost:9090/siddhi-apps/{appName}/restore" -H "accept: application/json" -u admin:admin -k
```

#### Sample curl command

``` java
    curl -X POST "https://localhost:9443/siddhi-apps/TestSiddhiApp/restore?revision=1514981290838_TestSiddhiApp" -H "accept: application/json" -u admin:admin -k
```

#### Sample output

The above sample curl command can generate either one of the following
responses:

-   If the API request is valid, a Siddhi application with the given
    name exists, and no revision information is passed as a query
    parameter, the following response is returned with response
    code 200.

    ``` java
            {
              "type": "success",
              "message": "State restored to last revision for Siddhi App :TestExecutionPlan"
            }
    ```

-   If the API request is valid, a Siddhi application with the given
    name exists, and revision information is passed as a query
    parameter, the following response is returned with response
    code 200. In this scenario, the Siddhi snapshot is created in the
    file system.

    ``` java
            {
              "type": "success",
              "message": "State restored to revision 1234563 for Siddhi App :TestExecutionPlan"
            }
    ```

-   If the API request is valid, but no Siddhi application is deployed
    with the given name, the following response is returned with
    response code 404.

    ``` java
            {
              "type": "not found",
              "message": "There is no Siddhi App exist with provided name : TestExecutionPlan1"
            }
    ```

-   If the API request is valid, but an exception occured when restoring
    the state at Siddhi level, the following response is returned with
    response code 500.

    ``` java
            {
              "code": 800102,
              "type": "file processing error",
              "message": <error-message>
            }
    ```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200, 404 or 500.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a>.</p></td>
</tr>
</tbody>
</table>

### Returning real-time statistics of a Streaming Integrator node

#### Overview

|                         |                                               |
|-------------------------|-----------------------------------------------|
| Description             | Returns the real-time statistics of a Streaming Integrator node. |
| API Context             | `             /statistics            `        |
| HTTP Method             | GET                                           |
| Request/Response format | application/json                              |
| Authentication          | Basic                                         |
| Username                | admin                                         |
| Password                | admin                                         |
| Runtime                 | server/tooling                               |



##### Parameter Description

#### curl command syntax

``` java
```

#### Sample curl command

``` java
    curl -X GET "https://localhost:9443/statistics" -H "accept: application/json" -u admin:admin -k
```

#### Sample output

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a>.</p></td>
</tr>
</tbody>
</table>

### Enabling/disabling statistics for Streaming Integrator nodes

#### Overview

|                         |                                                         |
|-------------------------|---------------------------------------------------------|
| Description             | Enables/diables generating statistics for Streaming Integrator nodes. |
| API Context             | `             /statistics            `                  |
| HTTP Method             | PUT                                                     |
| Request/Response format | application/json                                        |
| Authentication          | Basic                                                   |
| Username                | admin                                                   |
| Password                | admin                                                   |
| Runtime                 | server/tooling                                         |



##### Parameter Description

#### curl command syntax

``` java
```

#### Sample curl command

``` java
    curl -X PUT "https://localhost:9443/statistics" -H "accept: application/json" -H "Content-Type: application/json" -d "{“statsEnable”:”true”}" -u admin:admin -k
```

#### Sample output

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a>.</p></td>
</tr>
</tbody>
</table>

### Returning general details of a Streaming Integrator node

#### Overview

|                         |                                            |
|-------------------------|--------------------------------------------|
| Description             | Returns general details of a Streaming Integrator node.       |
| API Context             | `/system-details` |
| HTTP Method             | GET                                        |
| Request/Response format | application/json                           |
| Authentication          | Basic                                      |
| Username                | admin                                      |
| Password                | admin                                      |
| Runtime                 | server/tooling                             |



##### Parameter Description

#### curl command syntax

``` java
```

#### Sample curl command

``` java
    curl -X GET "https://localhost:9443/system-details" -H "accept: application/json" -u admin:admin -k
```

#### Sample output

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a>.</p></td>
</tr>
</tbody>
</table>

### Returning detailed statistics of all Siddhi applications

#### Overview

|                         |                                                                                                    |
|-------------------------|----------------------------------------------------------------------------------------------------|
| Description             | Returns the detailed statistics of all the Siddhi applications currently deployed in the SP setup. |
| API Context             | `             /siddhi-apps/statistics            `                                                 |
| HTTP Method             | GET                                                                                                |
| Request/Response format | application/json                                                                                   |
| Authentication          | Basic                                                                                              |
| Username                | admin                                                                                              |
| Password                | admin                                                                                              |
| Runtime                 | server/tooling                                                                                    |



##### Parameter Description

#### curl command syntax

``` java
```

#### Sample curl command

``` java
    curl -X GET "https://localhost:9443/siddhi-apps/statistics" -H "accept: application/json" -u admin:admin -k
```

#### Sample output

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a>.</p></td>
</tr>
</tbody>
</table>

### Enabling/disabling the statistics of a specific Siddhi application

#### Overview

|                         |                                                                 |
|-------------------------|-----------------------------------------------------------------|
| Description             | Enables/disables statistics for a specified Siddhi application. |
| API Context             | `             /siddhi-apps/{appName}/statistics            `    |
| HTTP Method             | PUT                                                             |
| Request/Response format | application/json                                                |
| Authentication          | Basic                                                           |
| Username                | admin                                                           |
| Password                | admin                                                           |
| Runtime                 | server/tooling                                                 |



##### Parameter Description

| Parameter                          | Description                                                                                       |
|------------------------------------|---------------------------------------------------------------------------------------------------|
| `             appName            ` | The name of the Siddhi application for which the Siddhi applications need to be enabled/disabled. |



#### curl command syntax

``` java
```

#### Sample curl command

``` java
    curl -X PUT "https://localhost:9443/siddhi-apps/TestSiddhiApp/statistics" -H "accept: application/json" -H "Content-Type: application/json" -d "{“statsEnable”:”true”}" -u admin:admin -k
```

#### Sample output

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a>.</p></td>
</tr>
</tbody>
</table>

### Enabling/disabling the statistics of all Siddhi applications

#### Overview

|                         |                                                              |
|-------------------------|--------------------------------------------------------------|
| Description             | Enables/disables statistics for all the Siddhi applications. |
| API Context             | `             /siddhi-apps/statistics            `           |
| HTTP Method             | PUT                                                          |
| Request/Response format | application/json                                             |
| Authentication          | Basic                                                        |
| Username                | admin                                                        |
| Password                | admin                                                        |
| Runtime                 | server/tooling                                              |



##### Parameter Description

#### curl command syntax

``` java
```

#### Sample curl command

``` java
    curl -X PUT "https://localhost:9443/siddhi-apps/statistics" -H "accept: application/json" -H "Content-Type: application/json" -d "{“statsEnable”:”true”}" -u admin:admin -k
```

#### Sample output

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a>.</p></td>
</tr>
</tbody>
</table>


## Event Simulation APIs

## Status Monitoring APIs

## Dashboard APIs

## Authentication APIs

### Log in to a dashboard application

#### Overview

<table>
<tbody>
<tr class="odd">
<td>Overview</td>
<td>Logs in to the apps in dashboard runtime such as portal, monitoring or business-rules app.</td>
</tr>
<tr class="even">
<td>API Context</td>
<td><code>             /login/{appName}            </code></td>
</tr>
<tr class="odd">
<td>HTTP Method</td>
<td><code>             POST            </code></td>
</tr>
<tr class="even">
<td>Request/Response Format</td>
<td><pre><code>application/x-www-form-urlencoded</code></pre></td>
</tr>
<tr class="odd">
<td>Runtime</td>
<td>Dashboard</td>
</tr>
</tbody>
</table>

##### Parameter description

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Type</th>
<th>Description</th>
<th>Possible Values</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             appName            </code></td>
<td>Path param</td>
<td>The application to which you need to log in.</td>
<td>portal/monitoring/business-rules</td>
</tr>
<tr class="even">
<td>username</td>
<td>Body param</td>
<td>Username for the login</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td>password</td>
<td>Body param</td>
<td>Password for the login</td>
<td><br />
</td>
</tr>
<tr class="even">
<td>grantType</td>
<td>Body param</td>
<td>Grant type used for the login</td>
<td>password/
<pre><code>          refresh_token
        </code></pre>
<pre><code>

        </code></pre>
<pre><code>          authorization_code
        </code></pre></td>
</tr>
<tr class="odd">
<td><pre><code>rememberMe</code></pre></td>
<td>Body param</td>
<td>Whether remember me function enabled</td>
<td>false/true</td>
</tr>
</tbody>
</table>



#### curl command syntax

``` java
    curl -X POST "https://analytics.wso2.com/login/{appName}" -H "accept: application/json" -H "Content-Type: application/x-www-form-urlencoded" -d "username={username}&password={password}&grantType={grantTypr}&rememberMe={rememberMe}"
```



#### Sample curl command

``` java
    curl -X POST "https://localhost:9643/login/portal"
    -H "Content-Type: application/x-www-form-urlencoded" -d "username=admin&password=admin&grantType=password"
```

#### Sample output

``` java
    {"authUser":"admin","pID":"71368eff-cc71-44ef","lID":"a60c1098-3de0-42fb","validityPeriod":3600}
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

### Log out of the dashboard application

### Overview

|                         |                                              |
|-------------------------|----------------------------------------------|
| Overview                | Logs out of the dashboard application.       |
| API Context             | `             /logout/{appName}            ` |
| HTTP Method             | `             POST            `              |
| Request/Response Format | `             application/json            `  |
| Runtime                 | Dashboard                                    |



#### curl command syntax

``` java
    curl -X POST "https://analytics.wso2.com/logout/{appName}" -H "accept: application/json" -H "Authorzation: Bearer {access token}"
```

####  Sample curl command

``` java
    curl -X POST "https://analytics.wso2.com/logout/portal" -H "accept: application/json" -H "Authorzation: Bearer 123456"
```

#### Sample output

``` java
    N/A
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>



### Redirect URL for login using authorization grant type

#### Overview

|                         |                                                               |
|-------------------------|---------------------------------------------------------------|
| Overview                | Redirects URL by the IS in authorization grant type - OAuth2. |
| API Context             | `             /login/callback/{appName}            `          |
| HTTP Method             | `             GET            `                                |
| Request/Response Format | JSON                                                          |
| Runtime                 | Dashbaord                                                     |

##### Parameter description

| Parameter                            | Description                                              |
|--------------------------------------|----------------------------------------------------------|
| `             {appName}            ` | The application of which the URL needs to be redirected. |



#### curl command syntax

``` java
```



#### Sample curl command

``` java
    curl -X GET "https://localhost:9643/login/callback/portal"
```

#### Sample output

``` java
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Permission APIs

## Business Rules APIs

### Lists available business rule instances

#### Overview

<table>
<tbody>
<tr class="odd">
<td>Description</td>
<td>Returns the list of business rule instances that are currently available.</td>
</tr>
<tr class="even">
<td>API Context</td>
<td><code>             /business-rules/instances            </code></td>
</tr>
<tr class="odd">
<td>HTTP Method</td>
<td><code>             GET            </code></td>
</tr>
<tr class="even">
<td>Request/Response Format</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td>Authentication</td>
<td>Basic</td>
</tr>
<tr class="even">
<td>Username</td>
<td><code>             admin            </code></td>
</tr>
<tr class="odd">
<td>Password</td>
<td><code>             admin            </code></td>
</tr>
<tr class="even">
<td>Runtime</td>
<td>Dashboard</td>
</tr>
</tbody>
</table>

#### curl command syntax

``` java
```



#### Sample curl command

``` java
    curl -X GET "https://localhost:9643/business-rules/instances" -u admin:admin -k
```

#### Sample output

``` java
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

### Delete business rule with given UUID

#### Overview

|                         |                                                                                                  |
|-------------------------|--------------------------------------------------------------------------------------------------|
| Description             | Deletes the business rule with the given UUID.                                                   |
| API Context             | `             /business-rules/instances/{businessRuleInstanceID}?force-delete=false            ` |
| HTTP Method             | `             DELETE            `                                                                |
| Request/Response Format | `             application/json            `                                                      |
| Authentication          | Basic                                                                                            |
| Username                | `             admin            `                                                                 |
| Password                | `             admin            `                                                                 |
| Runtime                 | Dashboard                                                                                        |

##### Parameter description

| Parameter                                           | Description                                                                       |
|-----------------------------------------------------|-----------------------------------------------------------------------------------|
| `             {businessRuleInstanceID}            ` | The UUID (Uniquely Identifiable ID) of the business rules instance to be deleted. |

#### curl command syntax

``` java
```



#### Sample curl command

``` java
    curl -X DELETE "https://localhost:9643/business-rules/instances/business-rule-1?force-delete=false" -H "accept: application/json" -u admin:adm
```

#### Sample output

``` java
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

### Fetch template group with the given UUID

#### Overview

<table>
<tbody>
<tr class="odd">
<td>Description</td>
<td>Returns the template group that has the given UUID.</td>
</tr>
<tr class="even">
<td>API Context</td>
<td><code>             /business-rules/template-groups/{templateGroupID}            </code></td>
</tr>
<tr class="odd">
<td>HTTP Method</td>
<td><code>             GET            </code></td>
</tr>
<tr class="even">
<td>Request/Response Format</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td>Authentication</td>
<td>Basic</td>
</tr>
<tr class="even">
<td>Username</td>
<td><code>             admin            </code></td>
</tr>
<tr class="odd">
<td>Password</td>
<td><code>             admin            </code></td>
</tr>
<tr class="even">
<td>Runtime</td>
<td>Dashboard</td>
</tr>
</tbody>
</table>

##### Parameter description

| Parameter                                    | Description                                   |
|----------------------------------------------|-----------------------------------------------|
| `             {templateGroupID}            ` | The UUID of the template group to be fetched. |

#### curl command syntax

``` java
```



#### Sample curl command

``` java
    curl -X GET "https://localhost:9643/business-rules/template-groups/sweet-factory" -u admin:admin -k
```

#### Sample output

``` java
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

### Fetch rule templates of the template group with given UUID

#### Overview

<table>
<tbody>
<tr class="odd">
<td>Description</td>
<td>Returns the rule templates of the template group with the given UUID.</td>
</tr>
<tr class="even">
<td>API Context</td>
<td><code>             /business-rules/template-groups/{templateGroupID}/templates            </code></td>
</tr>
<tr class="odd">
<td>HTTP Method</td>
<td><code>             GET            </code></td>
</tr>
<tr class="even">
<td>Request/Response Format</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td>Authentication</td>
<td>Basic</td>
</tr>
<tr class="even">
<td>Username</td>
<td><code>             admin            </code></td>
</tr>
<tr class="odd">
<td>Password</td>
<td><code>             admin            </code></td>
</tr>
<tr class="even">
<td>Runtime</td>
<td>Dashboard</td>
</tr>
</tbody>
</table>

##### Parameter description

| Parameter                                    | Description                                                                    |
|----------------------------------------------|--------------------------------------------------------------------------------|
| `             {templateGroupID}            ` | The UUID of the template group of which the rule templates need to be fetched. |

#### curl command syntax

``` java
```



#### Sample curl command

``` java
    curl -X GET "https://localhost:9643/business-rules/template-groups/sweet-factory/templates" -u admin:admin -k
```

#### Sample output

``` java
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

### Fetch rule template of specific UUID available under a template group with specific UUID

#### Overview

<table>
<tbody>
<tr class="odd">
<td>Description</td>
<td>Returns the rule template with the specified UUID that is defined under the template group with the specified UUID.</td>
</tr>
<tr class="even">
<td>API Context</td>
<td><code>             /business-rules             /template-groups/{templateGroupID}/templates/{ruleTemplateID}            </code></td>
</tr>
<tr class="odd">
<td>HTTP Method</td>
<td><code>             GET            </code></td>
</tr>
<tr class="even">
<td>Request/Response Format</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td>Authentication</td>
<td>Basic</td>
</tr>
<tr class="even">
<td>Username</td>
<td><code>             admin            </code></td>
</tr>
<tr class="odd">
<td>Password</td>
<td><code>             admin            </code></td>
</tr>
<tr class="even">
<td>Runtime</td>
<td>Dashboard</td>
</tr>
</tbody>
</table>

##### Parameter description

| Parameter                                    | Description                                                                                  |
|----------------------------------------------|----------------------------------------------------------------------------------------------|
| `             {templateGroupID}            ` | The UUID of the template group from which the specified rule template needs to be retrieved. |
| `             {ruleTemplateID}            `  | The UUID of the rule template that needs to be retrieved from the specified template group.  |

#### curl command syntax

``` java
```



#### Sample curl command

``` java
    curl -X GET "https://localhost:9643/business-rules/template-groups/sweet-factory/templates/identifying-continuous-production-decrease" -u admin:admin -k
```

#### Sample output

``` java
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

### Fetch available template groups

#### Overview

<table>
<tbody>
<tr class="odd">
<td>Description</td>
<td>Returns all the template groups that are currently available in the SP setup.</td>
</tr>
<tr class="even">
<td>API Context</td>
<td><code>             /business-rules/template-groups            </code></td>
</tr>
<tr class="odd">
<td>HTTP Method</td>
<td><code>             GET            </code></td>
</tr>
<tr class="even">
<td>Request/Response Format</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td>Authentication</td>
<td>Basic</td>
</tr>
<tr class="even">
<td>Username</td>
<td><code>             admin            </code></td>
</tr>
<tr class="odd">
<td>Password</td>
<td><code>             admin            </code></td>
</tr>
<tr class="even">
<td>Runtime</td>
<td>Dashboard</td>
</tr>
</tbody>
</table>

#### curl command syntax

``` java
```



#### Sample curl command

``` java
    curl -X GET "https://localhost:9643/business-rules/template-groups" -u admin:admin -k
```

#### Sample output

``` java
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

### Fetch business rule instance with given UUID

#### Overview

|                         |                                                                               |
|-------------------------|-------------------------------------------------------------------------------|
| Description             | Returns the business rule instance with the given UUID.                       |
| API Context             | `             /business-rules/instances/{businessRuleInstanceID}            ` |
| HTTP Method             | `             GET            `                                                |
| Request/Response Format | application/json                                                              |
| Authentication          | Basic                                                                         |
| Username                | `             admin            `                                              |
| Password                | `             admin            `                                              |
| Runtime                 | Dashboard                                                                     |

##### Parameter description

| Parameter                                           | Description                                            |
|-----------------------------------------------------|--------------------------------------------------------|
| `             {businessRuleInstanceID}            ` | The UUID of the business rules instance to be fetched. |

#### curl command syntax

``` java
```



#### Sample curl command

``` java
    curl -X GET "https://localhost:9643/business-rules/instances/business-rule-1" -H "accept: application/json" -u admin:admin -k
```

#### Sample output

``` java
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>



### Create and save a business rule

#### Overview

|                         |                                                                                             |
|-------------------------|---------------------------------------------------------------------------------------------|
| Description             | Creates and saves a business rule.                                                          |
| API Context             | `             /business-rules             /instances?deploy={deploymentStatus}            ` |
| HTTP Method             | `             POST            `                                                             |
| Request/Response Format | `             application/json            `                                                 |
| Authentication          | Basic                                                                                       |
| Username                | `             admin            `                                                            |
| Password                | `             admin            `                                                            |
| Runtime                 | Dashboard                                                                                   |

##### Parameter description

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             {deploymentStatus}            </code></td>
<td><br />
</td>
</tr>
</tbody>
</table>

#### curl command syntax

``` java
```



#### Sample curl command

``` java
    curl -X POST "https://localhost:9643/business-rules/instances?deploy=true" -H "accept: application/json" -H "content-type: multipart/form-data" -F 'businessRule={"name":"Business Rule 5","uuid":"business-rule-5","type":"template","templateGroupUUID":"sweet-factory","ruleTemplateUUID":"identifying-continuous-production-decrease","properties":{"timeInterval":"6","timeRangeInput":"5","email":"example@email.com"}}' -u admin:admin -k
```

#### Sample output

``` java
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

### Update business rules instance with given UUID

#### Overview

|                         |                                                                                                                      |
|-------------------------|----------------------------------------------------------------------------------------------------------------------|
| Description             | Updates the business rules instance with the given UUID.                                                             |
| API Context             | `             /business-rules             /instances/{businessRuleInstanceID}?deploy={deploymentStatus}            ` |
| HTTP Method             | `             PUT            `                                                                                       |
| Request/Response Format | `             application/json            `                                                                          |
| Authentication          | Basic                                                                                                                |
| Username                | `             admin            `                                                                                     |
| Password                | `             admin            `                                                                                     |
| Runtime                 | Dashboard                                                                                                            |

##### Parameter description

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             {businessRuleInstanceID}            </code></td>
<td>The UUID of the business rules instance to be updated.</td>
</tr>
<tr class="even">
<td><code>             {deploymentStatus}            </code></td>
<td><br />
</td>
</tr>
</tbody>
</table>

#### curl command syntax

``` java
```



#### Sample curl command

``` java
    curl -X PUT "https://localhost:9643/business-rules/instances/business-rule-5?deploy=true" -H "accept: application/json" -H "content-type: application/json" -d '{"name":"Business Rule 5","uuid":"business-rule-5","type":"template","templateGroupUUID":"sweet-factory","ruleTemplateUUID":"identifying-continuous-production-decrease","properties":{"timeInterval":"9","timeRangeInput":"8","email":"newexample@email.com"}}' -u admin:admin -k
```

#### Sample output

``` java
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>


## Store APIs

### Query records in Siddhi store

#### Overview

|                         |                                                                                                    |
|-------------------------|----------------------------------------------------------------------------------------------------|
| Description             | Queries records in the Siddhi store. For more information, see Managing Stored Data via REST API . |
| API Context             | `             /stores/query            `                                                           |
| HTTP Method             | `             POST            `                                                                    |
| Request/Response Format | `             application/json            `                                                        |
| Authentication          | Basic                                                                                              |
| Username                | `             admin            `                                                                   |
| Password                | `             admin            `                                                                   |
| Runtime                 | server/tooling                                                                                             |

#### curl command syntax

``` java
    curl -X POST https://localhost:9443/stores/query -H "content-type: application/json" -u "admin:admin"
    -d '{"appName" : "AggregationTest", "query" : "from stockAggregation select *" }' -k
```



#### Sample curl command

``` java
    curl -X POST https://localhost:9443/stores/query -H "content-type: application/json" -u "admin:admin" -d '{"appName" : "RoomService", "query" : "select 10 as roomNumber, 1 as arrival update RoomTypeTable  set RoomTypeTable.people = RoomTypeTable.people + arrival on RoomTypeTable.roomNo == roomNumber;" }' -k
```

#### Sample output

``` java
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Healthcheck APIs

#### Overview

<table>
<tbody>
<tr class="odd">
<td>Description</td>
<td><br />
</td>
</tr>
<tr class="even">
<td>API Context</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td>HTTP Method</td>
<td><code>             GET            </code></td>
</tr>
<tr class="even">
<td>Request/Response Format</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td>Authentication</td>
<td>Basic</td>
</tr>
<tr class="even">
<td>Username</td>
<td><code>             admin            </code></td>
</tr>
<tr class="odd">
<td>Password</td>
<td><code>             admin            </code></td>
</tr>
<tr class="even">
<td>Runtime</td>
<td><br />
</td>
</tr>
</tbody>
</table>

#### curl command syntax

``` java
```



#### Sample curl command

``` java
    curl -k -X GET http://localhost:9090/health
```

#### Sample output

``` java
```

#### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## HTTP Status Codes

When REST API requests are sent to carryout various actions, various HTTP status codes will be returned based on the state of the action (success or failure) and the HTTP method (`POST, GET, PUT, DELETE`) executed. The following are the definitions of the various HTTP status codes that are returned.


### HTTP status codes indicating successful delivery

| Code | Code Summary | Description                                                                                                                                                                                                                       |
|------|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 200  | Ok           | HTTP request was successful. The output corresponding to the HTTP request will be returned. Generally used as a response to a successful `             GET            ` and `             PUT            ` REST API HTTP methods. |
| 201  | Created      | HTTP request was successfully processed and a new resource was created. Generally used as a response to a successful `             POST            ` REST API HTTP method.                                                        |
| 204  | No content   | HTTP request was successfully processed. No content will be returned. Generally used as a response to a successful `             DELETE            ` REST API HTTP method.                                                        |
| 202  | Accepted     | HTTP request was accepted for processing, but the processing has not been completed. This generally occurs when your successful in trying to undeploy an application.                                                             |

### Error HTTP status codes

| Code | Code Summary          | Description                                                                                                                                                                                                                                 |
|------|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 404  | Not found             | Requested resource not found. Generally used as a response for unsuccessful `             GET            ` and `             PUT            ` REST API HTTP methods.                                                                        |
| 409  | Conflict              | Request could not be processed because of conflict in the request. This generally occurs when you are trying to add a resource that already exists. For example, when trying to add an auto-scaling policy that has an already existing ID. |
| 500  | Internal server error | Server error occurred.                                                                                                                                                                                                                      |
