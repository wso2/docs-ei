# Extracting Data from Static Sources

WSO2 Streaming Integrator can extract data from static sources such as databases, files and cloud storages in real-tme. 

## Consuming data from databases

A database table is a stored collection of records of a specific schema. Each record can be equivalent to an event. WSO2 Streaming Integrator can integrate databases into the streaming flow by extracting records in database tables as streaming events.

To understand how data is extracted from a database into a streaming flow, consider a scenario where an online booking site automatically save all online bookings of vacation packages in a database. The company wants to monitor the bookings in real-time. Therefore, this data stored in the database needs to be extracted in real time. 

You can use the [siddhi-io-cdc](https://siddhi-io.github.io/siddhi-io-cdc/api/latest/) extension for this purpose. This extension allows you to extract data from the database in two modes:

- Polling Mode
    Here, the data source is periodically polled in order to capture changes. The polling period can be specified. This mode allows only `insert` and `update` operations to be captured.

- Listening Mode

## Consuming data from files

## Consuming data from cloud storages