# Setting up Oracle

The following sections describe how to set up an Oracle database to
replace the default H2 database in your WSO2 product:

## Setting up the database and users

Follow the steps below to set up an Oracle database.

1.  Create a new database by using the Oracle database configuration
    assistant (dbca) or manually.

2.  Make the necessary changes in the Oracle
    `           tnsnames.ora          ` and
    `           listner.ora          ` files in order to define
    addresses of the databases for establishing connections to the newly
    created database.

3.  After configuring the `           .ora          ` files, start the
    Oracle instance using the following command:

     `sudo /etc/init.d/oracle-xe restart`

4.  Connect to Oracle using SQL\*Plus as SYSDBA as follows:

    `./$<ORACLE_HOME>/config/scripts/sqlplus.sh sysadm/password as SYSDBA`

5.  Connect to the instance with the username and password using the
    following command:

    `connect`

6.  As SYSDBA, create a database user and grant privileges to the user
    as shown below:

    ``` powershell
    Create user <USER_NAME> identified by <PASSWORD> account unlock;
    grant connect to <USER_NAME>;
    grant create session, create table, create sequence, create trigger to <USER_NAME>;
    alter user <USER_NAME> quota <SPACE_QUOTA_SIZE_IN_MEGABYTES> on '<TABLE_SPACE_NAME>';
    commit;
    ```

7.  Exit from the SQL\*Plus session by executing the
    `          quit         ` command.

## Setting up the JDBC driver

1.  Copy the Oracle JDBC libraries (for example, \<
    `          ORACLE_HOME/jdbc/lib/ojdbc14.jar)         ` to the \<
    `          PRODUCT_HOME>/repository/components/lib/         `
    directory.
2.  Remove the old database driver from the
    `          <PRODUCT_HOME>/repository/components/dropins/         `
    directory.

> If you get a "
`                   timezone region not found"                 ` error
when using the `         ojdbc6.jar        ` file with WSO2 servers, set
the Java property as follows:
`         export JAVA_OPTS="-Duser.timezone='+05:30'"        `
he value of this property should be the GMT difference of the country.
If it is necessary to set this property permanently, define it inside
the `         wso2server.sh        ` as a new
`         JAVA_OPT        ` property.


## Connecting the database to the server

To enable the two nodes to access the shared database, update the following parameters in the esb.toml file.

``` Java
// The config section that groups the parameters for the primary database that will be shared by both product nodes in the cluster.
[database.shared.db]

// Specify the type of database.
type = "oracle"

// Specify the connection URL of your database. The following default URL connects to the H2 database that is shipped with the product.
url="jdbc:oracle:thin:@SERVER_NAME:PORT/SID"

// The username for connecting to the database.
username = "regadmin"

// The password for connecting to the database.
password = "regadmin"

```

Find more parameters for [connecting to the primary database](../../../references/ei_config_catalog/#connecting-to-the-primary-data-store) and for 
[tuning the primary database](../../../references/ei_config_catalog/#tuning-the-primary-data-store-connection).