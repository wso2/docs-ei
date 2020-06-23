# Enabling Transaction Counter

A **Transaction** in WSO2 Micro Integrator is defined as an inbound request (a request coming to the server). According to this definition, any inbound request to a REST API, Proxy service, or Inbound Endpoint is considered as one transaction. 
However, there will be an exception for scenarios where the Micro Integrator is configured as both a JMS producer and a JMS consumer by which the message flow gets asynchronous in the middle. In these cases, the Micro Integrator considers
both the sending and listening requests as a single transaction.

If you need to track the number of transactions that come to your Micro Integrator deployment, you can enable the transaction counter component in each Micro Integrator instance of your deployment. Currently, the transaction counter is responsible for counting all requests received via the Passthru and JMS transports and for persisting the summary of the transaction count in a database for future use.

Follow the instructions given below to enable the transaction counter for WSO2 Micro Integrator.

## Prerequisites

To persist the transaction count, you need a relational database connected to the Micro Integrator. 

1.	Create an RDBMS.

	!!! Note
	    If you already have a datasource configured for the Micro Integrator, you can skip configuring a new datasource for transaction counter component and use the existing datasource to store the transaction count summary.
	    
2.	Use the database script stored in the `<MI_HOME>/dbscripts` folder and run it against your RDBMS to create the required tables.
3.	Open the `deployment.toml` file (stored in the `<MI_HOME>/conf` folder), add the following datasource configuration and update the values for your database. 
 
	```toml
	[[datasource]]
	id = "WSO2_TRANSACTION_DB"
	url= "jdbc:mysql://localhost:3306/transactionSummaryDB"
	username="username"
	password="password"
	driver="com.mysql.jdbc.Driver"
	```
	
	See the complete list of [database connection parameters](../../references/config-catalog/#database-connection) and their descriptions. 

## Configuring Transaction Counter

To configure the Transaction Counter, add the parameters given below to the `deployment.toml` file and update the values.

```toml
[transaction_counter]
enable = true
data_source = "WSO2_TRANSACTION_DB"
update_interval = 2
```

Parameters used above are explained below.

<table>
	<tr>
		<th>Parameter</th>
		<th>Value</th>
	</tr>
	<tr>
		<td>
			<code>enable</code>
		</td>
		<td>
			This paramter is used for enabling the Transaction Counter. Default value if 'false'.
		</td>
	</tr>
	<tr>
		<td>
			<code>data_source</code>
		</td>
		<td>
			The ID of the datasource. This refers the datasource ID confgiured under the datasource configuration.
		</td>
	</tr>
	<tr>
		<td>
			<code>update_interval</code>
		</td>
		<td>
			The transaction count is stored in the database with an interval (specified by this parameter, which will be taken as the number of minutes) between the insert queries. The default update interval is one minute.
		</td>
	</tr>
</table>

## Retrieve transaction count

The summary of the transaction count can be retrieved directly via the CLI or the management API of WSO2 Micro Integrator. 

-	See the [CLI commands for transaction count retrieval](../../administer-and-observe/using-the-command-line-interface).
-	See the [API resources for transaction count retrieval](../../administer-and-observe/working-with-management-api/#get-transaction-count).

## Generate transaction count report

A report for the transaction count can be also generated via the CLI or the management API of WSO2 Micro Integrator. 

-	See the [CLI commands for transaction count report generation](../../administer-and-observe/using-the-command-line-interface.md).
-	See the [API resources for transaction count report generation](../../administer-and-observe/working-with-management-api/#get-transaction-count). 
