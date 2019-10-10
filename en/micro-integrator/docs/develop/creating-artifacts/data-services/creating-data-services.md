# Creating a Data Service
## Instructions
### Creating the datasource connection

Follow the steps given below to create the data service file:

1.  Select the already created **Data Service Project** in the project
    navigator, right click and go to **New -> Data Service**.  
    The **New Data Service** window will open as shown below.  
    ![](/assets/img/tutorials/data_services/119130577/119130578.png)
2.  To start creating a data service from scratch, select **Create New
    Data Service** and click **Next**.
3.  Enter a name for the data service.
4.  Click **Next** and start adding the datasource connection details.
5.  Save the data service.

A data service file (DBS file) will now be created in your data service
project. Shown below is the project directory.

![](/assets/img/tutorials/data_services/119130577/119130593.png)

### Creating a query

1.  Select the data service you created in the previous step.
2.  Right-click and click **Add Query** .  
    ![](/assets/img/tutorials/data_services/119130577/119130591.png)
3.  Enter the query details:
4.  Save the query. The query element is now added to the data
    service:  
    ![](/assets/img/tutorials/data_services/119130577/119130590.png)

#### Add an SQL

1.  Right-click the query and click **Add SQL** to add the statement.
2.  Save the SQL statement.

#### Add Input mapping

1.  Right-click the query again and click **Add Input Mapping**.
2.  Enter the input mapping details:
3.  Save the input mapping.

#### Add Output mapping
1. Right-click the query again and click **Add Output Mapping**.
2. Enter the following value to group the output mapping:
3. Save the output mapping.

#### Add Output mapping element
1. Right-click the output mapping and go to **Add Output Mapping → Add  Element** to create an element.
2. Enter the following element details.
3. Save the element.
4. Save the output elements.

The data service should now have the query element added as shown below.

![](/assets/img/tutorials/data_services/119130577/119130589.png)

### Creating a resource to invoke the query

Now, let's create a REST resource that can be used to invoke the query.

1.  Right-click the data service and click **Add Resource**. Add the following resource details.
2.  Expand the GET resource, and click the **GetEmployeeDetails (call-query)**. Connect the query to the resource by adding the following:
3.  Save the resource.

The data service should now have the resource added as shown below.

![](/assets/img/tutorials/data_services/119130577/119130588.png)

## Examples

<ul>
	<li>
		<a href="../../../../use-cases/examples/data_integration/rdbms-data-service">Exposing an RDBMS Datasource</a>
	</li>
	<li>
		<a href="../../../../use-cases/examples/data_integration/json-with-data-service">Exposing Data in JSON Format</a>
	</li>
	<li>
		<a href="../../../../use-cases/examples/data_integration/odata-service">Using and OData Service</a>
	</li>
	<li>
		<a href="../../../../use-cases/examples/data_integration/nested-queries-in-data-service">Using Nested Data Queries</a>
	</li>
	<li>
		<a href="../../../../use-cases/examples/data_integration/batch-requesting">Batch Requesting</a>
	</li>
	<li>
		<a href="../../../../use-cases/examples/data_integration/request-box">Invoking Multiple Operations via Request Box</a>
	</li>
	<li>
		<a href="../../../../use-cases/examples/data_integration/distributed-trans-data-service">Using Distributed Transactions in Data Services</a>
	</li>
	<li>
		<a href="../../../../use-cases/examples/data_integration/data-service-notifications">Receiving Notifications from Data Services</a>
	</li>
	<!--
	<li>
		<a href="../../use-cases/examples/data_integration/data-input-validator">Validating Data Input</a>
	</li>
	<li>
		<a href="../../use-cases/examples/data_integration/data-service-scheduled-task">Scheduling Tasks in Data Services</a>
	</li>
-->
</ul>

## Tutorials

<li>
	See the tutorial on <a href="../../../../use-cases/tutorials/sending-a-simple-message-to-a-datasource">data integration</a>
</li>
