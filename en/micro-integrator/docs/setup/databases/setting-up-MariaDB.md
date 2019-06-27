# Setting up MariaDB

The following sections describe how to set up MariaDB to replace the
default H2 database in your WSO2 product

## Setting up the database and users

Follow the steps given below to set up MariaDB. See [Tested
DBMSs](https://docs.wso2.com/display/compatibility/Tested+DBMSs) for
information on the MariaDB versions that are tested with WSO2 products.

1.  Download, install and start MariaDB on your computer. See
    <https://downloads.mariadb.org/> .

    > You can install MariaDB standalone or as a [galera cluster](attachments/53125509/53287445.png) for high availability. Database clustering is independent of WSO2 product clustering. For instructions on installing MariaDB on MAC OS, go to
        [Homebrew](http://brew.sh/).

2.  Log in to MariaDB as the root user (or any other user with database
    creation privileges).

    ``` java
        mysql -u root -p
    ```

3.  Enter the password when prompted.

    > In most systems, there is no default root password. Press the
        **Enter** key without typing anything if you have not changed the
        default root password.
    

4.  In the MySQL command prompt, create the database using the following
    command:

    ``` java
        create database regdb;
    ```

5.  Give authorization to the regadmin user as follows:

    ``` java
            GRANT ALL ON regdb.* TO regadmin@localhost IDENTIFIED BY "regadmin";
    ```

6.  Once you have finalized the permissions, reload all the privileges
    by executing the following command:

    ``` java
            FLUSH PRIVILEGES;
    ```

7.  Log out from the MySQL prompt by executing the following command:

    ``` java
            quit;
    ```

## Setting up the drivers

Download the MySQL Java connector [JAR file](http://dev.mysql.com/downloads/connector/j/5.1.html), and copy it
to the \< `         PRODUCT_HOME>/repository/components/lib/        `
directory.

> **Note** that you must  use the MySQL connector that is compatible with
your MariaDB version. For example,
`         mysql-connector-java-5.1.36-bi        `
`         n.jar        ` is compatible with MariaDB version 10.0.20. See
[Tested DBMSs](https://docs.wso2.com/display/compatibility/Tested+DBMSs)
for information on the WSO2 products that have been tested for
compatibility with different versions of MariaDB and MySQL connectors.

## Connecting the database to the server

To enable the two nodes to access the shared database, update the following parameters in the esb.toml file.

``` Java
// The config section that groups the parameters for the primary database that will be shared by both product nodes in the cluster.
[database.shared.db]

// Specify the type of database.
type = "maria_db"

// Specify the connection URL of your database. The following default URL connects to the H2 database that is shipped with the product.
url="jdbc:h2:./repository/database/WSO2SHARED_DB;DB_CLOSE_ON_EXIT=FALSE;LOCK_TIMEOUT=60000"

// The username for connecting to the database.
username = "regadmin"

// The password for connecting to the database.
password = "regadmin"

```

To find additional parameters for configuring the database connection, see the [configuration catalog](../ref/config_catalog.md#connecting-to-the-user-store).


