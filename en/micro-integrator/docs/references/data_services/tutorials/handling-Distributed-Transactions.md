# Handling Distributed Transactions

The data integration feature in the ESB profile of WSO2 EI supports data
federation, which means that a single data service can expose data from
multiple datasources. However, if you have multiple RDBMSs connected to
your data service, and if you need to perform IN-ONLY operations
(operations that can insert data and modify data in the datasource) in a
coordinated manner, the RDBMSs need to be defined as XA datasources.

Let's consider a scenario where you have two MySQL databases. You can
define a single data service for these databases and insert data into
both as explained below.

-   [Setting up distributed MySQL
    databases](#HandlingDistributedTransactions-SettingupdistributedMySQLdatabases)
-   [Adding the datasources to a data
    service](#HandlingDistributedTransactions-Addingthedatasourcestoadataservice)
-   [Defining queries for the
    datasources](#HandlingDistributedTransactions-Definingqueriesforthedatasources)
-   [Inserting data into the distributed
    RDBMSs](#HandlingDistributedTransactions-InsertingdataintothedistributedRDBMSs)

------------------------------------------------------------------------

### Setting up distributed MySQL databases

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
4.  Set up a database for storing information of offices:
    1.  Create a database called **OfficeDetails** . **  
        **

        ``` java
                CREATE DATABASE OfficeDetails;
        ```

    2.  Create the **Offices** table:

        ``` java
                    USE OfficeDetails;
            
                    CREATE TABLE `Offices` (`OfficeCode` int(11) NOT NULL, `AddressLine1` varchar(255) NOT NULL, `AddressLine2` varchar(255) DEFAULT NULL, `City` varchar(255) DEFAULT NULL, `State` varchar(255) DEFAULT NULL, `Country` varchar(255) DEFAULT NULL, `Phone` varchar(255) DEFAULT NULL, PRIMARY KEY (`OfficeCode`));
        ```

5.  Set up a database to store the employee information:
    1.  Create a database called **EmployeeDetails** .

        ``` java
                    CREATE DATABASE EmployeeDetails;
        ```

    2.  Create the **Employees** table:

        ``` java
                    USE EmployeeDetails;
            
                    CREATE TABLE `Employees` (`EmployeeNumber` int(11) NOT NULL, `FirstName` varchar(255) NOT NULL, `LastName` varchar(255) DEFAULT NULL, `Email` varchar(255) DEFAULT NULL, `JobTitle` varchar(255) DEFAULT NULL, `OfficeCode` int(11) NOT NULL, PRIMARY KEY (`EmployeeNumber`));
        ```

------------------------------------------------------------------------

### Adding the datasources to a data service

Let's create a data service using the **Create Data Service** wizard:

1.  Start the WSO2 ESB profile.

    -   [**On MacOS/Linux/CentOS**](#77999931c38f4651b05008d1a8254af0)
    -   [**On Windows**](#54bdf8f5a5ea40ebb9f90a98a84ec64d)

    Open a terminal and execute the following command:

    ``` java
            sudo wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

2.  Access the management console of the ESB profile:
    `          https://localhost:9443/carbon         `
3.  Sign in using `          admin         ` as the username and
    password.
4.  On the **Data Service** menu click **Create** .
5.  Add a name for the data service and click **Next** .
6.  Select the **Enable Boxcarring** and **Disable Legacy Boxcarring
    Mode** check boxes.

        !!! note
    
        What is Boxcarring and Request Box?
    
        **Boxcarring** is a method of grouping a set of service calls so
        that they can be executed as a group (i.e., the individual service
        calls will be executed consecutively in the specified order). Note
        that we have now deprecated the boxcarring method for data services.
        Instead, we have replaced boxcarring with a new request type called
        **request\_box** .
    
        **Request box** is simply a wrapper element (request\_box), which
        wraps the service calls that need to be invoked. When the
        request\_box is invoked, the individual service calls will be
        executed in the specified order, and the result of the last service
        call in the list will be returned. In this tutorial, we are using
        the request box to invoke the following two service calls:
    
        1.  Add a new employee to the database
        2.  Get details of the office of the added employee
    
        When you click the **Enable Boxcarring** check box for the data
        service, both of the above functions ( **Boxcarring** and **Request
        box** ) are enabled. However, since boxcarring is deprecated in the
        product, it is recommended to disable boxcarring by clicking the
        **Disable Legacy Boxcarring Mode** check box.
    

7.  Click **Next** to go to the **Datasources** screen.
8.  Click **Add New Datasource** to create a datasource connection for
    the **OfficeDetails** database:

    <table>
    <tbody>
    <tr class="odd">
    <td>Datasource Id</td>
    <td>Enter <strong>XAoffices</strong> as the datasource ID.</td>
    </tr>
    <tr class="even">
    <td>Datasource Type</td>
    <td><p>Select <strong>RDBMS</strong> as the datasource type and s elect <strong>External Datasource</strong> from the adjoining field.</p></td>
    </tr>
    <tr class="odd">
    <td>Database Engine</td>
    <td><p>Select <strong>MySQL</strong> .</p></td>
    </tr>
    <tr class="even">
    <td>Datasource Class Name</td>
    <td><div class="content-wrapper">
    <p>Enter the database class that corresponds to <code>                 MysqlXADataSource.class                </code> .<br />
    Example: For MySQL 5, enter <code>                 com.mysql.jdbc.jdbc2.optional.MysqlXADataSource                </code></p>
        !!! tip
        <p>Make sure to check the path of the <code>                 MysqlXADataSource.class                </code> and enter the updated path here. Else, you run into errors.</p>

    </div></td>
    </tr>
    <tr class="odd">
    <td>Add New Property</td>
    <td><p>Specify the following connection settings for the <strong>OfficeDetails</strong> database:</p>
    <div class="table-wrap">
    <table>
    <colgroup>
    <col style="width: 50%" />
    <col style="width: 50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Property Name</th>
    <th>Value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><code>                    url                   </code></td>
    <td><code>                    jdbc:mysql://localhost:3306/OfficeDetails                   </code></td>
    </tr>
    <tr class="even">
    <td><code>                    user                   </code></td>
    <td>Enter your MySQL server's username.<br />
    Example: root</td>
    </tr>
    <tr class="odd">
    <td><code>                    password                   </code></td>
    <td><strong>Optionally:</strong> Enter your MySQL server's password.<br />
    If you have not assigned a password, do not add this property.</td>
    </tr>
    </tbody>
    </table>
    </div></td>
    </tr>
    </tbody>
    </table>

    Example: The **New Datasource** screen will now look as follows:  
    ![](attachments/119133292/119133293.png){width="800" height="301"}

9.  Save the **XAoffices** datasource.
10. Click **Add New Datasource** to create a datasource connection for
    the **EmployeeDetails** database as follows:

    <table>
    <tbody>
    <tr class="odd">
    <td>Datasource Id</td>
    <td>Enter <strong>XAemployees</strong> as the datasource ID.</td>
    </tr>
    <tr class="even">
    <td>Datasource Type</td>
    <td><p>Select <strong>RDBMS</strong> as the datasource type and select <strong>External Datasource</strong> from the adjoining field.</p></td>
    </tr>
    <tr class="odd">
    <td>Database Engine</td>
    <td><p>Select <strong>MySQL</strong> .</p></td>
    </tr>
    <tr class="even">
    <td>Datasource Class Name</td>
    <td><div class="content-wrapper">
    <p>Enter the database class that corresponds to <code>                 MysqlXADataSource.class                </code> .<br />
    Example: For MySQL 5, enter <code>                 com.mysql.jdbc.jdbc2.optional.MysqlXADataSource                </code> .</p>
        !!! tip
        <p>Make sure to check the path of the <code>                 MysqlXADataSource.class                </code> and enter the updated path here. Else, you run into errors.</p>

    </div></td>
    </tr>
    <tr class="odd">
    <td>Add New Property</td>
    <td><p>Specify the following connection settings for the <strong>EmployeeDetails</strong> database:</p>
    <div class="table-wrap">
    <table>
    <colgroup>
    <col style="width: 50%" />
    <col style="width: 50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Property Name</th>
    <th>Value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><code>                    url                   </code></td>
    <td><code>                    jdbc:mysql://localhost:3306/EmployeeDetails                   </code></td>
    </tr>
    <tr class="even">
    <td><code>                    user                   </code></td>
    <td>Enter your MySQL server's username.<br />
    Example: root</td>
    </tr>
    <tr class="odd">
    <td><code>                    password                   </code></td>
    <td><strong>Optionally:</strong> Enter your MySQL server's password.<br />
    If you have not assigned a password, do not add this property.</td>
    </tr>
    </tbody>
    </table>
    </div></td>
    </tr>
    </tbody>
    </table>

    Example: The **New Datasource** screen now look as follows:  
    ![](attachments/119133292/119133294.png){width="800" height="302"}

11. Save the **XAemployees** datasource and click **Next** .

------------------------------------------------------------------------

### Defining queries for the datasources

1.  Click **Add New Query** to specify an insert query for the
    **XAoffices** datasource:
    1.  Enter **InsertOfficeQuery** as the query ID.

    2.  Enter the following SQL dialect:

        ``` java
                insert into Offices (OfficeCode,AddressLine1,AddressLine2,City,State,Country,Phone) values(:OfficeCode,:AddressLine1,'test','test','test','USA','test')
        ```

    3.  Click **Generate Input Mapping** and the input mappings are
        generated for the fields in the datasource:  
        ![](attachments/119133292/119133297.png){width="800"}

2.  Save the query.
3.  Click **Add New Query** to specify an insert query for the
    **XAemployees** datasource:
    1.  Enter **InsertEmployeeQuery** as the query ID.

    2.  Enter the following SQL dialect:

        ``` java
                    insert into Employees (EmployeeNumber,FirstName,LastName,Email,JobTitle,OfficeCode) values(:EmployeeNumber,:FirstName,:LastName,'test','test',:OfficeCode)
        ```

4.  Click **Generate Input Mapping** and the input mappings are
    generated for the fields in the datasource:  
    ![](attachments/119133292/119133295.png){width="900"}
5.  Save the query.
6.  Click **Next** to go to the **Operations** section. Define
    operations to invoke the two queries defined above.
    -   Create the **InsertOfficeOp** for the **InsertOfficeQuery** .
    -   Create the **InsertEmployeeOp** for the **InsertEmployeeQuery**
        .
7.  Finish creating the data service.

------------------------------------------------------------------------

### Inserting data into the distributed RDBMSs

1.  Go to the **Deployed Services** page and you will see the data
    service listed.
2.  Click **Try this service** to open the TryIt tool.
3.  Select the batch operation that is created by default (
    **request\_box** ).
4.  Specify the values that should be inserted to the **OfficeDetails**
    database and **EmployeeDetails** database respectively.
5.  Click **Send** to invoke the operation.
6.  See that the data is successfully inserted into the two databases.  
    Go to the MySQL terminal and run the following commands:  
    -   Check the office details in the offices table:

        ``` java
                    USE OfficeDetails;
                    SELECT * FROM Offices;
        ```

    -   Check the employee details in the employees table.

        ``` java
                    USE EmployeeDetails;
                    SELECT * FROM Employees;
        ```

7.  Now, enter another set of values for the two operations but enter an
    erroneous value for one field.
8.  Invoke the operation.
9.  Check the database tables.  
    You see that no records have been entered into either database.
