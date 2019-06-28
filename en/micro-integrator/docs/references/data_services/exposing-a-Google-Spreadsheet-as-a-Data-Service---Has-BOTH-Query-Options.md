# Exposing a Google Spreadsheet as a Data Service - Has BOTH Query Options

!!! warning

This document is only visible to those at WSO2.  
There is an error that is thrown when the query mode is enabled.
Therefore, the content was deleted from \[1\]. Once issue \[2\] is
fixed, please delete \[1\] and use this documentation.  
  
\[1\] [Exposing a Google Spreadsheet as a Data
Service](_Exposing_a_Google_Spreadsheet_as_a_Data_Service_)

\[2\] <https://github.com/wso2/product-ei/issues/2421>


This tutorial guides you on how to expose data in a Google spreadsheet
as a data service.

See the following topics for instructions.  Also, see the samples in
[Data Integration
Samples](https://docs.wso2.com/display/EI650/Data+Integration+Samples) .

-   [Start the Create New Data Service
    wizard](#ExposingaGoogleSpreadsheetasaDataService-HasBOTHQueryOptions-StarttheCreateNewDataServicewizard)
-   [Add a Google spreadsheet
    datasource](#ExposingaGoogleSpreadsheetasaDataService-HasBOTHQueryOptions-AddaGooglespreadsheetdatasource)
-   [Define a query for the
    datasource](#ExposingaGoogleSpreadsheetasaDataService-HasBOTHQueryOptions-Defineaqueryforthedatasourcequery_mode_disabled)
-   [Define an operation to invoke the
    query](#ExposingaGoogleSpreadsheetasaDataService-HasBOTHQueryOptions-Defineanoperationtoinvokethequery)
-   [Finish creating the data
    service](#ExposingaGoogleSpreadsheetasaDataService-HasBOTHQueryOptions-Finishcreatingthedataservice)
-   [Invoking your data
    service](#ExposingaGoogleSpreadsheetasaDataService-HasBOTHQueryOptions-Invokingyourdataservice)
-   [Exposing a public spreadsheet as a data
    service](#ExposingaGoogleSpreadsheetasaDataService-HasBOTHQueryOptions-Exposingapublicspreadsheetasadataservice)

------------------------------------------------------------------------

### Start the Create New Data Service wizard

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

    -   [**On MacOS/Linux/CentOS**](#cf202e1cc3794137a37f23f0241a8237)
    -   [**On Windows**](#4494f330a7ab400aa8bf0294656cf4d6)

    Open a terminal and execute the following command:

    ``` java
        wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

3.  Access the management console of the ESB profile:
    `                       https://localhost:9443/carbon/                     `
    .

        !!! note
    
        For the purpose of this tutorial, be sure to use
        `           localhost          ` as the IP in the above URL.
    

4.  Sign in using `           admin          ` as the username and
    password.

5.  Click **Create** under the **Data Service** menu to open the
    **Create Data Service** wizard.

6.  Enter **GSpread** as the data service name as shown below. Leave the
    default values for the other fields.  
    ![](attachments/119130787/119130813.png){width="751" height="232"}

7.  Click **Next** to go to the **Datasources** screen.  
    ![](attachments/53119164/53285077.png){width="536" height="152"}

### Add a Google spreadsheet datasource

You can add a Google spreadsheet as the datasource by following the
steps given below.

1.  Click **Add New Datasource** to open the following screen.  
    ![](attachments/119130787/119130803.png){width="700" height="171"}
2.  Follow the instructions below to fill the datasource details.

    1.  In the **Datasource Id** field, enter **GoogleSpreadsheet** as
        the value.

    2.  In the **Datasource Type** field, Select **Google Spreadsheet**
        from the list.  
        This specifies the type of datasource for which the data service
        is created. You will now get the following screen:  
        ![](attachments/119130787/119130799.png){width="650"
        height="222"}

    3.  In the **Google Spreadsheet URL** field, specify
        [https://docs.Google.com/spreadsheets/d/1o1pCmMFcbWZ\_eJ54ymcb486GctTdsR6b4kFERmKrR1w/edit?hl=en&hl=en\#gid=0](https://docs.google.com/spreadsheets/d/1o1pCmMFcbWZ_eJ54ymcb486GctTdsR6b4kFERmKrR1w/edit?hl=en&hl=en#gid=0)
        as the path to your spreadsheet.

                !!! note
        
                Note that this is a private spreadsheet, which is not published
                on the web. If you want to use a public spreadsheet, see the
                topic on exposing a [public spreadsheet as a data
                service](#ExposingaGoogleSpreadsheetasaDataService-HasBOTHQueryOptions-Public)
                .
        
                You can use your own private spreadsheet too.
        

    4.  Select a value for the **Visibility** field based on whether the
        spreadsheet is private or public.  
        Since we have used a private spreadsheet in this example, set
        the visibility to **Private** .  
        ![](attachments/119130787/119130791.png){width="452"
        height="148"}  
        You are asked to provide credentials. Google no longer supports
        authentication through username and password. Therefore, you
        need to provide credentials in the form of a **Client ID** ,
        **Client Secret** , and **Refresh Token** .

    5.  Get the Client ID and Client Secret:

        1.  Go to [Google
            documentation](https://developers.google.com/identity/sign-in/web/devconsole-project)
            and click **CONFIGURE A PROJECT** .

        2.  Expand the drop-down, select a project you have already
            created or create a new project.

        3.  On the **Configure your OAuth client** window, select **Web
            Browser** .

        4.  Enter the URL given for **Redirect URI** in the management
            console as the value for ****Authorized Redirect URIs**** .

                        !!! info
            
                        Setting the hostname
            
                        Note that the **Redirect URI** should contain the same
                        hostname as the **Authorized Redirect** URl  that you
                        provided, as well as the host on which the management
                        console runs.
            
                        -   If the server is running on your machine, you can simply
                            use `                 localhost                ` as the
                            hostname (or the direct IP address, which is 127.0.0.1).
            
                        -   If the server is running on a local network, you must
                            always use a hostname instead of the direct IP address.
                            This is because publicly shared IPs cannot be used. You
                            also need to ensure that the hostname you use is known
                            to the browser by registering it in your
                            `                 /etc/hosts                ` file.
            

        5.  Click **CREATE** .

        6.  Copy the **Client ID** and **Client Secret** , and enter
            them in the management console.  
            ![](attachments/119130787/119130792.png){width="453"
            height="145"}

    6.  Click **Generate Token** . You will now be redirected to the
        Google consent page.  
        After you approve that, the refresh token will be inserted into
        the **New Datasource** screen automatically. Note that we store
        the refresh token because the access token is going to expire.  
        ![](attachments/119130787/119130789.png){width="460"
        height="146"}

    7.  Click **Test Connection** to ensure that the configured works as
        expected.  
        If you get a successful message, your configurations are
        correct. If you get an error, make sure you configure the fields
        as explained in the document.

    8.  The **Use Query Mode** option allows you to write queries for
        the spreadsheet in two different modes:

        -   **Non-Query** mode: Allows you to directly expose the
            content of a Google spreadsheet as a service.

        -   **Query** mode: Allows you to query a Google spreadsheet in
            a familiar, SQL-like manner, and expose the result as a
            service. You need to provide the name of the spreadsheet, in
            addition to the spreadsheet URL.

        You can try this tutorial by enabling or disbaling the query
        mode.

        -   [**Disable query mode**](#2a3574d30cba4bf79805cc276e70e389)
        -   [**Enable query mode**](#2a2d0db3ab9f42188f9a01bda32c9dd7)

        The query mode is disabled by default. Ensure that your
        datasource configurations looks as follows:  
        Example:

        ![](attachments/119130787/119130788.png){width="400"}

        Follow the steps given below:

        1.  Select **Use Query Mode** .

        2.  Provide the name of the spreadsheet, in addition to the
            spreadsheet URL.  
            For this usecase enter **customers** because the Google
            spreadsheet we used is named customers.

        ![](attachments/119130787/119130801.png){width="628"
        height="250"}

3.  Save the datasource.
4.  Click **Next** to go to the **Queries** screen

### Define a query for the datasource

You can define queries in two ways for a Google spreadsheet, depending
on whether or not [Query mode is
enabled](#ExposingaGoogleSpreadsheetasaDataService-HasBOTHQueryOptions-use_query_mode)
for the datasource.

Select the respective tab and follow the steps based on how you
configured your datasource.

-   [**Query mode disbaled**](#f2b9fa0bd1bc46f79d66b8364b9d4dc2)
-   [**Query mode enabled**](#fb72faf1d0364685866ca2f94793c3e6)

If query mode is disabled for the spreadsheet, you cannot use SQL
statements to query data in the spreadsheet.  Note that in non-query
mode, you can only get data from the spreadsheet and you cannot insert,
update, or modify any data.

Follow the steps given below:

1.  Click **Add New Query** to open the **Add New Query** screen.
2.  Enter **Q1** as the query id in the **Query ID** field.
3.  In the **Datasource** field, select the datasource for which you are
    going to write a query. Select the datasource for the Google
    spreadsheet that you created previously.

4.  You can directly specify the details of the spreadsheet as shown
    below.

    <table>
    <colgroup>
    <col style="width: 50%" />
    <col style="width: 50%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Worksheet Number</td>
    <td><code>                    1                   </code><br />
    You want the query to get the data from Sheet1 in the Google spreadsheet.</td>
    </tr>
    <tr class="even">
    <td>Start reading from</td>
    <td><code>                    2                   </code><br />
    You enter 2 because the 1st row of the spreadsheet is the header. The data is available from row 2 onwards.</td>
    </tr>
    <tr class="odd">
    <td>Rows to read</td>
    <td><code>                    5                   </code><br />
    The data in this row will be fetched by the query.</td>
    </tr>
    <tr class="even">
    <td>Headers available</td>
    <td>true</td>
    </tr>
    <tr class="odd">
    <td>Header row</td>
    <td><code>                    1                   </code><br />
    You enter 1 because the 1st row of the spreadsheet includes the header.</td>
    </tr>
    </tbody>
    </table>

    ![](attachments/119130787/119130797.png){width="461" height="250"}

5.  Define **Output Mapping** :  
    Now, let's specify how the data fetched from the datasource should
    be displayed in the output. The Google spreadsheet we are using
    contains several columns with customer data. We will create output
    mappings for the following columns: **ID** , **CustomerNumber** ,
    **CustomerName** , and **City** .

    <table>
    <tbody>
    <tr class="odd">
    <td><strong>Output type</strong></td>
    <td>Select <strong>XML</strong> .<br />
    You can select XML, JSON, or RDF. We will use XML for this tutorial.</td>
    </tr>
    <tr class="even">
    <td><strong>Grouped by element</strong></td>
    <td>Enter <strong>Customers</strong> .<br />
    This is the XML element that groups the query result.</td>
    </tr>
    <tr class="odd">
    <td><strong>Row Name</strong></td>
    <td>Enter <strong>Customer</strong> .<br />
    This is the XML element that groups each individual result.</td>
    </tr>
    <tr class="even">
    <td><strong>Add New Output Mapping</strong></td>
    <td><div class="content-wrapper">
    <p>Follow the steps given below to add a new output mapping:</p>
    <ol>
    <li><p>Click <strong>Add New Output Mapping</strong> to start creating the output mapping for the <strong>customerNumber</strong> column. Enter values as shown below:</p>
    <div class="table-wrap">
    <table>
    <thead>
    <tr class="header">
    <th>Mapping Type</th>
    <th>Element Name</th>
    <th>Datasource Type</th>
    <th>Datasource Column Name</th>
    <th>Parameter Type</th>
    <th>Schema Type</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><code>                            element                           </code></td>
    <td><code>                            customerNumber                           </code></td>
    <td><code>                            column                           </code></td>
    <td><code>                            customerNumber                           </code></td>
    <td><code>                            SCALAR                           </code></td>
    <td><code>                            string                           </code></td>
    </tr>
    </tbody>
    </table>
    </div>
    <p><img src="attachments/119130787/119130815.png" title="add the output mapping details" alt="add the output mapping details" /></p></li>
    <li>Click <strong>Add</strong> to save the output mapping. You will now have one output mapping listed for the query.</li>
    <li><p>Now, add output mappings for the following:</p>
    <div class="table-wrap">
    <table>
    <thead>
    <tr class="header">
    <th>Mapping Type</th>
    <th>Output Filed Name</th>
    <th>Datasource Type</th>
    <th>Datasource Column Name</th>
    <th>Parameter Type</th>
    <th>Schema Type</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><code>                            attribute                           </code></td>
    <td><code>                            customerName                           </code></td>
    <td><code>                            column                           </code></td>
    <td><code>                            customerName                           </code></td>
    <td><code>                            SCALAR                           </code></td>
    <td><code>                            string                           </code></td>
    </tr>
    <tr class="even">
    <td><code>                            attribute                           </code></td>
    <td><code>                            city                           </code></td>
    <td><code>                            column                           </code></td>
    <td><code>                            city                           </code></td>
    <td><code>                            SCALAR                           </code></td>
    <td><code>                            string                           </code></td>
    </tr>
    </tbody>
    </table>
    </div>
    <p>You will now have the following output mappings listed for the query:</p>
    <div>
    <img src="attachments/119130787/119130816.png" title="list of existing output mappings" alt="list of existing output mappings" />
    </div></li>
    </ol>
    </div></td>
    </tr>
    </tbody>
    </table>

6.  Click **Main Configuration** to return to the **Query** screen.
7.  Click **Save** .
8.  Click **Next** to go to the **Operations** screen.

You can query data in the spreadsheet using SQL statements.  At the
moment, the query mode supports only the basic SELECT, INSERT, UPDATE,
and DELETE queries. Note that the Google spreadsheet SQL driver does not
accept the SELECT \* command. Therefore, all the required select options
(e.g., column names) should be specified in the query. See the
following example:
`              <sql>SELECT employeeId,name,salary FROM Sheet1 WHERE employeeId = ?</sql>             `
. Nested queries will be supported in an upcoming release.

!!! info The
`             org.wso2.carbon.dataservices.sql.driver.TDriver            `
class  is used internally as the SQL driver to implement the query mode.
It is a JDBC driver implementation to be used with tabular data models

1.  Click **Add New Query** to open the **Add New Query** screen.
2.  Enter **Q1** as the query id in the **Query ID** field.
3.  In the **Datasource** field, select the datasource for which you are
    going to write a query. Select the datasource for the Google
    spreadsheet that you created previously.

4.  Specify an SQL query to insert data into the spreadsheet.

    ``` java
        SELECT customerNumber, customerName, city FROM SHEET1
    ```

        !!! tip
    
        A select query is used because the spreadsheet we are using has view
        permission only. If you want to try out an insert query, take a copy
        of the spreadsheet, and provide edit permissions. Make sure to
        update the spreadsheet link
        [here](#ExposingaGoogleSpreadsheetasaDataService-HasBOTHQueryOptions-spread-sheet-link)
        too.
    

5.  Click **Generate Response** to create the output mapping. This
    defines how the employee details retrieved from the datasource will
    be presented in the result.  
    Note that, by default, the output type is XML.

6.  Click **Next** to go to the **Operations** screen.

### Define an operation to invoke the query

Follow the steps given below:

1.  Click **Add New Operation** to open the **Add New Operation**
    screen.
2.  In the **Operation Name** field, enter **CustomerDetails** as
    the name for the operation.
3.  In the **Query ID** field, select **Q1.** This is the query that you
    created under [Define a query for the
    datasource](#ExposingaGoogleSpreadsheetasaDataService-HasBOTHQueryOptions-Defineaqueryforthedatasource)
    .
4.  Save the operation.

### Finish creating the data service

Once you have defined the operation, click **Finish** to complete the
data service creation process.

You will now be taken to the **Deployed Services** screen, which shows
all the data services deployed on the server.

------------------------------------------------------------------------

### Invoking your data service

You can try the data service you created by using the **TryIt** tool
that is in your product by default.

1.  Go to the **Deployed Services** screen.
2.  Click the **Try this service** link for the **GSpread** data service
    that you just created. The TryIt tool will open with the **GSpread**
    service.
3.  Select the **CustomerDetails** operation your created earlier and
    click **Send.  
    ** You will see the data displayed.

------------------------------------------------------------------------

!!! note

### Exposing a public spreadsheet as a data service

A Google spreadsheet can be a private spreadsheet or a publicly exposed
(published to the web) spreadsheet. The tutorial given above uses a
private spreadsheet.

Note the following if you are using a public spreadsheet:

-   Before you begin, the spreadsheet is required to be published to the
    web. To publish a spreadsheet, select **File \> Publish** **to the
    web** from the spreadsheet's user interface. Use the spreadsheet's
    URL for the **Google Spreadsheet URL** field.
-   When you come to the **Edit Datasource** step in the **Create New
    Data Service** wizard, the **[Use Query
    Mode](#ExposingaGoogleSpreadsheetasaDataService-HasBOTHQueryOptions-use_query_mode)**
    check box should be cleared as it is not possible to define SQL-like
    queries for a public spreadsheet. This also means that you cannot
    add or modify the data in the spreadsheet. That is, you can only get
    data from a public spreadsheet. You can follow the same instructions
    given in the section for [defining a query in 'non-query'
    mode](#ExposingaGoogleSpreadsheetasaDataService-HasBOTHQueryOptions-query_mode_disabled)
    (in the above tutorial) to define the query for a public
    spreadsheet.  
    ![](attachments/119130787/119130796.png){width="650" height="227"}

