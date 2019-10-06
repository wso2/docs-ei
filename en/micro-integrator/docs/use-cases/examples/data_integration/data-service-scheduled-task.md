# Scheduling Tasks

Task scheduling is used to invoke a data service operation periodically
or for a specified number of times. You cannot insert data into the
database using scheduling tasks. But you can use schedule tasks to
update operations, such as the SQL update and delete operations.

The scheduling functionality is useful when a specific data service
operation is associated with an [input or output
event-trigger](Receiving-Notifications-from-Data-Services_119133285.html#ReceivingNotificationsfromDataServices-Inputeventtrigger)
. When a scheduled task that is associated with an event-trigger is run,
the event is automatically fired by evaluating the event trigger
criteria. For example, we can schedule a task on the
`         getProductQuantity        ` operation and set an event to send
an email if the quantity goes down to some level.

In this tutorial, you create an operation to add data to an employee
database by keeping the Email column as `         NULL        ` . Next,
you create a scheduled task to update the emails that are
`         NULL        ` to `         no-reply@wso2.com        ` .

## Prerequisites

Let's create a MySQL database with the required data.

1.  Install the MySQL server.
2.  Create a database named `Employees`.

    ```bash
    CREATE DATABASE Employees;
    ```

3.  Create the Employee table inside the Employees database:

    ```bash
    USE Employees;

    CREATE TABLE Employees (EmployeeNumber int(11) NOT NULL, FirstName varchar(255) NOT NULL, LastName varchar(255) DEFAULT NULL, Email varchar(255) DEFAULT NULL, Salary varchar(255));
    ```

4.  Add the following records to the table:

    ```bash
    insert into Employees (EmployeeNumber, FirstName, LastName, Salary) values (100, 'Chris', 'Adam', '10000');
    insert into Employees (EmployeeNumber, FirstName, LastName, Email, Salary) values (101, 'Alex', 'pat', 'alex@wso2.com', '10000');
    insert into Employees (EmployeeNumber, FirstName, LastName, Salary) values (102, 'John', 'Doe', '10000');
    ```

5.  View the data added to the database by executing the command given
    below. Note that the emailn address is marked as
    `            NULL           ` for Chris and John.

    ```bash
    Select * from Employees;
    ```

    Example output:

    !!! Info
        More records are displayed if you have tried the previous tutorials.

    ```bash
    +----------------+-----------+----------+---------------+--------+
    | EmployeeNumber | FirstName | LastName | Email         | Salary |
    +----------------+-----------+----------+---------------+--------+
    |            100 | Chris     | Adam     | NULL          | 10000  |
    |            101 | Alex      | pat      | alex@wso2.com | 10000  |
    |            102 | John      | Doe      | NULL          | 10000  |
    +----------------+-----------+----------+---------------+--------+
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

Confirming the scheduled task functionality:

In the [before you begin](#SchedulingTasks-before_you_begin) section you
noticed that Chris and John had the email address as
`         NULL        ` . Now, let's check the email addresses of Chris
and John by running the command given below:

```bash
select * from Employees;
```

Example output: You see that their email addresses are updated to
`         no-reply@wso2.com        ` as you specified in the
[operation](#SchedulingTasks-Operation) above. The scheduled task takes
this operation and runs it to update the data.

```bash
+----------------+-----------+----------+-------------------+--------+
| EmployeeNumber | FirstName | LastName | Email             | Salary |
+----------------+-----------+----------+-------------------+--------+
|            100 | Chris     | Adam     | no-reply@wso2.com | 10000  |
|            101 | Alex      | pat      | alex@wso2.com     | 10000  |
|            102 | John      | Doe      | no-reply@wso2.com | 10000  |
+----------------+-----------+----------+-------------------+--------+
```
