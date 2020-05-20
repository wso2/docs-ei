# Setting up IBM DB2

Given below are the steps you need to follow in order to use an IBM DB2 database for cluster coordination in a Micro Integrator cluster.

## Prerequisites

Download the latest version of [DB2 Express-C](http://www-01.ibm.com/software/data/db2/express/download.html)
and install it on your computer.

For instructions on installing DB2 Express-C, see this [ebook](https://www.ibm.com/developerworks/community/wikis/home?lang=en#!/wiki/Big%20Data%20University/page/FREE%20eBook%20-%20Getting%20Started%20with%20DB2%20Express-C).

## Setting up the database and users

Create the database using either [DB2 command processor](#using-the-db2-command-processor) or [DB2 control center](#using-the-db2-control-center) as described below.

### Using the DB2 command processor

1.  Run DB2 console and execute the `          db2start         `
    command on a CLI to open DB2.
2.  Create the database using the following command:  
    `          create database WSO2_COORDINATION_DB         `
3.  Before issuing an SQL statement, establish the connection to the
    database using the following command:  
    `          connect to WSO2_COORDINATION_DB user <USER_ID> using <PASSWORD>         `
4.  Grant required permissions for users as follows:

    ```bash
    connect to DB_NAME
    grant <AUTHORITY> on database to user <USER_ID>
    ```

    For more information on DB2 commands, see the [DB2 Express-C Guide](https://www.ibm.com/developerworks/community/wikis/home?lang=en#!/wiki/Big%20Data%20University/page/FREE%20eBook%20-%20Getting%20Started%20with%20DB2%20Express-C).
    
### Using the DB2 control center

1.  Open the DB2 control center using the `db2cc` command:  

2.  Right-click **All Databases** in the control center tree (inside the
    object browser), click **Create Database** , and then click
    **Standard** and follow the steps in the **Create New Database**
    wizard.  
3.  Click **User and Group Objects** in the control center tree to
    create users for the newly created database.  
4.  Give the required permissions to the newly created users.  

## Setting up DB2 JDBC drivers

Copy the DB2 JDBC drivers (`db2jcc.jar` and `db2jcc_license_c0u.jar`) from `<DB2_HOME>/SQLLIB/java/` directory to the `MI_HOME/lib/` directory.

`<DB2_HOME>` refers to the installation directory of DB2 Express-C, and `MI_HOME` refers to the directory where you run the WSO2 product instance.

## Connecting to the database

Add the following parameters to the `deployment.toml` file in the `MI_HOME/conf` directory.

```toml
[[datasource]]
id = "WSO2_COORDINATION_DB"
url="jdbc:db2://SERVER_NAME:PORT/DB_NAME"
username="root"
password="root"
driver="com.ibm.db2.jcc.DB2Driver"
pool_options.maxActive=50
pool_options.maxWait = 60000
pool_options.testOnBorrow = true
```

Find more parameters for [connecting to the database](../../../../references/config-catalog/#database-connection).