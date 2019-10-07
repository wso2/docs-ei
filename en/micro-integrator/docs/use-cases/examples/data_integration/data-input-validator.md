# Validating Input Values in a Data Request

Validators are added to individual input mappings in a query. Input
validation allows data services to validate the input parameters in a
request and stop the execution of the request if the input doesn’t meet
the required criteria. WSO2 Micro Integrator provides a set of built-in validators for some of the most
common use cases. It also provides an extension mechanism to write
custom validators.

## Prerequisites

Let's create a MySQL database with the required data.

1.  Install the MySQL server.
2.  Create a database named `           EmployeeDatabse          ` .

    ```bash
    CREATE DATABASE EmployeeDatabase;
    ```

3.  Create the Employee table inside the Employees database:

    ```bash
    USE EmployeeDatabase;

    CREATE TABLE Employees (EmployeeNumber int(11) NOT NULL, FirstName varchar(255) NOT NULL, LastName varchar(255) DEFAULT NULL, Email varchar(255) DEFAULT NULL, Salary varchar(255));
    ```

## Synapse configuration
Given below is the data service configuration you need to build. See the instructions on how to [build and run](#build-and-run) this example.

```xml
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio). The path to this folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.
2.  Download the JDBC driver for MySQL from [here](http://dev.mysql.com/downloads/connector/j/) and copy it to
    your `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/lib/` directory.

    !!! Note
        If the driver class does not exist in the relevant folders when you create the datasource, you will get an exception, such as `Cannot load JDBC driver class com.mysql.jdbc.Driver`. 
        
3. [Create a Data Service project](../../../../develop/creating-projects/#data-services-project)
4. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.
5. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator. 

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
