!!! note
    **This page is still a work in progress!**
    
# Event Simulation APIs

## Sending a single event for simulation

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Sends a single event for simulation.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/single</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>POST</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><strong>Request</strong>: <code>text/plain</code><br />
<strong>Response</strong>: <code>application/json</code></td>
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

### curl command syntax

```java

```

### Sample curl command

```java
curl -X POST "http://localhost:9390/simulation/single" -H "accept: application/json" -H "content-type: text/plain" -d "{ \"streamName\": \"FooStream\", \"siddhiAppName\": \"TestSiddhiApp\", \"timestamp\": \"1500319950004\", \"data\": [ \"foo\", \"bar\", \"12345\" ]}"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200 and 404.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Uploading a feed simulation configuration

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Uploads a feed simulation configuration.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/feed</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>POST</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><strong>Request</strong>: <code>text/plain</code><br />
<strong>Response</strong>: <code>application/json</code></td>
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

### curl command syntax

```java

```

### Sample curl command

```java
curl -X POST "http://localhost:9390/simulation/feed" -H "accept: application/json" -H "content-type: text/plain" -d "{\"properties\":{\"simulationName\":\"TestFeedSimulation\",\"startTimestamp\":\"1500319950003\",\"endTimestamp\":\"1500319950009\",\"noOfEvents\":\"100\",\"description\":\"Test feed simulator\",\"timeInterval\":\"1000\"},\"sources\":[{\"siddhiAppName\":\"TestSiddhiApp\",\"streamName\":\"FooStream\",\"timestampInterval\":\"1000\",\"simulationType\":\"CSV_SIMULATION\",\"fileName\":\"foostream.csv\",\"delimiter\":\",\",\"isOrdered\":true,\"indices\":\"0,1,2\"}]}"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200, 403, 404, and 409.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Retrieving all feed simulation configurations

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Retrieves all feed simulation configurations.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/feed</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>GET</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

### curl command syntax

```java

```

### Sample curl command

```java
curl -X GET "http://localhost:9390/simulation/feed" -H "accept: application/json"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200 and 404.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Updating a feed simulation configuration

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Updates a feed simulation configuration.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/feed/{simulationName}</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>PUT</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><strong>Request</strong>: <code>text/plain</code><br />
<strong>Response</strong>: <code>application/json</code></td>
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

**Parameter description**

| **Parameter**    | **Description**                                                    |
|------------------|--------------------------------------------------------------------|
|`{simulationName}`| The name of the simulation configuration that needs to be updated. |

### curl command syntax

```java

```

### Sample curl command

```java
curl -X PUT "http://localhost:9390/simulation/feed/TestFeedSimulation" -H "accept: application/json" -H "content-type: text/plain" -d "{\"properties\":{\"simulationName\":\"TestFeedSimulation\",\"startTimestamp\":\"\",\"endTimestamp\":\"\",\"noOfEvents\":\"100\",\"description\":\"Test feed simulator\",\"timeInterval\":\"1000\"},\"sources\":[{\"siddhiAppName\":\"TestSiddhiApp\",\"streamName\":\"BarStream\",\"timestampInterval\":\"1000\",\"simulationType\":\"CSV_SIMULATION\",\"fileName\":\"foostream.csv\",\"delimiter\":\",\",\"isOrdered\":true,\"indices\":\"0,1,2\"}]}"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200 and 404.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Retrieving a specific feed simulation configuration

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Retrieves a specific feed simulation configuration.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/feed/{simulationName}</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>GET</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

**Parameter Description**

| Parameter        | Description                                                               |
|------------------|---------------------------------------------------------------------------|
|`{simulationName}`| The name of the feed simulation configuration that needs to be retrieved. |

### curl command syntax

```java

```

### Sample curl command

```java
curl -X GET "http://localhost:9390/simulation/feed/TestFeedSimulation" -H "accept: application/json"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200 and 404.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Deleting a feed simulation configuration

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Deletes a feed simulation configuration.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/feed/{simulationName}</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>DELETE</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

**Parameter Description**

| Parameter        | Description                                                               |
|------------------|---------------------------------------------------------------------------|
|`{simulationName}`| The name of the simulation configuration that needs to be deleted.        |

### curl command syntax

```java

