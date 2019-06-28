# Exposing Cassandra as a Data Service

This tutorial will guide you on how to expose data stored in Cassandra
as a data service . Also, see the samples in [Data Integration
Samples](https://docs.wso2.com/display/EI650/Data+Integration+Samples) .

-   [Prerequisites](#ExposingCassandraasaDataService-Prerequisites)
    -   [Installing and starting
        Cassandra](#ExposingCassandraasaDataService-InstallingandstartingCassandra)
    -   [Creating a keyspace and a
        table](#ExposingCassandraasaDataService-Creatingakeyspaceandatable)
-   [Creating a data
    service](#ExposingCassandraasaDataService-Creatingadataservice)
    -   [Connecting to the
        datasource](#ExposingCassandraasaDataService-Connectingtothedatasource)
    -   [Creating a query to ADD user
        information](#ExposingCassandraasaDataService-CreatingaquerytoADDuserinformation)
    -   [Creating a query to GET
        data](#ExposingCassandraasaDataService-CreatingaquerytoGETdata)
    -   [Creating SOAP operationa to invoke the
        queries](#ExposingCassandraasaDataService-CreatingSOAPoperationatoinvokethequeries)
    -   [Creating a REST resource to invoke the
        query](#ExposingCassandraasaDataService-CreatingaRESTresourcetoinvokethequery)
    -   [Finish creating the data
        service](#ExposingCassandraasaDataService-Finishcreatingthedataservice)
-   [Invoking your data service using
    SOAP](#ExposingCassandraasaDataService-InvokingyourdataserviceusingSOAP)
    -   [Post new data](#ExposingCassandraasaDataService-Postnewdata)
    -   [Get data](#ExposingCassandraasaDataService-Getdata)
-   [Invoking your data service using
    REST](#ExposingCassandraasaDataService-InvokingyourdataserviceusingREST)
    -   [Post new data](#ExposingCassandraasaDataService-Postnewdata.1)
    -   [Get data](#ExposingCassandraasaDataService-Getdata.1)

### Prerequisites

Set up the Cassandra server with a keyspace and table for this tutorial:

#### Installing and starting Cassandra

A Cassandra server of version 3.0 should be already running in the
default port. For instructions, go to [Apache Cassandra
Documentation](http://cassandra.apache.org/download/) .

!!! info

-   Get familiar with the basics of Cassandra, including the
    prerequisites for using Cassandra, by going through the following:
    <https://wiki.apache.org/cassandra/GettingStarted> .
-   Get familiar with using the Cassandra CLI, by going through the
    following: <http://wiki.apache.org/cassandra/CassandraCli> .
-   Functionality of the underlying connector used for the Cassandra
    datasource:
    [https://www.datastax.com/documentation/developer/java-driver/2.0/index.html](index)
    .
-   Also, note that Cassandra requires the most stable version of Java 7
    or 8 you can deploy; preferably the Oracle/Sun JVM.


#### Creating a keyspace and a table

Execute the SQL commands as given below.

-   Create the keyspace named **UsersKS** :

    ``` java
        CREATE KEYSPACE UsersKS WITH replication = {'class':'SimpleStrategy', 'replication_factor':3};
    ```

-   Create the table named **Users** in the **UsersKS** keyspace:

    ``` java
            CREATE TABLE UsersKS.Users (id uuid, name text, country text, age int, PRIMARY KEY (id));
    ```

### Creating a data service

Now, let's start creating the data service from scratch:

1.  Download the product installer from
    [here](http://wso2.com/integration/) , and run the installer.  
    Let's call the installation location of your product the
    **\<EI\_HOME\>** directory.

    If you installed the product using the **installer** , this is
    located in a place specific to your OS as shown below:

    <table style="width:100%;">
    <colgroup>
    <col style="width: 9%" />
    <col style="width: 90%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>OS</th>
    <th>Home directory</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Mac OS</td>
    <td><code>                /Library/WSO2/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    <tr class="even">
    <td>Windows</td>
    <td><code>                C:\Program Files\WSO2\EnterpriseIntegrator\6.5.0\               </code></td>
    </tr>
    <tr class="odd">
    <td>Ubuntu</td>
    <td><code>                /usr/lib/wso2/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    <tr class="even">
    <td>CentOS</td>
    <td><code>                /usr/lib64/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    </tbody>
    </table>

2.  Start the ESB profile:

    -   [**On MacOS/Linux/CentOS**](#d6275da4d79d4b00923ec65fc2cd57e1)
    -   [**On Windows**](#7615894b40c04ea3b2e38a8aec9ed75e)

    Open a terminal and execute the following command:

    ``` java
            wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

3.  Log in to the management console of ESB profile of WSO2 EI using the
    following URL on your browser:  "
    [https://localhost:9443/carbon/](https://10.100.5.65:9443/carbon/)
    ".
4.  Click **Create** under the **Data Service** menu to open the
    **Create Data Service** window.
5.  Enter the following data service name.

    <table>
    <colgroup>
    <col style="width: 40%" />
    <col style="width: 59%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Data Service Name</td>
    <td>CassandraDataService</td>
    </tr>
    </tbody>
    </table>

6.  Leave the default values for the other fields.
7.  Click **Next** to go to the **Datasources** screen

#### Connecting to the datasource

1.  Click **Add New Datasource** . and enter the following details:

    <table style="width:100%;">
    <colgroup>
    <col style="width: 15%" />
    <col style="width: 84%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Datasource ID</td>
    <td>Cassandra</td>
    </tr>
    <tr class="even">
    <td>Datasource Type</td>
    <td>Select <strong>Cassandra</strong> as the datasource.</td>
    </tr>
    <tr class="odd">
    <td>Cassandra Server</td>
    <td>Enter <strong>localhost</strong> .</td>
    </tr>
    </tbody>
    </table>

    The rest of the datasource properties are optional. See the
    descriptions given below.

    ![](images/icons/grey_arrow_down.png){.expand-control-image}
    Properties for a Cassandra datasource

    The following describes the properties supported by the Cassandra
    datasource.

    <table>
    <thead>
    <tr class="header">
    <th>Data Source Property</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Cassandra Servers*</td>
    <td>Host names of Cassandra servers in a comma-separated list.</td>
    </tr>
    <tr class="even">
    <td>Keyspace</td>
    <td>The default key space used by the Cassandra session.</td>
    </tr>
    <tr class="odd">
    <td>Port</td>
    <td>The port of Cassandra servers.</td>
    </tr>
    <tr class="even">
    <td>Cluster Name</td>
    <td>The Cassandra cluster name.</td>
    </tr>
    <tr class="odd">
    <td>Compression</td>
    <td>Compression used in communication.</td>
    </tr>
    <tr class="even">
    <td>User Name</td>
    <td>The authenticating user's username.</td>
    </tr>
    <tr class="odd">
    <td>Password</td>
    <td>The authenticating user's password.</td>
    </tr>
    <tr class="even">
    <td>Load Balancing Policy</td>
    <td>The client load balancing policy on how calls should be made to the provided servers.</td>
    </tr>
    <tr class="odd">
    <td>Enable JMX Reporting</td>
    <td>Enable JMX statistics for the connector.</td>
    </tr>
    <tr class="even">
    <td>Enable Metrics</td>
    <td>Enable metrics for the connector.</td>
    </tr>
    <tr class="odd">
    <td>Local Core Connections Per Host</td>
    <td>Connection pooling: Local core connections per server host.</td>
    </tr>
    <tr class="even">
    <td>Remote Core Connections Per Host</td>
    <td>Connection pooling: Remote core connections per server host.</td>
    </tr>
    <tr class="odd">
    <td><div class="content-wrapper">
    Local Max.Connections Per Host
    </div></td>
    <td>Connection pooling: Maximum local connections per server host.</td>
    </tr>
    <tr class="even">
    <td><div class="content-wrapper">
    Remote Max.Connections Per Host
    </div></td>
    <td>Connection pooling: Remote max connections per server host.</td>
    </tr>
    <tr class="odd">
    <td>Local New Connection Threshold</td>
    <td>This property determines the threshold in the connection pool, which will trigger the creation of a new connection when the connection pool has not reached the <a href="#ExposingCassandraasaDataService-Max_Con_Loc_Host">maximum capacity for local hosts</a> . Generally, it will not be required to change the default value for this parameter.</td>
    </tr>
    <tr class="even">
    <td>Remote New Connection Threshold</td>
    <td>This property determines the threshold in the connection pool, which will trigger the creation of a new connection when the connection pool has not reached the <a href="#ExposingCassandraasaDataService-Rem_Max_Con_Host">maximum capacity for remote hosts</a> . Generally, it will not be required to change the default value for this parameter.</td>
    </tr>
    <tr class="odd">
    <td>Local Max Requests Per Connection</td>
    <td>This property allows you to throttle the number of concurrent requests per connection for local hosts.</td>
    </tr>
    <tr class="even">
    <td>Remote Max Requests Per Connection</td>
    <td>This property allows you to throttle the number of concurrent requests per connection for remote hosts.</td>
    </tr>
    <tr class="odd">
    <td>Protocol Version</td>
    <td>The native protocol version to use. By default, it auto connects. "2" for Cassandra versions 2.0 and upwards, and "1" for Cassandra version 1.2.x.</td>
    </tr>
    <tr class="even">
    <td>Consistency Level</td>
    <td>The consistency level used for queries.</td>
    </tr>
    <tr class="odd">
    <td>Fetch Size</td>
    <td>Fetch size used by queries.</td>
    </tr>
    <tr class="even">
    <td>Serial Consistency Level</td>
    <td>The serial consistency level used for queries.</td>
    </tr>
    <tr class="odd">
    <td>Reconnection Policy</td>
    <td>The reconnection policy used for the cluster.</td>
    </tr>
    <tr class="even">
    <td>Constant Reconnection Policy Delay</td>
    <td>If "ConstantReconnectionPolicy" is used for Reconnection Policy, this represents the constant wait time between reconnection attempts in milliseconds.</td>
    </tr>
    <tr class="odd">
    <td>Exponential Reconnection Policy Base Delay</td>
    <td>If "ExponentialReconnectionPolicy" is used for Reconnection Policy, this represents the base delay in milliseconds.</td>
    </tr>
    <tr class="even">
    <td>Exponential Reconnection Policy Max. Delay</td>
    <td>If "ExponentialReconnectionPolicy" is used for Reconnection Policy, this represents the maximum delay in milliseconds.</td>
    </tr>
    <tr class="odd">
    <td>Retry Policy</td>
    <td>Configured the retry policy in this cluster.</td>
    </tr>
    <tr class="even">
    <td>Connection Timeout</td>
    <td>The socket connection timeout in milliseconds.</td>
    </tr>
    <tr class="odd">
    <td>Keep Alive</td>
    <td>Set if socket keeps alive.</td>
    </tr>
    <tr class="even">
    <td>Read Timeout</td>
    <td>Set per host socket read timeout in milliseconds.</td>
    </tr>
    <tr class="odd">
    <td>Receive Buffer Size</td>
    <td>The socket receive buffer size.</td>
    </tr>
    <tr class="even">
    <td>Send Buffer Size</td>
    <td>Thesocketsendbuffersize.</td>
    </tr>
    <tr class="odd">
    <td>Reuse Address</td>
    <td>The socket re-use address.</td>
    </tr>
    <tr class="even">
    <td>So Linger</td>
    <td>The socket linger on value.</td>
    </tr>
    <tr class="odd">
    <td>TCP no Delay</td>
    <td>Set socket TCP to no delay.</td>
    </tr>
    <tr class="even">
    <td>Enable SSL</td>
    <td>Enable SSL.</td>
    </tr>
    <tr class="odd">
    <td>Enable OData</td>
    <td><p>In WSO2 EI <a href="http://www.odata.org/">OData</a> protocol version 4 (OASIS standards) is supported, which mainly provides support for CRUD operations. You can easily expose databases as an odata service by selecting this check box. Currently, Odata service feature support is available for RDBMS datasources and Cassandra datasources. If you have enabled Odata for your data service, you can complete the data service creation process. That is, you are not required to define queries or operations for the service. This Odata service will now be accessible from the following endpoints:</p>
    <ul>
    <li>For super tenant: <code>                                       http://localhost:/odata/{dataserviceName}/{datasourceId}/                                     </code></li>
    <li>For normal tenants: <code>                                       http://localhost:/odata/t/{tenantId}/{dataserviceName}/{datasourceId}/                                     </code></li>
    </ul></td>
    </tr>
    <tr class="even">
    <td>Disable Native Batch Requests</td>
    <td>Disables native Cassandrabatchrequests,and reverts to emulated batch requests.</td>
    </tr>
    </tbody>
    </table>

    \* Mandatory fields.

#### Creating a query to ADD user information

Now let's write a query for adding user information to the **Users**
table of the **UsersKS** keyspace in your Cassandra server.

1.  Click **Add New Query** and enter the following details:

    <table>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td>Enter <strong>addUsers</strong> as the query ID.</td>
    </tr>
    <tr class="even">
    <td>Datasource</td>
    <td>Select the datasource for which you are going to write a query. Select the <strong>Cassandra</strong> datasource that you created previously.</td>
    </tr>
    <tr class="odd">
    <td>SQL</td>
    <td><div class="content-wrapper">
    <p>In this field, enter the SQL statement describing the data that should be added to the Cassandra datasource.</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>INSERT INTO UsersKS.<span class="fu">Users</span> (id, name, country, age) <span class="fu">values</span> (:id, :name, :country, :age)</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

2.  Click **Generate Input Mapping** to create the input mappings.

    ![](attachments/119130780/119130782.png)  

3.  Edit the **id** column and change the **SQL** **Type** to **UUID** .
4.  Save the mapping.
5.  Edit the **age** column and change the **SQL** **Type** to
    **INTEGER** .
6.  Save the mapping and click **Main Configuration** to return to the
    query. You will now have the following input mappings:  
    ![](attachments/119130780/119130781.png)  
7.  Save the query.

#### Creating a query to GET data

Now let's start writing a query for getting data from the datasource.
The query will specify the data that should be fetched by this query,
and the format that should be used to display data when the query is
invoked.

1.  Click **Add New Query** and enter the following details:

    <table>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td>Enter <strong>getUsersbyID</strong> as the query ID.</td>
    </tr>
    <tr class="even">
    <td>Datasource</td>
    <td>Select the datasource for which you are going to write a query. Select the <strong>Cassandra</strong> datasource that you created previously.</td>
    </tr>
    <tr class="odd">
    <td>SQL</td>
    <td><div class="content-wrapper">
    <p>In this field, enter the SQL statement describing the data that should be retrieved from the Cassandra datasource.</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>SELECT id, age, country, name FROM UsersKS.<span class="fu">Users</span> WHERE id = :id</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

2.  Click **Generate Input Mapping** to create the input mapping. The
    **id** is the input as shown below.

    ![](attachments/119130780/119130784.png)  

3.  Edit the **id** column and change the **SQL Type** to **UUID** .
4.  Save the mapping and click **Main Configuration** to return to the
    query.  
5.  Click **Generate Response** to create the output mapping. This
    defines how the employee details retrieved from the datasource will
    be presented in the result. Note that, by default, the output type
    is XML.

    ![](attachments/119130780/119130783.png)

6.  Save the query.
7.  Click **Next** to open the **Operations** screen.

#### Creating SOAP operationa to invoke the queries

1.  Click **Add New Operation** and enter the following information.  

    |                |            |
    |----------------|------------|
    | Operation Name | addUsersOp |
    | Query ID       | addUsers   |

2.  Save the operation.
3.  Click **Add New Operation** and enter the following information.  

    |                |                |
    |----------------|----------------|
    | Operation Name | getUsersbyIDOp |
    | Query ID       | getUsersbyID   |

4.  Save the operation.

You can now invoke the data service query using SOAP.

#### Creating a REST resource to invoke the query

Now, let's create REST resources to invoke the query created above.
Alternatively, you can create SOAP operations to invoke the queries. See
the [previous
section](#ExposingCassandraasaDataService-CreatingSOAPoperationatoinvokethequeries)
for instructions.

1.  Click **Add New Resource** and enter the following information.  

    |                 |          |
    |-----------------|----------|
    | Resource Path   | users    |
    | Resource Method | POST     |
    | Query ID        | addUsers |

2.  Save the resource.
3.  Click **Add New Resource** and enter the following information.  

    |                 |              |
    |-----------------|--------------|
    | Resource Path   | users/{id}   |
    | Resource Method | GET          |
    | Query ID        | getUsersbyID |

4.  Save the resource.  

You can now invoke the data service query using REST.

#### Finish creating the data service

Once you have defined the operation, click **Finish** to complete the
data service creation process. You will now be taken to the **Deployed
Services** screen, which shows all the data services deployed on the
server.

### Invoking your data service using SOAP

You can try the data service you created by using the TryIt tool that is
in your product by default.

1.  Go to the **Deployed Services** screen.
2.  Click the **Try this service** link for the Cassandra data service.
    The TryIt Tool will open with the data service.

#### Post new data

1.  Select the **addUsersOp** operation you created earlier.
2.  You need to provide the user details. Be sure to enter a UUID value
    as the user ID.

3.  Click **Send** .

The data is now added to the database.

#### Get data

1.  Select the **getUsersbyIDOp** operation you created earlier.
2.  Enter the UUID value that you entered as the user's ID previously.
3.  Click **Send** to see the details of the user you added previously.

  

### Invoking your data service using REST

The HTTP requests sent for each of the resources using cURL would be as
follows:

#### Post new data

1.  Create a file called users-payload.xml file, and define the XML
    payload for posting new data as shown below. Note that the id is a
    UUID value.

    ``` java
            <_postusers>
                <id>a05cdd7a-3d6a-11e8-b467-0ed5f89f718b</id>
                <name>Peter Parker</name>
                <country>USA</country>
                <age>20</age>
            </_postusers>
    ```

2.  Send the following HTTP request from the location where the
    users-payload.xml file is stored:

    ``` java
            curl -X POST -H 'Accept: application/xml'  -H 'Content-Type: application/xml' --data "@users-payload.xml" http://localhost:8280/services/CassandraDataService/users
    ```

#### Get data

The service can be invoked in REST-style via curl (
[http://curl.haxx.se](http://curl.haxx.se/) ). Shown below is the curl
command to invoke the GET resource:

``` java
    curl -X GET http://localhost:8280/services/CassandraDataService.HTTPEndpoint/users/a05cdd7a-3d6a-11e8-b467-0ed5f89f718b
```
