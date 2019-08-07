#Enriching Data

##Introduction

Enriching data involves integrated the data received into a streaming integration flow with data from other medium such 
as a data store, another data stream, or an external service to derive an expected result. To understand the different ways in which this is done,
follow the sections below.

## Enrich data by connecting with a data store 

This section explains how to enrich the data in a specific stream by joining it with a data store. For this purpose, consider 
a scenario where you receive sales records generated from multiple locations as events via a system. 

!!!Prerequisites
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
            - name: TransactionDataDB
             description: Datasource used for Sales Records
             jndiConfig:
               name: jdbc/test
               useJndiReference: true
             definition:
               type: RDBMS
               configuration:
                 jdbcUrl: 'jdbc:mysql://localhost:3306/TransactionDataDB?useSSL=false'
                 username: root
                 password: root
                 driverClassName: com.mysql.jdbc.Driver
                 maxPoolSize: 50
                 idleTimeout: 60000
                 connectionTestQuery: SELECT 1
                 validationTimeout: 30000
                 isAutoCommit: false
                 
           
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
                  
            - name: TriggerStateDataDB
              description: Datasource used to store the Last Processed ID
              jndiConfig:
                name: jdbc/test
                useJndiReference: true
              definition:
                type: RDBMS
                configuration:
                  jdbcUrl: 'jdbc:mysql://localhost:3306/TriggerStateDB?useSSL=false'
                  username: root
                  password: root
                  driverClassName: com.mysql.jdbc.Driver
                  maxPoolSize: 50
                  idleTimeout: 60000
                  connectionTestQuery: SELECT 1
                  validationTimeout: 30000
                  isAutoCommit: false
          ```
        7. To create three database tables named TransactionDataDB, UserDataDB, and TriggerStateDB, issue the following commands from the terminal.
           - To create the `TransactionDataDB` table:
             ```
             mysql> create database TransactionDataDB;
             mysql> use TransactionDataDB;
             mysql> source <SP_HOME>/wso2/editor/dbscripts/metrics/mysql.sql;
             mysql> grant all on TransactionDataDB.* TO username@localhost identified by "password";
             ```
           - To create the `UserDataDB` table:
             ```
             mysql> create database UserDataDB;
             mysql> use UserDataDB;
             mysql> source <SP_HOME>/wso2/editor/dbscripts/metrics/mysql.sql;
             mysql> grant all on UserDataDB.* TO username@localhost identified by "password";
             ```
           - To create the `TriggerStateDB` table.
             ```
             mysql> create database TriggerStateDB;
             mysql> use TrggerStateDB;
             mysql> source <SP_HOME>/wso2/editor/dbscripts/metrics/mysql.sql;
             mysql> grant all on TriggerStateDB.* TO username@localhost identified by "password";
             ```

## Enrich data by connecting with another stream of data 

This section explains how to enrich the data in a specific stream by joining it with another stream.

To understand how this is done, consider a scenario where you receive information about cash withdrawals and cash 
deposits at different bank branches from two separate applications. Therefore, this two types of information are captured via two 
separate streams. To compare the withdrawals with deposits and observe whether enough deposits are being made to manage the withdrawals, you need to join both these streams. To do this, follow the 
procedure below.

1. Start creating a new Siddhi application. You can name it `BankTransactionsApp` For instructions, see [Creating a Siddhi Application](../develop/creating-a-Siddhi-Application.md).
2. First, define the two input streams via which you are receiving informations about withdrawals and deposits.
    !!!info
        
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
