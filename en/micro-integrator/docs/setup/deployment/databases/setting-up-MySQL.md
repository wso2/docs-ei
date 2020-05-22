
# Configuration a MySQL database

Given below are the steps you need to follow in order to use a MySQL database for cluster coordination in a Micro Integrator cluster.

## Setting up MySQL

To set up MySQL:

1. Download and install [MySQL Server](http://dev.mysql.com/downloads/).
2. Download the [MySQL JDBC driver](http://dev.mysql.com/downloads/connector/j/).
3. Unzip the downloaded MySQL driver and copy the MySQL JDBC driver JAR (mysql-connector-java-x.x.xx-bin.jar) to the `<MI_HOME>/lib/` directory of the Micro Integrator nodes in your cluster.
4. Execute the following command in a terminal/command where the username is the username you want to use to access the databases:

	 `mysql -u username -p`

5. When prompted, specify the password to access the database with the username you specified.

## Creating the database

Execute the following command to create the DB by pointing to the MySQL script (`mysql_cluster.sql`) in the `<MI_HOME>/dbscripts/` directory:

```bash
mysql> create database mi_db;
mysql> use mi_db;
mysql> source <MI_HOME>/dbscripts/mysql_cluster.sql;
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
