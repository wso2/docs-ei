#Enriching Data

##Introduction

Enriching data involves integrated the data received into a streaming integration flow with data from other medium such 
as a data store, another data stream, or an external service to derive an expected result. To understand the different ways in which this is done,
follow the sections below.

## Enrich data by connecting with a data store 

This section explains how to enrich the data in a specific stream by joining it with a data store. For this purpose, consider 
a scenario where you receive sales records generated from multiple locations as events via a system. 

!!!tip "Before you begin:"
    In this scenario, you need to enrich the sales information you receive based on the records in a database table. You 
    can download and install MySQL, and create this table before you carry out the procedure in this subsection.<br/>
    For detailed instructions to create a database table, expand the following section. <br/>
    ???Create Table
        1. Download and install the [MySQL Server](https://dev.mysql.com/downloads/).
        2. Download the [MySQL JDBC driver](https://dev.mysql.com/downloads/connector/j/).
        3. Unzip the downloaded MySQL driver zipped archive, and copy the MySQL JDBC driver JAR (`mysql-connector-java-x.x.xx-bin.jar`) into the `<SI_HOME>/lib` directory.
        4. Enter the following command in a terminal/command window, where username is the username you want to use to access the databases.<br/>
           `mysql -u username -p `
        5. When prompted, specify the password you are using to access the databases with the username you specified.
        6. Add the following configurations for three database tables under the `Data Sources Configuration` section of the `<SI_HOME>/conf/server/deployment.yaml` file.<br/>
          ```         
            - name: UserDataDB
              description: Datasource used for User Data
              jndiConfig:
                name: jdbc/test
                useJndiReference: true
              definition:
                type: RDBMS
                configuration:
                  jdbcUrl: 'jdbc:mysql://localhost:3306/UserDataDB?useSSL=false'
                  username: root
                  password: root
                  driverClassName: com.mysql.jdbc.Driver
                  maxPoolSize: 50
                  idleTimeout: 60000
                  connectionTestQuery: SELECT 1
                  validationTimeout: 30000
                  isAutoCommit: false
          ```
        7. To create a database named `UserDataDB` with a table named `UserTable` issue the following commands from the terminal:
           - To create the `UserDataDB` table:
             ```
             mysql> create database UserDataDB;
             mysql> use UserDataDB;
             mysql> source <SP_HOME>/wso2/editor/dbscripts/metrics/mysql.sql;
             mysql> grant all on UserDataDB.* TO username@localhost identified by "password";
             ```
           - To create the `UserTable` table:
             ```
            create table UserTable (
            userId LONG,
            firstname VARCHAR,
            lastname VARCHAR,
            )
             ```
             
1. Start creating a new Siddhi application. You can name it `EnrichingTransactionsApp` For instructions, see [Creating a Siddhi Application](../develop/creating-a-Siddhi-Application.md).
 
2. Define the input stream and the database table that need to be joined as follows.
    1. Define the stream as follows. 
        `define stream TrasanctionStream (userId long, transactionAmount double, location string);`
    2. Define the table as follows.
        `define table UserTable (userId long, firstName string, lastName string);`
3. Then define the Siddhi query to join the stream and the table, and handle the result as required.
    1. Add the `from` clause as follows with the `join` key word to join the table and the stream.
       ```
       from TransactionStream as t join UserTable as u on t.userId == u.userId       
       ```
       !!!info
           Note the following about the `from` clause:
           - In this example, the input data is taken from both a stream and a table. You need to assign a unique reference
            for each of them to allow the query to differentiate between the common attributes. In this example, `TransactionStream` 
            stream is referred to as `t`, and the `UserTable` table is referred to as `u`.
           - The `join ` keyword joins the stream and the table together while specifying the unique references.
           - The condition for the stream and the table to be joined is `t.userId == u.userId `, which means that for an 
           event to be taken from the `TransactionStream` for the join, one or more events that have the same value for 
           the `userId` must exist in the `UserTable` table and vice versa.
                  
    2. To specify how the value for each attribute in the output stream is derived, add a `select` clause as follows.
       ```
        select t.userId, str:concat( u.firstName, " ", u.lastName) as userName, transactionAmount, location
       ```
       !!!info
           Note the following in the `select` statement:
           - The `userId` attribute name is common to both the stream and the table. Therefore, you need to specify from where this attribute needs gto be taken. Here, you can also specify `u.userId` instead of `t.userId`.
           - You are specifying the output generated to include an attribute named `userName`. The value for that is derived
            by concatenating the values of two attributes in the `UserTable` table (i.e., `firstName` and `lastName` attributes)
             by applying the `str:concat()` function. Similarly, you can apply any of the range of Siddhi functions available to further enrich the joined output. For more information, see [Siddhi Extensions](https://siddhi.io/en/v4.x/docs/extensions/#extensions-released-under-apache-20-license).
    3. To infer an output stream into which the enriched data must be directed, add the `insert into` clause as follows.
       `insert into EnrichedTrasanctionStream;`
       
The completed Siddhi application is as follows.
```sql
@App:name("EnrichingTransactionsApp")


define stream TrasanctionStream (userId long, transactionAmount double, location string);

define table UserTable (userId long, firstName string, lastName string);

from TrasanctionStream as t join UserTable as u on t.userId == u.userId 
select t.userId, str:concat( u.firstName, " ", u.lastName) as userName, transactionAmount, location
insert into EnrichedTrasanctionStream;
```

## Enrich data by connecting with another stream of data 

This section explains how to enrich the data in a specific stream by joining it with another stream.

To understand how this is done, consider a scenario where you receive information about cash withdrawals and cash 
deposits at different bank branches from two separate applications. Therefore, this two types of information are captured via two 
separate streams. To compare the withdrawals with deposits and observe whether enough deposits are being made to manage the withdrawals, you need to join both these streams. To do this, follow the 
procedure below.

1. Start creating a new Siddhi application. You can name it `BankTransactionsApp` For instructions, see [Creating a Siddhi Application](../develop/creating-a-Siddhi-Application.md).
2. First, define the two input streams via which you are receiving informations about withdrawals and deposits.
        
    1. Define a stream named `CashWithdrawalStream` to capture information about withdrawals as follows.
        `define stream CashWithdrawalStream(branchID int, amount long);`
    2. Define a stream named `CashDepositsStream` to capture information about deposits as follows.
        `define stream CashDepositsStream(branchID string, amount long);`
3. Now let's define an output stream to which the combined information from both the input streams need to be directed 
   after the join. 
    ```
    @sink(type='log', prefix='Cash withdrawals that go beyond sustainability threshold:')
    define stream CashFlowStream(branchID string, withdrawalAmount long, depositAmount long);
    ```
    !!!info
        A sink annotation is connected to the output stream to log the output events. For more information about adding sinks to publish events, see the [Publishing Data guide](publishing-data.md).
4. To specify how the join is performed, and how to use the combined information, write a Siddhi query as follows.
    1. To perform the join, add the `from` clause as follows.
        ```sql
        from CashWithdrawalStream as w
            join CashDepositStream as d
            on w.branchID == d.branchID
        ```
        !!!info
            Observe the following about the above `from` clause:
            - Both the input streams have attributes of the same name. To identify each name, you must specify a reference
             for each stream. In this example, the reference for the `CashWithdrawalStream` is `w`, and the reference for the `CashDepositsStream` stream is `d`.
            - You need to use `join` as the keyword to join two streams. The join condition is ` w.branchID == d.branchID` 
              where branch IDs are matched. An event in the `CashWithdrawalStream` stream is directed to the `CashFlowStream` if there are events with the same branch ID in the `CashDepositStream` and vice versa.
    2. To specify how the value for each attribute is derived, add a `select` statement as follows.
        `select w.branchID as branchID, w.amount as withdrawals, d.amount as deposits`
        !!!info
            The `branchID` attribute name is common to both input streams. Therefore, you can also specify `d.branchID as branchID` instead of `w.branchId as branchId`.
    3. To filter only events where total cash withdrawals are greater than 95% of the cash deposits, add a `having` clause as follows.
        `having w.amount > d.amount * 0.95 `
    4. To insert the results into the `CashFlowStream` output stream, add the `insert into` clause as follows.
        `insert into CashFlowStream;`
        
The completed Siddhi application is as follows:

```sql
@App:name("BankTransactionsApp");

define stream CashWithdrawalStream(branchID int, amount long);

define stream CashDepositsStream(branchID string, amount long);

@sink(type='log', prefix='Cash withdrawals that go beyond sustainability threshold:')
define stream CashFlowStream(branchID string, withdrawalAmount long, depositAmount long);

from CashWithdrawalStream as w
    join CashDepositStream as d
    on w.branchID == d.branchID
select w.branchID as branchID, w.amount as withdrawals, d.amount as deposits
having w.amount > d.amount * 0.95
```
For the different types of joins you can perform via Siddhi logic, see [Siddhi Query Guide - Join](https://siddhi.io/en/v4.x/docs/query-guide/#join-stream)

## Enrich data by connecting with external services 

This section explains how to enrich the data in a specific stream by joining it with an external service.

## Enrich data using built-in extensions

The following is a list of Siddhi extensions with which you can enrich data.

 - [Siddhi-execution-streamingml](https://siddhi-io.github.io/siddhi-execution-streamingml/)
 - [Siddhi-execution-env](https://wso2-extensions.github.io/siddhi-execution-env/)
 - [Siddhi-execution-math](https://wso2-extensions.github.io/siddhi-execution-math/)
 - [Siddhi-execution-string](https://siddhi-io.github.io/siddhi-execution-string/)
 - [Siddhi-execution-time](https://siddhi-io.github.io/siddhi-execution-time/)
 - [Siddhi-execution-json](https://siddhi-io.github.io/siddhi-execution-json/)
