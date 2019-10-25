# Exposing a Cassandra Datasource

This example demonstrates how data stored in Cassandra can be exposed as a data service.

## Prerequisites

Set up the Cassandra server with a keyspace and table for this tutorial:

### Installing and starting Cassandra

A Cassandra server of version 3.0 should be already running in the
default port. For instructions, go to [Apache Cassandra
Documentation](http://cassandra.apache.org/download/) .

!!! Info
	-   Get familiar with the basics of Cassandra, including the
	    prerequisites for using Cassandra, by going through the following:
	    <https://wiki.apache.org/cassandra/GettingStarted> .
	-   Get familiar with using the Cassandra CLI, by going through the
	    following: <http://wiki.apache.org/cassandra/CassandraCli> .
	-   Functionality of the underlying connector used for the Cassandra
	    datasource:
	    [https://www.datastax.com/documentation/developer/java-driver/2.0/index.html](index)
	    .
	-   Also, note that Cassandra requires the most stable version of Java 7
	    or 8 you can deploy; preferably the Oracle/Sun JVM.

### Creating a keyspace and a table

Execute the SQL commands as given below.

-   Create the keyspace named **UsersKS** :

    ```bash
    CREATE KEYSPACE UsersKS WITH replication = {'class':'SimpleStrategy', 'replication_factor':3};
    ```

-   Create the table named **Users** in the **UsersKS** keyspace:

    ```bash
    CREATE TABLE UsersKS.Users (id uuid, name text, country text, age int, PRIMARY KEY (id));
    ```

## Synapse configuration
Given below is the data service configuration you need to build. See the instructions on how to [build and run](#build-and-run) this example.

```xml
<data name="CassandraDataService" transports="http https local">
   <config enableOData="false" id="Cassandra">
      <property name="cassandraServers">localhost</property>
   </config>
   <query id="addUsers" useConfig="Cassandra">
      <expression>INSERT INTO UsersKS.Users (id, name, country, age) values (:id, :name, :country, :age)</expression>
      <param name="id" optional="false" sqlType="UUID"/>
      <param name="name" sqlType="STRING"/>
      <param name="country" sqlType="STRING"/>
      <param name="age" optional="false" sqlType="INTEGER"/>
   </query>
   <query id="getUsersbyID" useConfig="Cassandra">
      <expression>SELECT id, age, country, name FROM UsersKS.Users WHERE id = :id</expression>
      <result element="Entries" rowName="Entry">
         <element column="id" name="id" xsdType="string"/>
         <element column="age" name="age" xsdType="string"/>
         <element column="country" name="country" xsdType="string"/>
         <element column="name" name="name" xsdType="string"/>
      </result>
      <param name="id" sqlType="STRING"/>
   </query>
   <operation name="addUsersOp">
      <call-query href="addUsers">
         <with-param name="id" query-param="id"/>
         <with-param name="name" query-param="name"/>
         <with-param name="country" query-param="country"/>
         <with-param name="age" query-param="age"/>
      </call-query>
   </operation>
   <operation name="getUsersbyIDOp">
      <call-query href="getUsersbyID">
         <with-param name="id" query-param="id"/>
      </call-query>
   </operation>
   <resource method="GET" path="users/{id}">
      <call-query href="addUsersbyID">
         <with-param name="id" query-param="id"/>
         <with-param name="name" query-param="name"/>
         <with-param name="country" query-param="country"/>
         <with-param name="age" query-param="age"/>
      </call-query>
   </resource>
</data>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio). The path to this folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.      
2. [Create a Data Service project](../../../../develop/creating-projects/#data-services-project)
4. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.
5. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator. 

The HTTP requests sent for each of the resources using cURL would be as
follows:

-	**Post** new data

	1.  Create a file called users-payload.xml file, and define the XML
	    payload for posting new data as shown below. Note that the id is a
	    UUID value.

	    ```xml
	    <_postusers>
	        <id>a05cdd7a-3d6a-11e8-b467-0ed5f89f718b</id>
	        <name>Peter Parker</name>
	        <country>USA</country>
	        <age>20</age>
	    </_postusers>
	    ```

	2.  Send the following HTTP request from the location where the
	    users-payload.xml file is stored:

	    ```bash
	    curl -X POST -H 'Accept: application/xml'  -H 'Content-Type: application/xml' --data "@users-payload.xml" http://localhost:8290/services/CassandraDataService/users
	    ```

-	**Get** data

	The service can be invoked in REST-style via curl (
	[http://curl.haxx.se](http://curl.haxx.se/) ). Shown below is the curl
	command to invoke the GET resource:

	```bash
	curl -X GET http://localhost:8290/services/CassandraDataService.HTTPEndpoint/users/a05cdd7a-3d6a-11e8-b467-0ed5f89f718b
	```