```

### Sample curl command

```java
curl -X DELETE "http://localhost:9390/simulation/feed/TestFeedSimulation" -H "accept: application/json"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200 and 404.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Running a feed simulation configuration

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Runs a feed simulation configuration.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/feed/{simulationName}?action=run</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>POST</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

**Parameter Description**

| Parameter        | Description                                                               |
|------------------|---------------------------------------------------------------------------|
|`{simulationName}`| The name of the simulation configuration that needs to be run.            |

### curl command syntax

```java

```

### Sample curl command

```java
curl -X POST "http://localhost:9390/simulation/feed/TestFeedSimulation/?action=run" -H "accept: application/json"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200, 403, 404, and 409.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Pausing a feed simulation

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Pauses a currently active feed simulation.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/feed/{simulationName}?action=pause</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>POST</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

**Parameter Description**

| Parameter        | Description                                                               |
|------------------|---------------------------------------------------------------------------|
|`{simulationName}`| The name of the simulation configuration that needs to be paused.         |

### curl command syntax

```java
curl -X POST "http://<HOST_NAME>:<PORT>/simulation/feed/<FEED_NAME>/?action=pause" -H "accept: application/json"
```

### Sample curl command

```java
curl -X POST "http://localhost:9390/simulation/feed/TestFeedSimulation/?action=pause" -H "accept: application/json"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200, 403, 404, and 409.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Resuming a feed simulation

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Resumes a paused feed simulation.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/feed/{simulationName}?action=resume</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>POST</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

**Parameter Description**

| Parameter        | Description                                                               |
|------------------|---------------------------------------------------------------------------|
|`{simulationName}`| The name of the simulation configuration that needs to be resumed.        |

### curl command syntax

```java
curl -X POST "http://<HOST_NAME>:<PORT>/simulation/feed/<SIMULATION_NAME>/?action=resume" -H "accept: application/json"
```

### Sample curl command
```java
curl -X POST "http://localhost:9390/simulation/feed/TestFeedSimulation/?action=resume" -H "accept: application/json"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200, 403, 404, and 409.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Stopping a feed simulation

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Stops a currently active feed simulation.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/feed/{simulationName}?action=stop</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>POST</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

**Parameter Description**

| Parameter        | Description                                                               |
|------------------|---------------------------------------------------------------------------|
|`{simulationName}`| The name of the simulation configuration that needs to be stopped.        |

### curl command syntax

```java
curl -X POST "http://<HOST_NAME>:<PORT>/simulation/feed/<SIMULATION_NAME>/?action=stop" -H "accept: application/json"
```

### Sample curl command

```java
curl -X POST "http://localhost:9390/simulation/feed/TestFeedSimulation/?action=stop" -H "accept: application/json"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200, 404, and 409.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Retrieving a simulation configuration status by name

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Retrieves the status of a given simulation configuration.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/feed/{simulationName}/status</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>POST</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

**Parameter Description**

| Parameter        | Description                                                                      |
|------------------|----------------------------------------------------------------------------------|
|`{simulationName}`| The name of the simulation configuration of which the status needs to be checked.|

### curl command syntax

```java
curl -X GET "http://<HOST_NAME>:<PORT>/simulation/feed/<SIMULATION_NAME>/status" -H "accept: application/json"
```

### Sample curl command

```java
curl -X GET "http://localhost:9390/simulation/feed/TestFeedSimulation/status" -H "accept: application/json"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200 and 404.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Uploading a CSV file

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Uploads a CSV file for feed simulation.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/files</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>GET</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

### curl command syntax

```java

```

### Sample curl command

```java

