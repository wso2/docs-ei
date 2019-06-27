# Setting up PostgreSQL

The following sections describe how to set up PostgreSQL to replace the
default H2 database in your WSO2 product:

## Setting up the database and login role

Follow the steps below to set up a PostgreSQL database.

1.  Install PostgreSQL on your computer as follows:  
    ![](attachments/53125515/53287605.png)
2.  Start the PostgreSQL service using the following command:  
    ![](attachments/53125515/53287604.png)
3.  Create a database and the login role from a GUI using the
    [PGAdminIII tool](http://www.pgadmin.org/download/) .
4.  To connect PGAdminIII to a PostgreSQL database server, locate the
    server from the object browser, right-click the client and click
    **Connect** . This will show you the databases, tablespaces, and
    login roles as follows:  
    ![](attachments/53125515/53287590.png){width="800"}
5.  To create a database, click **Databases** in the tree (inside the
    object browser), and click **New Database** .
6.  In the **New Database** dialog box, give a name to the database,
    e.g., gregdb and click **OK** .
7.  To create a login role, click **Login Roles** in the tree (inside
    the object browser), and click **New Login Role** . Enter the role
    name and a password.

    > These values will be used in the product configurations as described
        in the following sections. In the sample configuration,
        `           gregadmin          ` will be used as both the role name
        and the password.
    

8.  Optionally, enter other policies, such as the expiration time for
    the login and the connection limit.
9.  Click **OK** to finish creating the login role.

## Setting up the drivers

1.  Download the [PostgreSQL JDBC4
    driver](http://jdbc.postgresql.org/download.html) .
2.  Copy the driver to your WSO2 product's \<
    `           PRODUCT_HOME>/repository/components/lib          `
    directory.    

## Connecting the database to the server

To enable the two nodes to access the shared database, update the following parameters in the esb.toml file.

``` Java
// The config section that groups the parameters for the primary database that will be shared by both product nodes in the cluster.
[database.shared.db]

// Specify the type of database.
type = "postgre_sql"

// Specify the connection URL of your database. The following default URL connects to the H2 database that is shipped with the product.
url="jdbc:postgresql://localhost:5432/gregdb"

// The username for connecting to the database.
username = "regadmin"

// The password for connecting to the database.
password = "regadmin"

```

Find more parameters for [connecting to the primary database](../../../references/ei_config_catalog/#connecting-to-the-primary-data-store) and for 
[tuning the primary database](../../../references/ei_config_catalog/#tuning-the-primary-data-store-connection).