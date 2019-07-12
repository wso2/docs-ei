# Invoking an Operation with Multiple Records

The batch requests feature allows you to send multiple (IN-Only)
requests to a datasource using a single operation (batch operation).
Follow the steps given below to define a data service that can invoke
batch requests:

-   [Setting up
    a datasource](#InvokinganOperationwithMultipleRecords-Settingupadatasource)
-   [Define a data service to insert records in
    batches](#InvokinganOperationwithMultipleRecords-Defineadataservicetoinsertrecordsinbatches)
-   [Invoking the data
    service](#InvokinganOperationwithMultipleRecords-Invokingthedataservice)

------------------------------------------------------------------------

### Setting up a datasource

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

5.  Create the **Employees** table:

    ``` java
            USE Company;
        
        
            CREATE TABLE `Employees` (`EmployeeNumber` int(11) NOT NULL, `FirstName` varchar(255) NOT NULL, `LastName` varchar(255) DEFAULT NULL, `Email` varchar(255) DEFAULT NULL, `JobTitle` varchar(255) DEFAULT NULL, `OfficeCode` int(11) NOT NULL, PRIMARY KEY (`EmployeeNumber`,`OfficeCode`));
    ```

------------------------------------------------------------------------

### Define a data service to insert records in batches

Let's create a data service using the **Create Data Service** wizard:

Start the WSO2 ESB profile.

-   [**On MacOS/Linux/CentOS**](#d9c7f22ca0cd46d3883cd4bace58b4e6)
-   [**On Windows**](#4fee889428054457bc4769cf2cc08b98)

Open a terminal and execute the following command:

``` java
    sudo wso2ei-6.5.0-integrator
```

Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator 6.5.0
Integrator.** This will open a terminal and start the ESB profile.

Open the ESB profile's Management Console using
`                     https://localhost:9443/carbon                   `
, and log in using `          admin         ` as the username and the
password.

Click **Data Service \> Create** .

Add a name for the data service, such as
`          BatchRequestDataService         ` .

Select the **Enable Batch Requests** check box.  
![](attachments/119133265/119133270.png){width="525" height="233"}

Click **Next** and add a new datasource.

Give an ID for the datasource, and create the connection to the
**Company** database.

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
<td><code>               jdbc:mysql://localhost:3306/Company              </code></td>
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

Example:  
![](attachments/119133265/119133268.png){width="577" height="250"}

Save the datasource details, and click **Next** to open the **Queries**
screen.

Click **Add New Query** to specify the query details:

1.  Enter **addEmployeeQuery** as the query ID.

2.  Enter the following SQL dialect:

    ``` java
            insert into Employees (EmployeeNumber, FirstName, LastName, Email, JobTitle, OfficeCode) values(:EmployeeNumber,:FirstName,:LastName,:Email,:JobTitle,:Officecode)
    ```

3.  Click **Generate Input Mapping** and input mappings will be
    generated automatically for the EmployeeNumber, FirstName, LastName,
    Email, JobTitle, and OfficeCode fields.

Save the query, and click **Next** to open the **Operations** screen.

Click **Add New Operation** , and create an operation for the
**addEmployeeQuery** query as shown below.  
![](attachments/119133265/119133267.png){width="500" height="399"}

Save the operation.

Click **Finish** to complete the data service creation process.

### Invoking the data service

You can try the data service you created by using the TryIt tool that is
in your product by default.

1.  Go to the **Deployed Services** screen.
2.  Click **Try this Service** to open the data service from the TryIt
    tool.
3.  Click **Try this Service** to open the data service from the TryIt
    tool. There will be an **addemployeeOp\_batch\_request operation**
    automatically generated.
4.  Select the batch operation.
5.  Enter multiple transactions as shown below. In this example, we are
    sending two transactions with details of two employees.

    ``` java
            <p:addEmployeeOp_batch_req xmlns:p="http://ws.wso2.org/dataservice">
                  <!--1 or more occurrences-->
                  <addEmployeeOp xmlns="http://ws.wso2.org/dataservice">
                     <!--Exactly 1 occurrence-->
                     <xs:EmployeeNumber xmlns:xs="http://ws.wso2.org/dataservice">1002</xs:EmployeeNumber>
                     <!--Exactly 1 occurrence-->
                     <xs:FirstName xmlns:xs="http://ws.wso2.org/dataservice">John</xs:FirstName>
                     <!--Exactly 1 occurrence-->
                     <xs:LastName xmlns:xs="http://ws.wso2.org/dataservice">Doe</xs:LastName>
                     <!--Exactly 1 occurrence-->
                     <xs:Email xmlns:xs="http://ws.wso2.org/dataservice">johnd@wso2.com</xs:Email>
                     <!--Exactly 1 occurrence-->
                     <xs:JobTitle xmlns:xs="http://ws.wso2.org/dataservice">Consultant</xs:JobTitle>
                     <!--Exactly 1 occurrence-->
                     <xs:Officecode xmlns:xs="http://ws.wso2.org/dataservice">01</xs:Officecode>
                  </addEmployeeOp>
                  <addEmployeeOp xmlns="http://ws.wso2.org/dataservice">
                     <!--Exactly 1 occurrence-->
                     <xs:EmployeeNumber xmlns:xs="http://ws.wso2.org/dataservice">1004</xs:EmployeeNumber>
                     <!--Exactly 1 occurrence-->
                     <xs:FirstName xmlns:xs="http://ws.wso2.org/dataservice">Peter</xs:FirstName>
                     <!--Exactly 1 occurrence-->
                     <xs:LastName xmlns:xs="http://ws.wso2.org/dataservice">Parker</xs:LastName>
                     <!--Exactly 1 occurrence-->
                     <xs:Email xmlns:xs="http://ws.wso2.org/dataservice">peterp@wso2.com</xs:Email>
                     <!--Exactly 1 occurrence-->
                     <xs:JobTitle xmlns:xs="http://ws.wso2.org/dataservice">Consultant</xs:JobTitle>
                     <!--Exactly 1 occurrence-->
                     <xs:Officecode xmlns:xs="http://ws.wso2.org/dataservice">01</xs:Officecode>
                  </addEmployeeOp>
               </p:addEmployeeOp_batch_req>
    ```

6.  Execute the batch operation. You will find that all the records have
    been inserted into the database simultaneously.

        !!! tip
    
        Want to confirm that the records are added to the database? Run the
        following MySQL command.
    
    ``` java
            SELECT * FROM Employees
    ```
        
            You see all the records that are in the
            `           Employees          ` table, including the two employee
            records that you added in the previous step.  
            ![](attachments/119133265/119133266.png){width="885"}
        
