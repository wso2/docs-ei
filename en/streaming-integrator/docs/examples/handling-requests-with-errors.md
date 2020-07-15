# Managing Streaming Data with Errors

In this tutorial, let's learn how you can handle streaming data that has errors (e.g., events that do not have values for certain attributes). WSO2 Streaming Integrator allows you to log such events, direct them to a separate stream or store them in a data store. If these errors occur at the time of publishing (e.g., due to a connection error), WSO2 SI also provides the option to wait and then resume to publish once the connection is stable again. For detailed information about different ways to handle errors, see the [Handling Errors guide](../guides/handling-errors.md).

In this scenario, you are handling erroneous events by directing them to a MySQL store.

!!! Tip "Before you begin:"
    In order to save streaming data with erors in a MySQL store, complete the following prerequisites.<br/>    
    - Start the SI server by navigating to the `<SI_HOME>/bin` directory and issuing one of the following commands as appropriate, based on your operating system:<br/>
        <br/>
          - For Windows: `streaming-integrator.bat`<br/>
        <br/>
          - For Linux:  `sh server.sh`<br/>
        <br/>
      The following log appears in the Streaming Integrator console once you have successfully started the server. <br/>
      <br/>
      `INFO {org.wso2.carbon.kernel.internal.CarbonStartupHandler} - WSO2 Streaming Integrator started in 4.240 sec`
      <br/>
    - You need to have access to a MySQL instance.
      
## Step 1: Create the data store

Let's create the MySQL data store in which the events with errors can be saved. To do this, follow the steps below:

1. Download the MySQL JDBC driver from [the MySQL site](https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.45.tar.gz).

2. Unzip the archive.<br/>

3. Copy the `mysql-connector-java-5.1.45-bin.jar` to the `<SI_HOME>/lib` directory.<br/>

4. Start the MySQL server as follows:

    `mysql -u <USERNAME> -p <PASSWORD>`
    
5. To create a database named `errorstoredb` in which the events with errors can be saved, issue the following command.

    `mysql> create database errorstoredb;`
    
6. To switch to the new database, issue the following command.

    `mysql> use errorstoredb;`

## Step 2: Create the Siddhi applications
