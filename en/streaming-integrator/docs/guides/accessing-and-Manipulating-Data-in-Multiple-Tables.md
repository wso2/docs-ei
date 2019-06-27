# Accessing and Manipulating Data in Multiple Tables

WSO2 SP allows you to perform CUD operations (i.e., inserting updating
and deleting data) and retrieval queries for multiple normalized tables
within a single data store. This is supported via the
[siddhi-store-rdbms
extension](https://wso2-extensions.github.io/siddhi-store-rdbms/) .

## Performing CUD operations for multiple tables

In order to perform CUD operations, the system parameter named
`         perform.CUD.operations        ` needs to be set to
`         true in deployment.yaml file        ` .

The syntax for a Siddhi query to perform a CUD operation in multiple
tables is as follows:

``` sql
    from TriggerStream#rdbms:cud(<STRING> datasource.name, <STRING> query)
    select numRecords 
    insert into OutputStream;
```

e.g., If you need to change the details of a customer in customer
details table connected to a datasource named,
`         SAMPLE_DB        ` a Siddhi query can be written as follows:

``` sql
    from Trigger Stream#rdbms:cud('SAMPLE_DB', 'UPDATE Customers ON CUSTOMERS SET ContactName='Alfred Schmidt', City='Frankfurt' WHERE CustomerID=1;')
    select numRecords 
    insert into OutputStream
```

## Retrieving data from multiple tables

In order to retrieve information from multiple tables in a data store,
the syntax is as follows:

``` sql
    from TriggerStream#rdbms:query(<STRING> dataource.name, <STRING> query, <STRING> stream definition)
    select <attributes>
    insert into OutputStream
```

e.g., If you need to find matching records in both customer and orders
table based on orderId and customerId in the SAMPLE\_DB database, a
Siddhi query can be written as follows:

``` sql
    from TriggerStream#rdbms:query('SAMPLE_DB', 'SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate FROM Orders INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID', 'orderId string, customerName string, orderDate string')
    select orderId, customerName, orderDate
    insert into OutputStream;
```

  

  