```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200 and 404.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Fetching CSV file names

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Fetches the names of CSV files that are currently uploaded in the system.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/files</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>GET</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

### curl command syntax

```java
curl -X GET "http://<HOST_NAME>:<PORT>/simulation/files" -H "accept: application/json"
```

### Sample curl command

```java
curl -X GET "http://localhost:9390/simulation/files" -H "accept: application/json"
```

### Sample output

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200 and 404.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Updating a CSV file

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Updates a CSV file that is already uploaded in the system.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/files/{fileName}</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>PUT</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code></code></td>
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

**Parameter Description**

| Parameter  | Description                                       |
|------------|---------------------------------------------------|
|`{fileName}`| The name of the CSV file that needs to be updated.|

### curl command syntax

```java

```

### Sample curl command

```java
curl -X PUT -F 'file=@foostream.csv' http://localhost:9390/simulation/files/foostream.csv?fileName=foostream.csv
```

### Sample output

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200 and 404.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Deleting a CSV file

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Deletes the specified CSV file.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td><code>/simulation/files/{fileName}</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>DELETE</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

**Parameter Description**

| Parameter  | Description                                       |
|------------|---------------------------------------------------|
|`{fileName}`| The name of the CSV file that needs to be deleted.|

### curl command syntax

```java
curl -X DELETE "http://<HOST_NAME>:<PORT>/simulation/files/<FILE_NAME>.csv" -H "accept: application/json"
```

### Sample curl command

```java
curl -X DELETE "http://localhost:9390/simulation/files/CSVTestFile.csv" -H "accept: application/json"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200 and 404.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Testing a database connection

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Tests a database connection.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td>/simulation/connectToDatabase</code></td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>POST</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

### curl command syntax

```java

```

### Sample curl command

```java
curl -X POST "http://localhost:9090/simulation/connectToDatabase" -H "accept: application/json" -H "content-type: application/json" -d "{ \"dataSourceLocation\": \"jdbc:mysql://localhost:3306/DatabaseFeedSimulation\", \"driver\": \"com.mysql.jdbc.Driver\", \"username\": \"root\", \"password\": \"password\"}"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200 and 404.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Retrieving database tables

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Retrieves database tables.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td>/simulation/connectToDatabase/retrieveTableNames</td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>POST</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

### curl command syntax

```java

```

### Sample curl command

```java
curl -X POST "http://localhost:9090/simulation/connectToDatabase/retrieveTableNames" -H "accept: application/json" -H "content-type: application/json" -d "{ \"dataSourceLocation\": \"jdbc:mysql://localhost:3306/DatabaseFeedSimulation\", \"driver\": \"com.mysql.jdbc.Driver\", \"username\": \"root\", \"password\": \"password\"}"
```

### Sample output

```java

```

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200 and 404.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>

## Retrieving database table columns

### Overview

<table>
<tbody>
<tr class="odd">
<th>Description</th>
<td>Retrieves database table columns.</td>
</tr>
<tr class="even">
<th>API Context</th>
<td>/simulation/connectToDatabase/{tableName}/retrieveColumnNames</td>
</tr>
<tr class="odd">
<th>HTTP Method</th>
<td>POST</td>
</tr>
<tr class="even">
<th>Request/Response format</th>
<td><code>application/json</code></td>
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

**Parameter Description**

| Parameter   | Description                                                              |
|-------------|--------------------------------------------------------------------------|
|`{tableName}`| The name of the database table of which the columns need to be retrieved.|

### curl command syntax

```java

```

### Sample curl command

```java
curl -X POST "http://localhost:9090/simulation/connectToDatabase/DataTable/retrieveColumnNames" -H "accept: application/json" -H "content-type: application/json" -d "{ \"dataSourceLocation\": \"jdbc:mysql://localhost:3306/DatabaseFeedSimulation\", \"driver\": \"com.mysql.jdbc.Driver\", \"username\": \"root\", \"password\": \"password\"}"
```

### Sample output

### Response

<table>
<tbody>
<tr class="odd">
<th>HTTP Status Code</th>
<td><p>Possible codes are 200 and 404.</p>
<p>For descriptions of the HTTP status codes, see <a href="_HTTP_Status_Codes_">HTTP Status Codes</a> .</p></td>
</tr>
</tbody>
</table>