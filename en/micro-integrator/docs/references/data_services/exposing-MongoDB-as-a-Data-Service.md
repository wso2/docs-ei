# Exposing MongoDB as a Data Service

This tutorial will guide you on how to expose data from a MongoDB
datasource as a data service using the ESB profile of WSO2 Enterprise
Integrator (WSO2 EI).

Follow the steps given below. Also, see the samples in [Data Integration
Samples](https://docs.wso2.com/display/EI650/Data+Integration+Samples) .

-   [Install and start
    MongoDB](#ExposingMongoDBasaDataService-InstallandstartMongoDB)
-   [Creating a data
    service](#ExposingMongoDBasaDataService-Creatingadataservice)
    -   [Connecting to the
        datasource](#ExposingMongoDBasaDataService-Connectingtothedatasource)
    -   [Creating a query to POST
        data](#ExposingMongoDBasaDataService-CreatingaquerytoPOSTdata)
    -   [Creating a query to GET
        data](#ExposingMongoDBasaDataService-CreatingaquerytoGETdata)
    -   [Creating SOAP operations to invoke the
        queries](#ExposingMongoDBasaDataService-CreatingSOAPoperationstoinvokethequeries)
    -   [Creating REST resources to invoke the
        queries](#ExposingMongoDBasaDataService-CreatingRESTresourcestoinvokethequeries)
    -   [Finish creating the data
        service](#ExposingMongoDBasaDataService-Finishcreatingthedataservice)
-   [Invoking your data service using
    SOAP](#ExposingMongoDBasaDataService-InvokingyourdataserviceusingSOAP)
-   [Invoking your data service using
    REST](#ExposingMongoDBasaDataService-InvokingyourdataserviceusingREST)

### Install and start MongoDB

A MongoDB server v2.4.x or v2.2.x should be already running in the
default port.

Create a collection as below in the command shell.

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

    -   [**On MacOS/Linux/CentOS**](#68390ab861dd444280697b9ea805b540)
    -   [**On Windows**](#1b64ab8273694c7bb284bbec3eb8f4db)

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

    |                   |         |
    |-------------------|---------|
    | Data Service Name | MongoDB |

6.  Leave the default values for the other fields.
7.  Click **Next** to go to the **Datasources** screen

#### Connecting to the datasource

When you get to the **Add New Data Source** screen, c lick **Add New
Datasource** to open the corresponding screen.

1.  Start by entering the following values:

    <table style="width:100%;">
    <colgroup>
    <col style="width: 27%" />
    <col style="width: 72%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Datasource ID</td>
    <td>MongoDB</td>
    </tr>
    <tr class="even">
    <td>Datasource Type</td>
    <td>MongoDB</td>
    </tr>
    </tbody>
    </table>

2.  You can then specify the connection details to the MongoDB database
    you set up previously. The fields available for the MongoDB
    datasource type are as follows:

    <table style="width:100%;">
    <colgroup>
    <col style="width: 23%" />
    <col style="width: 76%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Servers</td>
    <td>Enter <strong>localhost</strong> . This can be a comma separated list of server hosts and ports where the database is running. <strong>E.g.</strong> : "localhost" - "125.10.5.3, 125.10.5.4" - "192.168.3.1:27017, 192.168.3.2:27017".</td>
    </tr>
    <tr class="even">
    <td>Database Name</td>
    <td>Enter <strong>mydb</strong> . The name of the database to which you want to connect.</td>
    </tr>
    <tr class="odd">
    <td>Write Concern</td>
    <td><div class="content-wrapper">
    <p>Select <strong>NONE</strong> from the list. <strong></strong> The write concern value to control the write behavior as well as exception raising on error conditions:</p>
    <div id="expander-1292938137" class="expand-container">
    <div id="expander-control-1292938137" class="expand-control">
    <img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> Descriptions of Write Concern options
    </div>
    <div id="expander-content-1292938137" class="expand-content">
    <div class="table-wrap">
    <table>
    <thead>
    <tr class="header">
    <th>Option</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>FSYNC_SAFE</td>
    <td>Exceptions are raised for network issues, and server errors. The write operation waits for the server to flush the data to disk.</td>
    </tr>
    <tr class="even">
    <td>NONE</td>
    <td>No exceptions are raised, even for network issues.</td>
    </tr>
    <tr class="odd">
    <td>NORMAL</td>
    <td>Exceptions are raised for network issues, but not server errors.</td>
    </tr>
    <tr class="even">
    <td>REPLICAS_SAFE</td>
    <td>Exceptions are raised for network issues, and server errors; waits for at least 2 servers for the write operation.</td>
    </tr>
    <tr class="odd">
    <td>SAFE</td>
    <td>Exceptions are raised for network issues, and server errors; waits on a server for the write operation.</td>
    </tr>
    </tbody>
    </table>
    </div>
    </div>
    </div>
    </div></td>
    </tr>
    <tr class="even">
    <td>Read Preference</td>
    <td><div class="content-wrapper">
    <p>Select <strong>PRIMARY</strong> from the list. The read preference value, which describes how MongoDB clients route read operations to members of a replica set . It has the following options.</p>
    <div id="expander-2091096157" class="expand-container">
    <div id="expander-control-2091096157" class="expand-control">
    <img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> Descriptions of Read Preference options
    </div>
    <div id="expander-content-2091096157" class="expand-content">
    <p><br />
    </p>
    <p><br />
    </p>
    <div class="table-wrap">
    <table>
    <thead>
    <tr class="header">
    <th>Option</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>PRIMARY</td>
    <td>Default mode. All operations read from the current replica set primary .</td>
    </tr>
    <tr class="even">
    <td>SECONDARY</td>
    <td>All operations read from the secondary members of the replica set.</td>
    </tr>
    </tbody>
    </table>
    </div>
    </div>
    </div>
    </div></td>
    </tr>
    <tr class="odd">
    <td>Auto Connect Retry</td>
    <td>Controls whether or not to connect. That is, the system retries to connect automatically.</td>
    </tr>
    <tr class="even">
    <td>Connection Timeout</td>
    <td>Connection timeout in milliseconds. 0 is default and infinite.</td>
    </tr>
    <tr class="odd">
    <td>Max. Wait</td>
    <td>Max wait time of a blocking thread for a connection <strong>.</strong></td>
    </tr>
    <tr class="even">
    <td>Socket Timeout</td>
    <td>Socket timeout value. 0 is default and infinite.</td>
    </tr>
    <tr class="odd">
    <td>Connections per Host</td>
    <td>If the number of connections allowed per host is exceeded, further connections will be blocked.</td>
    </tr>
    <tr class="even">
    <td>Threads Allowed to Block For Connection Multiplier</td>
    <td>The value in this field, multiplied by the connections per host, gives the maximum number of threads that may be waiting for a connection to become available from the pool. All further threads will get an exception. For example, if connections per host are 10 and the 'threads allowed to block for connection multiplier' is 5, up to 50 threads can wait for a connection.</td>
    </tr>
    </tbody>
    </table>

#### Creating a query to POST data

Follow the steps given below.

1.  Click **Add New Query** and enter the following details:

    <table>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td>Enter <strong>mongo_insert</strong> as the query ID.</td>
    </tr>
    <tr class="even">
    <td>Datasource</td>
    <td>Select the datasource for which you are going to write a query. Select the <strong>mongo</strong> datasource that you created previously.</td>
    </tr>
    <tr class="odd">
    <td>SPARQL</td>
    <td><div class="content-wrapper">
    <p>In this field, enter the SPARQL query describing the data that should be retrieved from the RDF datasource. We will use the following query:<br />
    </p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>things.<span class="fu">insert</span>(<span class="st">&quot;{id:#, name:#}&quot;</span>)</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

2.  Now let's specify input mappings for the data in the MongoDB
    database. We will create output mappings for the id and name fields
    .  

    1.  Click **Add New Input Mapping** to start creating the input
        mapping. We want to add data into the database using the **id**
        and **name** fields. First, create the input mapping for **id**
        .  
    2.  Click **Add** to save the input mapping.
    3.  Create the input mapping for the **name** field.  
          

3.  Save the query.

#### Creating a query to GET data

Now, let's start writing a query for getting data from the MongoDB
datasource. The query will specify the data that should be fetched by
this query, and the format that should be used to display data when the
query is invoked.

Click **Add New Query** , and enter the following data:

<table>
<tbody>
<tr class="odd">
<td>Query ID</td>
<td>Enter <strong>mongo_find</strong> as the query ID.</td>
</tr>
<tr class="even">
<td>Datasource</td>
<td>Select the datasource for which you are going to write a query. Select the <strong>mongo</strong> datasource that you created previously.</td>
</tr>
<tr class="odd">
<td>SPARQL</td>
<td><div class="content-wrapper">
<p>In this field, enter the SPARQL query describing the data that should be retrieved from the MongoDB datasource. We will use the following query:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>things.<span class="fu">find</span>()</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

Define **Output Mapping** : Now, let's specify how the data fetched from
the datasource should be displayed in the output. We will create output
mappings for the **Data** field .  

1.  Start by giving the following information:  

    <table style="width:100%;">
    <colgroup>
    <col style="width: 16%" />
    <col style="width: 83%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Output type</td>
    <td>Specify the format in which the query results should be presented. You can select XML, JSON or RDF. We will use <strong>XML</strong> for this tutorial.</td>
    </tr>
    <tr class="even">
    <td>Grouped by element</td>
    <td>Specify a grouping for all the output mappings. This will be the XML element that will group the query result. Enter <strong>Documents</strong> in this field.</td>
    </tr>
    <tr class="odd">
    <td>Row Name</td>
    <td>Specify the XML element that should group each individual result. Enter <strong>Document</strong> in this field.</td>
    </tr>
    </tbody>
    </table>

2.  Click **Add New Output Mapping** to start creating the output
    mapping. Enter the following values:  
    ![](attachments/53119196/56993524.png){width="527" height="250"}
3.  Click **Main Configuration** to return to the **Query** screen.  

Save the query, and click **Next** to go to the **Operations** screen.

#### Creating SOAP operations to invoke the queries

To invoke the query, you need to define an operation.

1.  Click **Add New Operation** and enter the following information.  

    |                |                   |
    |----------------|-------------------|
    | Operation Name | mongo\_insert\_op |
    | Query ID       | mongo\_insert     |

2.  Save the operation.
3.  Click **Add New Operation** and enter the following information.  

    |                |                 |
    |----------------|-----------------|
    | Operation Name | mongo\_find\_op |
    | Query ID       | mongo\_find     |

4.  Save the operation.  

You can now invoke the data service query using SOAP.

#### Creating REST resources to invoke the queries

Now, let's create REST resources to invoke the query created above.
Alternatively, you can create SOAP operations to invoke the queries. See
the [previous section](_Exposing_MongoDB_as_a_Data_Service_) for
instructions.

1.  Click **Add New Resource** and enter the following information.  

    |                 |               |
    |-----------------|---------------|
    | Resource Path   | insert/{id}   |
    | Resource Method | POST          |
    | Query ID        | mongo\_insert |

2.  Save the resource.
3.  Click **Add New Resource** and enter the following information.  

    |                 |             |
    |-----------------|-------------|
    | Resource Path   | find        |
    | Resource Method | GET         |
    | Query ID        | mongo\_find |

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
2.  Click the **Try this service** link for the MongoDB data service.
    The **TryIt Tool** will open with the MongoDB service.  
3.  Select the **mongo\_insert\_op** operation and enter values for
    **ID** and **Name** .  
4.  Click **Send** to execute the operation. The data will be inserted
    to the database.
5.  Now, select the **mongo\_find\_op** operation and click **Send** .
    The response will be published in the TryIt tool

### Invoking your data service using REST

You can send an HTTP GET request to invoke the data service using cURL
as shown below.

``` java
    curl -X GET http://localhost:8280/services/MongoDB.HTTPEndpoint/find
```

This will return the response in XML.
