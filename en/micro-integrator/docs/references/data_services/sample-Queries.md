# Sample Queries

This section gives examples of writing queries using the features
discussed so far.

-   [Sample 1: Using input mappings in a
    query](#SampleQueries-Sample1:Usinginputmappingsinaquery)
-   [Sample 2: Adding output mappings to a
    query](#SampleQueries-Sample2:Addingoutputmappingstoaquery)
-   [Sample 3: Using a result row
    namespace](#SampleQueries-Sample3:Usingaresultrownamespace)
-   [Sample 4: Querying a RDF data
    source](#SampleQueries-Sample4:QueryingaRDFdatasource)
-   [Sample 5: Querying a Web data
    source](#SampleQueries-Sample5:QueryingaWebdatasource)
-   [Sample 6: Querying a google
    spreadsheet](#SampleQueries-Sample6:Queryingagooglespreadsheet)
-   [Sample 7: Calling a MySQL
    function](#SampleQueries-Sample7:CallingaMySQLfunction)
-   [Sample 8: Calling an Oracle
    function](#SampleQueries-Sample8:CallinganOraclefunction)
-   [Sample 9: Defining a dynamic SQL
    query](#SampleQueries-Sample9:DefiningadynamicSQLquery)
-   [Sample 10: Defining Named
    parameters](#SampleQueries-Sample10:DefiningNamedparameters)
-   [Sample 11: Grouping data into complex
    elements](#SampleQueries-Sample11:Groupingdataintocomplexelements)

#### Sample 1: Using input mappings in a query

This sample demonstrates how to write a simple query with input
mappings.

**Query ID** : customerAddressSQL

**SQL Statement** : select \* from Customers where contactLastName = ?
and contactFirstName = ?

The following query needs two parameters for execution. The **Input
Mapping** section is used to specify these input parameters.

![](attachments/16844915/17214772.png){width="720"}

For information on adding validations to your input mappings, see [Input
Validators](Input-Validators_119130629.html#InputValidators-Validators)
.

#### Sample 2: Adding output mappings to a query

Following sample shows how to query the Cassandra data source created in
[Cassandra](_Exposing_Cassandra_as_a_Data_Service_) and add output
mappings to it.

**Query ID** : getUsers

**Data Source** : CassandraDatasource

**CQL** : select 'key', 'username', 'password' from USER

![](attachments/16844924/17214884.png){width="500"}

You can define how the output of this query looks by adding output
mappings as follows:

![](attachments/16844924/17214883.png){width="700"}

#### Sample 3: Using a result row namespace

This example shows how to use a results row namespace in output
mappings. See [Defining Namespaces](_Using_Namespaces_) for information
on namespaces.

![](attachments/16844918/17214797.png)

#### Sample 4: Querying a RDF data source

Following sample shows a query written for the RDF data source created
in [Exposing RDF Data as a Data
Service](_Exposing_RDF_Data_as_a_Data_Service_) , which is based on the
added RDF file. This sample uses the following SPARQL query to extract
movies information from the data source.

``` java
    PREFIX cd: <http://www.popular.movies/cd#> 
    
    SELECT ?title ?director ?year ?genre ?actor  
    WHERE {    
          ?movie cd:title ?title.  
          ?movie cd:director ?director.  
          ?movie cd:year ?year.  
          ?movie cd:genre ?genre.  
          ?movie cd:actor ?actor. 
    }
```

The input mapping section is used to specify parameters to the query.
The above query extracts movie information according to the genre.
Therefore, we add genre as an input parameter. Next, the output mappings
are used to map the response to an output XML.

![](attachments/51485618/51449952.png)

#### Sample 5: Querying a Web data source

Following sample shows a query written for the Web data source created
in [Web Resource](_Exposing_a_Web_Resource_as_a_Data_Service_) . When
you add a query to a Web data source, you must enter a **Scraper
Variable** . This scraper variable must be the same as the output name
in the Web data source configuration, which returns the output from the
configuration. In this example, the var-def name in the configuration is
weatherInfo ( `         <        ` `         var-def        `
`         name        ` `         =        `
`         'weatherInfo'        ` `         >        ` ). Therefore, the
Web resource output variable name is also specified as weatherInfo as
shown below:

![](attachments/16844925/17214893.png){width="550"}

It defines output mappings as follows to specify how the output looks
like.

![](attachments/16844925/17214892.png){width="600"}

#### Sample 6: Querying a google spreadsheet

Given below are sample queries that can be written for a google
spreadsheet datasource. Note that SQL queries of this type can only be
used for a google spreadsheet if the **Query mode** is enabled for the
datasource in the data service.

**Sample 1:**

``` java
    SELECT customerNumber, customerName, phone, state, country
    FROM customers
```

![](attachments/16844919/17214810.png){width="550"}

**Sample 2:**

``` java
    INSERT INTO customers (customerNumber, customerName, contactLastName)
    VALUES(?,?,?)
```

![](attachments/16844919/17214809.png){width="700"}

**Sample 3:**

``` java
    UPDATE customers
    SET contactFirstName=?, contactLastName=?
    WHERE customerNumber=?
```

![](attachments/16844919/17214808.png){width="600"}

**Sample 4:**

``` java
    DELETE FROM customers
    WHERE customerNumber=?
```

![](attachments/16844919/17214807.png){width="400"}

You can also create new sheets in the Excel or drop existing sheets.

**Sample 5:**

``` java
    CREATE SHEET ProductCategories (ProductCode, Category)
```

![](attachments/16844919/17214806.png){width="450"}

**Sample 6:**

``` java
    DROP SHEET ProductCategories
```

![](attachments/16844919/17214805.png){width="420"}

#### Sample 7: Calling a MySQL function

Assume you have the following MySQL function, which takes a string
parameter and returns the same as output. Create a database before
executing the query.

``` sql
    create function myFunction(p_inparam varchar(20)) 
      returns varchar(20)
      begin
         declare output_text varchar(20);
         set output_text = p_inparam;
         return output_text;
      end
```

To call this function from the data service, add a query to the data
service definition file (.dbs) pointing to an RDBMS datasource that
connects to the MySQL database. For example,

`         <sql>select myFunction('WSAS')                   a         ` s
ABC\</sql\>

For example, see the following data service configuration:

``` html/xml
    <data name="sqlfunctionService">
       <config id="mynew">
          <property name="driverClassName">com.mysql.jdbc.Driver</property>
          <property name="url">jdbc:mysql://localhost:3306/sample&lt;/property>
          <property name="username">root</property>
          <property name="password">root</property>
       </config>
       <query id="NewfunctionQuery" useConfig="mynew">
          <sql>select myFunction('WSAS') as ABC</sql>
          <result element="wsas" rowName="wsas">
             <element column="output_text" name="n_param" xsdType="string"/>
          </result>
          <param name="imparam" sqlType="STRING"/>
       </query>
       <operation name="functionop">
          <call-query href="NewfunctionQuery">
             <with-param name="imparam" query-param="imparam"/>
          </call-query>
       </operation>
    </data> 
```

You can also call this function in the **Query Details** page of the
management console as follows:

![](attachments/119130642/119130643.png){width="500"}

#### Sample 8: Calling an Oracle function

Assume you have the following Oracle stored function, which returns the
total number of entries in a table:

``` sql
    CREATE OR REPLACE FUNCTION myfunction(ename IN VARCHAR, eid IN NUMBER) RETURN INTEGER 
    AS myCount INTEGER;
    BEGIN
        INSERT INTO TEAMS values(eid, ename);
        SELECT COUNT(*) into myCount from TEAMS;
        RETURN myCount;
    END;
    /
```

Create a table before executing the query as follows:

``` sql
    CREATE TABLE TEAMS(id INTEGER, team VARCHAR(30));
```

To call this function from the data service, add a query to the data
service definition file (.dbs). For example,

`         "{call ?:=myfunction(?,?)}"        `

First input parameter carries the return value of the function. Other
two parameters are inputs to the function. You must define an Input
parameter with OUT type to get the result of function (i.e., the first
parameter in the query above). Then, define a Output parameter to get
this value as a result set from the data service. The following code
segment does this:

``` html/xml
    <result element="TotalTeams" rowName="">
        <element name="totalTeams" column="totalTeams" xsdType="xs:integer" />
    </result>
    <param name="totalTeams" sqlType="INTEGER" type="OUT" ordinal="1" />
```

For example, see the following data service configuration:

``` html/xml
    <data name="testOracleFunction">
       <config id="or">
          <property name="org.wso2.ws.dataservice.driver">oracle.jdbc.driver.OracleDriver</property>
          <property name="org.wso2.ws.dataservice.protocol">jdbc:oracle:thin:user/pwd@localhost:1521/XE</property>
          <property name="org.wso2.ws.dataservice.user">user</property>
          <property name="org.wso2.ws.dataservice.password">pwd</property>
       </config>
       <query id="q1" useConfig="or">
          <sql>{call ?:=myfunction(?,?)}</sql>
          <result element="TotalTeams" rowName="">
             <element name="totalTeams" column="totalTeams" xsdType="xs:integer" />
          </result>
          <param name="totalTeams" sqlType="INTEGER" type="OUT" ordinal="1" />
          <param name="ename" sqlType="STRING" ordinal="2" />
          <param name="eid" sqlType="INTEGER" ordinal="3" />
       </query>
       <operation name="op1">
          <call-query href="q1">
             <with-param name="ename" query-param="ename" />
             <with-param name="eid" query-param="eid" />
          </call-query>
       </operation>
    </data>
```

You can call this function in the **Query Details** page of the
management console as before.

#### Sample 9: Defining a dynamic SQL query

Dynamic SQL query support allows you to change SQL queries (e.g.,
defining additional conditions in the SQL) in the runtime without
changing the data service configuration. For this to work, you must
specify required SQL query statements (e.g., with WHERE clause) as a
`         QUERY_STRING        ` data type. These statements will be
directed to the final SQL query in the runtime.  

!!! info

Dynamic query support can lead to SQL injection attacks. Therefore, we
recommend that the clients validate the values set to
`         QUERY_STRING        ` at runtime.


The `         QUERY_STRING        ` data type is available as an SQL
type when creating Input Mappings for queries. For example,

![](attachments/119130642/119130644.png){width="400"}  

You can add the SQL query using the mapping name:

![](attachments/119130642/119130645.png){width="500"}  

A sample configuration for the data service is shown below:

  

``` java
    <data name="DynamicQuerySample" serviceNamespace="http://ws.wso2.org/dataservice/samples/rdbms_sample">
       <config id="default">
          <property name="driverClassName">org.h2.Driver</property>
          <property name="url">jdbc:h2:file:./samples/database/DATA_SERV_SAMP</property>
          <property name="username">wso2ds</property>
          <property name="password">wso2ds</property>
          <property name="minIdle">1</property>
          <property name="maxActive">10</property>
          <property name="autoCommit">false</property>
       </config>
       <query id="employeesSQL" useConfig="default">
          <sql>select * from Employees :filterQuery</sql>
          <result element="employees" rowName="employee">
             <element column="lastName" name="last-name" xsdType="string"/>
             <element column="firstName" name="first-name" xsdType="string"/>
             <element column="email" name="email" xsdType="string"/>
             <element column="salary" name="salary" xsdType="double"/>
          </result>
          <param name="filterQuery" sqlType="QUERY_STRING"/>
       </query>
       <query id="customerInCountrySQL" useConfig="default">
          <sql>select * from Customers where country = :country :filter</sql>
          <result element="customer-addresses" rowName="customer-address">
             <element column="customerNumber" name="customer-number" xsdType="integer"/>
             <element column="contactLastName" name="contact-last-name" xsdType="string"/>
             <element column="contactFirstName" name="contact-first-name" xsdType="string"/>
             <element column="addressLine1" name="address-line1" xsdType="string"/>
             <element column="addressLine2" name="address-line2" xsdType="string"/>
             <element column="city" name="city" xsdType="string"/>
             <element column="state" name="state" xsdType="string"/>
             <element column="postalCode" name="postal-code" xsdType="string"/>
             <element column="country" name="country" xsdType="string"/>
          </result>
          <param name="country" sqlType="STRING"/>
          <param name="filter" sqlType="QUERY_STRING"/>
       </query>
       <query id="insertUpdateQuery" useConfig="default">
          <sql>:query</sql>
          <param name="query" sqlType="QUERY_STRING"/>
       </query>
       <operation name="getEmployees">
          <call-query href="employeesSQL">
             <with-param name="filterQuery" query-param="filterQuery"/>
          </call-query>
       </operation>
       <operation name="getCustomersInCountry">
          <call-query href="customerInCountrySQL">
             <with-param name="country" query-param="country"/>
             <with-param name="filter" query-param="filter"/>
          </call-query>
       </operation>
       <operation name="insertUpdateOp">
          <call-query href="insertUpdateQuery">
             <with-param name="query" query-param="query"/>
          </call-query>
       </operation>
    </data>
```

#### Sample 10: Defining Named parameters

Named Parameters enable reusability of parameters and reduce the
complexity of database configurations. Named parameters are specified in
SQL queries of data services.

A named parameter must have the same name as the input parameter along
with a colon ':' in front. For example,

![](http://docs.wso2.org/wiki/download/attachments/4885656/namedParam.png?version=1&modificationDate=1328226529000){width="700"}

#### Sample 11: Grouping data into complex elements

Complex Elements help represent data in a structured manner. For
example, let's take a table containing customer information. There can
be several columns that keep data related to the address of employees
such as number, street, city, postal code. You can group them into one
element called address using complex elements and present them in a more
structured manner.

Following example illustrates how to use complex elements:

![](http://docs.wso2.org/wiki/download/attachments/4885647/step1.png?version=1&modificationDate=1328223366000)

After defining the query, proceed to adding an Output Mapping. In output
mappings, select the **Mapping Type** as complex element. Specify an
**Element Name** and click the **Add Nested Element** button. For
example,

![](http://docs.wso2.org/wiki/download/attachments/4885647/step3.png?version=1&modificationDate=1328224227000)

The added element appears under **Complex Elements** in the Output
Mapping.

According to the figure below, addressline1, addressline2 and city are
nested elements that come under address element.

![](http://docs.wso2.org/wiki/download/attachments/4885647/step4.png?version=1&modificationDate=1328224416000)
