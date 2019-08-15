# Storage Integration

##Introduction

This section explains how to integrate data stores into Streaming Integration flows. The Streaming integrator can consume
 data from stores in a streaming manner as well as publish data into them. You can perform CRUD operations and Change Data Capture (CDC).
 
## Configuring stores

You can store data in-memory or in a physical data store. In a production environment, it is recommended to store data in a physical store because data stored in-memory can be lost if a system failure occurs.
 
![Defining a Data Store](../../images/storage-integration/defining-stores.png)

If you want to share a database across multiple Siddhi applications, you must define a data store in the `<SI_HOME>/conf/server/deployment.yaml` 
file. If you want a single connection pool, you can define a store as a ref in the `ref` section of the `<SI_HOME>/conf/server/deployment.yaml` file. If you want to define a unique data store for your Siddhi application, you can define it inline.

To understand all three methods consider an example where a factory manager wants to store the purchase records of raw 
material supplies to be stored in order to able to check the availability of materials at any given time.
### Configuring stores inline

To understand how to define a store inline, follow the procedure below:

1. Start creating a new Siddhi application. You can name it `ShipmentHistoryApp` For instructions, see [Creating a Siddhi Application](../develop/creating-a-Siddhi-Application.md).
   `@App:name('ShipmentHistory');`
   
2. First, define an input stream to capture the data that you need to store as follows.

    ```
    @source(type = 'http', @map(type = 'json'))
    define stream RawMaterialStream(transRef long, material string, supplier string, amount double);

    ```
    
    Here, each event representing a material shipment transaction is received via the `http` transport and in JSON format. This is specified via the source annotation. For more information about consuming data via sources, see the [Consuming Data guide](consuming-messages.md).
    
3. Now let's define the table in which you are storing the data as follows.
    `define table ShipmentDetails(puchaseID long, material string, supplier string, amount double);`
    
4. To save the data in the required database, connect the table you defined to the datasource that you previously created.
    ```
    @Store(type='rdbms', datasource='FactoryMaterialDB')@PrimaryKey("purchaseID")
    define table ShipmentDetails(puchaseID long, transRef string, material string, supplier string, amount double);
    ```
    
5. To store the information arriving at the `RawMaterialStream` stream to the `ShipmentDetails` table, write a Siddhi query as follows:
    1. To specify that events to be stored are taken from the `RawMaterialStream` input stream, add the `from` clause as follows.
        `from `
    2. To derive the exact attribute names with which the records are stored in the table, add the `select` statement as follows.
    3.If a record from the stream already exists in a the `ShipmentDetails` table, it should be updated with the latest information from the stream

### Referring to externally defined stores

To understand how to refer to am externally defined store, follow the procedure below:


###Storing data in existing data sources

To understand how to store data in existing data sources, follow the procedure below:

## Performing CRUD operations

###Performing CRUD operations via streams

###Performing CRUD operations via REST API


## Change Data Capture

## Manupulating data in multiple tables



