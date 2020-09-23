# Step 1: Download Streaming Integrator and Set It Up

First, you are required to download the Streaming Integrator and the other software needed for the scenario you are trying out. To do this, follow the topics below.

!!! tip "Before you begin:"
    - Install [Oracle Java SE Development Kit (JDK) version 1.8](https://www.oracle.com/technetwork/java/javase/downloads/index.html).<br/>
    - [Set the Java home](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) environment variable.<br/>
    
## Downloading the Streaming Integrator runtime and tooling

- To download the Streaming Integrator runtime, visit the [Streaming Integrator Product Page](https://wso2.com/integration/streaming-integrator/). Enter you email address and agree to the license. Then click **Zip Archive** download the Streaming Integrator as a zip file.

- To download Streaming Integrator Tooling, click **Tooling** in the [Streaming Integrator Product Page](https://wso2.com/integration/streaming-integrator/). Enter you email address and agree to the license. Then click **MacOS Installer pkg** download the Streaming Integrator as a zip file.

## Setting up your production environment

This section shows how to prepare your production environment for the scenario described in the [Streaming Integration Overview section](download-install-and-start-si.md).

## Setting up a MySQL database table

In this scenario, the Streaming Integrator reads input data from a MySQL database table. Therefore, let's download and install MySQL and define the database and the database table as follows:

1. Download MySQL 5.1.49 from [MySQL Community Downloads](https://dev.mysql.com/downloads/connector/j/5.1.html).

2. Enable binary logging in the MySQL server. For detailed instructions, see [Enabling the Binlog tutorial by debezium](https://debezium.io/docs/connectors/mysql/#enabling-the-binlog).

3. Add the MySQL JDBC driver into the `<SI_HOME>/lib` directory as follows:

    1. Download the MySQL JDBC driver from [the MySQL site](https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.45.tar.gz).
    
    2. Unzip the archive.
    
    3. Copy the `mysql-connector-java-5.1.45-bin.jar` to the `<SI_HOME>/lib` directory.
    
    4. Start the SI server.
    
4. Once you install MySQL and start the MySQL server, create the database and the database table you require as follows:

    1. Let's create a new database in the MySQL server which you are to use throughout this tutorial. To do this, execute the following query.
        ```
        CREATE SCHEMA production;
        ```
       
    2. Create a new user by executing the following SQL query.<br/>
        ```
        GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'wso2si' IDENTIFIED BY 'wso2';
        ```<br/>
    3. Switch to the `production` database and create a new table, by executing the following queries:<br/>
        `use production;`<br/>
        `CREATE TABLE SweetProductionTable (name VARCHAR(20),amount double(10,2));`<br/> 

## What's Next?

Once you have successfully downloaded the WSO2 Streaming Integrator, you can proceed to [Step 2: Create a Siddhi Application](create-the-siddhi-application.md).