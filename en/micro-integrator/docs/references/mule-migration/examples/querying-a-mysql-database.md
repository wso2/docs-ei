---
title: Querying a MySQL Database
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example illustrates how to use the a [Data Service](https://docs.wso2.com/display/EI630/Working+with+Data+Services) 
to connect to a MySQL database. After reading this document, creating and running the example in Micro Integrator, you 
should be able to create an application that connects to a MySQL database.
<p align="center">
  <img width="70%" src="../../../assets/img/migration-examples/querying-a-mysql-database-use-case.png">
</p>

### Assumptions

This document assumes that you are familiar with WSO2 EI and the 
[Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/), 
WSO2 EI’s graphical developer tool. To increase your familiarity with Integration Studio, consider completing one or more 
[WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case

This application listens for HTTP GET requests with the form: http://<host>:8290/employee?lastname=name. 
The datasource is configured to use the <parameter> to use it for the SQL query listed below.

	select first_name from employees where last_name = :lastName 

So if the application receives `http://localhost:8290/employee?lastname=Smith`, the SQL query will select first_name from 
employees where last_name = Smith.

The datasource service instructs the database server to run the SQL query, retrieves the result of the query, and 
converts the result to JSON. 

### Set Up and Run the Example

1. Install the MySQL server.

2. Download the JDBC driver for MySQL from [here](https://dev.mysql.com/downloads/connector/j/) and copy it to your `<MI_HOME>/lib` directory.

3. Start the MySQL server.

4. Create the database
    ```sql
    create database company;
    ```

5. Create the table employee
    ```sql
    create table employees (id int NOT NULL AUTO_INCREMENT, first_name varchar(100), last_name varchar(100), age int, PRIMARY KEY (id));
    ```

6. Enter the following data into the table:
    ```sql
    insert into employees(first_name,last_name,age) values("Chava", "Puckett", 28);
    insert into employees(first_name,last_name,age) values("Quentin", "Puckett", 26);
    insert into employees(first_name,last_name,age) values("Bob", "Smith", 35);
    ```

7. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

8. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

9. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

10. Browse and select the file path to the downloaded sample of this Github project (``integration-studio-examples/migration/mule/querying-a-mysql-database``) and click **Finish**.

11. Open the **RDBMSDataService.dbs** under **querying-a-mysql-database/QueryingAMysqlDatabaseDataService/dataservice/RDBMSDataService.dbs** directory. 

12. Configure connection attributes : url, username, password.

13. Lets [export composite application](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/exporting-artifacts/) in Micro Integrator. Right click on the **QueryingAMysqlDatabaseProjectCompositeApplication** under the main **querying-a-mysql-database** project and select **Export Composite Application Project**. Browse and provide carbonapps location of Micro Integrator(`[MI_HOME]/repository/deployment/server/carbonapps` where `MI_HOME` is the home directory of the distribution you downloaded). 

14. Open a terminal and navigate to the `MI_HOME/bin/` directory and execute the relevant command:
    * On MacOS/Linux/CentOS:<br/>
    ``sh micro-integrator.sh``
    * On Windows:<br/>
    ``micro-integrator.bat``

15. Open HTTP Client in Integration Studio. Follow [HTTP Client Guidelines](../../../docs/common/adding-http-client-to-integration-studio.md) to open HTTP Client if the window is not visible in the interface.

16. Make a GET request using HTTP Client.<br/>
        Request URI: http://localhost:8290/employee?lastname=Puckett<br/>
        Request method: GET<br/>
        Headers : Accept=application/json<br/>      

17. You should get the following JSON response
  ```json
  [
      {
          "first_name": "Chava"
      },
      {
          "first_name": "Quentin"
      }
  ]
  ```

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/querying-a-mysql-database.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Learn more on [WSO2 Datasources](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/creating-artifacts/data-services/creating-datasources/)
* Learn more on [Creating A Data Service](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/creating-artifacts/data-services/creating-data-services/)
