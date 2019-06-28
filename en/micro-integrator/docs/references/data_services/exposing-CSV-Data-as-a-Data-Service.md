# Exposing CSV Data as a Data Service

This tutorial guides you on how to expose the data in a CSV file as a
data service using WSO2 Enterprise Integrator (WSO2 EI) . We will create
a data service that can search for data in the file.

!!! note

Note!

You can only read data from CSV files. The ESB profile of WSO2 EI does
not support inserting, updating, or modifying data in a CSV file.


We will use the `         Products.csv        ` file that is shipped
with WSO2 EI by default. The `         Products.csv        ` file stored
in the `         <EI_HOME>/samples/data-services/resources        `
folder contains data about products (cars/motorcycles) that are
manufactured in an automobile company. The data table has the following
columns: `         ID        ` , `         Name        ` ,
`         Classification        ` , and `         Price        ` .

Follow the steps given below to create a data service for this
datasource. Also, see the samples in [Data Integration
Samples](https://docs.wso2.com/display/EI650/Data+Integration+Samples) .

-   [Creating the data
    service](#ExposingCSVDataasaDataService-Creatingthedataservice)
    -   [Connecting to the
        datasource](#ExposingCSVDataasaDataService-Connectingtothedatasource)
    -   [Creating a query to GET
        data](#ExposingCSVDataasaDataService-CreatingaquerytoGETdata)
    -   [Creating a SOAP operation to invoke the
        query](#ExposingCSVDataasaDataService-CreatingaSOAPoperationtoinvokethequery)
    -   [Creating a REST resource to invoke the
        query](#ExposingCSVDataasaDataService-CreatingaRESTresourcetoinvokethequery)
    -   [Finish creating the data
        service](#ExposingCSVDataasaDataService-Finishcreatingthedataservice)
-   [Invoking your data service using
    SOAP](#ExposingCSVDataasaDataService-InvokingyourdataserviceusingSOAP)
-   [Invoking your data service using
    REST](#ExposingCSVDataasaDataService-InvokingyourdataserviceusingREST)

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

    -   [**On MacOS/Linux/CentOS**](#47337397d902463c979d143d3529cc03)
    -   [**On Windows**](#a62334f8ada848fbb9a0d9f46c326054)

    Open a terminal and execute the following command:

    ``` java
        wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

3.  Access the management console of the ESB profile:
    `                     https://localhost:9443/carbon/                   `
    .
4.  Log in using `          admin         ` as the username and
    password.
5.  Click **Data Service \> **Create**** to open the **Create Data
    Service** window.
6.  Enter the following data service name.

    |                   |     |
    |-------------------|-----|
    | Data Service Name | CSV |

7.  Leave the default values for the other fields.
8.  Click **Next** to go to the **Datasources** screen.

#### Connecting to the datasource

You can add a CSV file as the datasource as explained below.

1.  Click **Add New Datasource** and enter values as shown below.

    <table style="width:100%;">
    <colgroup>
    <col style="width: 19%" />
    <col style="width: 80%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Datasource ID</td>
    <td><code>               CSV              </code></td>
    </tr>
    <tr class="even">
    <td>Datasource Type</td>
    <td><code>               CSV              </code></td>
    </tr>
    <tr class="odd">
    <td>CSV File Location</td>
    <td><code>               ./samples/data-services/resources/Products.csv              </code> .<br />
    In this tutorial, we are using a sample CSV file that is stored in the above location of your product distribution.</td>
    </tr>
    <tr class="even">
    <td>Column Separator</td>
    <td><code>               comma              </code></td>
    </tr>
    <tr class="odd">
    <td>Start Reading From Row</td>
    <td><code>               2              </code><br />
    You enter 2 because the 1st row of the CSV file is the header. The data is available from row 2 onwards.</td>
    </tr>
    <tr class="even">
    <td>Contains Column Header Row</td>
    <td><code>               true              </code></td>
    </tr>
    <tr class="odd">
    <td>Header row</td>
    <td><code>               1              </code><br />
    You enter 1 because the 1st row of the CSV file includes the header.</td>
    </tr>
    </tbody>
    </table>

2.  Save the datasource.
3.  Click **Next** to go to the **Queries** screen.

#### Creating a query to GET data

Now let's start writing a query to search for data in the CSV
datasource. The query specifies the data that should be fetched, and the
format that should be used to display data when the query is invoked.

1.  Click **Add New Query** to open the **Add New Query** screen and
    enter the following details.

    |            |     |
    |------------|-----|
    | Query Name | Q1  |
    | Datasource | CSV |

2.  Now let's specify **output mappings** , which will determine how the
    result from your query will be presented when the query is
    invoked.  
    The sample CSV datasource we are using contains four columns: **ID**
    , **Name** , **Classification** , and **Price** . We will create an
    output mapping for each of these columns.  
    1.  Start by giving the following information:

        <table style="width:100%;">
        <colgroup>
        <col style="width: 14%" />
        <col style="width: 85%" />
        </colgroup>
        <tbody>
        <tr class="odd">
        <td>Output type</td>
        <td>Select <strong>XML</strong> .<br />
        You can select XML, JSON, or RDF. We will use XML for this tutorial. This specifies the format in which the query results should be presented.</td>
        </tr>
        <tr class="even">
        <td>Grouped by element</td>
        <td>Enter <strong>Products</strong> .<br />
        This will be the XML element that will group the query result.</td>
        </tr>
        <tr class="odd">
        <td>Row Name</td>
        <td>Enter <strong>Product</strong> .<br />
        This is the XML element that should group each individual result.</td>
        </tr>
        </tbody>
        </table>

    2.  Click **Add New Output Mapping** to start creating the output
        mapping for the **ID** column. Enter values as shown below:

        | Mapping Type                               | Datasource Type                           | Output Field Name                     | Datasource Column Name                | Parameter Type                            | Schema Type                               |
        |--------------------------------------------|-------------------------------------------|---------------------------------------|---------------------------------------|-------------------------------------------|-------------------------------------------|
        | `                 Element                ` | `                 Column                ` | `                 ID                ` | `                 ID                ` | `                 SCALAR                ` | `                 string                ` |

        ![](attachments/119130710/119130721.png){width="600"
        height="529"}

    3.  Click **Add** to save the output mapping. You will now have one
        output mapping listed for the **Q1** query.
    4.  Create output mappings for the remaining columns given below.

        | Mapping Type                               | Datasource Type                           | Output Field Name                                 | Datasource Column Name                            | Parameter Type                            | Schema Type                               |
        |--------------------------------------------|-------------------------------------------|---------------------------------------------------|---------------------------------------------------|-------------------------------------------|-------------------------------------------|
        | `                 Element                ` | `                 Column                ` | `                 Classification                ` | `                 Classification                ` | `                 SCALAR                ` | `                 string                ` |
        | `                 Element                ` | `                 Column                ` | `                 Price                `          | `                 Price                `          | `                 SCALAR                ` | `                 string                ` |
        | `                 Element                ` | `                 Column                ` | `                 Name                `           | `                 Name                `           | `                 SCALAR                ` | `                 string                ` |

3.  Click **Main Configuration** to return to the **Query** screen.
4.  Click **Save** to save the query.
5.  Click **Next** to go to the **Operations** screen.

#### Creating a SOAP operation to invoke the query

To invoke the query, you need to define an operation.

1.  Click **Add New Operation** and enter the following information.

    |                |               |
    |----------------|---------------|
    | Operation Name | GetProductsOp |
    | Query ID       | Q1            |

2.  Save the operation.

You can now invoke the data service query using SOAP.

#### Creating a REST resource to invoke the query

Now, let's create REST resources to invoke the query created above.
Alternatively, you can create SOAP operations to invoke the queries. See
the [previous section](_Exposing_CSV_Data_as_a_Data_Service_) for
instructions.

1.  Click **Add New Resource** and enter the following information.

    |                 |          |
    |-----------------|----------|
    | Resource Path   | Products |
    | Resource Method | GET      |
    | Query ID        | Q1       |

2.  Save the resource.

You can now invoke the data service query using REST.

#### Finish creating the data service

Once you have defined the operation, click **Finish** to complete the
data service creation process.

You will now be taken to the **Deployed Services** screen, which shows
all the data services deployed on the server.

### Invoking your data service using SOAP

You can try the data service you created by using the **TryIt** tool
that is in your product by default.

1.  Go to the **Deployed Services** screen.
2.  Click the **Try this service link** for the **CSV** data service.  
    The **TryIt** tool will open with the CSV service.
3.  Select the **GetProductOp** operation that you created previously
    and click **Send** .
4.  The result of the **Q1** query will be published.

Example:  
![](attachments/119130710/119130711.png){width="900"}

### Invoking your data service using REST

You can send an HTTP GET request to invoke the data service using cURL
as shown below.

``` java
    curl -X GET http://localhost:8280/services/CSV.HTTPEndpoint/Products
```

This will return the response in XML.

Example:

``` java
    <Products xmlns="http://ws.wso2.org/dataservice"><Product><ID>S10_1678</ID><Category>Motorcycles</Category><Price>1000</Price><Name>1969 Harley Davidson
     Ultimate Chopper</Name></Product><Product><ID>S10_1949</ID><Category>Classic Cars</Category><Price>600</Price><Name>1952 Alpine Renault 1300</Name>
    </Product><Product><ID>S10_2016</ID><Category>Motorcycles</Category><Price>456</Price><Name>1996 Moto Guzzi 1100i</Name></Product><Product><ID>S10_4698</ID>
    <Category>Motorcycles</Category><Price>345</Price><Name>2003 Harley-Davidson Eagle Drag Bike</Name></Product><Product><ID>S10_4757</ID><Category>Classic Cars
    </Category><Price>230</Price><Name>1972 Alfa Romeo GTA</Name></Product><Product><ID>S10_4962</ID><Category>Classic Cars</Category><Price>890</Price>
    <Name>1962 LanciaA Delta 16V</Name></Product><Product><ID>S12_1099</ID><Category>Classic Cars</Category><Price>560</Price><Name>1968 Ford Mustang</Name>
    </Product><Product><ID>S12_1108</ID><Category>Classic Cars</Category><Price>900</Price><Name>2001 Ferrari Enzo</Name></Product></Products>
```
