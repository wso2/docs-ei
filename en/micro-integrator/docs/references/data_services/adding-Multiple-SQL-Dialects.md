# Adding Multiple SQL Dialects

SQL Dialect support allows users to create multiple SQL queries for
different SQL dialects by checking the driver. This helps maintain
driver-specific functionality/key words in SQL statements of a single
data service query.

For example, to determine the data length retrieved from a column, there
can be different ways to write the SQL query depending on the SQL
driver.

**On MySQL and PostgreSQL:**

``` java
    SELECT OCTET_LENGTH(employeeNumber)
    FROM Employees;
```

**On Microsoft SQL Server:**

``` java
    SELECT DATALENGTH(employeeNumber)
    FROM Employees
```

**On Oracle:**

``` java
    SELECT LENGTH(employeeNumber)
    FROM Employees
```

To avoid writing different data service queries for the same operation
depending on the configuration, write all three SQL queries in the same
data service query as shown below.

![](attachments/29919843/29897946.png)

Follow the steps below to add SQL dialects to a query.

1.  The SQL dialect option appears when adding a query to datasources
    such as RDBMS. For example,  
    ![](attachments/29919843/29897945.png){width="430"}
2.  Click **Add New SQL Dialect** to open the **SQL Dialect** window.
    Select the required SQL driver and define the SQL statement as shown
    below.  
    ![](attachments/29919843/29897944.png){width="430"}  
      
3.  If the SQL statement should be the same for multiple drivers, select
    all drivers (e.g., MySQL, PostgreSQL and H2) at once and define the
    statement as shown below.  
      
    ![](attachments/29919843/29897943.png){width="430"}  
    To define an SQL dialect for a driver other than the ones listed in
    the supported drivers list, provide the driver prefix in the text
    field and define the SQL query as shown above.
