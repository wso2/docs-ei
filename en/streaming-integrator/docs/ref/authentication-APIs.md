# Authentication APIs

-   [Log in to a dashboard
    application](#AuthenticationAPIs-Logintoadashboardapplication)
-   [Log out of the dashboard
    application](#AuthenticationAPIs-Logoutofthedashboardapplication)
-   [Redirect URL for login using authorization grant
    type](#AuthenticationAPIs-RedirectURLforloginusingauthorizationgranttype)

## Log in to a dashboard application

### Overview

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

#### Parameter description

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

  

### curl command syntax

``` java
    curl -X POST "https://analytics.wso2.com/login/{appName}" -H "accept: application/json" -H "Content-Type: application/x-www-form-urlencoded" -d "username={username}&password={password}&grantType={grantTypr}&rememberMe={rememberMe}"
```

  

### Sample curl command

``` java
    curl -X POST "https://localhost:9643/login/portal" 
    -H "Content-Type: application/x-www-form-urlencoded" -d "username=admin&password=admin&grantType=password"
```

### Sample output

``` java
    {"authUser":"admin","pID":"71368eff-cc71-44ef","lID":"a60c1098-3de0-42fb","validityPeriod":3600}
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

## Log out of the dashboard application

### Overview

|                         |                                              |
|-------------------------|----------------------------------------------|
| Overview                | Logs out of the dashboard application.       |
| API Context             | `             /logout/{appName}            ` |
| HTTP Method             | `             POST            `              |
| Request/Response Format | `             application/json            `  |
| Runtime                 | Dashboard                                    |

  

### curl command syntax

``` java
    curl -X POST "https://analytics.wso2.com/logout/{appName}" -H "accept: application/json" -H "Authorzation: Bearer {access token}"
```

###  Sample curl command

``` java
    curl -X POST "https://analytics.wso2.com/logout/portal" -H "accept: application/json" -H "Authorzation: Bearer 123456"
```

### Sample output

``` java
    N/A
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

  

## Redirect URL for login using authorization grant type

### Overview

|                         |                                                               |
|-------------------------|---------------------------------------------------------------|
| Overview                | Redirects URL by the IS in authorization grant type - OAuth2. |
| API Context             | `             /login/callback/{appName}            `          |
| HTTP Method             | `             GET            `                                |
| Request/Response Format | JSON                                                          |
| Runtime                 | Dashbaord                                                     |

#### Parameter description

| Parameter                            | Description                                              |
|--------------------------------------|----------------------------------------------------------|
| `             {appName}            ` | The application of which the URL needs to be redirected. |

  

### curl command syntax

``` java
```

  

### Sample curl command

``` java
    curl -X GET "https://localhost:9643/login/callback/portal"
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
