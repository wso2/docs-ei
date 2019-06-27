# Setting up Oracle RAC

The following sections describe how to set up Oracle RAC to replace the
default H2 database in your WSO2 product:

Oracle Real Application Clusters (RAC) is an option that facilitates
clustering and high availability in Oracle database environments. In the
Oracle RAC environment, some of the commands used in
`         oracle.sql        ` are considered inefficient. Therefore, the
product has a separate SQL script ( `         oracle_rac.sql        ` )
for Oracle RAC. The Oracle RAC-friendly script is located in the
`         dbscripts        ` folder together with other
`         .sql        ` scripts.

> To test products on Oracle RAC, rename `         oracle_rac.sql        `
to `         oracle.sql        ` before running
`         -Dsetup        ` .


## Setting up the database and users

Follow the steps below to set up an Oracle RAC database.

1.  Set environment variables \< `          ORACLE_HOME>         ` ,
    `          PATH         ` , `         ` and
    `          ORACLE_SID         ` with the corresponding values (
    `          /oracle/app/oracle/product/11.2.0/dbhome_1         ` ,
    `          $PATH:<ORACLE_HOME>/bin         ` , and
    `          orcl1         ` ) as follows:  
    ![](attachments/53125514/53287565.png){width="600"}
2.  Connect to Oracle using SQL\*Plus as SYSDBA.  
    ![](attachments/53125514/53287577.png){width="700"}
3.  Create a database user and grant privileges to the user as shown
    below:

    ``` powershell
    Create user <USER_NAME> identified by password account unlock;
    grant connect to <USER_NAME>;
    grant create session, create table, create sequence, create trigger to <USER_NAME>;
    alter user <USER_NAME> quota <SPACE_QUOTA_SIZE_IN_MEGABYTES> on '<TABLE_SPACE_NAME>';
    commit;
    ```

4.  Exit from the SQL\*Plus session by executing the
    `          quit         ` command.

## Setting up the JDBC driver

Copy the Oracle JDBC libraries (for example, the
`         <ORACLE_HOME>/jdbc/lib/ojdbc14.jar        ` file) to the
`         <PRODUCT_HOME>/repository/components/lib/        ` directory.

> Remove the old database driver from the
`<PRODUCT_HOME>/repository/components/dropins`
directory when you upgrade the database driver.


## Connecting the database to the server

To enable the two nodes to access the shared database, update the following parameters in the esb.toml file.

``` Java
// The config section that groups the parameters for the primary database that will be shared by both product nodes in the cluster.
[database.shared.db]

// Specify the type of database.
type = "oracle_rac"

// Specify the connection URL of your database. The following default URL connects to the H2 database that is shipped with the product.
url="jdbc:oracle:thin:@(DESCRIPTION=(LOAD_BALANCE=on)(ADDRESS=(PROTOCOL=TCP)(HOST=racnode1) (PORT=1521))(ADDRESS=(PROTOCOL=TCP)(HOST=racnode2) (PORT=1521))CONNECT_DATA=(SERVICE_NAME=rac)))"

// The username for connecting to the database.
username = "regadmin"

// The password for connecting to the database.
password = "regadmin"

```

Find more parameters for [connecting to the primary database](../../../references/ei_config_catalog/#connecting-to-the-primary-data-store) and for 
[tuning the primary database](../../../references/ei_config_catalog/#tuning-the-primary-data-store-connection).