# Exposing an Excel Datasource

This example demonstrates how Excel data can be exposed as a data service.

## Prerequisites

!!! Info
    The Micro Integrator uses the Apache POI library version 3.9.0 to work with Excel datasources, and thereby, supports both XLS and XLSX formats. For more information, see the [Apache POI Documentation](index) .

[Download](https://github.com/wso2-docs/WSO2_EI/blob/master/data-service-resources/Products.xls) the `Products.xls` file.

This file contains data about products (cars/motorcycles) that are
manufactured in an automobile company. The data table has the following
columns: `         ID        ` , `         Name        ` ,
`         Classification        ` , and `         Price        `.

## Synapse configuration
Given below is the data service configuration you need to build. See the instructions on how to [build and run](#build-and-run) this example.

**Be sure** to update the CSV datasource path.

-   Query mode enabled

    ```xml
    <data name="Excel" transports="http https local">
       <config enableOData="false" id="Excel">
          <property name="driverClassName">org.wso2.micro.integrator.dataservices.sql.driver.TDriver</property>
          <property name="url">jdbc:wso2:excel:filePath=/path/to/excel/Products.xls</property>
       </config>
       <query id="GetProductbyID" useConfig="Excel">
          <sql>select ID, Model, Classification from Sheet1 where ID=:ID</sql>
          <result element="Entries" rowName="Entry">
             <element column="ID" name="ID" xsdType="string"/>
             <element column="Model" name="Model" xsdType="string"/>
             <element column="Classification" name="Classification" xsdType="string"/>
          </result>
          <param name="ID" sqlType="STRING"/>
       </query>
       <query id="AddProducts" useConfig="Excel">
          <sql>INSERT INTO sheet1 (ID, Model, Classification) VALUES(:ID, :Model, :Classification)</sql>
          <param name="ID" sqlType="STRING"/>
          <param name="Model" sqlType="STRING"/>
          <param name="Classification" sqlType="STRING"/>
       </query>
       <operation name="GetProductsbyIDOp">
          <call-query href="GetProductbyID">
             <with-param name="ID" query-param="ID"/>
          </call-query>
       </operation>
       <operation name="AddProductsOp">
          <call-query href="AddProducts">
             <with-param name="ID" query-param="ID"/>
             <with-param name="Model" query-param="Model"/>
             <with-param name="Classification" query-param="Classification"/>
          </call-query>
       </operation>
       <resource method="GET" path="Products/{ID}">
          <call-query href="GetProductbyID">
             <with-param name="ID" query-param="ID"/>
          </call-query>
       </resource>
       <resource method="POST" path="Products">
          <call-query href="AddProducts">
             <with-param name="ID" query-param="ID"/>
             <with-param name="Model" query-param="Model"/>
             <with-param name="Classification" query-param="Classification"/>
          </call-query>
       </resource>
    </data>
    ```

-   Query mode disabled

    ```xml
    <data name="Excel2" transports="http https local">
       <config enableOData="false" id="Excel">
          <property name="excel_datasource">/path/to/excel/Products.xls</property>
       </config>
       <query id="GetProducts" useConfig="Excel">
          <excel>
             <workbookname>Sheet1</workbookname>
             <hasheader>true</hasheader>
             <startingrow>2</startingrow>
             <maxrowcount>5</maxrowcount>
             <headerrow>1</headerrow>
          </excel>
          <result element="Products" rowName="Product">
             <element column="ID" name="ID" xsdType="string"/>
             <element column="Model" name="Model" xsdType="string"/>
             <element column="Classification" name="Classification" xsdType="string"/>
          </result>
       </query>
       <operation name="GetProducts">
          <call-query href="GetProducts"/>
       </operation>
       <resource method="GET" path="Products">
          <call-query href="GetProducts"/>
       </resource>
    </data>
    ```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio). The path to this folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.      
2. [Create a Data Service project](../../../../develop/creating-projects/#data-services-project)
4. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.
5. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator. 

You can send an HTTP GET request to invoke the data service using a curl
command as shown below.

-   **Query mode is disabled**:

    Run the following curl command to get the product details:

    ```bash
    curl -X GET http://localhost:8290/services/Excel2.HTTPEndpoint/Products
    ```

    You get an output similar to the following:

    ```xml
    <Products xmlns="http://ws.wso2.org/dataservice"><Product><ID>S10_1678</ID><Model>1969 Harley Davidson Ultimate Chopper</Model>
    <Classification>Motorcycles</Classification></Product><Product><ID>S10_1949</ID><Model>1952 Alpine Renault 1300</Model><Classification>Classic Cars</Classification>
    </Product><Product><ID>S10_2016</ID><Model>1996 Moto Guzzi 1100i</Model><Classification>Motorcycles</Classification></Product><Product>
    <ID>S10_4698</ID><Model>2003 Harley-Davidson Eagle Drag Bike</Model><Classification>Motorcycles</Classification></Product><Product>
    <ID>S10_4757</ID><Model>1972 Alfa Romeo GTA</Model><Classification>Classic Cars</Classification></Product></Products>
    ```

-   **Query mode is enabled**

    Invoke the following command to get details of a product.

    ```bash
        curl -X GET http://localhost:8290/services/Excel.HTTPEndpoint/Products/{PRODUCT_ID}
    ```

    Example:

    ```bash
        curl -X GET http://localhost:8290/services/Excel.HTTPEndpoint/Products/S10_4757
    ```

    Follow the steps given below to insert data to the excel sheet:

    1.  Copy code given below to a file, name it
        **`                 product-data.xml                `** , and save
        it.
        ```xml
          <_putproduct>
                <ID>S410_5443</ID>
                <Model>1972 Alfa Romeo GTA</Model>
                <Classification>Classic Cars</Classification>
          </_putproduct>
        ```

    2.  Open the terminal, navigate the location where the file was saved
        and run the following command to insert data to the excel sheet.

        ```bash
        curl -X POST -H 'Accept: application/xml' -H 'Content-Type: application/xml' --data "@product-data.xml" -k -v http://localhost:8290/services/Excel.HTTPEndpoint/Products
        ```

        !!! Info
            To confirm that the data got added you can run the GET curl command again using the product ID that you used in the `product-data.xml` file.
        
    These will return the response in XML.