# Defining Nested Queries

Nested queries help you to use the result of one query as an input
parameter of another, and the queries executed in a nested query works
in a transactional manner. Follow the steps given below to add a nested
query to a data service.

-   [Setting up
    a datasource](#DefiningNestedQueries-Settingupadatasource)
-   [Creating the data
    service](#DefiningNestedQueries-Creatingthedataservice)
    -   [Connecting to the
        datasource](#DefiningNestedQueries-Connectingtothedatasource)
    -   [Creating a query to GET employee details by
        the office](#DefiningNestedQueries-CreatingaquerytoGETemployeedetailsbytheoffice)
    -   [Creating a nested query to GET office
        details](#DefiningNestedQueries-CreatinganestedquerytoGETofficedetails)
    -   [Creating a SOAP operation to invoke the
        query](#DefiningNestedQueries-CreatingaSOAPoperationtoinvokethequery)
    -   [Creating a REST resource to invoke the
        query](#DefiningNestedQueries-CreatingaRESTresourcetoinvokethequery)
    -   [Finish creating the data
        service](#DefiningNestedQueries-Finishcreatingthedataservice)
-   [Invoking the data service using
    SOAP](#DefiningNestedQueries-InvokingthedataserviceusingSOAP)
-   [Invoking the data service using
    REST](#DefiningNestedQueries-InvokingthedataserviceusingREST)

------------------------------------------------------------------------

### Setting up a datasource

!!! tip

If you have already tried all the previous tutorials, you can skip this
section because you have already created the `         Company        `
database and added the data mentioned.


Follow the steps given below to set up a MySQL database for this
tutorial.

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
    <td><code>               /Library/WSO2/EnterpriseIntegrator/6.5.0              </code></td>
    </tr>
    <tr class="even">
    <td>Windows</td>
    <td><code>               C:\Program Files\WSO2\EnterpriseIntegrator\6.5.0\              </code></td>
    </tr>
    <tr class="odd">
    <td>Ubuntu</td>
    <td><code>               /usr/lib/wso2/EnterpriseIntegrator/6.5.0              </code></td>
    </tr>
    <tr class="even">
    <td>CentOS</td>
    <td><code>               /usr/lib64/EnterpriseIntegrator/6.5.0              </code></td>
    </tr>
    </tbody>
    </table>

2.  Install the MySQL server.
3.  Download the JDBC driver for MySQL from
    [here](http://dev.mysql.com/downloads/connector/j/) and copy it to
    your `          <EI_HOME>/lib         ` directory.
4.  Create the following database: Company

    ``` java
        CREATE DATABASE Company;
    ```

5.  Create the following tables:

    -   **Offices** table:

        ``` java
                USE company;
        
                CREATE TABLE `OFFICES` (`OfficeCode` int(11) NOT NULL, `AddressLine1` varchar(255) NOT NULL, `AddressLine2` varchar(255) DEFAULT NULL, `City` varchar(255) DEFAULT NULL, `State` varchar(255) DEFAULT NULL, `Country` varchar(255) DEFAULT NULL, `Phone` varchar(255) DEFAULT NULL, PRIMARY KEY (`OfficeCode`)); 
        ```

    -   **Employees** table:

        ``` java
                    CREATE TABLE `EMPLOYEES` (`EmployeeNumber` int(11) NOT NULL, `FirstName` varchar(255) NOT NULL, `LastName` varchar(255) DEFAULT NULL, `Email` varchar(255) DEFAULT NULL, `JobTitle` varchar(255) DEFAULT NULL, `OfficeCode` int(11) NOT NULL, PRIMARY KEY (`EmployeeNumber`,`OfficeCode`), CONSTRAINT `employees_ibfk_1` FOREIGN KEY (`OfficeCode`) REFERENCES `OFFICES` (`OfficeCode`));
        ```

6.  Insert the following data into the tables:

    -   Add to the **Offices** table:

        ``` java
                    INSERT INTO OFFICES VALUES (1,"51","Glen Street","Norwich","London","United Kingdom","+441523624");
                    INSERT INTO OFFICES VALUES (2,"72","Rose Street","Pasadena","California","United States","+152346343");
        ```

    -   Add to the **Employees** table:

        ``` java
                    INSERT INTO EMPLOYEES VALUES (1,"John","Gardiner","john@office1.com","Manager",1);
                    INSERT INTO EMPLOYEES VALUES (2,"Jane","Stewart","jane@office2.com","Head of Sales",2);
                    INSERT INTO EMPLOYEES VALUES (3,"David","Green","david@office1.com","Manager",1); 
        ```

You will now have two tables in the **Company** database as shown below:

-   **Offices** table:  
    To view the data, you can run the following command:
    `          SELECT * FROM Offices;         `  
    ![](https://lh3.googleusercontent.com/3Pm7SBLvEv3KfcZYWpan3P-1C7WL8dzP1Z1ICrGUp3gl9mQNj_18rbseOD6yZi5Z8BkWsuyn02mThqNsdfHXQy35JCQCIYKDAua9Wp7vxJX-Dmsaw7NHMPPgskqHG0gmL_hs07Y9){width="624"
    height="75"}
-   **Employees** table:  
    To view the data, you can run the following command:
    `          SELECT * FROM Employees;         `  
    ![](https://lh3.googleusercontent.com/E-iZN7ZlfqXEELiRL3w08QIsUM2yI4e8bo-KlaDltujPoW7ABoqeuLc44WR6b0qIOHfpAp1sLvoPz9p-_Hfxg3Qeav9jPrp5ns7PTOy-hKltCsmAylu0ouOgW1YXv8qXRiRCg6jK){width="624"
    height="93"}  
    If you tried out the previous tutorials, you will have more data
    than what is shown above.

------------------------------------------------------------------------

### Creating the data service

Let's create a data service using the **Create Data Service** wizard:

1.  Start the WSO2 ESB profile.

    -   [**On MacOS/Linux/CentOS**](#5c23d64bbdb4416494d8dcee8457085e)
    -   [**On Windows**](#7281409c279d4364b9905318d48804f3)

    Open a terminal and execute the following command:

    ``` java
            sudo wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

2.  Open the ESB profile's Management Console using
    `                     https://localhost:9443/carbon                   `
    , and log in using `          admin         ` as the username and
    the password.
3.  Click **Create** under **Data Service** .
4.  Enter the following name for the data service

    |                   |                      |
    |-------------------|----------------------|
    | Data Service Name | EmployeesDataService |

5.  Click **Next** to enter the datasource connection details.

#### Connecting to the datasource

Follow the steps given below.

1.  Click **Add New Datasource** and enter the following details:

    <table>
    <colgroup>
    <col style="width: 37%" />
    <col style="width: 62%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Datasource ID</td>
    <td>Datasource</td>
    </tr>
    <tr class="even">
    <td>Datasource Type</td>
    <td>RDBMS</td>
    </tr>
    <tr class="odd">
    <td>Datasource Type (Default/External)</td>
    <td>Leave <strong>Default</strong> selected.</td>
    </tr>
    <tr class="even">
    <td>Database Engine</td>
    <td>MySQL</td>
    </tr>
    <tr class="odd">
    <td>Driver Class</td>
    <td><code>               com.mysql.jdbc.Driver              </code></td>
    </tr>
    <tr class="even">
    <td>URL</td>
    <td><code>               jdbc:mysql://localhost:3306/Company              </code></td>
    </tr>
    <tr class="odd">
    <td>User Name</td>
    <td>Enter your MySQL server's username.<br />
    Example: Root</td>
    </tr>
    <tr class="even">
    <td>Password</td>
    <td>Enter your MySQL server's password.<br />
    If you have not assigned a password, keep this field empty.</td>
    </tr>
    </tbody>
    </table>

2.  Save the datasource.
3.  Click **Next** to start creating queries.

#### Creating a query to GET employee details by the office

Let's create a query that can retrieve employee data, based on the
office code. When the office code is provided as an input, the data
service should get the relevant employee details and present the result.

1.  Click **Add New Query** to specify the query details.
2.  Enter the following details:

    <table>
    <tbody>
    <tr class="odd">
    <td>Query Name</td>
    <td><code>               EmployeeOfficeSQL              </code></td>
    </tr>
    <tr class="even">
    <td>Datasource</td>
    <td>Datasource</td>
    </tr>
    <tr class="odd">
    <td>SQL</td>
    <td><div class="content-wrapper">
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>select EmployeeNumber, FirstName, LastName, Email, JobTitle, OfficeCode from EMPLOYEES where OfficeCode=:OfficeCode</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

3.  Generate input and output mappings:
    1.  Click **Generate Input Mapping** and an input mapping will be
        generated automatically for the **OfficeCode** field:  
        ![](attachments/119133274/119133280.png){width="961"
        height="127"}
    2.  Click **Generate Response** to automatically generate output
        mappings for the **EmployeeNumber** , **FirstName** ,
        **LastName** , **Email** , **Job Title** , and **Office Code**
        fields.  
        ![](attachments/119133274/119133279.png){width="700"
        height="339"}
4.  Save the **EmployeeOfficeSQL** query.

#### Creating a nested query to GET office details

Let's create a query that can retrieve details of an office based on the
office code. When the query is invoked, the data service should get the
relevant details of the office premises. Additionally, we will nest the
**EmployeeOfficeSQL** query that was [created
previously](#DefiningNestedQueries-CreatingaquerytoGETemployeedetailsbyoffice)
to make sure that the details of the employees attached to each office
code are also included in the office details.

1.  Click **Add New Query** to specify the query details.
2.  Enter the following details:

    <table>
    <tbody>
    <tr class="odd">
    <td>Query Name</td>
    <td><code>               listOfficeSQL              </code></td>
    </tr>
    <tr class="even">
    <td>Datasource</td>
    <td>Datasource</td>
    </tr>
    <tr class="odd">
    <td>SQL</td>
    <td><div class="content-wrapper">
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>select OfficeCode, AddressLine1, AddressLine2, City, <span class="bu">State</span>, Country, Phone from OFFICES where OfficeCode=:OfficeCode</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

3.  Click **Generate Input Mappings** to create the input mapping. The
    office code is the input as shown below.  
    ![](attachments/119133274/119133275.png){width="700"}
4.  Now, you need to create the output mapping for the query, which will
    determine how the output is determined.  
    You can use an **XML** format, **JSON** format, or **RDF** format
    for the output. Let's look at how to use an XML output or a JSON
    output:
    -   If you want to map the query output to an **XML format** :  
        1.  Click **Generate Response** , and the required fields will
            be generated as shown below.  
            ![](attachments/119133274/119133278.png){width="700"
            height="332"}
        2.  Now, let's nest the **EmployeeOfficeSQL** query in the
            **listOfficeSQL** query:  
            Click **Add New Output Mapping** and specify the following
            values.

            |              |                                                          |
            |--------------|----------------------------------------------------------|
            | Mapping Type | `                   query                  `             |
            | Select Query | `                   EmployeeOfficeSQL                  ` |

            When you specify the **Select Query** , the query parameters
            of the selected query will be added by the system as shown
            below.  
            ![](https://lh3.googleusercontent.com/NZB0c5GuTw3grBXnAAUKhXMZGTl9AjMp4YHrNboOgSuhAH1v6eUt6t_A3j5ODc1q3A_hUn75BrOdmvofmRYcWRC9Wf_xjA2-moRcyBGoKd4WGxFQgwG91JjXt_L0YUYY6GpH8881){width="575"
            height="344"}

        3.  Click **Save** and click **Main Configuration** to return to
            the query.

    <!-- -->

    -   If you want to map the query output to **JSON** :
        1.  Select **JSON** for the **Output Type** field.
        2.  Enter the following JSON script:

            **JSON Mapping with Nested Queries**

            ``` javascript
                        {  
                           "Offices":{  
                              "Office":[  
                                 {  
                                    "OfficeCode":"$OfficeCode(type:integer)",
                                    "City":"$City",
                                    "Country":"$Country",
                                    "Phone":"$Phone",
                                    "@EmployeeOfficeSQL":"$OfficeCode->OfficeCode"
                                 }
                              ]
                           }
                        }
            ```

                        !!! info
            
                        **Note the following:**
            
                        As shown above, nested queries are mentioned in the JSON
                        mapping by giving the query details as a JSON object
                        attribute. That is, the name of the target query to be
                        called and the property value (the fields in the result
                        mapped with the target query parameters) are included in the
                        JSON mapping as the object attribute name.
            
                        In the above example:
            
                        -   The target query name is mentioned by prefixing the
                            query name with " `                @               ` ".
                            Note "
                            `                @EmployeeOfficeSQL               ` " in
                            the example given above.
                        -   The parameter mapping is added to the query by giving
                            the following values: The field name in the result
                            prefixed by " `                $               ` ", and
                            the name of the target query parameter.
                        -   These two values in the parameter mapping are separated
                            by " `                ->               ` ". See "
                            `                $OfficeCode->OfficeCode               `
                            " in the example given above.
                        -   Note that the target query name and the parameter
                            mapping are separated by a colon as follows:
                            `                "@EmployeeOfficeSQL": "$OfficeCode->OfficeCode"               `
            

5.  Save the output mapping for the nested query.
6.  Save the query.
7.  Click **Next** to open the **Operations** screen.

#### Creating a SOAP operation to invoke the query

Now, let's create SOAP operations to invoke the queries created above.
Alternatively, you can create REST resources to invoke the queries. See
the [next
section](#DefiningNestedQueries-CreatingaRESTresourcetoinvokethequery)
for instructions.

1.  Click Add New Operation and enter the details given below.

    |                |                                                |
    |----------------|------------------------------------------------|
    | Operation Name | `               listOfficeSQLOP              ` |
    | Query ID       | `               listOfficeSQL              `   |

2.  Save the **listOfficeSQLOP** operation.

#### Creating a REST resource to invoke the query

Now, let's create REST resources to invoke the queries created above.
Alternatively, you can create SOAP operations to invoke the queries. See
the [previous
section](#DefiningNestedQueries-CreatingaSOAPoperationtoinvokethequery)
, for instructions.

1.  Click **Add New Resource** and enter the details as shown below.

    |                 |                                                     |
    |-----------------|-----------------------------------------------------|
    | Resource Path   | `               offices/{OfficeCode}              ` |
    | Resource Method | `               Get              `                  |
    | Query ID        | `               listOfficeSQL              `        |

2.  Save the resource.

#### Finish creating the data service

Once you have defined the operation, click **Finish** to complete the
data service creation process. You will now be taken to the **Deployed
Services** screen, which shows all the data services deployed on the
server.

------------------------------------------------------------------------

### Invoking the data service using SOAP

You can try the data service you created by using the TryIt tool that is
in your product by default.

1.  Go to the **Deployed Services** screen.
2.  Click **Try this Service** to open the data service from the TryIt
    tool.
3.  Select the **listOfficeSQLOP** operation.
4.  Copy the content given below, and paste it into the TryIt tool to
    get the details of the office that has the office code 1 and all the
    employees that belong to office code 1.

    ``` java
        <body>
           <p:listOfficeSQLOP xmlns:p="http://ws.wso2.org/dataservice">
              <!--Exactly 1 occurrence-->
              <xs:OfficeCode xmlns:xs="http://ws.wso2.org/dataservice">1</xs:OfficeCode>
           </p:listOfficeSQLOP>
        </body>
    ```

5.  Click **Send** and you will see the
    [result](#DefiningNestedQueries-result) .

### Invoking the data service using REST

The service can be invoked in REST-style via curl (
[http://curl.haxx.se](http://curl.haxx.se/) ). Shown below is the curl
command to invoke the GET resource.  
It gets the details of the office that has the office code 1, and all
the employees that belong to office code 1.

``` java
    curl -X GET http://localhost:8280/services/EmployeesDataService.HTTPEndpoint/offices/1
```

!!! tip

If you configured the [output mapping of the
`          listOfficeSQL         ` query to be in the JSON
format](#DefiningNestedQueries-JSON) , you need to add the header
`         -H 'Accept: application/json'        ` to your curl command to
get the output in the JSON format.

``` java
    curl -H 'Accept: application/json' -X GET http://localhost:8280/services/EmployeesDataService.HTTPEndpoint/offices/1
```
    

You will now see the following result:

-   [**XML format**](#5fb1917cd3cc443ab0a85a57bb71cb5e)
-   [**JSON format**](#262d297eaffc4832961548840f9310aa)

``` java
    <Entries xmlns="http://ws.wso2.org/dataservice">
       <Entry>
          <OfficeCode>1</OfficeCode>
          <AddressLine1>51</AddressLine1>
          <AddressLine2>Glen Street</AddressLine2>
          <City>Norwich</City>
          <State>London</State>
          <Country>United Kingdom</Country>
          <Phone>+441523624</Phone>
          <Entries>
             <Entry>
                <EmployeeNumber>1</EmployeeNumber>
                <FirstName>John</FirstName>
                <LastName>Gardiner</LastName>
                <Email>john@office1.com</Email>
                <JobTitle>Manager</JobTitle>
                <OfficeCode>1</OfficeCode>
             </Entry>
             <Entry>
                <EmployeeNumber>3</EmployeeNumber>
                <FirstName>David</FirstName>
                <LastName>Green</LastName>
                <Email>david@office1.com</Email>
                <JobTitle>Manager</JobTitle>
                <OfficeCode>1</OfficeCode>
             </Entry>
          </Entries>
       </Entry>
    </Entries>
```

``` java
    {
       "Offices":{
          "Office":[
             {
                "Phone":"+441523624",
                "Country":"United Kingdom",
                "OfficeCode":1,
                "City":"Norwich",
                "Entries":{
                   "Entry":[
                      {
                         "EmployeeNumber":"1",
                         "FirstName":"John",
                         "LastName":"Gardiner",
                         "Email":"john@office1.com",
                         "JobTitle":"Manager",
                         "OfficeCode":"1"
                      },
                      {
                         "EmployeeNumber":"3",
                         "FirstName":"David",
                         "LastName":"Green",
                         "Email":"david@office1.com",
                         "JobTitle":"Manager",
                         "OfficeCode":"1"
                      },
                      {
                         "EmployeeNumber":"1002",
                         "FirstName":"Peter",
                         "LastName":"Parker",
                         "Email":"peter@wso2.com",
                         "JobTitle":null,
                         "OfficeCode":"1"
                      },
                      {
                         "EmployeeNumber":"1003",
                         "FirstName":"Chris",
                         "LastName":"Sam",
                         "Email":"chris@sam.com",
                         "JobTitle":null,
                         "OfficeCode":"1"
                      },
                      {
                         "EmployeeNumber":"1006",
                         "FirstName":"Chris",
                         "LastName":"Sam",
                         "Email":"chris@sam.com",
                         "JobTitle":null,
                         "OfficeCode":"1"
                      },
                      {
                         "EmployeeNumber":"1007",
                         "FirstName":"John",
                         "LastName":"Doe",
                         "Email":"johnd@wso2.com",
                         "JobTitle":null,
                         "OfficeCode":"1"
                      },
                      {
                         "EmployeeNumber":"1008",
                         "FirstName":"Peter",
                         "LastName":"Parker",
                         "Email":"peterp@wso2.com",
                         "JobTitle":null,
                         "OfficeCode":"1"
                      }
                   ]
                }
             }
          ]
       }
    }
```
