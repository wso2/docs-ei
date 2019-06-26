
# Configuration a MySQL database

By default, WSO2 products use the embedded H2 database as the database for storing user management and registry data. Given below are the steps you need to follow in order to use a MySQL database for this purpose.

## Setting up MySQL

Following the steps below to create the necessary databases.

1. Download and install [MySQL Server](http://dev.mysql.com/downloads/).
2. Download the [MySQL JDBC driver](http://dev.mysql.com/downloads/connector/j/).
3. [Download](http://wso2.com/integration) and unzip the WSO2 EI binary distribution.
4. Unzip the downloaded MySQL driver, and copy the MySQL JDBC driver JAR (mysql-connector-java-x.x.xx-bin.jar) into the <EI_HOME>/lib/ directory of **both**
    ESB nodes.
5. Execute the following command in a terminal/command window, where the username is the username you want to use to access the databases:

	 `mysql -u username -p`

6. When prompted, specify the password to access the databases with the username you specified.

## Creating the database

Create the databases using the following commands:

> If you are using MySQL 5.7 or a later version, you need to use the mysql5.7.sql script instead of the mysql.sql script. This script has been tested on MySQL 5.7 and MySQL 8.

```
mysql> create database WSO2_USER_DB;
mysql> use WSO2_USER_DB;
mysql> source <EI_HOME>/dbscripts/mysql.sql;
mysql> grant all on WSO2_USER_DB.* TO regadmin@"carbondb.mysql-wso2.com" identified by "regadmin";

mysql> create database REGISTRY_DB;
mysql> use REGISTRY_DB;
mysql> source <EI_HOME>/dbscripts/mysql.sql;
mysql> grant all on REGISTRY_DB.* TO regadmin@"carbondb.mysql-wso2.com" identified by "regadmin";

mysql> create database REGISTRY_LOCAL1;
mysql> use REGISTRY_LOCAL1;
mysql> source <EI_HOME>/dbscripts/mysql.sql;
mysql> grant all on REGISTRY_LOCAL1.* TO regadmin@"carbondb.mysql-wso2.com" identified by "regadmin";

mysql> create database REGISTRY_LOCAL2;
mysql> use REGISTRY_LOCAL2;
mysql> source <EI_HOME>/dbscripts/mysql.sql;
mysql> grant all on REGISTRY_LOCAL2.* TO regadmin@"carbondb.mysql-wso2.com" identified by "regadmin";
```

> **About using MySQL in different operating systems**

> For users of Microsoft Windows, when creating the database in MySQL, it is important to specify the character set as latin1. Failure to do this may result in an error (error code: 1709) when starting your cluster. This error occurs in certain versions of MySQL (5.6.x) and is related to the UTF-8 encoding. MySQL originally used the latin1 character set by default, which stored characters in a 2-byte sequence. However, in recent versions, MySQL defaults to UTF-8 to be friendlier to international users. Hence, you must use latin1 as the character set as indicated below in the database creation commands to avoid this problem. Note that this may result in issues with non-latin characters (like Hebrew, Japanese, etc.). The following is how your database creation command should look: `mysql> create database <DATABASE_NAME> character set latin1;`

> For users of other operating systems, the standard database creation commands will suffice. For these operating systems, the following is how your database creation command should look: `mysql> create database <DATABASE_NAME>;`

## Connecting the database to the server

To enable the two nodes to access the shared database, update the following parameters in the esb.toml file.

``` Java
// The config section that groups the parameters for the primary database that will be shared by both product nodes in the cluster.
[database.shared.db]

// Specify the type of database.
type = "mysql"

// Specify the connection URL of your database. The following default URL connects to the H2 database that is shipped with the product.
url="jdbc:h2:./repository/database/WSO2SHARED_DB;DB_CLOSE_ON_EXIT=FALSE;LOCK_TIMEOUT=60000"

// The username for connecting to the database. By default, 'root' is the MySQL username.
username = "root"

// The password for connecting to the database. By default, 'root' is the MySQL password.
password = "root"

```

Find more parameters for [connecting to the primary database](../../../references/ei_config_catalog/#connecting-to-the-primary-data-store) and for 
[tuning the primary database](../../../references/ei_config_catalog/#tuning-the-primary-data-store-connection).