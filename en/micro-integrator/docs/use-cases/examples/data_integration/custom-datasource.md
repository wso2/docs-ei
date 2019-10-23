# Exposing a Custom Datasource as a Data Service

This example demonstrates how a custom datasource can be exposed as a data service.

## About custom datasources

Custom datasources allow you to interface data services with your own
datasource implementation. There are two options for writing a custom
datasource, and these two options cover most of the common business use
cases as follows:

-   **Custom tabular datasources:** Used to represent data in tables,
    where a set of named tables contain data rows that can be queried
    later. A tabular datasource is typically associated with an SQL data
    services query. This is done by internally using our own SQL parser
    to execute SQL against the custom datasource. You can use the
    `                     org.wso2.carbon.dataservices.core.custom.datasource.TabularDataBasedDS                   `
    interface to implement tabular datasources. For a sample
    implementation of a tabular custom datasource, see
    `                     org.wso2.carbon.dataservices.core.custom.datasource.InMemoryDataSource                   `
    . Also, this is supported in Carbon datasources with the following
    datasource reader implementation:
    `          org.wso2.carbon.dataservices.core.custom.datasource.CustomTabularDataSourceReader         `
    .
-   **Custom query datasources:** Used when the datasource has some form
    of query expression support. Custom query datasources are
    implemented using the
    `                     org.wso2.carbon.dataservices.core.custom.datasource.CustomQueryBasedDS                   `
    interface. You can create any non-tabular datasource using the
    query-based approach. Even if the target datasource does not have a
    query expression format, you can create your own. For example, you
    can support any NoSQL type datasource this way. For a sample
    implementation of a query-based custom datasource, see
    `                     org.wso2.carbon.dataservices.core.custom.datasource.EchoDataSource                   `
    . This is supported in Carbon datasources with the following
    datasource reader implementation:
    `          org.wso2.carbon.dataservices.core.custom.datasource.CustomQueryDataSourceReader         `
    .

In the `         init        ` methods of all custom datasources,
user-supplied properties are parsed to initialize the datasource
accordingly. Also, a property named `         __DATASOURCE_ID__        `
, which contains a UUID to uniquely identify the current datasource, is
passed.Custom datasource authors use this to identify
the datasources accordingly. For example, scenarios
like datasource instances communicating within a server cluster for data
synchronisation.

!!! Info
    Find the custom connectors used from the github project located at
[https://github.com/wso2/wso2-dss-connectors.](https://github.com/wso2/wso2-dss-connectors/)

## Adding a custom tabular datasource to the data service

Follow the steps given below to create a custom tabular datasource.'

### Synapse configuration

    Given below is the data service configuration you need to build. See the instructions on how to [build and run](#build-and-run) this example.

    ```xml
    <data name="CustomTabularDataService" transports="http https local">
       <config enableOData="false" id="CustomTabular">
          <property name="custom_tabular_datasource_class">org.wso2.carbon.dataservices.core.custom.datasource.InMemoryDataSource</property>
          <property name="custom_datasource_props">
             <property name="inmemory_datasource_schema">{Users:[ID,Name]}</property>
             <property name="inmemory_datasource_records">{Users:[[</property>
          </property>
       </config>
       <query id="getAllUsers" useConfig="CustomTabular">
          <sql>SELECT ID,Name FROM Users</sql>
          <result element="Users" rowName="User">
             <element column="ID" name="ID" xsdType="string"/>
             <element column="Name" name="Name" xsdType="string"/>
          </result>
       </query>
       <operation name="GetAllUsersOp">
          <call-query href="getAllUsers"/>
       </operation>
       <resource method="GET" path="Users">
          <call-query href="getAllUsers"/>
       </resource>
    </data>
    ```

### Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio). The path to this folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.      
2. [Create a Data Service project](../../../../develop/creating-projects/#data-services-project)
4. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.
5. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator. 

You can send an HTTP GET request to invoke the data service using cURL
as shown below.

```bash
curl -X GET http://localhost:8280/services/CustomTabularDataService.HTTPEndpoint/Users
```

This will return the response in XML.

## Adding a custom query datasource to the data service

Follow the steps given below to create a custom query datasource.

### Synapse configuration

    Given below is the data service configuration you need to build. See the instructions on how to [build and run](#build-and-run) this example.

    ```xml
    <data name="CustomQueryDataService" transports="http https local">
       <config enableOData="false" id="CustomQuery">
          <property name="custom_query_datasource_class">org.wso2.carbon.dataservices.core.custom.datasource.EchoDataSource</property>
          <property name="custom_datasource_props">
             <property name="prop1">prop_value1</property>
             <property name="prop2">prop_value2</property>
          </property>
       </config>
       <query id="getValues" useConfig="CustomQuery">
          <expression>column1,column2;R1C1 :param1,R1C2 :param2;R2C1 :param1,R2C2 :param2</expression>
          <result element="Rows" rowName="Row">
             <element column="column1" name="C1" xsdType="string"/>
             <element column="C2" name="C2" xsdType="string"/>
          </result>
          <param name="column1" optional="false" sqlType="STRING"/>
          <param name="column2" optional="false" sqlType="STRING"/>
       </query>
       <operation name="GetValuesOp">
          <call-query href="getValues">
             <with-param name="column1" query-param="column1"/>
             <with-param name="column2" query-param="column2"/>
          </call-query>
       </operation>
       <resource method="GET" path="Values/{column1}">
          <call-query href="getValues">
             <with-param name="column1" query-param="column1"/>
             <with-param name="column2" query-param="column2"/>
          </call-query>
       </resource>
    </data>
    ```

### Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio). The path to this folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.      
2. [Create a Data Service project](../../../../develop/creating-projects/#data-services-project)
4. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.
5. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator. 

You can send an HTTP GET request to invoke the data service using cURL
as shown below.

```bash
curl -X GET http://localhost:8280/services/CustomQueryDataService.HTTPEndpoint/values/1
```

This will return the response in XML.