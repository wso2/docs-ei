# Exposing a MongoDB Datasource

This example demonstrates how a MongoDB database can be exposed as a data service.

## Prerequisites

A MongoDB server v2.4.x or v2.2.x should be already running in the default port.

Create a collection as below in the command shell.

```bash
mongo
use mydb
db.createCollection("things")
db.things.insert( { id: 1, name: "Document1" } )
```

## Synapse configuration
Given below is the data service configuration you need to build. See the instructions on how to [build and run](#build-and-run) this example.

```xml
<data name="MongoDB" serviceNamespace="" serviceGroup="" transports="http https">
  <description />
  <config id="MongoDB">
    <property name="mongoDB_servers">Localhost</property>
    <property name="mongoDB_database">Mydb</property>
    <property name="mongoDB_read_preference">PRIMARY</property>
  </config>
  <query id="mongoinsert" useConfig="MongoDB">
    <expression>things.insert("{id:#, name:#}")</expression>
    <param name="Id" paramType="SCALAR" sqlType="STRING" type="IN" optional="false" />
    <param name="Name" paramType="SCALAR" sqlType="STRING" type="IN" optional="false" />
  </query>
  <query id="Mongofind" useConfig="MongoDB">
    <expression>things.find()</expression>
    <result element="Group" rowName="eo" />
  </query>
  <operation name="Mongoinsertop">
    <call-query href="mongoinsert">
      <with-param name="Id" query-param="Id" />
      <with-param name="Name" query-param="Name" />
    </call-query>
  </operation>
  <operation name="Mongofindop">
    <call-query href="Mongofind" />
  </operation>
  <resource method="GET" path="Find">
    <description />
    <call-query href="Mongofind" />
  </resource>
</data>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio). The path to this folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.       
3. [Create a Data Service project](../../../../develop/create-data-services-configs).
4. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.
5. [Deploy the artifacts](../../../../develop/deploy-artifacts) in your Micro Integrator. 

Let's take a look at the curl commands that are used to send the HTTP
requests for each of the resources:

### Post new data

You can send an HTTP POST request to invoke the data service using cURL as shown below.

1.  Create a file called
    `           document-details-payload.xml          ` file, and define
    the XML payload for updating an existing employee record as shown
    below.

    ```bash
    <mongoinsert>
        <id>3</id>
        <name>Smith</name>
    </mongoinsert>
    ```

2.  Send the following HTTP request from the location where the
    `           document-details-payload.xml          ` file is stored:

    ```bash
    curl -X PUT -H 'Accept: application/xml'  -H 'Content-Type: application/xml' --data "@document-details-payload.xml" http://localhost:8290/services/mongodb/insert/id
    ```

### Get data

You can send an HTTP GET request to invoke the data service using cURL as shown below.

```bash
curl -X GET http://localhost:8290/services/MongoDB.HTTPEndpoint/find
```

This will return the response in XML.