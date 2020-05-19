
# Configuration a MySQL database

Given below are the steps you need to follow in order to use a MySQL database for cluster coordination in a Micro Integrator cluster.

## Setting up MySQL

Following the steps below to create the necessary databases.

1. Download and install [MySQL Server](http://dev.mysql.com/downloads/).
2. Download the [MySQL JDBC driver](http://dev.mysql.com/downloads/connector/j/).
3. [Download](http://wso2.com/integration) and unzip the database scripts for the Micro Integrator.
4. Unzip the downloaded MySQL driver and copy the MySQL JDBC driver JAR (mysql-connector-java-x.x.xx-bin.jar) into the `MI_HOME/lib/` directory of your Micro Integrator.
5. Execute the following command in a terminal/command where the username is the username you want to use to access the databases:

	 `mysql -u username -p`

6. When prompted, specify the password to access the databases with the username you specified.

## Creating the database

Create the user DB by pointing to the MySQL script that you downloaded. Execute the following command:

!!! Tip
	If you are using MySQL 5.7 or a later version, you need to use the mysql5.7.sql script instead of the mysql.sql script. This script has been tested on MySQL 5.7 and MySQL 8.

```bash
mysql> create database clusterdb;
mysql> use clusterdb;
mysql> source <MI_HOME>dbscripts/mysql_cluster.sql;
mysql> grant all on WSO2_USER_DB.* TO regadmin@"carbondb.mysql-wso2.com" identified by "regadmin";
```

!!! Info
	**About using MySQL in different operating systems**

	-	For users of Microsoft Windows, when creating the database in MySQL, it is important to specify the character set as latin1. Failure to do this may result in an error (error code: 1709) when starting your cluster. This error occurs in certain versions of MySQL (5.6.x) and is related to the UTF-8 encoding. MySQL originally used the latin1 character set by default, which stored characters in a 2-byte sequence. However, in recent versions, MySQL defaults to UTF-8 to be friendlier to international users. Hence, you must use latin1 as the character set as indicated below in the database creation commands to avoid this problem. Note that this may result in issues with non-latin characters (like Hebrew, Japanese, etc.). The following is how your database creation command should look: `mysql> create database <DATABASE_NAME> character set latin1;`
	-	For users of other operating systems, the standard database creation commands will suffice. For these operating systems, the following is how your database creation command should look: `mysql> create database <DATABASE_NAME>;`

## Connecting to the database

Add the following parameters to the `deployment.toml` file in the `MI_HOME/conf` directory.

```toml
[[datasource]]
id = "WSO2_COORDINATION_DB"
url= "jdbc:mysql://localhost:3306/clusterdb"
username="root"
password="root"
driver="com.mysql.jdbc.Driver"
pool_options.maxActive=50
pool_options.maxWait = 60000
pool_options.testOnBorrow = true
```

Find more parameters for [connecting to the database](../../../../references/config-catalog/#database-connection).