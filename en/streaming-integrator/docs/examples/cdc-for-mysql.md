# Change Data Capturing for MySQL

## Introduction

Streaming Integrator allows you to capture changes to a data base table, in a streaming way.

This tutorial takes you through the different modes and  options you could use to perform Change Data Capturing (CDC) using the Streaming Integrator. In this tutorial, we will be using a MySQL datasource.  

#### Polling mode and Listening mode

There are two modes you could perform CDC using the Streaming Integrator: **Polling mode** and **Listening mode**. 

In polling mode, the datasource is periodically polled for capturing the changes. The polling period can be configured. 
 
On listening mode, the Streaming Integrator will keep listening to the Change Log of the database and notify in case a change has taken place.

#### Type of events captured

You could capture following type of changes done to a database table:
- Insert operations 
- Update operations
- Delete operations

## Tutorial Outline
- Polling mode
    - Preparing server and database for the tutorial
    - Capturing inserts
    - Capturing updates
    - Capturing deletes
- Listening mode
    - Preparing server and database for the tutorial
    - Capturing inserts
    - Capturing updates
    - Capturing deletes    
- Enabling persistence

## Polling mode

### Preparing server and database for the tutorial

1. It is assumed that you have access to a MySQL server instance. Enable binary logging in the MySQL server. You may follow https://debezium.io/docs/connectors/mysql/#enabling-the-binlog. 

2. Add the MySQL JDBC driver into {WSO2SPHome}/lib as follows:
    - Download the MySQL JDBC driver from: https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.45.tar.gz
    - Unzip the archive.
    - Copy mysql-connector-java-5.1.45-bin.jar to {WSO2SPHome}/lib directory.




### Capturing inserts