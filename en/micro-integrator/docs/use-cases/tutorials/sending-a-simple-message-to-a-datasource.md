# Data Integration

## What you'll build

A **data service** provides a Web service interface to access data that is stored in relational databases, CSV files, Microsoft Excel sheets,
Google spreadsheets, and more. The following sections describe how you can use WSO2 Integration Studio to work with data services' artifacts. 

## Let's get started!

### Step 1: Set up the workspace

To set up the tools:

-   Download the relevant [WSO2 Integration Studio](https://wso2.com/integration/tooling/) based on your operating system. The path to the extracted/installed folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.
-   Optionally, you can set up the **CLI tool** for artifact monitoring. This will later help you get details of the artifacts that you deploy in your Micro Integrator.
    1.  Go to the [WSO2 Micro Integrator website](https://wso2.com/integration/#). 
    2.  Click **Download -> Other Resources** and click **CLI Tooling** to download the tool. 
    3.  Extract the downloaded ZIP file. This will be your `MI_CLI_HOME` directory. 
    4.  Export the `MI_CLI_HOME/bin` directory path as an environment variable. This allows you to run the tool from any location on your computer using the `mi` command. Read more about the [CLI tool](../../../administer-and-observe/using-the-command-line-interface).

To demonstrate how data services work, we will use a MySQL database as the datasource. Follow the steps given below to set up a MySQL database:

1.  Install the MySQL server.
2.  Download the JDBC driver for MySQL from [here](http://dev.mysql.com/downloads/connector/j/) and copy it to the `lib` directory of the embedded Micro Integrator of WSO2 Integration Studio.
    
    !!! Note
        The `lib` directory of the embedded Micro Integrator of WSO2 Integration Studio is located in `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/` (for Linux/MacOS/CentOS) or `MI_TOOLING_HOME/runtime/microesb/lib/` (for Windows). 

    If the driver class does not exist in the relevant directory when you create the datasource, you will get an exception such as `Cannot load JDBC driver class com.mysql.jdbc.Driver`.
    
3.  Create a database named `Employees`.

    ```bash
    CREATE DATABASE Employees;
    ```

4.  Create the Employee table inside the Employees database:

    ```bash
    USE Employees;
    CREATE TABLE Employees (EmployeeNumber int(11) NOT NULL, FirstName varchar(255) NOT NULL, LastName varchar(255) DEFAULT NULL, Email varchar(255) DEFAULT NULL, Salary varchar(255));
    ```

### Step 2: Creating a data service

Follow the steps given below to create a new data service.

#### Creating a data service project

All the data services' artifacts that you create should be stored in a
Data Service project. Follow the steps given below to create a project:

1.  Open **WSO2 Integration Studio,** and click **DS Project → Create
    New** in the **Getting Started** tab as shown below.  
    ![](../../assets/img/tutorials/data_services/119130577/119135178.png)

2.  In the **New Data Service Project** dialog that opens, give a name
    for the project and click **Next**.
3.  If required, change the Maven information about the project.
4.  Click **Finish**. The new project will be listed in the project
    explorer.

#### Creating the datasource connection

Follow the steps given below to create the data service file:

1.  Select the already created **Data Service Project** in the project
    navigator, right click and go to **New -> Data Service**.  
    The **New Data Service** window will open as shown below.  
    ![](../../assets/img/tutorials/data_services/119130577/119130578.png)
2.  To start creating a data service from scratch, select **Create New
    Data Service** and click **Next**.

3.  Enter a name for the data service:

    <table>
        <tr>
            <th>Property</th>
            <th>Description</th>
        </tr>
    <tbody>
    <tr class="odd">
    <td>Data Service Name</td>
    <td>RDBMSDataService</td>
    </tr>
    </tbody>
    </table>

4.  Click **Next** and start adding the datasource connection details
    given below.

    |       Property                     |       Description                     |
    |------------------------------------|---------------------------------------|
    | Datasource ID                      | Datasource                            |
    | Datasource Type                    | RDBMS                                 |
    | Datasource Type (Default/External) | Leave **Default** selected.           |
    | Database Engine                    | MySQL                                 |
    | Driver Class                       | com.mysql.jdbc.Driver                 |
    | URL                                | jdbc:mysql://localhost:3306/Employees |
    | User Name                          | root                                  |

5.  Save the data service.

A data service file (DBS file) will now be created in your data service
project. Shown below is the project directory.

![](../../assets/img/tutorials/data_services/119130577/119130593.png)

#### Creating a query

Let's write an SQL query to GET data from the MySQL datasource that you
configured in the previous step:

1.  Select the data service you created in the previous step.
2.  Right-click and click **Add Query** .  
    ![](../../assets/img/tutorials/data_services/119130577/119130591.png)
3.  Enter the following query details:

    | Parameter  |  Description       |
    |------------|--------------------|
    | Query ID   | GetEmployeeDetails |
    | Datasource | Datasource         |

4.  Save the query. The query element is now added to the data
    service:  
    ![](../../assets/img/tutorials/data_services/119130577/119130590.png)
5.  Right-click the **GetEmployeeDetails** query and click **Add SQL**
    to add the following SQL statement:

    ```bash
    select EmployeeNumber, FirstName, LastName, Email from Employees where EmployeeNumber=:EmployeeNumber
    ```

6.  Save the SQL statement.
7.  Right-click the query again and click **Add Input Mapping** .

8.  Enter the following input mapping details:

    | Property       | Description    |
    |----------------|----------------|
    | Mapping Name   | EmployeeNumber |
    | Parameter Type | SCALAR         |
    | SQL Type       | STRING         |

9.  Save the input mapping.
10. Right-click the query again and click **Add Output Mapping**.
11. Enter the following value to group the output mapping:

    <table>
        <tr>
            <th>Property</th>
            <th>Description</th>
        </tr>
    <tr class="odd">
    <td>Grouped by Element</td>
    <td>Employees</td>
    </tr>
    </table>

12. Save the output mapping.

13. Right-click the output mapping and go to **Add Output Mapping → Add
    Element** to create an element.

14. Enter the following element details.

    <table>
    <tr>
            <th>Property</th>
            <th>Description</th>
        </tr>
    <tbody>
    <tr class="odd">
    <td>Datasource Type</td>
    <td>column</td>
    </tr>
    <tr class="even">
    <td>Output Field Name</td>
    <td>EmployeeNumber</td>
    </tr>
    <tr class="odd">
    <td>Datasource Column Name</td>
    <td>EmployeeNumber</td>
    </tr>
    <tr class="even">
    <td>Schema Type</td>
    <td>String</td>
    </tr>
    </tbody>
    </table>

15. Save the element.
16. Follow the same steps to create the following output elements:

    | Datasource Type | Output Field Name | Datasource Column Name | Schema Type |
    |-----------------|-------------------|------------------------|-------------|
    | column          | FirstName         | FirstName              | string      |
    | column          | LastName          | LastName               | string      |
    | column          | Email             | Email                  | string      |

17. Save the output elements.

The data service should now have the query element added as shown below.

![](../../assets/img/tutorials/data_services/119130577/119130589.png)

#### Creating a resource to invoke the query

Now, let's create a REST resource that can be used to invoke the query.

1.  Right-click the data service and click **Add Resource**. Add the following resource details.

    <table>
    <tr>
            <th>Property</th>
            <th>Description</th>
        </tr>
    <tbody>
    <tr class="odd">
    <td>Resource Method</td>
    <td>GET</td>
    </tr>
    <tr class="even">
    <td>Resource Path</td>
    <td>Employee/{EmployeeNumber}</td>
    </tr>
    </tbody>
    </table>

2.  Expand the GET resource, and click the **GetEmployeeDetails (call-query)**. Connect the query to the resource by adding the following:

    <table>
    <tr>
            <th>Property</th>
            <th>Description</th>
        </tr>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td>GetEmployeeDetails</td>
    </tr>
    </tbody>
    </table>

3.  Save the resource.

The data service should now have the resource added as shown below.

![](../../assets/img/tutorials/data_services/119130577/119130588.png)

### Step 3: Package the artifacts

Create a new composite application project:

1.  Open the **Getting Started** view and click **Miscellaneous → Create New Composite Application**.  
    ![Create new CAPP](../../assets/img/create_project/create_new_capp.png) 
2.  In the **New Composite Application Project** dialog that opens, select the data service file, and click **Finish**.  
    ![Create new CAPP](../../assets/img/create_project/create_new_capp_dialog.png)

Package the artifacts in your composite application project to be able to deploy the artifacts in the server.

1.  Open the `pom.xml` file in the composite application project POM editor.
2.  Ensure that your data service file is selected in the POM file.
3.  Save the project.

### Step 4: Build and run the artifacts

To test the artifacts, deploy the [packaged artifacts](#step-3-package-the-artifacts) in the embedded Micro Integrator:

1.  Right-click the composite application project and click **Export Project Artifacts and Run**.
2.  In the dialog that opens, select the composite application project that you want to deploy.  
4.  Click **Finish**. The artifacts will be deployed in the embedded Micro Integrator and the server will start. See the startup log in the **Console** tab. 

### Step 5: Testing the data service

Let's test the use case by sending a simple client request that invokes the service.

#### Get details of deployed artifacts (Optional)

Let's use the **CLI Tool** to find the URL of the data service (that is deployed in the Micro Integrator) to which you send a request. 

!!! Tip
    Be sure to set up the CLI tool for your work environment as explained in the [first step](#step-1-set-up-the-workspace) of this tutorial.

1.  Open a terminal and execute the following command to start the tool:
    ```bash
    mi
    ```
    
2.  Log in to the CLI tool. Let's use the server administrator user name and password:
    ```bash
    mi remote login admin admin
    ```

    You will receive the following message: *Login successful for remote: default!*

3.  Execute the following command to find the data services deployed in the server:
    ```bash
    mi dataservice show
    ```

Read more about [using the CLI tool](../../../administer-and-observe/using-the-command-line-interface).

#### Send the client request

Open a command line terminal and enter the following request:

```bash
curl -X GET http://localhost:8290/services/RDBMSDataService.HTTPEndpoint/Employee/3
```

You will receive the employee's details in the response.