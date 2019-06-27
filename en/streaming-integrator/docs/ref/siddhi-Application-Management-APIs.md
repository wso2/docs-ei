# Siddhi Application Management APIs

-   [Updating a Siddhi
    Application](#SiddhiApplicationManagementAPIs-UpdatingaSiddhiApplication)
-   [Deleting a Siddhi
    application](#SiddhiApplicationManagementAPIs-DeletingaSiddhiapplication)
-   [Listing all active Siddhi
    applications](#SiddhiApplicationManagementAPIs-ListingallactiveSiddhiapplications)
-   [Retrieving a specific Siddhi
    application](#SiddhiApplicationManagementAPIs-RetrievingaspecificSiddhiapplication)
-   [Fetching the status of a Siddhi
    Application](#SiddhiApplicationManagementAPIs-FetchingthestatusofaSiddhiApplication)
-   [Taking a snapshot of a Siddhi
    Application](#SiddhiApplicationManagementAPIs-TakingasnapshotofaSiddhiApplicationSnapshot)
-   [Restoring a Siddhi Application via a
    snapshot](#SiddhiApplicationManagementAPIs-RestoringaSiddhiApplicationviaasnapshot)
-   [Returning real-time statistics of a
    worker](#SiddhiApplicationManagementAPIs-Returningreal-timestatisticsofaworker)
-   [Enabling/disabling worker
    statistics](#SiddhiApplicationManagementAPIs-Enabling/disablingworkerstatistics)
-   [Returning general details of a
    worker](#SiddhiApplicationManagementAPIs-Returninggeneraldetailsofaworker)
-   [Returning detailed statistics of all Siddhi
    applications](#SiddhiApplicationManagementAPIs-ReturningdetailedstatisticsofallSiddhiapplications)
-   [Enabling/disabling the statistics of a specific Siddhi
    application](#SiddhiApplicationManagementAPIs-Enabling/disablingthestatisticsofaspecificSiddhiapplication)
-   [Enabling/disabling the statistics of all Siddhi
    applications](#SiddhiApplicationManagementAPIs-Enabling/disablingthestatisticsofallSiddhiapplications)

### Creating a Siddhi application

### Overview

<table>
<tbody>
<tr class="odd">
<td>Description</td>
<td>Creates a new Siddhi Application.</td>
</tr>
<tr class="even">
<td>API Context</td>
<td><code>             /siddhi-apps            </code></td>
</tr>
<tr class="odd">
<td>HTTP Method</td>
<td>POST</td>
</tr>
<tr class="even">
<td>Request/Response format</td>
<td><strong>Request</strong> : text/plain<br />
<strong>Response</strong> : application/json</td>
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
<td>worker/manager</td>
</tr>
</tbody>
</table>

### curl command syntax

``` java
    curl -X POST "https://localhost:9443/siddhi-apps" -H "accept: application/json" -H "Content-Type: text/plain" -d @TestSiddhiApp.siddhi -u admin:admin -k
```

### Sample curl command

``` java
    curl -X POST "https://localhost:9443/siddhi-apps" -H "accept: application/json" -H "Content-Type: text/plain" -d @TestSiddhiApp.siddhi -u admin:admin -k
```

### Sample output

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

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>Possible codes are 201, 409, 400, and 500.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Updating a Siddhi Application

### Overview

<table>
<tbody>
<tr class="odd">
<td>Description</td>
<td>Updates a Siddhi Application.</td>
</tr>
<tr class="even">
<td>API Context</td>
<td><code>             /siddhi-apps            </code></td>
</tr>
<tr class="odd">
<td>HTTP Method</td>
<td>PUT</td>
</tr>
<tr class="even">
<td>Request/Response format</td>
<td><strong>Request</strong> : text/plain<br />
<strong>Response</strong> : application/json</td>
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
<td>worker/manager</td>
</tr>
</tbody>
</table>

### curl command syntax

``` java
    curl -X PUT "http://localhost:9090/siddhi-apps" -H "accept: application/json" -H "Content-Type: text/plain" -d @<SIDDHI_APPLICATION_NAME>.siddhi -u admin:admin -k
```

### Sample curl command

``` java
    curl -X PUT "https://localhost:9443/siddhi-apps" -H "accept: application/json" -H "Content-Type: text/plain" -d @TestSiddhiApp.siddhi -u admin:admin -k
```

### Sample output

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

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>Possible codes are 200, 201, 400, and 500.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

  

## Deleting a Siddhi application

### Overview

|                         |                                                            |
|-------------------------|------------------------------------------------------------|
| Description             | Sends the name of a Siddhi application as a URL parameter. |
| API Context             | `             /siddhi-apps/{appName}            `          |
| HTTP Method             | DELETE                                                     |
| Request/Response format | application/json                                           |
| Authentication          | Basic                                                      |
| Username                | admin                                                      |
| Password                | admin                                                      |
| Runtime                 | worker/manager                                             |

  

#### Parameter Description

| Parameter                            | Description                                       |
|--------------------------------------|---------------------------------------------------|
| `             {appName}            ` | The name of the Siddhi application to be deleted. |

  

### curl command syntax

``` java
    curl -X DELETE "http://localhost:9090/siddhi-apps/{app-name}" -H "accept: application/json" -u admin:admin -k
```

### Sample curl command

``` java
    curl -X DELETE "https://localhost:9443/siddhi-apps/TestSiddhiApp" -H "accept: application/json" -u admin:admin -k
```

### Sample output

The respose for the sample curl command given above can be one of the
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

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200, 404, 500 or 400.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Listing all active Siddhi applications

### Overview

<table>
<tbody>
<tr class="odd">
<td>Description</td>
<td><p>Lists all the currently active Siddhi applications.</p>
<p>If the <code>              isActive=true             </code> parameter is set, all the active Siddhi Applications are listed. If not, all the inactive Siddhi applications are listed.</p></td>
</tr>
<tr class="even">
<td>API Context</td>
<td><code>             /siddhi-apps            </code></td>
</tr>
<tr class="odd">
<td>HTTP Method</td>
<td>GET</td>
</tr>
<tr class="even">
<td>Request/Response format</td>
<td><strong>Request content type</strong> : any<br />
<strong>Response content type</strong> : <code>             application/json            </code></td>
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
<td>worker/manager</td>
</tr>
</tbody>
</table>

### curl command syntax

``` java
    curl -X GET "http://localhost:9090/siddhi-apps" -H "accept: application/json" -u admin:admin -k
```

### Sample curl command

``` java
    curl -X GET "https://localhost:9443/siddhi-apps?isActive=true" -H "accept: application/json" -u admin:admin -k
```

### Sample output

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
    
        If these conditions are met, but the `           isActive          `
        parameter is set to `           false          ` , the response
        contains only inactive Siddhi applications.
    

    ``` java
        ["TestExecutionPlan3"]
    ```

-   If the API request is valid, but there are no Siddhi applications
    deployed in your SP setup, the following response is returned.

    ``` java
            []
    ```

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Retrieving a specific Siddhi application

### Overview

|                         |                                                   |
|-------------------------|---------------------------------------------------|
| Description             | Retrieves the given Siddhi application.           |
| API Context             | `             /siddhi-apps/{appName}            ` |
| HTTP Method             | GET                                               |
| Request/Response format | application/json                                  |
| Authentication          | Basic                                             |
| Username                | admin                                             |
| Password                | admin                                             |
| Runtime                 | worker/manager                                    |

  

#### Parameter Description

| Parameter                            | Description                                         |
|--------------------------------------|-----------------------------------------------------|
| `             {appName}            ` | The name of the Siddhi application to be retrieved. |

  

### curl command syntax

``` java
    curl -X GET "http://localhost:9090/siddhi-apps/{app-name}" -H "accept: application/json" -u admin:admin -k
```

### Sample curl command

``` java
    curl -X GET "https://localhost:9443/siddhi-apps/SiddhiTestApp" -H "accept: application/json" -u admin:admin -k
```

### Sample output

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

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Fetching the status of a Siddhi Application

### Overview

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
<td>worker/manager</td>
</tr>
</tbody>
</table>

  

#### Parameter Description

| Parameter                            | Description                                                                 |
|--------------------------------------|-----------------------------------------------------------------------------|
| `             {appName}            ` | The name of the Siddhi application of which the status needs to be fetched. |

  

### curl command syntax

``` java
    curl -X GET "http://localhost:9090/siddhi-apps/{app-file-name}/status" -H "accept: application/json" -u admin:admin -k
```

### Sample curl command

``` java
    curl -X GET "https://localhost:9443/siddhi-apps/TestSiddhiApp/status" -H "accept: application/json" -u admin:admin -k
```

### Sample output

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

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Taking a snapshot of a Siddhi Application

### Overview

|                         |                                                           |
|-------------------------|-----------------------------------------------------------|
| Description             | This takes a snapshot of the specific Siddhi application. |
| API Context             | `             /siddhi-apps/{appName}/backup            `  |
| HTTP Method             | POST                                                      |
| Request/Response format | application/json                                          |
| Authentication          | Basic                                                     |
| Username                | admin                                                     |
| Password                | admin                                                     |
| Runtime                 | worker/manager                                            |

  

#### Parameter Description

| Parameter                            | Description                                                               |
|--------------------------------------|---------------------------------------------------------------------------|
| `             {appName}            ` | The name of the Siddhi application of which a snapshot needs to be taken. |

  

### curl command syntax

``` java
    curl -X POST "http://localhost:9090/siddhi-apps/{appName}/backup" -H "accept: application/json" -u admin:admin -k
```

### Sample curl command

``` java
    curl -X POST "https://localhost:9443/siddhi-apps/TestSiddhiApp/backup" -H "accept: application/json" -u admin:admin -k
```

### Sample output

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

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>201, 404, or 500.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Restoring a Siddhi Application via a snapshot

!!! info

In order to call this API, you need tohave already taken a snapshot of
the Siddhi application to be restored. For more information about the
API via which the snapshow is taken, see [Taking a snapshot of a Siddhi
application](#SiddhiApplicationManagementAPIs-Snapshot) .


### Overview

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
<td><p>worker/manager</p></td>
</tr>
</tbody>
</table>

#### Parameter Description

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

  

### curl command syntax

``` java
    curl -X POST "http://localhost:9090/siddhi-apps/{appName}/restore" -H "accept: application/json" -u admin:admin -k
```

### Sample curl command

``` java
    curl -X POST "https://localhost:9443/siddhi-apps/TestSiddhiApp/restore?revision=1514981290838_TestSiddhiApp" -H "accept: application/json" -u admin:admin -k
```

### Sample output

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

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200, 404 or 500.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Returning real-time statistics of a worker

### Overview

|                         |                                               |
|-------------------------|-----------------------------------------------|
| Description             | Returns the real-time statistics of a worker. |
| API Context             | `             /statistics            `        |
| HTTP Method             | GET                                           |
| Request/Response format | application/json                              |
| Authentication          | Basic                                         |
| Username                | admin                                         |
| Password                | admin                                         |
| Runtime                 | worker/manager                                |

  

#### Parameter Description

### curl command syntax

``` java
```

### Sample curl command

``` java
    curl -X GET "https://localhost:9443/statistics" -H "accept: application/json" -u admin:admin -k
```

### Sample output

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Enabling/disabling worker statistics

### Overview

|                         |                                                         |
|-------------------------|---------------------------------------------------------|
| Description             | Enables/diables generating statistics for worker nodes. |
| API Context             | `             /statistics            `                  |
| HTTP Method             | PUT                                                     |
| Request/Response format | application/json                                        |
| Authentication          | Basic                                                   |
| Username                | admin                                                   |
| Password                | admin                                                   |
| Runtime                 | worker/manager                                          |

  

#### Parameter Description

### curl command syntax

``` java
```

### Sample curl command

``` java
    curl -X PUT "https://localhost:9443/statistics" -H "accept: application/json" -H "Content-Type: application/json" -d "{“statsEnable”:”true”}" -u admin:admin -k
```

### Sample output

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Returning general details of a worker

### Overview

|                         |                                            |
|-------------------------|--------------------------------------------|
| Description             | Returns general details of a worker.       |
| API Context             | `             /system-details            ` |
| HTTP Method             | GET                                        |
| Request/Response format | application/json                           |
| Authentication          | Basic                                      |
| Username                | admin                                      |
| Password                | admin                                      |
| Runtime                 | worker/manager                             |

  

#### Parameter Description

### curl command syntax

``` java
```

### Sample curl command

``` java
    curl -X GET "https://localhost:9443/system-details" -H "accept: application/json" -u admin:admin -k
```

### Sample output

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Returning detailed statistics of all Siddhi applications

### Overview

|                         |                                                                                                    |
|-------------------------|----------------------------------------------------------------------------------------------------|
| Description             | Returns the detailed statistics of all the Siddhi applications currently deployed in the SP setup. |
| API Context             | `             /siddhi-apps/statistics            `                                                 |
| HTTP Method             | GET                                                                                                |
| Request/Response format | application/json                                                                                   |
| Authentication          | Basic                                                                                              |
| Username                | admin                                                                                              |
| Password                | admin                                                                                              |
| Runtime                 | worker/manager                                                                                     |

  

#### Parameter Description

### curl command syntax

``` java
```

### Sample curl command

``` java
    curl -X GET "https://localhost:9443/siddhi-apps/statistics" -H "accept: application/json" -u admin:admin -k
```

### Sample output

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Enabling/disabling the statistics of a specific Siddhi application

### Overview

|                         |                                                                 |
|-------------------------|-----------------------------------------------------------------|
| Description             | Enables/disables statistics for a specified Siddhi application. |
| API Context             | `             /siddhi-apps/{appName}/statistics            `    |
| HTTP Method             | PUT                                                             |
| Request/Response format | application/json                                                |
| Authentication          | Basic                                                           |
| Username                | admin                                                           |
| Password                | admin                                                           |
| Runtime                 | worker/manager                                                  |

  

#### Parameter Description

| Parameter                          | Description                                                                                       |
|------------------------------------|---------------------------------------------------------------------------------------------------|
| `             appName            ` | The name of the Siddhi application for which the Siddhi applications need to be enabled/disabled. |

  

### curl command syntax

``` java
```

### Sample curl command

``` java
    curl -X PUT "https://localhost:9443/siddhi-apps/TestSiddhiApp/statistics" -H "accept: application/json" -H "Content-Type: application/json" -d "{“statsEnable”:”true”}" -u admin:admin -k
```

### Sample output

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Enabling/disabling the statistics of all Siddhi applications

### Overview

|                         |                                                              |
|-------------------------|--------------------------------------------------------------|
| Description             | Enables/disables statistics for all the Siddhi applications. |
| API Context             | `             /siddhi-apps/statistics            `           |
| HTTP Method             | PUT                                                          |
| Request/Response format | application/json                                             |
| Authentication          | Basic                                                        |
| Username                | admin                                                        |
| Password                | admin                                                        |
| Runtime                 | worker/manager                                               |

  

#### Parameter Description

### curl command syntax

``` java
```

### Sample curl command

``` java
    curl -X PUT "https://localhost:9443/siddhi-apps/statistics" -H "accept: application/json" -H "Content-Type: application/json" -d "{“statsEnable”:”true”}" -u admin:admin -k
```

### Sample output

### Response

<table>
<tbody>
<tr class="odd">
<td>HTTP Status Code</td>
<td><p>200 or 404</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>
