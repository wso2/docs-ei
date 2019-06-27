# Exposing Excel Data as a Data Service

This tutorial guides you on how to expose the data in an Excel sheet as
a data service. We will create a data service that can search for data
on the file and insert data into the file.

!!! info

WSO2 EI uses the Apache POI library version 3.9.0 to work with Excel
datasources, and thereby, supports both XLS and XLSX formats. For more
information, see the [Apache POI Documentation](index) .


To demonstrate this feature, we will use the
`         Products.xls        ` file that is shipped with WSO2 EI. The
`         Products.xls        ` file is stored in the
`         <EI_HOME>/samples/data-services/resources/        ` folder and
it contains data about products ( cars/motorcycles) that are
manufactured in an automobile company. The data table has the following
columns: `         ID        ` , `         Model        ` , and
`         Classification        ` .

-   [Creating the data
    service](#ExposingExcelDataasaDataService-Creatingthedataservice)
    -   [Connecting to the
        datasource](#ExposingExcelDataasaDataService-Connectingtothedatasource)
    -   [Define a query for the
        datasource](#ExposingExcelDataasaDataService-Defineaqueryforthedatasource)
    -   [Create a SOAP operation to invoke the
        queries](#ExposingExcelDataasaDataService-CreateaSOAPoperationtoinvokethequeries)
    -   [Creating a REST resource to invoke the
        query](#ExposingExcelDataasaDataService-CreatingaRESTresourcetoinvokethequery)
    -   [Finish creating the data
        service](#ExposingExcelDataasaDataService-Finishcreatingthedataservice)
-   [Invoking your data service using
    SOAP](#ExposingExcelDataasaDataService-InvokingyourdataserviceusingSOAP)
-   [Invoking your data service using
    REST](#ExposingExcelDataasaDataService-InvokingyourdataserviceusingREST)

### Creating the data service

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

    -   [**On MacOS/Linux/CentOS**](#5cf9919ea1784afaa70e1a598a5d9f9f)
    -   [**On Windows**](#035b047876e742f0a3ca4d6eebc8c63b)

    Open a terminal and execute the following command:

    ``` java
        wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

3.  Access the ESB profile's management console using the following URL:
    `          https://localhost:9443/carbon/         ` .
4.  Sign in using `          admin         ` as the username and
    password.
5.  Click **Create** under **Data Service** to open the **Create Data
    Service** window.
6.  Enter the following data service name.

    |                   |       |
    |-------------------|-------|
    | Data Service Name | Excel |

7.  Leave the default values for the other fields.
8.  Click **Next** to go to the **Datasources** screen.

#### Connecting to the datasource

Follow the steps given below to add an Excel file as the datasource.

1.  Click **Add New Datasource** and enter the following details:

    <table style="width:100%;">
    <colgroup>
    <col style="width: 13%" />
    <col style="width: 86%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Datasource ID</td>
    <td><code>               Excel              </code></td>
    </tr>
    <tr class="even">
    <td>Datasource Type</td>
    <td><code>               EXCEL              </code></td>
    </tr>
    <tr class="odd">
    <td>Excel URL</td>
    <td>Enter <code>               ./samples/data-services/resources/Products.xls              </code> .<br />
    In this tutorial, we are using a sample excel file that is stored in the above location of your product pack.</td>
    </tr>
    <tr class="even">
    <td>Use Query Mode</td>
    <td><p>Select <strong>Use Query Mode</strong> to enable it or keep it deselected to disable query mode. You can try out this tutorial using either option.<br />
    The <strong>Use Query Mode</strong> option allows you to write queries for the datasource in two different ways.</p>
    <ul>
    <li><p>If query mode is <strong>enabled</strong> , you can write SQL queries for the excel sheet.</p></li>
    <li><p>If query mode is <strong>disabled</strong> , you can get data from the excel datasource without an SQL query.</p></li>
    </ul>
        !!! info
        <p>See the next section on defining queries for more information on how the query mode affects how you query data in the excel sheet.</p>
    </td>
    </tr>
    </tbody>
    </table>

2.  Save the datasource.
3.  Click **Next** to go to the **Queries** screen.

#### Define a query for the datasource

-   [**Query mode disabled**](#4b495dd57b9e4d2e93e2cbd9513a3f74)
-   [**Query mode enabled**](#1fc1b09c9c2844278b7ecea3d388dd84)

If ****Query Mode is** disabled** for the spreadsheet, you cannot write
SQL statements to query the spreadsheet. Instead, you need to specify
the query details by filling in the data extraction parameters. This
also means that, when query mode is disabled, you cannot use the data
service to insert, update, or modify data in the spreadsheet. This data
service can only be used to get data that is already stored in the
spreadsheet.

Follow the steps given below:

1.  Click **Add New Query** and add the following details:

    |                    |                                                                                                                                                                            |
    |--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Query ID           | `                    GetProducts                   `                                                                                                                       |
    | Datasource Type    | `                    EXCEL                   `                                                                                                                             |
    | Workbook Name      | `                    Sheet1                                       ` You want the query to get the data from Sheet1 in the excel spreadsheet.                               |
    | Start reading from | `                    2                                       ` You enter 2 because the 1st row of the spreadsheet is the header. The data is available from row 2 onwards. |
    | Rows to read       | `                    5                                       ` Five rows of data will be fetched by the query.                                                             |
    | Headers available  | `                    true                   `                                                                                                                              |
    | Header row         | `                    1                                       ` You enter 1 because the 1st row of the spreadsheet includes the header.                                     |

2.  Define **Output Mapping** :  
    Now, let's specify how the data fetched from the datasource should
    be displayed in the output. The Excel datasource we are using
    contains three columns: ID, Model, and Classification. We will
    create an output mapping for each of these columns.
    1.  Start by giving the following information:

        <table>
        <tbody>
        <tr class="odd">
        <td>Output type</td>
        <td>Select <strong>XML</strong> .<br />
        You can select XML, JSON, or RDF. This specifies the format in which the query results should be presented.</td>
        </tr>
        <tr class="even">
        <td>Grouped by element</td>
        <td>Enter <strong>Products</strong> .<br />
        This is the XML element that groups the query result.</td>
        </tr>
        <tr class="odd">
        <td>Row Name</td>
        <td>Enter <strong>Product</strong> .<br />
        This is the XML element that should group each individual result.</td>
        </tr>
        </tbody>
        </table>

    2.  Click **Add New Output Mapping** to start creating the output
        mapping. Listed below are the output mappings that should be
        created.  
        Fill in the details and click **Add** to add each output
        mapping.

        | Mapping Type                                         | Element Name                                                | Datasource Type                                     | Datasource Column Name                                      | Parameter Type                                      | Schema Type                                         |
        |------------------------------------------------------|-------------------------------------------------------------|-----------------------------------------------------|-------------------------------------------------------------|-----------------------------------------------------|-----------------------------------------------------|
        | `                      Element                     ` | `                      ID                     `             | `                      Column                     ` | `                      ID                     `             | `                      SCALAR                     ` | `                      string                     ` |
        | `                      Element                     ` | `                      Model                     `          | `                      Column                     ` | `                      Model                     `          | `                      SCALAR                     ` | `                      string                     ` |
        | `                      Element                     ` | `                      Classification                     ` | `                      Column                     ` | `                      Classification                     ` | `                      SCALAR                     ` | `                      string                     ` |

3.  Once you have created all of the above mappings, click **Main
    Configuration** to return to the **Query** screen.
4.  Save the query.

If **Query Mode is enabled** for the datasource, follow the steps given
below.

-   [Creating a query to GET
    data](#ExposingExcelDataasaDataService-CreatingaquerytoGETdata)
-   [Creating a query to POST
    data](#ExposingExcelDataasaDataService-CreatingaquerytoPOSTdata)

##### Creating a query to GET data

1.  Click **Add New Query** and add the following details.

    <table>
    <colgroup>
    <col style="width: 15%" />
    <col style="width: 84%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td><code>                    GetProductbyID                   </code></td>
    </tr>
    <tr class="even">
    <td>Datasource</td>
    <td><code>                    EXCEL                   </code></td>
    </tr>
    <tr class="odd">
    <td>SQL</td>
    <td><div class="content-wrapper">
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>select ID, Model, Classification from Sheet1 where ID=:ID</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

2.  Click **Generate Input Mapping** , and the default input mappings
    will be generated for all the fields that you have specified in your
    SQL statement.  
    You need to create input mappings for the ID, Model and
    Classification columns specified in the above SQL statement. These
    input mapping parameters will be used for inserting data into the
    relevant columns.
3.  Click **Generate Response** to create the output mapping.  
    This defines how the employee details retrieved from the datasource
    will be presented in the result. Note that, by default, the output
    type is XML.
4.  Save the query.

##### Creating a query to POST data

You can query data in the spreadsheet using SQL statements. At the
moment, the query mode supports only basic
`              SELECT             ` ,
`              INSERT             ` ,
`              UPDATE             ` , and
`              DELETE             ` queries. Nested queries will be
supported in an upcoming release. The
`              org.wso2.carbon.dataservices.sql.driver.TDriver             `
class is used internally as the SQL Driver. It is a JDBC driver
implementation used with tabular data models such as Google
spreadsheets, Excel sheets, etc.

Follow the steps given below.

1.  Click **Add New Query** and add the following details.

    <table>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td><code>                    AddProducts                   </code></td>
    </tr>
    <tr class="even">
    <td>Datasource</td>
    <td><code>                    EXCEL                   </code></td>
    </tr>
    <tr class="odd">
    <td>SQL</td>
    <td><div class="content-wrapper">
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>INSERT INTO <span class="fu">sheet1</span> (ID, Model, Classification) <span class="fu">VALUES</span>(:ID, :Model, :Classification)</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

2.  Click **Generate Input Mapping** , and the default input mappings
    will be generated for all the fields that you have specified in your
    SQL statement.  
    You need to create input mappings for the ID, Model, and
    Classification columns specified in the above SQL statement. These
    input mapping parameters will be used for inserting data into the
    relevant columns.
3.  Save the query.

#### Create a SOAP operation to invoke the queries

To invoke the query, you need to define an operation.

-   [**Query mode disabled**](#644d846c43d54eacb1aae2b89f6c5c60)
-   [**Query mode enabled**](#d9c014efcdff435eb32031d445daed45)

To Follow the steps given below

1.  Click **Add New Operation** and enter the following information.

    |                |                                                      |
    |----------------|------------------------------------------------------|
    | Operation Name | `                    GetProducts                   ` |
    | Query ID       | `                    GetProducts                   ` |

2.  Save the operation.

You can now invoke the data service query using SOAP.

To invoke the query, you need to define an operation.

1.  Click **Add New Operation** and enter the following information.

    |                |                                                            |
    |----------------|------------------------------------------------------------|
    | Operation Name | `                    GetProductsbyIDOp                   ` |
    | Query ID       | `                    GetProductsbyID                   `   |

2.  Save the operation.
3.  Click **Add New Operation** and enter the following information.

    |                |                                                        |
    |----------------|--------------------------------------------------------|
    | Operation Name | `                    AddProductsOp                   ` |
    | Query ID       | `                    AddProducts                   `   |

4.  Save the operation.

You can now invoke the data service query using SOAP.

#### Creating a REST resource to invoke the query

Now, let's create REST resources to invoke the query created above.
Alternatively, you can create SOAP operations to invoke the queries. See
the [previous section](_Exposing_Excel_Data_as_a_Data_Service_) or
instructions.

-   [**Query mode disabled**](#5075f2fff1dc438db8c764587a8df2d1)
-   [**Query mode enabled**](#4d4f3c9284ac4c5b8960520f17d2f188)

Follow the steps given below:

1.  Click **Add New Resource** and enter the following information.

    |                 |                                                      |
    |-----------------|------------------------------------------------------|
    | Resource Path   | `                    Products                   `    |
    | Resource Method | `                    GET                   `         |
    | Query ID        | `                    GetProducts                   ` |

2.  Save the resource.

You can now invoke the data service query using REST.

Follow the steps given below:

1.  Click **Add New Resource** and enter the following information.

    |                 |                                                          |
    |-----------------|----------------------------------------------------------|
    | Resource Path   | `                    Products/{ID}                   `   |
    | Resource Method | `                    GET                   `             |
    | Query ID        | `                    GetProductsbyID                   ` |

2.  Save the resource.
3.  Click **Add New Resource** and enter the following information.

    |                 |                                                      |
    |-----------------|------------------------------------------------------|
    | Resource Path   | `                    Products                   `    |
    | Resource Method | `                    POST                   `        |
    | Query ID        | `                    AddProducts                   ` |

4.  Save the resource.

You can now invoke the data service query using REST.

#### Finish creating the data service

Once you have defined the operation, click **Finish** to complete the
data service creation process.  
You will now be taken to the **Deployed Services** screen, which shows
all the data services deployed on the server.

### Invoking your data service using SOAP

You can try the data service you created by using the TryIt tool that is
in your product by default.

1.  Go to the Deployed Services screen.
2.  Click the **Try this service** link for the Excel data service. The
    TryIt tool will open with the Excel data service.
3.  Run the operation.

    -   [**Query mode disabled**](#1fcb6a97d7c541adab1c17c738073824)
    -   [**Query mode enabled**](#60f7eba1ec54467ab70a3bf0be712d96)

    Select the **GetProducts** operation your created earlier and click
    Send.  
    You see an output similar to the following:  
    ![](attachments/119130726/119130727.png){width="900"}

    **Invoking the AddProductsOp operation**

    This operation can insert data into the excel sheet, you will need
    to specify the values that should be inserted.

    1.  Click **AddProductsOp** .

    2.  Provide the product details by copying the code given below.

        ``` java
                <p:AddProductsOp xmlns:p="http://ws.wso2.org/dataservice">
                 <!--Exactly 1 occurrence-->
                 <xs:ID xmlns:xs="http://ws.wso2.org/dataservice">S310_5686</xs:ID>
                 <!--Exactly 1 occurrence-->
                 <xs:Model xmlns:xs="http://ws.wso2.org/dataservice">1972 Alfa Romeo GTA</xs:Model>
                 <!--Exactly 1 occurrence-->
                 <xs:Classification xmlns:xs="http://ws.wso2.org/dataservice">Classic Cars</xs:Classification>
                </p:AddProductsOp>
        ```

    3.  Click Send.

    **Invoking the GetProductsbyIDOp operation**

    This operation can get details of a product. You need to define the
    product ID for the data to be fetched from the excel sheet. **  
    **

    -   1.  Click **GetProductsbyIDOp** .

        2.  Provide the product ID by copying the code given below. To
            confirm that the data you added above was included in the
            spreadsheet, let's use the same ID.

            ``` java
                        <p:_getproducts_id xmlns:p="http://ws.wso2.org/dataservice">
                           <!--Exactly 1 occurrence-->
                           <xs:ID xmlns:xs="http://ws.wso2.org/dataservice">S310_5686</xs:ID>
                        </p:_getproducts_id>
            ```

        3.  Click **Send** .

### Invoking your data service using REST

You can send an HTTP GET request to invoke the data service using a curl
command as shown below.

-   [**Query mode disabled**](#6dc816bb3b92428299707b2a0314effe)
-   [**Query mode enabled**](#92bca44528f6478cb0d549c757e06366)

Run the following curl command to get the product details:

``` java
    curl -X GET http://localhost:8280/services/Excel.HTTPEndpoint/Products
```

You get an output similar to the following:

``` java
    <Products xmlns="http://ws.wso2.org/dataservice"><Product><ID>S10_1678</ID><Model>1969 Harley Davidson Ultimate Chopper</Model>
    <Classification>Motorcycles</Classification></Product><Product><ID>S10_1949</ID><Model>1952 Alpine Renault 1300</Model><Classification>Classic Cars</Classification>
    </Product><Product><ID>S10_2016</ID><Model>1996 Moto Guzzi 1100i</Model><Classification>Motorcycles</Classification></Product><Product>
    <ID>S10_4698</ID><Model>2003 Harley-Davidson Eagle Drag Bike</Model><Classification>Motorcycles</Classification></Product><Product>
    <ID>S10_4757</ID><Model>1972 Alfa Romeo GTA</Model><Classification>Classic Cars</Classification></Product></Products>
```

**Get product details**

Invoke the following command to get details of a product.

``` java
    curl -X GET http://localhost:8280/services/Excel.HTTPEndpoint/Products/{PRODUCT_ID}
```

Example:

``` java
    curl -X GET http://localhost:8280/services/Excel.HTTPEndpoint/Products/S10_4757
```

**Add a product**

Follow the steps given below to insert data to the excel sheet:

1.  Copy code given below to a file, name it
    **`                 product-data.xml                `** , and save
    it.

    ``` java
            <_putproduct>
                  <ID>S410_5443</ID>
                  <Model>1972 Alfa Romeo GTA</Model>
                  <Classification>Classic Cars</Classification>
            </_putproduct>
    ```

2.  Open the terminal, navigate the location where the file was saved
    and run the following command to insert data to the excel sheet.

    ``` java
            curl -X POST -H 'Accept: application/xml' -H 'Content-Type: application/xml' --data "@product-data.xml" -k -v http://localhost:8280/services/Excel.HTTPEndpoint/Products
    ```

        !!! info
    
        To confirm that the data got added you can run the GET curl command
        again using the product ID that you used in the
        `                product-data.xml               ` file.
    

These will return the response in XML.

## What's next?

Try out the samples under [Data Integration
Samples](https://docs.wso2.com/display/EI650/Data+Integration+Samples) .
