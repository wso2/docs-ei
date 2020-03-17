# Validating Input Data in a Data Request

Validators are added to individual input mappings in a query. Input
validation allows data services to validate the input parameters in a
request and stop the execution of the request if the input doesn’t meet
the required criteria. WSO2 Micro Integrator provides a set of built-in validators for some of the most
common use cases. It also provides an extension mechanism to write
custom validators.

## Prerequisites

Let's create a MySQL database with the required data.

1.  Install the MySQL server.
2.  Create a database named `           EmployeeDatabse          ` .

    ```bash
    CREATE DATABASE EmployeeDatabase;
    ```

3.  Create the Employee table inside the Employees database:

    ```bash
    USE EmployeeDatabase;

    CREATE TABLE Employees (EmployeeNumber int(11) NOT NULL, FirstName varchar(255) NOT NULL, LastName varchar(255) DEFAULT NULL, Email varchar(255) DEFAULT NULL, Salary varchar(255));
    ```

## Synapse configuration
Given below is the data service configuration you need to build. See the instructions on how to [build and run](#build-and-run) this example.

```xml
<data name="input_validator_sample" transports="http https local">
   <config enableOData="false" id="Datasource">
      <property name="driverClassName">com.mysql.jdbc.Driver</property>
      <property name="url">jdbc:mysql://localhost:3306/Company</property>
   </config>
   <query id="addEmployeeQuery" useConfig="Datasource">
      <sql>insert into Employees (EmployeeNumber, FirstName, LastName, Email, JobTitle, OfficeCode) values(:EmployeeNumber,:FirstName,:LastName,:Email,:JobTitle,:Officecode)</sql>
      <param name="EmployeeNumber" sqlType="STRING"/>
      <param name="FirstName" sqlType="STRING">
            <validateLength maximum="10" minimum="3"/>
      </param>
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
2.  Download the JDBC driver for MySQL from [here](http://dev.mysql.com/downloads/connector/j/) and copy it to the `MI_HOME/lib` directory.
    
    !!! Note
        If you are running this sample using the embedded Micro Integrator of WSO2 Integration Studio, the `lib` directory is located in `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/` (for Linux/MacOS/CentOS) or `MI_TOOLING_HOME/runtime/microesb/lib/` (for Windows). 

    If the driver class does not exist in the relevant directory when you create the datasource, you will get an exception such as `Cannot load JDBC driver class com.mysql.jdbc.Driver`.
        
3. [Create a Data Service project](../../../../develop/creating-projects/#data-services-project)
4. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.
5. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator. 

The **Deployed Services** window allows you to manage data services. You
can try the data service you created by using the TryIt tool in this
screen, which is in your product by default.

1.  Invoke the **addEmployeeOp** operation:
```xml
<addEmployeeOp xmlns="http://ws.wso2.org/dataservice">
     <!--Exactly 1 occurrence-->
     <xs:EmployeeNumber xmlns:xs="http://ws.wso2.org/dataservice">6001</xs:EmployeeNumber>
     <!--Exactly 1 occurrence-->
     <xs:FirstName xmlns:xs="http://ws.wso2.org/dataservice">AB</xs:FirstName>
     <!--Exactly 1 occurrence-->
     <xs:LastName xmlns:xs="http://ws.wso2.org/dataservice">Nick</xs:LastName>
     <!--Exactly 1 occurrence-->
     <xs:Email xmlns:xs="http://ws.wso2.org/dataservice">test@test.com</xs:Email>
     <!--Exactly 1 occurrence-->
     <xs:Salary xmlns:xs="http://ws.wso2.org/dataservice">1500</xs:Salary>
</addEmployeeOp>
```
A validation error is thrown as the response because the addEmployeeOp operation has failed. This is because the FirstName only has 2 characters.

2. Now, change the FirstName in the request as
```xml
<addEmployeeOp xmlns="http://ws.wso2.org/dataservice">
     <!--Exactly 1 occurrence-->
     <xs:EmployeeNumber xmlns:xs="http://ws.wso2.org/dataservice">6001</xs:EmployeeNumber>
     <!--Exactly 1 occurrence-->
     <xs:FirstName xmlns:xs="http://ws.wso2.org/dataservice">ABC</xs:FirstName>
     <!--Exactly 1 occurrence-->
     <xs:LastName xmlns:xs="http://ws.wso2.org/dataservice">Nick</xs:LastName>
     <!--Exactly 1 occurrence-->
     <xs:Email xmlns:xs="http://ws.wso2.org/dataservice">test@test.com</xs:Email>
     <!--Exactly 1 occurrence-->
     <xs:Salary xmlns:xs="http://ws.wso2.org/dataservice">1500</xs:Salary>
</addEmployeeOp>
```
The employee details are added to the database table.  
