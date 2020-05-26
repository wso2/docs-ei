# Setting up Microsoft SQL

Given below are the steps you need to follow in order to use an MSSQL database for cluster coordination in a Micro Integrator cluster.

## Enable TCP/IP

1. In the start menu, click **Programs** and launch **Microsoft SQL Server 2012.**
2. Click **Configuration Tools** , and then click **SQL Server Configuration Manager** .
3. Enable **TCP/IP** and disable **Named Pipes** from protocols of your Microsoft SQL server.
4. Double click **TCP/IP** to open the TCP/IP properties window and set **Listen All** to `Yes` on the **Protocol** tab.
5. On the **IP Address** tab, disable **TCP Dynamic Ports** by leaving it blank and give a valid TCP port, so that Microsoft SQL server will listen on that port. The best practice is to use port 1433, because you can use it in order processing services.
6. Similarly, enable TCP/IP from **SQL Native Client Configuration** and disable **Named Pipes**. Also, check whether the port is set correctly to 1433.
7. Restart Microsoft SQL server.

## Create the database and user

1. Open the Microsoft SQL Management Studio to create a database and user.
2. Click **New Database** from the **Database** menu and specify all the options to create a new database.
3. Click **New Login** from the **Logins** menu, and specify all the necessary options.

## Grant permissions

Assign newly created users the required grants/permissions to log in and
create tables, to insert, index, select, update and delete data in
tables in the newly created database. These are the minimum set of SQL
server permissions.

## Setting up the JDBC driver
[Download](https://msdn.microsoft.com/en-us/data/aa937724.aspx) and copy the `sqljdbc4` Microsoft SQL JDBC driver file to the `MI_HOME/lib/` directory. Use `com.microsoft.sqlserver.jdbc.SQLServerDriver` as the <driverClassName> in your datasource configuration as explained below.

## Connecting to the database

Add the following parameters to the `deployment.toml` file in the `MI_HOME/conf` directory.

```bash
[[datasource]]
id = "WSO2_COORDINATION_DB"
url= "jdbc:sqlserver://<IP>:1433;databaseName=clusterdb;SendStringParametersAsUnicode=false"
username="root"
password="root"
driver="com.microsoft.sqlserver.jdbc.SQLServerDriver"
pool_options.maxActive=50
pool_options.maxWait = 60000
pool_options.testOnBorrow = true
```

Find more parameters for [connecting to the database](../../../../references/config-catalog/#database-connection).
