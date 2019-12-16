# Adding Multiple SQL Dialects

To avoid writing different data service queries for the same operation
depending on the configuration, write all three SQL queries in the same
data service query.

SQL Dialect support allows users to create multiple SQL queries for
different SQL dialects by checking the driver. This helps maintain
driver-specific functionality/key words in SQL statements of a single
data service query. 

## Example configuration

For example, to determine the data length retrieved from a column, there
can be different ways to write the SQL query depending on the SQL
driver.

**On MySQL and PostgreSQL:**

```sql
SELECT OCTET_LENGTH(employeeNumber)
FROM Employees;
```

**On Microsoft SQL Server:**

```sql
SELECT DATALENGTH(employeeNumber)
FROM Employees
```

**On Oracle:**

```sql
SELECT LENGTH(employeeNumber)
FROM Employees
```

Shown below is an example data service configuration that uses the multiple SQL dialects explained above.

```xml
<data name="adding_sql_dialects" serviceNamespace="" serviceGroup="">
    <description/>
    <config id="default">
        <property name="org.wso2.ws.dataservice.user">root</property>
        <property name="org.wso2.ws.dataservice.password"/>
        <property name="org.wso2.ws.dataservice.protocol">jdbc:mysql://localhost:3306/Employees</property>
        <property name="org.wso2.ws.dataservice.driver">com.mysql.jdbc.Driver</property>
    </config>
    <query id="SQL" useConfig="default">
        <sql>SELECT DATALENGTH(employeeNumber) FROM Employees</sql>
        <sql dialect="oracle">SELECT LENGTH(employeeNumber) FROM Employees</sql>
        <sql dialect="h2,mysql,postgresql">SELECT OCTET_LENGTH(employeeNumber) FROM Employees</sql>
        <sql dialect="mssql">SELECT DATALENGTH(employeeNumber) FROM Employees</sql>
        <result element="Employees" rowName="Employee">
            <element column="employeeNumber" name="employeeNumberr" xsdType="string"/>
        </result>
    </query>
</data>
```

<!--
Follow the steps below to add SQL dialects to a query:

1. Open you data service file in WSO2 Integration Studio.
2. Select the data service and add the required dialects in the <b>Properties</b> tab.
-->