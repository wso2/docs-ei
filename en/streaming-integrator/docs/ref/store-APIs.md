# Store APIs

-   [Query records in Siddhi store](#StoreAPIs-QueryrecordsinSiddhistore)

## Query records in Siddhi store

### Overview

|                         |                                                                                                    |
|-------------------------|----------------------------------------------------------------------------------------------------|
| Description             | Queries records in the Siddhi store. For more information, see Managing Stored Data via REST API . |
| API Context             | `/stores/query`                                                                                    |
| HTTP Method             | `POST`                                                                                             |
| Request/Response Format | `application/json`                                                                                 |
| Authentication          | Basic                                                                                              |
| Username                | `admin`                                                                                            |
| Password                | `admin`                                                                                            |
| Runtime                 | Worker                                                                                             |

### curl command syntax

``` java
    curl -X POST https://localhost:9443/stores/query -H "content-type: application/json" -u "admin:admin" 
    -d '{"appName" : "AggregationTest", "query" : "from stockAggregation select *" }' -k
```

  

### Sample curl command

``` java
    curl -X POST https://localhost:7443/stores/query -H "content-type: application/json" -u "admin:admin" -d '{"appName" : "ApiRequestSummary", "query" : "from API_REQUEST_SUMMARY within 1586249325000L, 1586335725000L per \"days\" select userId, apiPublisher, sum(totalRequestCount) as net_total_requests group by userId, apiPublisher order by net_total_requests DESC;" }' -k
```

### Sample output

``` java
    {"records":[["admin","admin",66]]}
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
