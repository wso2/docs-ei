# Exposing a Datasource as a Data Service

In this tutorial, we will run through the process of service enabling an
**RDBMS** as a data service.  For more information on the datasource
types supported by WSO2 EI, see the [What's
Next](#ExposingaDatasourceasaDataService-What'sNext) section.

!!! tip

Before you begin!

Follow the steps given below to set up a MySQL database for this
tutorial.

1.  Install the MySQL server.
2.  Download the product installer from
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

3.  Download the JDBC driver for MySQL from
    [here](http://dev.mysql.com/downloads/connector/j/) . Unzip it, get
    the
    `           <MySQL_HOME>/mysql-connector-java-8.0.16.jar          `
    JAR, and place it in the `           <EI_HOME>/lib          `
    directory.

    !!! note

    If the driver class does not exist in the relevant folders when you
    create the datasource, you will get an exception, such as '
    `           Cannot load JDBC driver class com.mysql.jdbc.Driver          `
    '.


4.  Create a database named `           Employees          ` .

    ``` java
        CREATE DATABASE Employees;
    ```

5.  Create the Employee table inside the Employees database:

    ``` java
            USE Employees;
        
            CREATE TABLE Employees (EmployeeNumber int(11) NOT NULL, FirstName varchar(255) NOT NULL, LastName varchar(255) DEFAULT NULL, Email varchar(255) DEFAULT NULL, Salary varchar(255));
    ```


Let's get started

-   [Creating the data
    service](#ExposingaDatasourceasaDataService-Creatingthedataservice)
    -   [Connecting to the
        datasource](#ExposingaDatasourceasaDataService-Connectingtothedatasource)
    -   [Creating a query to Get
        data](#ExposingaDatasourceasaDataService-CreatingaquerytoGetdata)
    -   [Creating a query to Post
        data](#ExposingaDatasourceasaDataService-CreatingaquerytoPostdata)
    -   [Creating a query to Update
        data](#ExposingaDatasourceasaDataService-CreatingaquerytoUpdatedata)
    -   [Create SOAP operations to invoke
        queries](#ExposingaDatasourceasaDataService-CreateSOAPoperationstoinvokequeries)
    -   [Create REST resources to invoke
        queries](#ExposingaDatasourceasaDataService-CreateRESTresourcestoinvokequeries)
-   [Invoking your data service using
    SOAP](#ExposingaDatasourceasaDataService-InvokingyourdataserviceusingSOAP)
    -   [Post new data](#ExposingaDatasourceasaDataService-Postnewdata)
    -   [Get data](#ExposingaDatasourceasaDataService-Getdata)
    -   [Update data](#ExposingaDatasourceasaDataService-Updatedata)
-   [Invoking your data service using
    REST](#ExposingaDatasourceasaDataService-InvokingyourdataserviceusingREST)
    -   [Post new
        data](#ExposingaDatasourceasaDataService-Postnewdata.1)
    -   [Get data](#ExposingaDatasourceasaDataService-Getdata.1)
    -   [Update data](#ExposingaDatasourceasaDataService-Updatedata.1)

### Creating the data service

Follow the steps given below.

1.  Start the WSO2 ESB profile.

    -   [**On MacOS/Linux/CentOS**](#3ac4441c000941289b4095122e24b29e)
    -   [**On Windows**](#2b536f66d6944ec7a2a9a652c491214b)

    Open a terminal and execute the following command:

    ``` java
        sudo wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

2.  Open the ESB profile's Management Console using
    <https://localhost:9443/carbon> , and log in using
    `          admin         ` as the username and the password.
3.  Click **Data Service → Create** , to start creating a data service.
4.  Enter the following name for the data service.

    |                   |                  |
    |-------------------|------------------|
    | Data Service Name | RDBMSDataService |

5.  Click **Next** to enter the datasource connection details.

#### Connecting to the datasource

Follow the steps given below.

1.  Click **Add New Datasource** and enter the following details:

    <table>
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
    <td><code>                               jdbc:mysql://localhost:3306/Employees                             </code></td>
    </tr>
    <tr class="odd">
    <td>User Name</td>
    <td>Enter your MySQL server's username.</td>
    </tr>
    <tr class="even">
    <td>Password</td>
    <td>Enter your MySQL server's password.<br />
    If you have not assigned a password, keep this field empty.</td>
    </tr>
    </tbody>
    </table>

        !!! info
    
        If you enter **External** instead of the Default datasource type,
        your datasource should be supported by an external provider class,
        such as
        `           com.mysql.jdbc.jdbc2.optional.MysqlXADataSource.          `
        You can select the **External** option and enter the name and value
        of connection properties by clicking **Add Property** . For
        example,  
        ![](attachments/119133236/119133242.png){width="533" height="250"}  
        After an external datasource is created, it can be used as a
        usual datasource in queries. See the tutorial on [handling
        distributed transactions](_Handling_Distributed_Transactions_) for
        more information on using external datasources.
    

2.  Save the datasource.
3.  Click **Next** , to start creating queries.

#### Creating a query to Get data

Let's create a query that can retrieve employee data, based on the
employee number. That is, when the employee number is provided as an
input, the data service should get the relevant employee details and
present the result.

1.  Click **Add New Query** to start creating a new query.

2.  Enter the following details:

    <table>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td><code>               GetEmployeeDetails              </code></td>
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
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>select EmployeeNumber, FirstName, LastName, Email, Salary from Employees where EmployeeNumber=:EmployeeNumber</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

3.  Click **Generate Input Mapping** to create the input mapping. The
    employee number is the input as shown below.

    ![](attachments/119133236/119133237.png){width="946" height="131"}

4.  Click **Generate Response** to create the output mapping. This
    defines how the employee details retrieved from the datasource will
    be presented in the result. Note that, by default, the output type
    is XML.

        !!! note
    
        If required, you can choose RDF, or JSON as the output type. See
        [Using JSON with Data Services](_Using_JSON_with_Data_Services_) for
        more information on exposing data in JSON format.
    

    ![](attachments/119133236/119133238.png){width="700"}

5.  Save the query.

#### Creating a query to Post data

Let's create another query to post new employee data to the datasource.

1.  Click **Add New Query** to start creating a new query.

2.  Enter the following details:

    <table>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td><code>               AddEmployeeDetails              </code></td>
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
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>insert into <span class="fu">Employees</span> (EmployeeNumber, FirstName, LastName, Email, Salary) <span class="fu">values</span>(:EmployeeNumber,:FirstName,:LastName,:Email,:Salary)</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

3.  Click **Generate Input Mapping** to create the input mapping. You
    need to specify values for the following elements when posting new
    employee data.

    ![](attachments/119133236/119133239.png){width="941" height="244"}

4.  Save the query.

#### Creating a query to Update data

Now, let's create a query that can update an existing employee record in
the datasource.

1.  Click **Add New Query** to start creating a new query.

2.  Enter the following details:

    <table>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td><code>               UpdateEmployeeDetails              </code></td>
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
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>update Employees set LastName=:LastName, FirstName=:FirstName, Email=:Email, Salary=:Salary where EmployeeNumber=:EmployeeNumber</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

3.  Click **Generate Input Mapping** to create the input mapping. You
    need to specify values for the following elements when posting new
    employee data.

    ![](attachments/119133236/119133240.png){width="943" height="241"}

4.  Save the query.

You should now have the following three queries created:  
![](attachments/119133236/119133241.png){width="658" height="192"}

#### Create SOAP operations to invoke queries

Now, let's create SOAP operations to invoke the queries created above.
Alternatively, you can create REST resources to invoke the queries. See
the [next
section](#ExposingaDatasourceasaDataService-CreateRESTresourcestoinvokequeries)
for instructions.

1.  Click **Add New Operation** and enter the details as shown below.

    |                |                                                   |
    |----------------|---------------------------------------------------|
    | Operation Name | `               GetEmployeeOp              `      |
    | Query ID       | `               GetEmployeeDetails              ` |

2.  Save the operation.
3.  Click **Add New Operation** and enter the details as shown below.

    |                |                                                   |
    |----------------|---------------------------------------------------|
    | Operation Name | `               AddEmployeeOp              `      |
    | Query ID       | `               AddEmployeeDetails              ` |

4.  Save the operation.
5.  Click **Add New Operation** and enter the details as shown below.

    |                |                                                      |
    |----------------|------------------------------------------------------|
    | Operation Name | `               UpdateEmployeeOp              `      |
    | Query ID       | `               UpdateEmployeeDetails              ` |

6.  Save the operation.

You can now invoke the data service query using SOAP.

#### Create REST resources to invoke queries

Now, let's create REST resources to invoke the queries created above.
Alternatively, you can create SOAP operations to invoke the queries. See
the [previous
section](#ExposingaDatasourceasaDataService-CreateSOAPoperationstoinvokequeries)
, for instructions.

1.  Click **Add New Resource** and enter the details as shown below.

    |                 |                                                          |
    |-----------------|----------------------------------------------------------|
    | Resource Path   | `               Employee/{EmployeeNumber}              ` |
    | Resource Method | `               GET              `                       |
    | Query ID        | `               GetEmployeeDetails              `        |

2.  Save the resource.
3.  Click **Add New Resource** and enter the details as shown below.

    |                 |                                                   |
    |-----------------|---------------------------------------------------|
    | Resource Path   | `               Employee              `           |
    | Resource Method | `               POST              `               |
    | Query ID        | `               AddEmployeeDetails              ` |

4.  Save the resource.
5.  Click **Add New Resource** and enter the details as shown below.

    |                 |                                                      |
    |-----------------|------------------------------------------------------|
    | Resource Path   | `               Employee              `              |
    | Resource Method | `               PUT              `                   |
    | Query ID        | `               UpdateEmployeeDetails              ` |

    Save the resource.

6.  Click **Finish** to complete creating the data service.

Once you have defined the operation, click **Finish** to complete the
data service creation process. You will now be taken to the **Deployed
Services** screen, which shows all the data services deployed on the
server.  
You can now invoke the data service query using REST.

------------------------------------------------------------------------

### Invoking your data service using SOAP

You can try the data service you created by using the TryIt tool that is
in your product by default.

1.  Go to the **Deployed Services** screen and refresh the page.
2.  Click the **Try this service** link for the **RDBMS** data service.
    The **TryIt** Tool will open with the data service.

#### Post new data

1.  Select the **AddEmployeeOp** operation you created earlier.
2.  Provide the employee details by copying the code given below.

    ``` java
        <p:AddEmployeeOp xmlns:p="http://ws.wso2.org/dataservice">
              <!--Exactly 1 occurrence-->
              <xs:EmployeeNumber xmlns:xs="http://ws.wso2.org/dataservice">1</xs:EmployeeNumber>
              <!--Exactly 1 occurrence-->
              <xs:FirstName xmlns:xs="http://ws.wso2.org/dataservice">John</xs:FirstName>
              <!--Exactly 1 occurrence-->
              <xs:LastName xmlns:xs="http://ws.wso2.org/dataservice">Doe</xs:LastName>
              <!--Exactly 1 occurrence-->
              <xs:Email xmlns:xs="http://ws.wso2.org/dataservice">JohnDoe@gmail.com</xs:Email>
              <!--Exactly 1 occurrence-->
              <xs:Salary xmlns:xs="http://ws.wso2.org/dataservice">10000</xs:Salary>
           </p:AddEmployeeOp>
    ```

3.  Click **Send** .

The data is now added to the database.

#### Get data

1.  Select the GetEmployeeOp operation you created earlier. You need to
    provide the employee number as an input. Enter '
    `          1         ` '.
2.  Click **Send** to see the details of the employee you added
    previously:

    ``` java
            <Entries xmlns="http://ws.wso2.org/dataservice">
               <Entry>
                  <EmployeeNumber>1</EmployeeNumber>
                  <FirstName>John</FirstName>
                  <LastName>Doe</LastName>
                  <Email>JohnDoe@gmail.com</Email>
               </Entry>
            </Entries>
    ```

#### Update data

1.  Select the **UpdateEmployeeOp** operation you created earlier.
2.  Update the employee details by copying the code given below. Note
    that the salary value is changed to 20000.

    ``` java
            <p:UpdateEmployeeOp xmlns:p="http://ws.wso2.org/dataservice">
                  <!--Exactly 1 occurrence-->
                  <xs:LastName xmlns:xs="http://ws.wso2.org/dataservice">Doe</xs:LastName>
                  <!--Exactly 1 occurrence-->
                  <xs:FirstName xmlns:xs="http://ws.wso2.org/dataservice">John</xs:FirstName>
                  <!--Exactly 1 occurrence-->
                  <xs:Email xmlns:xs="http://ws.wso2.org/dataservice">JohnDoe@gmail.com</xs:Email>
                  <!--Exactly 1 occurrence-->
                  <xs:Salary xmlns:xs="http://ws.wso2.org/dataservice">20000</xs:Salary>
                  <!--Exactly 1 occurrence-->
                  <xs:EmployeeNumber xmlns:xs="http://ws.wso2.org/dataservice">1</xs:EmployeeNumber>
            </p:UpdateEmployeeOp>
    ```

3.  Click **Send** .

The data is now updated in the database.

### Invoking your data service using REST

Let's take a look at the curl commands that are used to send the HTTP
requests for each of the resources:

#### Post new data

1.  Create a file called `           employee-payload.xml          `
    file, and define the XML payload for posting new data as shown
    below.

    ``` java
            <_postemployee>
                <EmployeeNumber>3</EmployeeNumber>
                <FirstName>Will</FirstName>
                <LastName>Smith</LastName>
                <Email>will@google.com</Email>
                <Salary>15500.0</Salary>
            </_postemployee>
    ```

2.  Send the following HTTP request from the location where the
    `           employee-payload.xml          ` file is stored:

    ``` java
            curl -X POST -H 'Accept: application/xml'  -H 'Content-Type: application/xml' --data "@employee-payload.xml" http://localhost:8280/services/RDBMSDataService/employee
    ```

#### Get data

The service can be invoked in REST-style via curl (
[http://curl.haxx.se](http://curl.haxx.se/) ). Shown below is the curl
command to invoke the GET resource:

``` java
    curl -X GET http://localhost:8280/services/RDBMSDataService.HTTPEndpoint/Employee/3
```

This generates a response as follows.

``` java
    <Entries xmlns="http://ws.wso2.org/dataservice"><Entry><EmployeeNumber>3</EmployeeNumber><FirstName>Will</FirstName><LastName>Smith</LastName><Email>will@google.com</Email><Salary>15500.0</Salary></Entry><Entry><EmployeeNumber>3</EmployeeNumber><FirstName>Will</FirstName><LastName>Smith</LastName><Email>will@google.com</Email><Salary>15500.0</Salary></Entry><Entry><EmployeeNumber>3</EmployeeNumber><FirstName>Will</FirstName><LastName>Smith</LastName><Email>will@google.com</Email><Salary>15500.0</Salary></Entry></Entries>
```

#### Update data

1.  Create a file called
    `           employee-update-payload.xml          ` file, and define
    the XML payload for updating an existing employee record as shown
    below.

    ``` java
            <_putemployee>
                <EmployeeNumber>3</EmployeeNumber>
                <LastName>Smith</LastName>
                <FirstName>Will</FirstName>
                <Email>will@google.com</Email>
                <Salary>30000.0</Salary>
            </_putemployee>
    ```

2.  Send the following HTTP request from the location where the
    `           employee-update-payload.xml          ` file is stored:

    ``` java
            curl -X PUT -H 'Accept: application/xml'  -H 'Content-Type: application/xml' --data "@employee-update-payload.xml" http://localhost:8280/services/RDBMSDataService/employee
    ```

## What's Next

-   Want to try out the datasource types supported by WSO2 EI? See the
    sections given below:
    -   [Exposing a Google Spreadsheet as a Data
        Service](https://docs.wso2.com/display/EI650/Exposing+a+Google+Spreadsheet+as+a+Data+Service)

    -   [Exposing CSV Data as a Data
        Service](https://docs.wso2.com/display/EI650/Exposing+CSV+Data+as+a+Data+Service)

    -   [Exposing Excel Data as a Data
        Service](https://docs.wso2.com/display/EI650/Exposing+Excel+Data+as+a+Data+Service)

    -   [Exposing RDF Data as a Data
        Service](https://docs.wso2.com/display/EI650/Exposing+RDF+Data+as+a+Data+Service)

    -   [Exposing MongoDB as a Data
        Service](https://docs.wso2.com/display/EI650/Exposing+MongoDB+as+a+Data+Service)

    -   [Exposing a Web Resource as a Data
        Service](https://docs.wso2.com/display/EI650/Exposing+a+Web+Resource+as+a+Data+Service)

    -   [Exposing a Carbon Datasource as a Data
        Service](https://docs.wso2.com/display/EI650/Exposing+a+Carbon+Datasource+as+a+Data+Service)

    -   [Exposing a Custom Datasource as a Data
        Service](https://docs.wso2.com/display/EI650/Exposing+a+Custom+Datasource+as+a+Data+Service)

    -   [Exposing Cassandra as a Data
        Service](https://docs.wso2.com/display/EI650/Exposing+Cassandra+as+a+Data+Service)

    -   [Exposing a JNDI Datasource as a Data
        Service](https://docs.wso2.com/display/EI650/Exposing+a+JNDI+Datasource+as+a+Data+Service)

-   Try out the [Data Integration
    Samples](https://docs.wso2.com/display/EI650/Data+Integration+Samples)
    .
