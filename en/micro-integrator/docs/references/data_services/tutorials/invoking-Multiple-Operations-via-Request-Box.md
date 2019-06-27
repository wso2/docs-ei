# Invoking Multiple Operations via Request Box

**In this tutorial** , you define a data service that can invoke request
box operations. The **request box** feature allows you to invoke
multiple operations (consecutively) to a datasource using a single
operation.

-   [Setting up
    a datasource](#InvokingMultipleOperationsviaRequestBox-Settingupadatasource)
-   [Define a data service to invoke request
    box operations](#InvokingMultipleOperationsviaRequestBox-Defineadataservicetoinvokerequestboxoperations)
-   [Invoking the data
    service](#InvokingMultipleOperationsviaRequestBox-Invokingthedataservice)

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
3.  Download the [JDBC driver for
    MySQL](https://dev.mysql.com/downloads/connector/j/) and copy it to
    your `          <EI_HOME>/lib         ` directory.
4.  Create a database named **Company** .

    ``` java
        CREATE DATABASE Company;
    ```

5.  Create the **Employees** table:

    ``` java
            USE Company;
        
            CREATE TABLE `Employees` (`EmployeeNumber` int(11) NOT NULL, `FirstName` varchar(255) NOT NULL, `LastName` varchar(255) DEFAULT NULL, `Email` varchar(255) DEFAULT NULL, `JobTitle` varchar(255) DEFAULT NULL, `OfficeCode` int(11) NOT NULL, PRIMARY KEY (`EmployeeNumber`,`OfficeCode`));
    ```

------------------------------------------------------------------------

### Define a data service to invoke request box operations

Let's create a data service using the **Create Data Service** wizard:

Start the WSO2 ESB profile.

-   [**On MacOS/Linux/CentOS**](#a8917114d9b24b27a835e2bb7e82da67)
-   [**On Windows**](#1d7e68afc5ef46948ed76f0d22099697)

Open a terminal and execute the following command:

``` java
    sudo wso2ei-6.5.0-integrator
```

Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator 6.5.0
Integrator.** This will open a terminal and start the ESB profile.

Open the ESB profile's Management Console using
<https://localhost:9443/carbon> , and log in using
`          admin         ` as the username and the password.

Click **Create** under **Data Service** .

Add a name for the data service.

Select the **Enable Boxcarring** check box.  
As a result of selecting this check box, the **Disable Legacy Boxcarring
Mode** check box will appear on your screen as shown below. Select this
new check box ( **Disable Legacy Boxcarring Mode** ) as well.

![](attachments/119133247/119133248.png){width="400" height="109"}

!!! note

What is Boxcarring and Request Box?

**Boxcarring** is a method of grouping a set of service calls so that
they can be executed as a group (i.e., the individual service calls are
executed consecutively in the specified order). Note that we have now
deprecated the boxcarring method for data services. Instead, we have
replaced boxcarring with a new request type called **request\_box** .

**Request box** is simply a wrapper element (request\_box), which wraps
the service calls that need to be invoked. When the request\_box is
invoked, the individual service calls are executed in the specified
order, and the result of the last service call in the list are returned.
In this tutorial, we are using the request box to invoke the following
two service calls:

1.  Add a new employee to the database
2.  Get details of the office of the added employee

When you click the **Enable Boxcarring** check box for the data service,
both of the above functions ( **Boxcarring** and **Request box** ) are
enabled. However, since boxcarring is deprecated in the product, it is
recommended to disable boxcarring by clicking the **Disable Legacy
Boxcarring Mode** check box.


Click **Next** and then click **Add New Datasource** .

Connect to the **Company** database that you defined above.

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

Click **Save** and then click **Next** to go to the **Queries** screen.

Click **Add New Query** to specify the query details:

1.  Enter **addEmployeeQuery** as the query ID.

2.  Enter the following SQL dialect:

    ``` java
        insert into Employees (EmployeeNumber, FirstName, LastName, Email,OfficeCode) values(:EmployeeNumber,:FirstName,:LastName,:Email,:OfficeCode)
    ```

3.  Click **Generate Input Mapping** and input mappings are generated
    automatically.

4.  Click **Save** .

Click **Add New Query** to Create another query for the **Company**
datasource:

1.  Enter **selectEmployeebyIDQuery** as the query ID.

2.  Enter the following SQL dialect:

    ``` java
            select EmployeeNumber, FirstName, LastName, Email, OfficeCode from Employees where EmployeeNumber=:EmployeeNumber
    ```

3.  Generate input and output mappings:

    1.  Click **Generate Input Mapping** and input mappings
        are generated automatically for the employee number.

    2.  Click **Generate Response** to automatically generate the output
        mappings for the fields in the **Employees** table.

4.  Click **Save** .

Click **Next** and then click **Add New Operation.**

Create two operations to add and select employees.

1.  Enter the following details to create the **addEmployeeOp**
    operation that adds new employees to the database.

    |                |                                                     |
    |----------------|-----------------------------------------------------|
    | Operation Name | `                 addEmployeeOp                `    |
    | Query ID       | `                 addEmployeeQuery                ` |

2.  Save the operation.
3.  Click **Add New Operation** and enter the details as shown below to
    create the **selectEmployeeOp** operation that gets the details of
    an employee from the database.

    |                |                                                            |
    |----------------|------------------------------------------------------------|
    | Operation Name | `                 selectEmployeeOp                `        |
    | Query ID       | `                 selectEmployeebyIDQuery                ` |

4.  Click **Save** .

Click **Finish** .

### Invoking the data service

The **Deployed Services** window allows you to manage data services. You
can try the data service you created by using the TryIt tool in this
screen, which is in your product by default.

1.  In the **Deployed Services** screen, click **Try this Service** to
    open the data service you just created from the TryIt tool.  
    You will find that there is an additional request\_box operation.
2.  Select the **request\_box** operation and enter the following
    values:

    -   Enter values for the **addEmployeeQuery** query.

    -   Enter the same employee number you used for the above query in
        the **selectEmployeebyID** query:

    Copy the following, paste it in the TryIt tool, and click **Send**
    to try it out:

    ``` java
            <body>
               <p:request_box xmlns:p="http://ws.wso2.org/dataservice">
                  <!--Exactly 1 occurrence-->
                  <addEmployeeOp xmlns="http://ws.wso2.org/dataservice">
                     <!--Exactly 1 occurrence-->
                     <xs:EmployeeNumber xmlns:xs="http://ws.wso2.org/dataservice">1003</xs:EmployeeNumber>
                     <!--Exactly 1 occurrence-->
                     <xs:FirstName xmlns:xs="http://ws.wso2.org/dataservice">Chris</xs:FirstName>
                     <!--Exactly 1 occurrence-->
                     <xs:LastName xmlns:xs="http://ws.wso2.org/dataservice">Sam</xs:LastName>
                     <!--Exactly 1 occurrence-->
                     <xs:Email xmlns:xs="http://ws.wso2.org/dataservice">chris@sam.com</xs:Email>
                     <!--Exactly 1 occurrence-->
                     <xs:OfficeCode xmlns:xs="http://ws.wso2.org/dataservice">1</xs:OfficeCode>
                  </addEmployeeOp>
                  <!--Exactly 1 occurrence-->
                  <selectEmployeeOp xmlns="http://ws.wso2.org/dataservice">
                     <!--Exactly 1 occurrence-->
                     <xs:EmployeeNumber xmlns:xs="http://ws.wso2.org/dataservice">1003</xs:EmployeeNumber>
                  </selectEmployeeOp>
               </p:request_box>
            </body>
    ```

        !!! info
    
        The following events take place once you click **Send** :
    
        -   First, the **addEmployee** **Op** operation is executed. The
            data is successfully added to the database.
        -   Secondly, the **selectEmployee** **Op** operationis executed and
            the details of the office attached to the employee are returned
            in the result as shown below.
    

    You get the following response:

    ``` java
        <axis2ns26:DATA_SERVICE_REQUEST_BOX_RESPONSE xmlns:axis2ns26="http://ws.wso2.org/dataservice">
           <Entries xmlns="http://ws.wso2.org/dataservice">
              <Entry>
                 <EmployeeNumber>1003</EmployeeNumber>
                 <FirstName>Chris</FirstName>
                 <LastName>Sam</LastName>
                 <Email>chris@sam.com</Email>
                 <OfficeCode>1</OfficeCode>
              </Entry>
           </Entries>
        </axis2ns26:DATA_SERVICE_REQUEST_BOX_RESPONSE>
    ```
