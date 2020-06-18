# Enabling Transaction Counter

A **Transaction** in WSO2 Micro Integrator is defined as an inbound request coming to the server. According to this definition, any inbound request to an API, Proxy service, Inbound Endpoint will be considered as one transaction. 
However, there will be an exception for scenarios where the WSO2 MI is configured to work as both a JMS producer and a JMS consumer by which the message flow gets asynchronous in the middle. In these cases, the server treats 
both sending and listening requests as a single transaction.

If you need to track the number of transactions that come to your WSO2 Micro Integrator deployment, you can enable the transaction counter component in each WSO2 Micro Integrator server instances. Currently, the transaction count handler is responsible for counting all the requests coming via passthru, jms transports and persist the summary of the transaction count in a database for future usage.

The following section walks you through the steps to enable the transaction counter for WSO2 Micro Integrator.

## Prerequisites
In order to persist the transaction count, be sure to configure any relational database as a datasource for the Micro Integrator. You can add 
the following datasource configuration and update the values for your database. 
 
```toml
[[datasource]]
id = "WSO2_TRANSACTION_DB"
url= "jdbc:mysql://localhost:3306/transactionSummaryDB"
username="username"
password="password"
driver="com.mysql.jdbc.Driver"
```
See the complete list of [database connection parameters](../../references/config-catalog/#database-connection) and their descriptions. 

!!! Note
If you have already configured a datasource for the Micro Integrator, you can skip configuring a new datasource for transaction counter component 
and use the existing datasource to store the transaction count summary.

## Configuring Transaction Counter

To configure the Transaction Counter, add the parameters given below to the deployment.toml file and update the values.

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
				This paramter is used for enabling Transaction Counter. Default value if false
			</td>
		</tr>
		<tr>
			<td>
				<code>data_source</code>
			</td>
			<td>
				Id of the datasource. This refers to the datasource id confgiured under datasource configurations.
			</td>
		</tr>
		<tr>
			<td>
				<code>update_interval</code>
			</td>
			<td>
				Transaction count is stored in the database with an interval (specified by this parameter which will be taken as the number of minutes) between the insert queries. Default update interval is one minute.
			</td>
		</tr>
	</table>

## Retrieve transaction count

The summary of the transaction count can be retrieved directly via the CLI or using the management API of WSO2 Micro Integrator. 
Please see the [CLI commands for transaction count retrieval](../../administer-and-observe/using-the-command-line-interface.md) for available CLI commands and 
see the [API resources for transaction count retrieval](../../administer-and-observe/working-with-management-api/#get-transaction-count) for available commands in Management API and how to access them. 


## Generate transaction count report

A report for the transaction count can be also generated via the CLI or using the management API of WSO2 Micro Integrator. 
Please see the [CLI commands for transaction count report generation](../../administer-and-observe/using-the-command-line-interface.md) for available CLI commands 
and [API resources for transaction count report generation](../../administer-and-observe/working-with-management-api/#get-transaction-count) for available commands in Management API and how to access them. 
