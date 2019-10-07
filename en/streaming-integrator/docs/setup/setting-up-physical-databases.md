# Setting Up Physical Databases

## Defining data tables

This section explains how to configure data tables to store the events you need to persist to carry out time series aggregation.

The data handled by WSO2 Stream Processor are stored in the following two types of tables:

- **In-memory tables**: If no  store-backed tables are defined, data is stored in in-memory tables by default.

- **Store-backed tables**: These are tables that are defined by you in an external database. For a list of database types supported and instructions to define table for different database types, see [Defining Tables for Physical Stores](#defining-tables-for-physical-stores).

**Adding primary and index keys**

Both in-memory tables and tables backed by external databases support primary and index keys. These are defined to allow stored information to be searched and retrieved in an effective and efficient manner.

- **Adding primary keys**

    - **Description**

        Attribute(s) within the event stream for which the event table is created can be specified as the primary key for the table. The purpose of primary key is to ensure that the value for a selected attribute is unique for each entry in the table. This prevents the duplication of entries saved in the table.

        Primary keys are configured via the `@PrimaryKey` annotation. Only one `@PrimaryKey` annotation is allowed per event table.

        When several attributes are given within Primary key annotation (e.g `@PrimaryKey( 'key1', 'key2'))`, those attributes would act as a composite primary key.

    - **Syntax**

        ```
        @PrimaryKey('<attribute_1>')
        define table <event_table> (<attribute_1> <attribute_type>, <attribute_2> <attribute_type>, <attribute_3> <attribute_type>);
        ```

    - **Example**

        ```
        @PrimaryKey('symbol')
        define table StockTable (symbol string, price float, volume long);
        ```

        The above configuration ensures that each entry saved in the `StockTable` event table should have a unique value for the `symbol` attribute because this attribute is defined as the primary key.



- **Adding index keys**

    - **Description**

        An attribute within the event stream for which the event table is created can be specified as the primary key for the table. This allows the entries stored within the table to be indexed by that attribute.

        Indexes are configured via the `@Index` annotation.

        An event table can have multiple attributes defined as index attributes. However, only one `@Index` annotation can be added per event table.

    - **Syntax**

        To index by a single attribute:

        ```
        @Index('<attribute_1>')
        define table <event_table> (<attribute_1> <attribute_type>, <attribute_2> <attribute_type>, <attribute_3> <attribute_type>);
        ```

        To index by multiple attributes:

        ```
        @Index('<attribute_1>''<attribute_2>')
        define table <event_table> (<attribute_1> <attribute_type>, <attribute_2> <attribute_type>, <attribute_3> <attribute_type>);
        ```

    - **Example**

        ```
        @Index('symbol')
        define table StockTable (symbol string, price float, volume long);
        ```

        The above configuration ensures that the entries stored in the `StockTable` event table are indexed by the `symbol` attribute.

### Defining Tables for Physical Stores

This section explains how to define data tables to store data handled by the Streaming Integrator in physical databases. The `@store` annotation syntax for defining these tables differ based on the database type as well as where the properties are defined.

The store properties(such as URL, username and password) can be defined in the following ways:

- **Inline definition**: The data store can be defined within the Siddhi application as shown in the example below:

    ``` sql
        @Store(type='hbase', hbase.zookeeper.quorum='localhost')
        @primaryKey('name')
        define table SweetProductionTable (name string, amount double);
    ```

    !!! info
        This method is not recommended in a production environment because it is less secure compared to the other methods.


- **As references in the deployment file**: In order to do this, the store configuration needs to be defined for the relevant deployment environment in the `<SI_HOME>|<SI_TOOLING_HOME>/conf/server/deployment.yaml` file as a `ref` (i.e., in a separate section `siddhi:` and subsection `refs:`) as shown in the example below.

    !!! info
        The database connection is started when a Siddhi application is deployed, and disconnected when the Siddhi application is undeployed. Therefore, this method is not recommended if the same database is used across multiple Siddhi applications.


    ``` xml
        siddhi:
          refs:
            -
               ref:
                 name: 'store1'
                 type: 'rdbms'
                 properties:
                   jdbc.url: 'jdbc:h2:./repository/database/ANALYTICS_EVENT_STORE'
                   username: 'root'
                   password: ${sec:store1.password}
                   field.length='currentTime:100'
                   jdbc.driver.name: 'org.h2.Driver'
    ```


    Then you need to refer to that store via the `@store` annotation as in the Siddhi application as shown in the example below.

    ``` sql
            @Store(ref='store1')
            @PrimaryKey('id')
            @Index('houseId')
            define table SmartHomeTable (id string, value float, property bool, plugId int, householdId int, houseId int, currentTime string);
    ```

- **Using WSO2 data sources configuration**: Once a data source is defined in the `wso2.datasources` section of the `<SI_HOME>|<SI_TOOLING_HOME>/conf/server/deployment.yaml` file, the same connection can be used across different Siddhi applications. This is done by specifying the data source to which you need to connect via the `@store` annotation in the following format.

    ``` xml
            @Store(type='<DATABASE_TYPE>', datasource=’<carbon.datasource.name>’)
    ```

    !!! info
        The database connection pool is initialized at server startup, and destroyed at server shut down.

    This is further illustrated by the following example.

    ``` sql
        @Store(type='rdbms', datasource=’SweetFactoryDB’)@PrimaryKey("symbol")
        define table FooTable (symbol string, price float, volume long);
    ```

    For more information about defining datasources, see [Configuring Datasources](configuring-data-sources).

The following database types are currently supported for the Streaming Integrator.

!!!info "Before you begin:"
    In order to create and use an event table to store data, the following should be completed:<br/>
    - The required database (MySql, MongoDB, Oracle Database, etc) should be downloaded and installed.
    - A database instance should be started.
    - The user IDs used to perform the required table operations must be granted the relevant privileges.
    - The relevant JDBC Driver must be downloaded and the jar must be placed in the `<SI_HOME>|<SI_TOOLING_HOME>/lib` directory.

- [RDBMS](#rdbms)
- [Apache HBase](#apache-hbase)
- [Apache Solr](#apache-solr)
- [MongoDB](#mongodb)



#### RDBMS

The RDBMS database types that are currently supported are as follows:

- H2
- MySQL
- Oracle database
- My SQL Server
- PostgreSQL
- IBM DB2

**Query syntax**

The following is the syntax for an RDBMS event table configuration:

``` sql
    @store(type="rdbms", jdbc.url="<jdbc.url>", username="<username>", password="<password>",pool.properties="<prop1>:<val1>,<prop2>:<val2>")
    @PrimaryKey("col1")
    @IndexBy("col3")
    define table <table_name>(col1 datatype1, col2 datatype2, col3 datatype3);
```

**Parameters**

The following parameters are configured in the definition of an RDBMS event table.

| Parameter             | Description                                                                                                                                                                                                            | Required/Optional |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|
| **`jdbc.url`**        | The JDBC URL via which the RDBMS data store is accessed.                                                                                                                                                               | Required          |
| **`username`**        | The username to be used to access the RDBMS data store.                                                                                                                                                                | Required          |
| **`password`**        | The password to be used to access the RDBMS data store.                                                                                                                                                                | Required          |
| **`pool.properties`** | Any pool parameters for the database connection must be specified as key value pairs.                                                                                                                                  | Required          |
| **`jndi.resource`**   | The name of the JNDI resource through which the connection is attempted. If this is found, the pool properties described above are not taken into account.                                                             | Optional          |
| **`table.name`**      | The name of the RDBMS table created.                                                                                                                                                                                   | Optional          |
| **`field.length`**    | The number of characters that the values for fields of the `STRING` type in the table definition must contain. If this is not specified, the default number of characters specific to the database type is considered. | Optional          |

In addition to the above parameters, you can add the `@primary` and `@index` annotations in the RDBMS table configuration.

- `@primary`: This specifies a list of comma-separated values to be treated as unique fields in the table. Each record in the table must have a unique combination of values for the fields specified here.

- `@index`: This specifies the fields that must be indexed at the database level. You can specify multiple values as a comma-separated list.

**Example**

The following is an example of an RDBMS table definition.

``` sql
    @Store(type="rdbms", jdbc.url="jdbc:h2:repository/database/ANALYTICS_EVENT_STORE", username="root", password="root",field.length="symbol:254")
    @PrimaryKey("symbol")
    define table FooTable (symbol string, price float, volume long);
```

#### Apache HBase

**Query syntax**

The query syntax to define an HBase table is as follows.

``` sql
    @Store(type="hbase", any.hbase.property="<STRING>", table.name="<STRING>", column.family.name="<STRING>")
    @PrimaryKey("PRIMARY_KEY")
    @Index("INDEX")
```

**Parameters**

The following parameters are configured in the definition of an HBase event table.

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Required/Optional</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>                             table.name                           </code></p></td>
<td><p>The name with which the table should be persisted in the store. If no table name is specified, the table in the store is assigned the same name as the corresponding Siddhi table.</p></td>
<td>Optional</td>
</tr>
<tr class="even">
<td><code>                           column.family.name                         </code></td>
<td>The name of the HBase column family from which data must be stored/referred to.</td>
<td>Required</td>
</tr>
<tr class="odd">
<td>any.hbase.property</td>
<td><p>Any property that can be specified for HBase connectivity in hbase-site.xml is also accepted by the HBase Store implementation. The most frequently used properties are...</p>
<p>hbase.zookeeper.quorum - The hostname of the server in which the zookeeper node is run.</p>
<p>hbase.zookeeper.property.clientPort - The port of the zookeeper node.</p></td>
<td>Required</td>
</tr>
</tbody>
</table>

In addition, the following annotations are used in the HBase definition.

- `@primary`: This specifies a list of comma-separated values to be treated as unique fields in the table. Each record in the table must have a unique combination of values for the fields specified here. Primary keys allow you to fetch records in a table by providing a unique reference for each record. Therefore, if you do not include one or more primary keys in the table definition, it is not possible to perform table operations such as searching, updating, deleting and joining. For more information about table operations, see [Managing Stored Data via Streams](#managing-stored-data-via-streams) and [Managing Stored Data via REST APIs](#managing-stored-data-via-rest-apis).

- `@index`: This specifies the fields that must be indexed at the database level. You can specify multiple values as a comma-separated list.

**Example**

``` sql
    @Store(type=”hbase”, table.name=”SomeTestTable”, column.family.name=”SomeCF”, hbase.zookeeper.quorum=”localhost”, hbase.zookeeper.property.clientPort=”2181”)
    @PrimaryKey(symbol)
    define table FooTable (symbol string, price float, volume long);
```

#### Apache Solr

**Query syntax**

The query syntax to define an SOLR table is as follows.

``` sql
    @PrimaryKey("id")
    @store(type=“solr”, url=<solr-cloud-zookeeper-url>, collection=<solr-collection-name>, base.config=<config-name>, shards=<no-of-shards>, replicas=<no-of-replicas>, schema=<schema-definition>, commit.async=true|false)
    define table Footable (time long, date string);
```

**Parameters**

The following parameters are configured in an SOLR table definition.

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Required/Optional</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             collection            </code></td>
<td>The name of the solr collection/table.</td>
<td>Required</td>
</tr>
<tr class="even">
<td><code>             url            </code></td>
<td>The URL of the zookeeper master of SOLR cloud.</td>
<td>Required</td>
</tr>
<tr class="odd">
<td><p><code>              base.config             </code></p></td>
<td>The default configuration that should be used for the SOLR schema.</td>
<td>Optional</td>
</tr>
<tr class="even">
<td><code>             shards            </code></td>
<td>The number of shards.</td>
<td>Optional</td>
</tr>
<tr class="odd">
<td><code>             replica            </code></td>
<td>The number of replica.</td>
<td>Optional</td>
</tr>
<tr class="even">
<td><code>             schema            </code></td>
<td>The SOLR schema definition.</td>
<td>Optional</td>
</tr>
<tr class="odd">
<td><code>             commit.async            </code></td>
<td><p>If this is set to <code>              true             </code> , the results all the operations carried out for the table (described below) are applied at a specified time interval. If this is set to <code>              false             </code> , the results of the operations are applied soon after they are performed with the vent arrival.</p>
<p>e.g., If this is set to <code>              false             </code> , an event selected to be inserted into the table is inserted as soon as it arrives to the event stream.</p></td>
<td>N/A</td>
</tr>
</tbody>
</table>



**Example**

This query defines an SOLR table named `FooTable` in which a schema that consists of the two attributes `time` (of `long` type) and date (of the `string` type) is maintained. The values for both attributes are stored.

``` sql
"shards='2', replicas='2', schema='time long stored, date string stored', commit.async='true') " +
"define table Footable(time long, date string);
```

#### MongoDB

**Query syntax**

The following is the query syntax to define a MongoDB event table.

``` sql
@Store(type="mongodb", mongodb.uri="<MONGODB CONNECTION URI>")
@PrimaryKey("ATTRIBUTE_NAME")
@IndexBy("<ATTRIBUTE_NAME> <SORTING ORDER> <INDEX OPTIONS>")
define table <TABLE_NME> (<ATTRIBUTE1_NAME> <ATTRIBUTE1_TYPE>, <ATTRIBUTE2_NAME> <ATTRIBUTE2_TYPE>, <ATTRIBUTE3_NAME> <ATTRIBUTE3_TYPE>, ...);
```

**Parameters**

The `mongodb.uri` parameter specifies the URI via which MongoDB user store is accessed.

In addition, the following annotations are used in the MongoDB definition.

- `@primary`: This specifies a list of comma-separated values to be treated as unique fields in the table. Each record in the table must have a unique combination of values for the fields specified here.

- `@index`: This specifies
    the fields that must be indexed at the database level. You can
    specify multiple values as a comma separated list.

**Example**

The following query defines a MongoDB table named `FooTable` with the `symbol`, `price`, and `volume` attributes. The `symbol` attribute is considered the primary key and it is also indexed.

``` sql
@Store(type="mongodb", mongodb.uri="mongodb://admin:admin@localhost:27017/Foo?ssl=true")
@PrimaryKey("symbol")
@IndexBy("symbol 1 {background:true}")
define table FooTable (symbol string, price float, volume long);
```

## Managing Stored Data via Streams

## Managing Stored Data via REST APIs

## Accessing and Manipulating Data in Multiple Tables

The Streaming Integrator allows you to perform CRUD operations (i.e., inserting updating and deleting data) and retrieval queries for multiple normalized tables within a single data store. This is supported via the [siddhi-store-rdbms extension](https://wso2-extensions.github.io/siddhi-store-rdbms/).

**Performing CRUD operations for multiple tables**

In order to perform CRUD operations, the system parameter named `perform.CRUD.operations` needs to be set to `true` in the `deployment.yaml` file.

The syntax for a Siddhi query to perform a CRUD operation in multiple tables is as follows.

```sql
    from TriggerStream#rdbms:cud(<STRING> datasource.name, <STRING> query)
    select numRecords
    insert into OutputStream;
```

e.g., If you need to change the details of a customer in customer details table connected to a data source named, `SAMPLE_DB` a Siddhi query can be written as follows.

```
    from Trigger Stream#rdbms:cud('SAMPLE_DB', 'UPDATE Customers ON CUSTOMERS SET ContactName='Alfred Schmidt', City='Frankfurt' WHERE CustomerID=1;')
    select numRecords
    insert into OutputStream
```

**Retrieving data from multiple tables**

In order to retrieve information from multiple tables in a data store, the syntax is as follows:

```
    from TriggerStream#rdbms:query(<STRING> dataource.name, <STRING> query, <STRING> stream definition)
    select <attributes>
    insert into OutputStream
```

e.g., If you need to find matching records in both customer and orders table based on `orderId` and `customerId` in the `SAMPLE_DB` database, a Siddhi query can be written as follows.

```
    from TriggerStream#rdbms:query('SAMPLE_DB', 'SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate FROM Orders INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID', 'orderId string, customerName string, orderDate string')
    select orderId, customerName, orderData
    insert into OutputStream;
```