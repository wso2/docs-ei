# Business Rules APIs

-   [Lists available business rule
    instances](#BusinessRulesAPIs-Listsavailablebusinessruleinstances)
-   [Delete business rule with given
    UUID](#BusinessRulesAPIs-DeletebusinessrulewithgivenUUID)
-   [Fetch template group with the given
    UUID](#BusinessRulesAPIs-FetchtemplategroupwiththegivenUUID)
-   [Fetch rule templates of the template group with given
    UUID](#BusinessRulesAPIs-FetchruletemplatesofthetemplategroupwithgivenUUID)
-   [Fetch rule template of specific UUID available under a template
    group with specific
    UUID](#BusinessRulesAPIs-FetchruletemplateofspecificUUIDavailableunderatemplategroupwithspecificUUID)
-   [Fetch available template
    groups](#BusinessRulesAPIs-Fetchavailabletemplategroups)
-   [Fetch business rule instance with given
    UUID](#BusinessRulesAPIs-FetchbusinessruleinstancewithgivenUUID)
-   [Create and save a business
    rule](#BusinessRulesAPIs-Createandsaveabusinessrule)
-   [Update business rules instance with given
    UUID](#BusinessRulesAPIs-UpdatebusinessrulesinstancewithgivenUUID)

## Lists available business rule instances

### Overview

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

### curl command syntax

``` java
```

  

### Sample curl command

``` java
    curl -X GET "https://localhost:9643/business-rules/instances" -u admin:admin -k
```

### Sample output

``` java
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

## Delete business rule with given UUID

### Overview

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

#### Parameter description

| Parameter                                           | Description                                                                       |
|-----------------------------------------------------|-----------------------------------------------------------------------------------|
| `             {businessRuleInstanceID}            ` | The UUID (Uniquely Identifiable ID) of the business rules instance to be deleted. |

### curl command syntax

``` java
```

  

### Sample curl command

``` java
    curl -X DELETE "https://localhost:9643/business-rules/instances/business-rule-1?force-delete=false" -H "accept: application/json" -u admin:adm
```

### Sample output

``` java
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

## Fetch template group with the given UUID

### Overview

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

#### Parameter description

| Parameter                                    | Description                                   |
|----------------------------------------------|-----------------------------------------------|
| `             {templateGroupID}            ` | The UUID of the template group to be fetched. |

### curl command syntax

``` java
```

  

### Sample curl command

``` java
    curl -X GET "https://localhost:9643/business-rules/template-groups/sweet-factory" -u admin:admin -k
```

### Sample output

``` java
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

## Fetch rule templates of the template group with given UUID

### Overview

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

#### Parameter description

| Parameter                                    | Description                                                                    |
|----------------------------------------------|--------------------------------------------------------------------------------|
| `             {templateGroupID}            ` | The UUID of the template group of which the rule templates need to be fetched. |

### curl command syntax

``` java
```

  

### Sample curl command

``` java
    curl -X GET "https://localhost:9643/business-rules/template-groups/sweet-factory/templates" -u admin:admin -k
```

### Sample output

``` java
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

## Fetch rule template of specific UUID available under a template group with specific UUID

### Overview

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

#### Parameter description

| Parameter                                    | Description                                                                                  |
|----------------------------------------------|----------------------------------------------------------------------------------------------|
| `             {templateGroupID}            ` | The UUID of the template group from which the specified rule template needs to be retrieved. |
| `             {ruleTemplateID}            `  | The UUID of the rule template that needs to be retrieved from the specified template group.  |

### curl command syntax

``` java
```

  

### Sample curl command

``` java
    curl -X GET "https://localhost:9643/business-rules/template-groups/sweet-factory/templates/identifying-continuous-production-decrease" -u admin:admin -k
```

### Sample output

``` java
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

## Fetch available template groups

### Overview

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

### curl command syntax

``` java
```

  

### Sample curl command

``` java
    curl -X GET "https://localhost:9643/business-rules/template-groups" -u admin:admin -k
```

### Sample output

``` java
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

## Fetch business rule instance with given UUID

### Overview

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

#### Parameter description

| Parameter                                           | Description                                            |
|-----------------------------------------------------|--------------------------------------------------------|
| `             {businessRuleInstanceID}            ` | The UUID of the business rules instance to be fetched. |

### curl command syntax

``` java
```

  

### Sample curl command

``` java
    curl -X GET "https://localhost:9643/business-rules/instances/business-rule-1" -H "accept: application/json" -u admin:admin -k
```

### Sample output

``` java
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

  

## Create and save a business rule

### Overview

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

#### Parameter description

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

### curl command syntax

``` java
```

  

### Sample curl command

``` java
    curl -X POST "https://localhost:9643/business-rules/instances?deploy=true" -H "accept: application/json" -H "content-type: multipart/form-data" -F 'businessRule={"name":"Business Rule 5","uuid":"business-rule-5","type":"template","templateGroupUUID":"sweet-factory","ruleTemplateUUID":"identifying-continuous-production-decrease","properties":{"timeInterval":"6","timeRangeInput":"5","email":"example@email.com"}}' -u admin:admin -k
```

### Sample output

``` java
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

## Update business rules instance with given UUID

### Overview

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

#### Parameter description

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

### curl command syntax

``` java
```

  

### Sample curl command

``` java
    curl -X PUT "https://localhost:9643/business-rules/instances/business-rule-5?deploy=true" -H "accept: application/json" -H "content-type: application/json" -d '{"name":"Business Rule 5","uuid":"business-rule-5","type":"template","templateGroupUUID":"sweet-factory","ruleTemplateUUID":"identifying-continuous-production-decrease","properties":{"timeInterval":"9","timeRangeInput":"8","email":"newexample@email.com"}}' -u admin:admin -k
```

### Sample output

``` java
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
