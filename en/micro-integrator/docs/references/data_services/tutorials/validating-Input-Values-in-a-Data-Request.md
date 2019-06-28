# Validating Input Values in a Data Request

Validators are added to individual input mappings in a query. Input
validation allows data services to validate the input parameters in a
request and stop the execution of the request if the input doesn’t meet
the required criteria. The ESB profile of WSO2 Enterprise Integrator
(WSO2 EI) provides a set of built-in validators for some of the most
common use cases. It also provides an extension mechanism to write
custom validators.

!!! tip

Before you begin!

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
    [here](http://dev.mysql.com/downloads/connector/j/) and copy it
    to your `           <EI_HOME>/lib          ` directory.

    !!! note

    If the driver class does not exist in the relevant folders when you
    create the datasource, you will get an exception, such as '
    `           Cannot load JDBC driver class com.mysql.jdbc.Driver          `
    '.


4.  Create a database named `           EmployeeDatabse          ` .

    ``` java
        CREATE DATABASE EmployeeDatabase;
    ```

5.  Create the Employee table inside the Employees database:

    ``` java
            USE EmployeeDatabase;
        
            CREATE TABLE Employees (EmployeeNumber int(11) NOT NULL, FirstName varchar(255) NOT NULL, LastName varchar(255) DEFAULT NULL, Email varchar(255) DEFAULT NULL, Salary varchar(255));
    ```


Let's get started!

-   [Define a data
    service](#ValidatingInputValuesinaDataRequest-Defineadataservice)
-   [Define a query with an input
    validator](#ValidatingInputValuesinaDataRequest-Defineaquerywithaninputvalidator)
-   [Try it out](#ValidatingInputValuesinaDataRequest-Tryitout)

### Define a data service

Let's create a data service using the **Create Data Service** wizard:

1.  Start the WSO2 ESB profile.

    -   [**On MacOS/Linux/CentOS**](#5c98bc21fc594e6da657643ef636a628)
    -   [**On Windows**](#1c401fe1b4c7457799500839083c2ef8)

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
4.  Add a name for the data service.

5.  Click **Next** and then click **Add New Datasource** .
6.  Connect the datasource to the **Company** database that you defined
    above.

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
    <td><code>               jdbc:mysql://localhost:3306/EmployeeDatabase              </code></td>
    </tr>
    <tr class="odd">
    <td>User Name</td>
    <td><p>Enter your MySQL server's username.<br />
    Example: root</p></td>
    </tr>
    <tr class="even">
    <td>Password</td>
    <td>Enter your MySQL server's password.<br />
    If you have not assigned a password, keep this field empty.</td>
    </tr>
    </tbody>
    </table>

7.  Click **Save** and then click **Next** to go to the **Queries**
    screen.

### Define a query with an input validator

Let's add a validator for the first name. If the user enters a name that
is less than 3 characters for the first name an error needs to be thrown
due to the validation.

!!! info

You can set up different validators based on the type of the parameter
or you can create your own custom validator. For more information, see
[Input Validators](https://docs.wso2.com/display/EI650/Input+Validators)
.


Follow the steps given below.

Click **Add New Query** to specify the query details:

1.  Enter **addEmployeeQuery** as the query ID.

2.  Enter the following SQL dialect:

    ``` java
        insert into Employees (EmployeeNumber, FirstName, LastName, Email,Salary) values(:EmployeeNumber,:FirstName,:LastName,:Email,:Salary)
    ```

3.  Click **Generate Input Mapping** and input mappings will be
    generated automatically.

4.  Click **Edit** that is next to **FirstName** .
5.  Update the input mapping:

    <table>
    <tbody>
    <tr class="odd">
    <td>SQL Type</td>
    <td>Select <strong>String</strong> .</td>
    </tr>
    <tr class="even">
    <td>IN/OUT Type</td>
    <td>Select <strong>IN</strong> .</td>
    </tr>
    <tr class="odd">
    <td>Validator</td>
    <td>Select <strong>Length Validator</strong> .
    <ul>
    <li><strong>Maximum Value</strong> : Enter 30 as the maximum number of characters that can be entered for the first name.</li>
    <li><p><strong>Minimum Value</strong> : Enter 3 as the minimum number of characters that need to be there for the first name.</p></li>
    </ul>
    <p>Click <strong>Add Validator</strong> .</p></td>
    </tr>
    </tbody>
    </table>

6.  Click **Save** and click **Main Configurations** to move to the
    query configurations.

7.  Click **Save** .

Click **Next** and then click **Add New Operation.**

1.  Enter the following details to create the **addEmployeeOp**
    operation that adds new employees to the database.

    |                |                                                     |
    |----------------|-----------------------------------------------------|
    | Operation Name | `                 addEmployeeOp                `    |
    | Query ID       | `                 addEmployeeQuery                ` |

2.  Save the operation.

Click **Finish** .

### Try it out

The **Deployed Services** window allows you to manage data services. You
can try the data service you created by using the TryIt tool in this
screen, which is in your product by default.

1.  Click **Try this service** to open the TryIt tool.
2.  Click the **addEmployeeOp** operation and enter the following
    parameter values to the request:
    -   EmployeeNumber: 6001
    -   FirstName: AB
    -   LastName: Nick
    -   Email: test@test.com
    -   Salary: 1500
3.  Click **Send** to execute the operation.  
    A validation error is thrown as the response because the
    addEmployeeOp operation has failed. This is because
    the FirstName only has 2 characters.
4.  Now, change the lastName and the email address in the request as
    shown below and execute the operation again.
    -   EmployeeNumber: 6001
    -   FirstName: ABC
    -   lastName: Nick
    -   Email: test@test.com
    -   Salary: 1500

    The employee details are added to the database table.  
    ![](attachments/119133322/119133323.png){width="1000"}
