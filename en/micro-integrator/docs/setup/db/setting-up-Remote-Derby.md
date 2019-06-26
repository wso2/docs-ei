# Setting up Remote Derby

The following sections describe how to set up a remote Derby database to
replace the default H2 database in your WSO2 product:

## Setting up the database

Follow the steps below to set up a remote Derby database.

1.  Download [Apache
    Derby](http://apache.mesi.com.ar/db/derby/db-derby-10.8.2.2/) .
2.  Install Apache Derby on your computer.

    > For instructions on installing Apache Derby, see the [Apache Derby documentation](http://db.apache.org/derby/manuals/) .
    

3.  Go to the `          <DERBY_HOME>/bin         ` / directory and run
    the Derby network server start script. Usually, it is named
    `          startNetworkServer         ` .

## Setting up the drivers

Copy the `         derby.jar        ` ,
`         derbyclient.jar        ` JAR and the
`         derbynet.jar        ` JAR from the \<
`         DERBY_HOME>/lib/        ` directory to the \<
`         PRODUCT_HOME>/repository/components/extensions/        `
directory (the classpath of the Carbon web application).

## Connecting the database to the server

To enable the two nodes to access the shared database, update the following parameters in the esb.toml file.

``` Java
// The config section that groups the parameters for the primary database that will be shared by both product nodes in the cluster.
[database.shared.db]

// Specify the type of database.
type = "remote_derby"

// Specify the connection URL of your database. The following default URL connects to the H2 database that is shipped with the product.
url="jdbc:h2:./repository/database/WSO2SHARED_DB;DB_CLOSE_ON_EXIT=FALSE;LOCK_TIMEOUT=60000"

// The username for connecting to the database. By default, 'root' is the MySQL username.
username = "root"

// The password for connecting to the database. By default, 'root' is the MySQL password.
password = "root"

```

Find more parameters for [connecting to the primary database](../../../references/ei_config_catalog/#connecting-to-the-primary-data-store) and for 
[tuning the primary database](../../../references/ei_config_catalog/#tuning-the-primary-data-store-connection).
