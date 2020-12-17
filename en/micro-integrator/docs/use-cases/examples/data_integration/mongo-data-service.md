# Exposing a Mongo Datasource

This example demonstrates how Mongo data can be exposed as a data service.

## Prerequisites

Let's create a simple Mongo database that stores employee information.

1.  Download and set up a <b>MongoDB</b> server.
2.  Use the following commands to create the database.

    ```bash
    mongo
    use employeesdb
    db.createCollection("employees")
    db.things.insert( { id: 1, name: "Document1" } )
    db.employees.insert( { id: 1, name: "Sam" } )
    db.employees.insert( { id: 2, name: "Mark" } )
    db.employees.insert( { id: 3, name: "Steve" } )
    db.employees.insert( { id: 4, name: "Jack" } )
    ```

## Synapse configuration
Given below is the data service configuration you need to build. See the instructions on how to [build and run](#build-and-run) this example.

```xml
<data name="MongoDB" transports="http https local">
   <config enableOData="false" id="MongoDB">
      <property name="mongoDB_servers">localhost</property>
      <property name="mongoDB_database">employeesdb</property>
      <property name="mongoDB_read_preference">PRIMARY</property>
   </config>
   <query id="mongo_insert" useConfig="MongoDB">
      <expression>employees.insert("{id:#, name:#}")</expression>
      <param name="id" optional="false" sqlType="INTEGER"/>
      <param name="name" optional="false" sqlType="STRING"/>
   </query>
   <query id="mongo_find" useConfig="MongoDB">
      <expression>employees.find()</expression>
      <result element="Documents" rowName="Document">
         <element column="document" name="Data" xsdType="string"/>
      </result>
    </query>
   <operation name="mongo_insert_op">
      <call-query href="mongo_insert">
         <with-param name="id" query-param="id"/>
         <with-param name="name" query-param="name"/>
      </call-query>
   </operation>
   <operation name="mongo_find_op">
      <call-query href="mongo_find"/>
   </operation>
   <resource method="POST" path="insert/{id}">
      <call-query href="mongo_insert">
         <with-param name="id" query-param="id"/>
         <with-param name="name" query-param="name"/>
      </call-query>
   </resource>
   <resource method="GET" path="find">
      <call-query href="mongo_find"/>
   </resource>
</data>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio). The path to this folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.        
2. [Create a Data Service project](../../../../develop/create-data-services-configs).
3. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-artifacts) in your Micro Integrator. 

Let's take a look at the curl commands that are used to send the HTTP
requests for each of the resources:

<!--

#### Post new data

1.  Create a file called `data-payload.json          `
    file, and define the json payload for posting new data as shown
    below.

    ```bash
    {  
    "id": "5",
    "name":"Tom",
    }
    ```

2.  Send the following HTTP request from the location where the
    `           data-payload.json          ` file is stored:

    ```bash
    curl -X POST -H 'Accept: application/json'  -H 'Content-Type: application/json' --data "@data-payload.json" http://localhost:8290/services/MongoDB/insert
    ```

#### Get data

-->

The service can be invoked in REST-style via curl (
[http://curl.haxx.se](http://curl.haxx.se/) ). Shown below is the curl
command to invoke the GET resource:

```bash
curl -X GET http://localhost:8290/services/MongoDB/find
```

This generates a response as follows.

```bash
<Documents xmlns="http://ws.wso2.org/dataservice">
  <Document>
    <Data>{ "_id" : { "$oid" : "5fd21a7dc9b921afa1c9b83c"} , "id" : 1.0 , "name" : "Sam"}</Data>
  </Document>
  <Document>
    <Data>{ "_id" : { "$oid" : "5fd21a87c9b921afa1c9b83d"} , "id" : 2.0 , "name" : "Mark"}</Data>
  </Document>
  <Document>
    <Data>{ "_id" : { "$oid" : "5fd21a90c9b921afa1c9b83e"} , "id" : 3.0 , "name" : "Steve"}</Data>
  </Document>
  <Document>
    <Data>{ "_id" : { "$oid" : "5fd21a9fc9b921afa1c9b83f"} , "id" : 4.0 , "name" : "Jack"}</Data>
  </Document>
</Documents>
```
