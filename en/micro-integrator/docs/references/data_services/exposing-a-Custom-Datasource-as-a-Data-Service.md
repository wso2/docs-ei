# Exposing a Custom Datasource as a Data Service

See the topics given below to expose a custom datasource as a data
service. Also, see the samples in [Data Integration
Samples](https://docs.wso2.com/display/EI650/Data+Integration+Samples)
.  

-   [About custom
    datasources](#ExposingaCustomDatasourceasaDataService-Aboutcustomdatasources)
-   [Adding a custom tabular datasource to the data
    service](#ExposingaCustomDatasourceasaDataService-Addingacustomtabulardatasourcetothedataservice)
    -   [Connecting to the
        datasource](#ExposingaCustomDatasourceasaDataService-Connectingtothedatasource)
    -   [Creating a query to GET
        data](#ExposingaCustomDatasourceasaDataService-CreatingaquerytoGETdata)
    -   [Creating a SOAP operation to invoke the
        query](#ExposingaCustomDatasourceasaDataService-CreatingaSOAPoperationtoinvokethequery)
    -   [Creating a REST resource to invoke the
        query](#ExposingaCustomDatasourceasaDataService-CreatingaRESTresourcetoinvokethequery)
    -   [Finish creating the data
        service](#ExposingaCustomDatasourceasaDataService-Finishcreatingthedataservice)
    -   [Invoking the custom tabular data service using
        SOAP](#ExposingaCustomDatasourceasaDataService-InvokingthecustomtabulardataserviceusingSOAP)
    -   [Invoking the custom tabular data service using
        REST](#ExposingaCustomDatasourceasaDataService-InvokingthecustomtabulardataserviceusingREST)
-   [Adding a custom query datasource to the data
    service](#ExposingaCustomDatasourceasaDataService-Addingacustomquerydatasourcetothedataservice)
    -   [Connecting to the
        datasource](#ExposingaCustomDatasourceasaDataService-Connectingtothedatasource.1)
    -   [Creating a query to GET
        data](#ExposingaCustomDatasourceasaDataService-CreatingaquerytoGETdata.1)
    -   [Creating a SOAP operation to invoke the
        query](#ExposingaCustomDatasourceasaDataService-CreatingaSOAPoperationtoinvokethequery.1)
    -   [Creating a REST resource to invoke the
        query](#ExposingaCustomDatasourceasaDataService-CreatingaRESTresourcetoinvokethequery.1)
    -   [Finish creating the data
        service](#ExposingaCustomDatasourceasaDataService-Finishcreatingthedataservice.1)
    -   [Invoking the custom query data service using
        SOAP](#ExposingaCustomDatasourceasaDataService-InvokingthecustomquerydataserviceusingSOAP)
    -   [Invoking your custom query data service using
        REST](#ExposingaCustomDatasourceasaDataService-InvokingyourcustomquerydataserviceusingREST)

### About custom datasources

Custom datasources allow you to interface data services with your own
datasource implementation. There are two options for writing a custom
datasource, and these two options cover most of the common business use
cases as follows:

-   **Custom tabular datasources:** Used to represent data in tables,
    where a set of named tables contain data rows that can be queried
    later. A tabular datasource is typically associated with an SQL data
    services query. This is done by internally using our own SQL parser
    to execute SQL against the custom datasource. You can use the
    `                     org.wso2.carbon.dataservices.core.custom.datasource.TabularDataBasedDS                   `
    interface to implement tabular datasources. For a sample
    implementation of a tabular custom datasource, see
    `                     org.wso2.carbon.dataservices.core.custom.datasource.InMemoryDataSource                   `
    . Also, this is supported in Carbon datasources with the following
    datasource reader implementation:
    `          org.wso2.carbon.dataservices.core.custom.datasource.CustomTabularDataSourceReader         `
    .
-   **Custom query datasources:** Used when the datasource has some form
    of query expression support. Custom query datasources are
    implemented using the
    `                     org.wso2.carbon.dataservices.core.custom.datasource.CustomQueryBasedDS                   `
    interface. You can create any non-tabular datasource using the
    query-based approach. Even if the target datasource does not have a
    query expression format, you can create your own. For example, you
    can support any NoSQL type datasource this way. For a sample
    implementation of a query-based custom datasource, see
    `                     org.wso2.carbon.dataservices.core.custom.datasource.EchoDataSource                   `
    . This is supported in Carbon datasources with the following
    datasource reader implementation:
    `          org.wso2.carbon.dataservices.core.custom.datasource.CustomQueryDataSourceReader         `
    .

In the `         init        ` methods of all custom datasources,
user-supplied properties are parsed to initialize the datasource
accordingly. Also, a property named `         __DATASOURCE_ID__        `
, which contains a UUID to uniquely identify the current datasource, is
passed.Custom datasource authors use this to identify
the datasources accordingly. For example, scenarios
like datasource instances communicating within a server cluster for data
synchronisation.

!!! info

Find the custom connectors used from the github project located at
[https://github.com/wso2/wso2-dss-connectors.](https://github.com/wso2/wso2-dss-connectors/)


### Adding a custom tabular datasource to the data service

Follow the steps given below to create a custom tabular datasource.'

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

    -   [**On MacOS/Linux/CentOS**](#1e7367c1b962444da0a0b0ed885c7190)
    -   [**On Windows**](#8afb2809bb5241c4af4cfbf4f9b70153)

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

    |                   |                          |
    |-------------------|--------------------------|
    | Data Service Name | CustomTabularDataService |

6.  Leave the default values for the other fields.

#### Connecting to the datasource

1.  Click **Add New Datasource** , and enter the following details:

    |                         |                                                                                                                                                                       |
    |-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Datasource ID           | CustomTabular                                                                                                                                                         |
    | Datasource Type         | Custom Datasource                                                                                                                                                     |
    | Custom Datasource Class | Enter the following interface: `                               org.wso2.carbon.dataservices.core.custom.datasource.InMemoryDataSource                             ` . |

2.  Click **Add New Property** to enter properties that will identify
    the data in the datasource.  

    <table>
    <thead>
    <tr class="header">
    <th>Property</th>
    <th>Value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><pre><code>inmemory_datasource_schema</code></pre></td>
    <td><pre><code>{Users:[ID,Name]}</code></pre></td>
    </tr>
    <tr class="even">
    <td><pre><code>inmemory_datasource_records</code></pre></td>
    <td>{Users:[["1","Will Smith"],["2","Denzel Washington"]]</td>
    </tr>
    </tbody>
    </table>

3.  Save the datasource.
4.  Click **Next** to go to the **Queries** screen.

#### Creating a query to GET data

1.  Click **Add New Query** to start defining a query. We will write a
    query to find and display all users in the datasource.

    <table>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td>getAllUsers</td>
    </tr>
    <tr class="even">
    <td>Datasource</td>
    <td>CustomTabular</td>
    </tr>
    <tr class="odd">
    <td>SQL</td>
    <td><div class="content-wrapper">
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>SELECT ID,<span class="bu">Name</span> FROM Users</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

2.  Now let's specify output mappings for the data. We will create
    output mappings for the following data: **ID** and **Name** .
    1.  Start by giving the following information:

        |                    |                                                                                                                                                   |
        |--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
        | Output type        | You can select XML, JSON or RDF. We will use **XML** for this tutorial. This specifies the format in which the query results should be presented. |
        | Grouped by element | Enter **Users** . This will be the XML element that will group the query result.                                                                  |
        | Row Name           | Enter **User** . This is the XML element that should group each individual result.                                                                |

    2.  Click **Add New Output Mapping** to start creating the output
        mapping for the **ID** field.
    3.  Add another output mapping for the **Name** column.
    4.  You will now have the following output mappings listed for the
        **getAllUsers** query:  
        ![](attachments/119130764/119130776.png){width="943"
        height="117"}
    5.  Click **Main Configuration** to return to the **Query** screen.

3.  Click **Next** to go to the **Operations** screen.

#### Creating a SOAP operation to invoke the query

To invoke the query, you need to define an operation.

1.  Click **Add New Operation** and enter the following information.  

    |                |               |
    |----------------|---------------|
    | Operation Name | GetAllUsersOp |
    | Query ID       | getAllUsers   |

2.  Save the operation.

#### Creating a REST resource to invoke the query

Now, let's create REST resources to invoke the query created above.
Alternatively, you can create SOAP operations to invoke the queries. See
the [previous section](_Exposing_a_Custom_Datasource_as_a_Data_Service_)
for instructions.

1.  Click **Add New Resource** and enter the following information.  

    |                 |             |
    |-----------------|-------------|
    | Resource Path   | Users       |
    | Resource Method | GET         |
    | Query ID        | getAllUsers |

2.  Save the resource.

You can now invoke the data service query using REST.

#### Finish creating the data service

Once you have defined the operation, click **Finish** to complete the
data service creation process. You will now be taken to the **Deployed
Services** screen, which shows all the data services deployed on the
server.

#### Invoking the custom tabular data service using SOAP

You can try the data service you created by using the TryIt tool that is
in your product by default.

1.  Go to the **Deployed Services** screen.
2.  Click the **Try this service** link for the **CustomTabular** data
    service. The TryIt Tool will open with the data service.  
3.  Select the getAllUsersOp operation.
4.  Click **Send** to see the result:  

    ``` java
            <Users xmlns="http://ws.wso2.org/dataservice">
               <User>
                  <ID>1</ID>
                  <Name>Will Smith</Name>
               </User>
               <User>
                  <ID>2</ID>
                  <Name>Denzel Washington</Name>
               </User>
            </Users>
    ```

#### Invoking the custom tabular data service using REST

You can send an HTTP GET request to invoke the data service using cURL
as shown below.

``` java
    curl -X GET http://localhost:8280/services/CustomTabularDataService.HTTPEndpoint/Users
```

This will return the response in XML.

### Adding a custom query datasource to the data service

Follow the steps given below to create a custom query datasource.

1.  Log in to the management console of ESB profile of WSO2 EI using the
    following URL on your browser:  "
    [https://localhost:9443/carbon/](https://10.100.5.65:9443/carbon/)
    ".
2.  Click **Create** under the **Data Service** menu to open the
    **Create Data Service** window.
3.  Enter the following data service name.

    |                   |                        |
    |-------------------|------------------------|
    | Data Service Name | CustomQueryDataService |

4.  Leave the default values for the other fields.
5.  Click **Next** to go to the **Datasources** screen.

#### Connecting to the datasource

1.  Click **Add New Datasource** , and enter the values shown below.

    |                          |                                                                                                                                                                   |
    |--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Datasource ID            | CustomQuery                                                                                                                                                       |
    | Datasource Type          | Custom Datasource                                                                                                                                                 |
    | Custom Query Data Source | Select this check box.                                                                                                                                            |
    | Custom Datasource Class  | Enter the following interface: `                               org.wso2.carbon.dataservices.core.custom.datasource.EchoDataSource                             ` . |

2.  Click Add New Property to enter properties that will identify the
    data in the datasource.

    <table>
    <thead>
    <tr class="header">
    <th>Property</th>
    <th>Value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><pre><code>prop1</code></pre></td>
    <td><pre><code>prop_value1</code></pre></td>
    </tr>
    <tr class="even">
    <td><pre><code>prop2</code></pre></td>
    <td><pre><code>prop_value2</code></pre></td>
    </tr>
    </tbody>
    </table>

    ``` java
            <property name="prop1">prop_value1</property>
            <property name="prop2">prop_value2</property>
    ```

3.  Save the datasource.  
4.  Click **Next** to go to the **Queries** screen.

#### Creating a query to GET data

1.  Click **Add New Query** to open the **Add New Query** screen and
    enter the following details.

    <table>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td>getValues</td>
    </tr>
    <tr class="even">
    <td>Datasource</td>
    <td>CustomQuery</td>
    </tr>
    <tr class="odd">
    <td>Expression</td>
    <td><div class="content-wrapper">
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>column1,column2;R1C1 :param1,R1C2 :param2;R2C1 :param1,R2C2 :param2</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

2.  Now let's specify input mappings to enter data. We will create input
    mappings for the fields given in the expression above: **Column1**
    and **Column2**
    1.  Click **Add New Input Mapping** to open the respective screen.
        Define a mapping for column 1.
    2.  Add another input mapping for column 2. You will now have two
        input mappings for the two columns as shown below.  
        ![](attachments/119130764/119130778.png){width="720"
        height="131"}
3.  Now let's specify **output mappings** , which will determine how the
    result from your query will be presented when the query is
    invoked. The sample d atasource we are using contains the following
    columns: **Column 1** and **Column 2** . We will create an output
    mapping for each of these columns.
    1.  Start by entering the following values:

        |                    |                                                                                                                                                   |
        |--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
        | Output type        | You can select XML, JSON or RDF. We will use **XML** for this tutorial. This specifies the format in which the query results should be presented. |
        | Grouped by element | Enter **Rows** . This will be the XML element that will group the query result.                                                                   |
        | Row Name           | Enter **Row** . This is the XML element that should group each individual result.                                                                 |

    2.  Click **Add New Output Mapping** to start creating the output
        mapping for column. Enter a mappings for C1 and C1.
    3.  Click **Add** to save the output mapping. You will now have one
        output mapping listed for the **Q1** query.
    4.  Create output mappings for the remaining columns given below.  
        ![](attachments/119130764/119130768.png){width="897"
        height="128"}

    5.  Click **Main Configuration** to return to the **Query** screen.

4.  Click **Next** to go to the **Operations** screen.

#### Creating a SOAP operation to invoke the query

To invoke the query, you need to define an operation.

1.  Click **Add New Operation** and enter the following information.  

    |                |             |
    |----------------|-------------|
    | Operation Name | GetValuesOp |
    | Query ID       | getValues   |

2.  Save the operation.

You can now invoke the data service query using SOAP.

#### Creating a REST resource to invoke the query

Now, let's create REST resources to invoke the query created above.
Alternatively, you can create SOAP operations to invoke the queries. See
the [previous section](_Exposing_a_Custom_Datasource_as_a_Data_Service_)
for instructions.

1.  Click **Add New Resource** and enter the following information.  

    |                 |                  |
    |-----------------|------------------|
    | Resource Path   | Values/{column1} |
    | Resource Method | GET              |
    | Query ID        | getValues        |

2.  Save the resource.

#### Finish creating the data service

Once you have defined the operation, click **Finish** to complete the
data service creation process. You will now be taken to the **Deployed
Services** screen, which shows all the data services deployed on the
server.

#### Invoking the custom query data service using SOAP

You can try the data service you created by using the TryIt tool that is
in your product by default.

1.  Go to the **Deployed Services** screen.
2.  Click the **Try this service** link for the
    **CustomQueryDataService** data service. The TryIt Tool will open
    with the data service.  
3.  Select the getValuesOp operation.
4.  Enter any values for the input parameters as shown below.

    ``` java
            <!--Exactly 1 occurrence-->
                  <xs:column1 xmlns:xs="http://ws.wso2.org/dataservice">1</xs:column1>
                  <!--Exactly 1 occurrence-->
                  <xs:column2 xmlns:xs="http://ws.wso2.org/dataservice">2</xs:column2
    ```

5.  Click **Send** to see the result:  

    ``` java
            <Rows xmlns="http://ws.wso2.org/dataservice">
               <Row>
                  <C1>R1C1 :param1</C1>
                  <C2>R1C2 :param2</C2>
               </Row>
               <Row>
                  <C1>R2C1 :param1</C1>
                  <C2>R2C2 :param2</C2>
               </Row>
            </Rows>
    ```

#### Invoking your custom query data service using REST

You can send an HTTP GET request to invoke the data service using cURL
as shown below.

``` java
    curl -X GET http://localhost:8280/services/CustomQueryDataService.HTTPEndpoint/values/1
```

This will return the response in XML.
