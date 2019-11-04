# Batch Requesting

The batch requests feature allows you to send multiple (IN-Only)
requests to a datasource using a single operation (batch operation).

## Prerequisites

Let's create a MySQL database with the required data.

1.  Install the MySQL server.
2.  Create the following database: Company

    ```bash
    CREATE DATABASE Company;
    ```

3.  Create the **Employees** table:

    ```bash
    USE Company;


    CREATE TABLE `Employees` (`EmployeeNumber` int(11) NOT NULL, `FirstName` varchar(255) NOT NULL, `LastName` varchar(255) DEFAULT NULL, `Email` varchar(255) DEFAULT NULL, `JobTitle` varchar(255) DEFAULT NULL, `OfficeCode` int(11) NOT NULL, PRIMARY KEY (`EmployeeNumber`,`OfficeCode`));
    ```

## Synapse configuration

Given below is the data service configuration you need to build. See the instructions on how to [build and run](#build-and-run) this example.

```xml
<data name="batch_requesting_sample" transports="http https local">
   <config enableOData="false" id="Datasource">
      <property name="driverClassName">com.mysql.jdbc.Driver</property>
      <property name="url">jdbc:mysql://localhost:3306/Company</property>
   </config>
   <query id="addEmployeeQuery" useConfig="Datasource">
      <sql>insert into Employees (EmployeeNumber, FirstName, LastName, Email, JobTitle, OfficeCode) values(:EmployeeNumber,:FirstName,:LastName,:Email,:JobTitle,:Officecode)</sql>
      <param name="EmployeeNumber" sqlType="STRING"/>
      <param name="FirstName" sqlType="STRING"/>
      <param name="LastName" sqlType="STRING"/>
      <param name="Email" sqlType="STRING"/>
      <param name="JobTitle" sqlType="STRING"/>
      <param name="Officecode" sqlType="STRING"/>
   </query>
   <operation name="addEmployeeOp">
      <call-query href="addEmployeeQuery">
         <with-param name="EmployeeNumber" query-param="EmployeeNumber"/>
         <with-param name="FirstName" query-param="FirstName"/>
         <with-param name="LastName" query-param="LastName"/>
         <with-param name="Email" query-param="Email"/>
         <with-param name="JobTitle" query-param="JobTitle"/>
         <with-param name="Officecode" query-param="Officecode"/>
      </call-query>
   </operation>
</data>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio). The path to this folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.
2. Download the JDBC driver for MySQL from [here](http://dev.mysql.com/downloads/connector/j/) and copy it to the `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/lib/` (for MacOS) or 
`MI_TOOLING_HOME/runtime/microesb/lib/` (for Windows) directory. 

    !!! Note
        If the driver class does not exist in the relevant folders when you create the datasource, you will get an exception such as `Cannot load JDBC driver class com.mysql.jdbc.Driver`.
        
3. [Create a Data Service project](../../../../develop/creating-projects/#data-services-project)
4. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.
5. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator. 

Send a request with multiple transactions as shown below. In this example, we are sending two transactions with details of two employees.

```xml
<p:addEmployeeOp_batch_req xmlns:p="http://ws.wso2.org/dataservice">
      <!--1 or more occurrences-->
      <addEmployeeOp xmlns="http://ws.wso2.org/dataservice">
         <!--Exactly 1 occurrence-->
         <xs:EmployeeNumber xmlns:xs="http://ws.wso2.org/dataservice">1002</xs:EmployeeNumber>
         <!--Exactly 1 occurrence-->
         <xs:FirstName xmlns:xs="http://ws.wso2.org/dataservice">John</xs:FirstName>
         <!--Exactly 1 occurrence-->
         <xs:LastName xmlns:xs="http://ws.wso2.org/dataservice">Doe</xs:LastName>
         <!--Exactly 1 occurrence-->
         <xs:Email xmlns:xs="http://ws.wso2.org/dataservice">johnd@wso2.com</xs:Email>
         <!--Exactly 1 occurrence-->
         <xs:JobTitle xmlns:xs="http://ws.wso2.org/dataservice">Consultant</xs:JobTitle>
         <!--Exactly 1 occurrence-->
         <xs:Officecode xmlns:xs="http://ws.wso2.org/dataservice">01</xs:Officecode>
      </addEmployeeOp>
      <addEmployeeOp xmlns="http://ws.wso2.org/dataservice">
         <!--Exactly 1 occurrence-->
         <xs:EmployeeNumber xmlns:xs="http://ws.wso2.org/dataservice">1004</xs:EmployeeNumber>
         <!--Exactly 1 occurrence-->
         <xs:FirstName xmlns:xs="http://ws.wso2.org/dataservice">Peter</xs:FirstName>
         <!--Exactly 1 occurrence-->
         <xs:LastName xmlns:xs="http://ws.wso2.org/dataservice">Parker</xs:LastName>
         <!--Exactly 1 occurrence-->
         <xs:Email xmlns:xs="http://ws.wso2.org/dataservice">peterp@wso2.com</xs:Email>
         <!--Exactly 1 occurrence-->
         <xs:JobTitle xmlns:xs="http://ws.wso2.org/dataservice">Consultant</xs:JobTitle>
         <!--Exactly 1 occurrence-->
         <xs:Officecode xmlns:xs="http://ws.wso2.org/dataservice">01</xs:Officecode>
      </addEmployeeOp>
   </p:addEmployeeOp_batch_req>
```

When you execute the batch operation, you will find that all the records have been inserted into the database simultaneously.

!!! Tip
    Want to confirm that the records are added to the database? Run the following MySQL command.
    
    ```bash
    SELECT * FROM Employees
    ```
        
    You will see all the records that are in the `Employees` table, including the two employee records that you added in the previous step.  