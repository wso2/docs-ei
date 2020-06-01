# Setting up an IBM DB2

Follow the steps given below to set up the required IBM databases for your Micro Integrator.

## Prerequisites

Download the latest version of [DB2 Express-C](http://www-01.ibm.com/software/data/db2/express/download.html)
and install it on your computer.

For instructions on installing DB2 Express-C, see this [ebook](https://www.ibm.com/developerworks/community/wikis/home?lang=en#!/wiki/Big%20Data%20University/page/FREE%20eBook%20-%20Getting%20Started%20with%20DB2%20Express-C).

## Setting up the database and users

The following IBM DB scripts are stored in the `<MI_HOME>/dbscripts/` directory of your Micro Integrator.

<table>
	<tr>
		<th>Script</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>db2_cluster.sql</td>
		<td>This script creates the database tables that are required for cluster coordination.</td>
	</tr>
	<tr>
		<td>db2_user.sql</td>
		<td>This script creates the database tables that are required for storing users and roles.</td>
	</tr>
</table>

First create the databases, and then create the DB tables by pointing to the relevant script in the `<MI_HOME>/dbscripts/` directory. It is recommended to maintain separate databases for these use cases.

Create the database using either [DB2 command processor](#using-the-db2-command-processor) or [DB2 control center](#using-the-db2-control-center) as described below.

### Using the DB2 command processor

1.  Run DB2 console and execute the `          db2start         `
    command on a CLI to open DB2.
2.  Create the database using the following command:  
    `create database clusterdb`
3.  Before issuing an SQL statement, establish the connection to the
    database using the following command:  
    `connect to clusterdb user <USER_ID> using <PASSWORD>`
4.  Grant required permissions for users as follows:

    ```bash
    connect to DB_NAME
    grant <AUTHORITY> on database to user <USER_ID>
    ```

    For more information on DB2 commands, see the [DB2 Express-C Guide](https://www.ibm.com/developerworks/community/wikis/home?lang=en#!/wiki/Big%20Data%20University/page/FREE%20eBook%20-%20Getting%20Started%20with%20DB2%20Express-C).

### Using the DB2 control center

1.  Open the DB2 control center using the `db2cc` command:  

2.  Right-click **All Databases** in the control center tree (inside the
    object browser), click **Create Database**, and then click
    **Standard** and follow the steps in the **Create New Database**
    wizard.  
3.  Click **User and Group Objects** in the control center tree to
    create users for the newly created database.  
4.  Give the required permissions to the newly created users.  

## Setting up DB2 JDBC drivers

Copy the DB2 JDBC drivers (`db2jcc.jar` and `db2jcc_license_c0u.jar`) from the `<DB2_HOME>/SQLLIB/java/` directory to the `MI_HOME/lib/` directory.

`<DB2_HOME>` refers to the installation directory of DB2 Express-C, and `<MI_HOME>` refers to the directory where you run the WSO2 product instance.

## Connecting to the database

Open the `deployment.toml` file in the `<MI_HOME>/conf` directory and add the following sections to create the connection between the Micro Integrator and the relevant database. Note that you need two separate configurations corresponding to the two separate databases (`clusterdb` and `userdb`).

```toml tab='Cluster DB Connection'
[[datasource]]
id = "WSO2_COORDINATION_DB"
url="jdbc:db2://SERVER_NAME:PORT/clusterdb"
username="root"
password="root"
driver="com.ibm.db2.jcc.DB2Driver"
pool_options.maxActive=50
pool_options.maxWait = 60000
pool_options.testOnBorrow = true
```

```toml tab='User DB Connection'
[[datasource]]
id = "WSO2_USER_DB"
url="jdbc:db2://SERVER_NAME:PORT/userdb"
username="root"
password="root"
driver="com.ibm.db2.jcc.DB2Driver"
pool_options.maxActive=50
pool_options.maxWait = 60000
pool_options.testOnBorrow = true

[realm_manager]
data_source = "WSO2_USER_DB"
```

Find more parameters for [connecting to the database](../../../../references/config-catalog/#database-connection).
