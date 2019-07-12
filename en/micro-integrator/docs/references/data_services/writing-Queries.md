# Writing Queries

The query in a data service specifies the type of task that should be
performed on the data in a particular data store. For example, consider
the task of retrieving data from a data store, or posting, updating, and
deleting data. Data consumers send requests to the data service by
invoking the relevant operation (or REST resource) in the data service.
Consequently, the query connected to the operation/resource is executed
to perform the task.

!!! info

REST resources and Operations are used depending on whether the
particular task should be invoked RESTfully, or by using SOAP. Read more
about REST resource and operations in data services.


For most data stores, a query typically represents an SQL statement.
However, some data stores such as Excel, and CSV, require queries to be
specified differently.

A query in a data service consists of the following elements:

-   [SQL/Query details](#WritingQueries-SQL/Querydetails)
-   [Input parameters](#WritingQueries-Inputparameters)
-   [Output parameters](#WritingQueries-Outputparameters)
-   [Events](#WritingQueries-Events)
-   [Advanced query parameters](#WritingQueries-Advancedqueryparameters)

### SQL/Query details

If the data store supports SQL, you need to specify the SQL statement to
execute the required task. In other data stores (such as Google
Spreadsheets), you need to specify the query details.

### Input parameters

If you are writing an SQL query that requires an input value, you need
to specify the parameters that can be used to provide the input. For
example, if the SQL statement is to get the price of a particular
product, it is necessary to provide the identifier of the product in the
data store. In the Input Mapping section, you can create a parameter and
map it to the column name of the product identifier in the database.

Input mappings allow you to add parameters to a query so that you can
set the parameter value when executing the query. For example, when you
define a query as
mapping is a parameter that sets the value of ID.

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 85%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Generate Input Mapping</td>
<td><div class="content-wrapper">
<p>If you have defined an SQL query, you can generate input mappings corresponding to the input fields specified in the query by clicking <strong>Generate Input Mappings</strong> . As shown in the example below, an input mapping is created for the <strong>emp_no</strong> field, which will allow you to invoke this query by specifying a value for this field as an input.<br />
<img src="attachments/119130823/119130824.png" width="600" height="337" /></p>
</div></td>
</tr>
<tr class="even">
<td>Parameter Type</td>
<td><div class="content-wrapper">
<div>
This is the data type of the input mapping, which determines how the input mapping parameter will be given in the target query.
</div>
<ul>
<li>SCALAR: In the target query, the parameter will be used as one value.</li>
<li>ARRAY: In the target query, the parameter will contain one or many values for a mapped parameter.<br />
</li>
</ul>
!!! note
<p>Note that ARRAY parameter type cannot be used with the QUERY_STRING data type (SQL type).</p>

<p>In the context of RDBMS and SQL datasources, an ARRAY parameter mapped to an SQL query will be expanded to multiple comma separated parameters at runtime. For example, this can be used in SQL statement conditions such as SELECT ... WHERE ... IN(?).</p>
</div></td>
</tr>
<tr class="odd">
<td>SQL Type</td>
<td>The data type of the corresponding SQL parameter can be selected from this menu. Note that the QUERY_STRING data type cannot be used if the <a href="#WritingQueries-Parametertype">parameter type</a> is set to ARRAY. Find more from <a href="https://docs.wso2.com/display/EI611/Data+Types+of+Mappings">here</a> about data types.</td>
</tr>
<tr class="even">
<td>Default Value</td>
<td><div class="content-wrapper">
<p>Default values help you automatically assign a value to a parameter when a user has not entered a specific parameter value in a request. Default values are added when defining queries. Therefore, this value gets automatically added to the query if it is ignored by the user.</p>
<p>You can refer to Internal Property Values using Default Values. You can use special system variables that are defined as default values. At the moment, it only provides a variable for retrieving the username of the current user authenticated in a secured data service. You can access this variable as follows:<br />
</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>#{USERNAME}: Dynamically replaces the input mapping with the current user&#39;s username when a data service request is processed.</span>
<span id="cb1-2"><a href="#cb1-2"></a></span>
<span id="cb1-3"><a href="#cb1-3"></a>#{NULL}: Sets the current input mapping value to <span class="kw">null</span>.</span>
<span id="cb1-4"><a href="#cb1-4"></a>         It&#39;s the same as providing <span class="st">&quot;xsi:nil&quot;</span> in the incoming message&#39;s input parameter element.</span>
<span id="cb1-5"><a href="#cb1-5"></a></span>
<span id="cb1-6"><a href="#cb1-6"></a>#{TENANT_ID}: Represents the current tenant ID.</span>
<span id="cb1-7"><a href="#cb1-7"></a>              This is useful in a Stratos deployment where multiple tenants live in the same server.</span>
<span id="cb1-8"><a href="#cb1-8"></a></span>
<span id="cb1-9"><a href="#cb1-9"></a>#{USER_ROLES}: This value contains the list of user roles that the current calling user has.</span>
<span id="cb1-10"><a href="#cb1-10"></a>               If the parameter mapped is of type ARRAY, it will have the full list of user roles.</span>
<span id="cb1-11"><a href="#cb1-11"></a>               If it&#39;s a SCALAR, it will only contain the first user role of the user.</span></code></pre></div>
</div>
</div>
<p>For a demonstration of the usage of default values, see <a href="https://docs.wso2.com/display/EI650/Default+Values+Sample">Default Values Sample</a> .</p>
</div></td>
</tr>
<tr class="odd">
<td>IN/OUT Type</td>
<td>These are used in stored procedures which takes out parameters and in/out parameters. IN is the usual parameter we give to provide some value inside. OUT only returns a value from a stored procedure. INOUT does both.</td>
</tr>
<tr class="even">
<td>Validators</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td>Returning Generated Keys</td>
<td><div class="content-wrapper">
<p>The <strong>Return Generated Keys</strong> option appears under <strong>Input Mappings</strong> on the <strong>Queries</strong> page.</p>
<p><img src="attachments/119130823/119130825.png" /></p>
<p>It inserts data to a table that has auto increment key columns. The auto incremented key value of the record is mapped to the result output mappings of the data service. For example, the sample query below is used to insert values to a table by the name <code>               wes_teams              </code> , which has an auto increment column:</p>
<p><code>               INSERT INTO wes_teams(TEAM) VALUES(?)              </code></p>
<p><img src="http://docs.wso2.org/wiki/download/attachments/4885658/insertquery.png?version=1&amp;modificationDate=1328229052000" width="450" /></p>
<p>Once the user selects <code>               Re               turn               Generate               d               Keys              </code> option, an auto increment key is added as an output mapping as follows:</p>
<p><img src="http://docs.wso2.org/wiki/download/attachments/4885658/returngenkeys2.png?version=1&amp;modificationDate=1328229177000" width="650" /></p>
</div></td>
</tr>
<tr class="even">
<td>Returning Updated Row Count</td>
<td><div class="content-wrapper">
<p>With the current data services functionality, we don't have a way to indicate that the update operation did not affect any rows. But, we can return the updated row count as a response to the client in queries like update/insert to indicate how may rows are affected by the query execution.</p>
<p><img src="attachments/119130823/119130826.png" /></p>
</div></td>
</tr>
</tbody>
</table>

### Output parameters

Just as Input mapping allows you to add parameters to a query, output
mapping determines how the output of a query should be presented. Use
this section to specify how the result of the query should be presented.
You can choose XML, JSON, or RDF as the format of the result, along with
the parameters that should be used to represent the data.

| Parameter   | Description                                                                                                                |
|-------------|----------------------------------------------------------------------------------------------------------------------------|
| Output Type | The output type determines the format in which the query output will be presented. You can select either XML, RDF or JSON. |

The following parameters are configurable for XML/RDF output types.

<table style="width:100%;">
<colgroup>
<col style="width: 5%" />
<col style="width: 94%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Generate Output Mapping</td>
<td><div class="content-wrapper">
!!! note
<p>Note that this option is only available for <code>               SELECT              </code> statements excluding <code>               SELECT *              </code> , and for datasources such as RDBMS.</p>

<p>If you have defined an SQL query, you can generate output mappings corresponding to the fields specified in the query by clicking <strong>Generate Response</strong> . In the example shown below, there is an SQL query that needs to output values for the <code>               customernumber              </code> and <code>               customername              </code> fields in the <code>               customers              </code> table.</p>
<p><br />
</p>
</div></td>
</tr>
<tr class="even">
<td>Use column numbers</td>
<td><div class="content-wrapper">
<p>If this option is selected the mapping will be done by the column number basis instead of the column name. The following screenshot provides an example for using column numbers</p>
<p><img src="attachments/119130823/119130827.png" /></p>
</div></td>
</tr>
<tr class="odd">
<td>Escape non-printable characters</td>
<td>Tick this option if the data in your database consists of characters that are not serializable to XML. Few examples are &amp; &lt; &gt; " '. When you invoke services that access such data and produce responses, the sever throws errors. Ticking this option ensures that non-printable characters will be ignored when producing the responses.</td>
</tr>
<tr class="even">
<td>Row Namespace</td>
<td>See <a href="https://docs.wso2.com/display/EI611/Using+Namespaces+in+Data+Services">Defining Namespaces</a> .</td>
</tr>
<tr class="odd">
<td>Query Result Export</td>
<td><div class="content-wrapper">
<p>When you click <strong>Add New Output Mapping</strong> in the <strong>Result(Output Mapping)</strong> section of the <strong>Queries</strong> page as explained above, the <strong>Edit Output Mapping</strong> page will open. You can specify the type of fields that will present the output of your query by giving the data source type, output field name, data source column name etc.<br />
</p>
<p>In the <strong>Edit Output Mapping</strong> page, you can define query result export options. Query Request Export feature must be used in conjunction with request box. It allows individual queries executed in a request r to communicate with each other. The concept is 'exporting' a specific result element so that the next calling query will get that result element as a query parameter. So, if you've two queries, namely, 'query1' and 'query2' that's executed sequentially in a request box, and if 'query1' has a specific result element and that element is exported with the name 'foo', then 'query2' also gets a query param named 'foo'. So when this request box session is executed, the query1's exported value will be passed into query2 as an input parameter.</p>
<p>This feature is very useful in situations where the result of an earlier-executed query is required for the execution of a subsequent query (e.g. a newly created primay key).<br />
</p>
<p>The following figure shows how a result element can be declared to be exported with a given name when defining a query in a data service.</p>
<p><img src="attachments/119130823/119130828.png" width="500" /></p>
<p>There are two export types that can be used.<br />
</p>
<ul>
<li><strong>SCALAR</strong> : The single element value is exported. If there are multiple instances of this value, the last one will be exported.</li>
<li><strong>ARRAY</strong> : An array of values will be exported. Each occurrence of the value is added to an array and exported.<br />
</li>
</ul>
<p>For a demonstration on the usage of export options, refer to request box sample .</p>
</div></td>
</tr>
</tbody>
</table>

### Events

This section can be used to trigger notifications from the query.

### Advanced query parameters

Advanced query properties help define additional features when querying
the database. This option is available when adding queries to
datasources such as RDBMS.

![](http://docs.wso2.org/wiki/download/attachments/4886256/advanced_query_prop.png?version=1&modificationDate=1329142182000){width="450"}

Query property details are described below.

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 83%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Property Name</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Timeout</p></td>
<td><p>Sets a timeout for the underlying JDBC query.</p></td>
</tr>
<tr class="even">
<td><p>Fetch Direction</p></td>
<td><p>Forward - rows in a result set will be processed in a forward direction; first-to-last.<br />
Backward - the ro ws in a result set will be processed in reverse direction; last-to-first.</p></td>
</tr>
<tr class="odd">
<td><p>Fetch Size</p></td>
<td><p>The number of rows that should be fetched from the database when more rows are needed. If the fetch size is zero, the JDBC driver ignores the value and is free to make its own best guess as to what the fetch size should be. Note that the fetch size is set to a lower value in the ESB profile of WSO2 EI by default. However, if you expect a very large number of rows to be fetched, you should increase the fetch size accordingly (e.g. 1000) to improve performance.</p></td>
</tr>
<tr class="even">
<td><p>Max Field Size</p></td>
<td><p>Maximum data size for the field.<br />
Used to reduce the size each field takes in order to eliminate the possibility of hitting a db limit.</p></td>
</tr>
<tr class="odd">
<td><p>Max Rows</p></td>
<td><p>Maximum number of rows to be returned. Zero means all rows.</p></td>
</tr>
<tr class="even">
<td><p>Force Stored Procedure</p></td>
<td><p>Forces the current SQl statement as a stored procedure.</p></td>
</tr>
<tr class="odd">
<td><p>Force JDBC Batch Requests</p></td>
<td><p>Forces to use native JDBC batch request.</p></td>
</tr>
</tbody>
</table>
