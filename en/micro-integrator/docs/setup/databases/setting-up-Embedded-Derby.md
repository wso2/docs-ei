# Setting up Embedded Derby

The following section describes how to set up an IBM DB2 database to
replace the default H2 database in your WSO2 product:

## Setting up the database

Follow the steps below to set up an embedded Derby database:

1.  Download [Apache
    Derby](http://apache.mesi.com.ar/db/derby/db-derby-10.8.2.2/) .
2.  Install Apache Derby on your computer.

    > For instructions on installing Apache Derby, see the [Apache Derby documentation](http://db.apache.org/derby/manuals/) .
    

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
