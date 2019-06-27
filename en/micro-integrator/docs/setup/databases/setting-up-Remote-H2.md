# Setting up Remote H2

The following sections describe how to set up a remote H2 database to
replace the default H2 database in your WSO2 product:

> **When to use the embedded H2 database?**

> The embedded H2 database is NOT recommended in enterprise testing and
production environments. It has lower performance, clustering
limitations, and can cause file corruption failures. Please use an
industry-standard RDBMS such as Oracle, PostgreSQL, MySQL, or MS SQL
instead.

However, you can use the embedded H2 database as the local registry in a
registry mount even in enterprise testing and production environments .


## Setting up the remote H2 database

Follow the steps below to set up a Remote H2 database.

1.  Download and install the H2 database engine on your computer as
    follows:

     > For instructions on installing, see the [H2 installation guide](http://www.h2database.com/html/quickstart.html) .
    

    ![](attachments/53125507/53287411.png)

2.  Go to the \< `          H2_HOME>/bin         ` directory and run the
    H2 network server starting script as follows, where \<
    `          H2_HOME>         ` is the H2 installation directory:  

    ![](attachments/53125507/53287410.png)

3.  Run the H2 database server with the following commands:

    - For Linux:  

       `$ ./h2.sh`

    - For Windows:  

       `$ h2.bat`

     > The script starts the database engine and opens a pop-up window.
    

4.  Click **Start Browser** to open a web browser containing a client application, which you use to connect to a database. If a database
    does not already exist by the name you provided in the **JDBC URL** text box, H2 will automatically create a database.

## Setting up the drivers

WSO2 currently ships H2 database engine version h2-1.2.140.\* and its related H2 database driver. If you want to use a different H2 database driver, take the following steps:

1.  Delete the following H2 database-related JAR file, which is shipped
    with WSO2 products:
    `<PRODUCT_HOME>/repository/components/plugins/h2-database-engine_1.2.140.wso2v3.jar`
2.  Find the JAR file of the new H2 database driver (`<H2_HOME>/bin/h2-*.jar` , where `<H2_HOME>` is the H2 installation directory)
    and copy it to your WSO2 product's `<PRODUCT_HOME>/repository/components/lib`
    directory.

## Connecting the database to the server

To enable the two nodes to access the shared database, update the following parameters in the esb.toml file.

``` Java
// The config section that groups the parameters for the primary database that will be shared by both product nodes in the cluster.
[database.shared.db]

// Specify the type of database.
type = "remote_H2"

// Specify the connection URL of your database. The following default URL connects to the H2 database that is shipped with the product.
url="jdbc:h2:./repository/database/WSO2SHARED_DB;DB_CLOSE_ON_EXIT=FALSE;LOCK_TIMEOUT=60000"

// The username for connecting to the database. By default, 'root' is the MySQL username.
username = "root"

// The password for connecting to the database. By default, 'root' is the MySQL password.
password = "root"

```

Find more parameters for [connecting to the primary database](../../../references/ei_config_catalog/#connecting-to-the-primary-data-store) and for 
[tuning the primary database](../../../references/ei_config_catalog/#tuning-the-primary-data-store-connection).