# Configuring a User Store

An external user store (such as an LDAP or RDBMS) can be used with the Micro Integrator for the following two scenarios:

-	When [WS-Security](../../../references/security/security-implementation) is enabled for your integration artifacts, the user store will be used for authenticating the credentials of users invoking the artifacts.

	See the following resources on how to enable WS security for integration artifacts:

	-	[Securing a proxy service](../../../develop/advanced-development/applying-security-to-a-proxy-service)
	-	[Securing a data service](../../../develop/creating-artifacts/data-services/securing-data-services)
	-	[Securing a REST API](../../../develop/advanced-development/applying-security-to-an-api)

-	Optionally, you can use the external user store for [securing the management API](../../../setup/security/securing_management_api). By default, the management API uses a file-based registry.

## Configuring an LDAP user store

An LDAP user store is recommended for the Micro Integrator. Follow the instruction given below.

### Step 1: Setting up an LDAP

See the documentation of your LDAP provider for instructions on setting up the LDAP.

### Step 2: Connecting to the LDAP

Follow the steps given below to connect the Micro Integrator to the LDAP user store.

1.	Open the `deployment.toml` file stored in the `<MI_HOME>/conf/` directory.
2.	Add the following configuration to disable the default file-based user store:

	```toml
	[internal_apis.file_user_store]
	enable = false
	```

3.	Add the following configurations and update the required values.

	```toml
	[user_store]
	connection_url = "ldap://localhost:10389"  
	connection_name = "uid=admin,ou=system"
	connection_password = "admin"  
	user_search_base = "ou=system" 
	type = "read_only_ldap"
	```

	Parameters used above are explained below.

	<table>
		<tr>
			<th>Parameter</th>
			<th>Value</th>
		</tr>
		<tr>
			<td>
				<code>connection_url</code>
			</td>
			<td>
				The URL for connecting to the LDAP. If you are connecting over ldaps (secured LDAP), you need to import the certificate of the user store to the truststore (wso2truststore.jks by default). See the instructions on how to <a href="../../../setup/security/importing_ssl_certificate">add certificates to the truststore</a>.
			</td>
		</tr>
		<tr>
			<td>
				<code>connection_name</code>
			</td>
			<td>
				The username used to connect to the user store and perform various operations. This user needs to be an administrator in the user store. That is, the user requires write permission to manage add, modify users and to perform search operations on the user store. The value you specify is used as the DN (Distinguish Name) attribute of the user who has sufficient permissions to perform operations on users and roles in LDAP.
			</td>
		</tr>
		<tr>
			<td>
				<code>connection_password</code>
			</td>
			<td>
				Password for the connection user name.
			</td>
		</tr>
		<tr>
			<td>
				<code>user_search_base</code>
			</td>
			<td>
				The DN of the context or object under which the user entries are stored in the user store. When the user store searches for users, it will start from this location of the directory.
			</td>
		</tr>
		<tr>
			<td>
				<code>type</code>
			</td>
			<td>
				Use one of the following values. </br></br>
				<b>read_only_ldap</b>: The LDAP connection does not provide write access.</br>
				<b>read_write_ldap</b>: The LDAP connection provides write access.
			</td>
		</tr>
	</table>

See the [complete list of parameters](../../../references/config-catalog/#ldap-user-store) you can configure for the ldap user store.

## Configuring an RDBMS user store

1.	Set up an RDBMS. You can use one of the following types.

	!!! Note
			Be sure to use the relevant database script stored in the `<MI_HOME>/dbscripts/` directory when you create the database.

		- [Setting up a MySQL database](../../../setup/deployment/databases/setting-up-MySQL)
		- [Setting up an MSSQL database](../../../setup/deployment/databases/setting-up-MSSQL)
		- [Setting up an Oracle database](../../../setup/deployment/databases/setting-up-Oracle)
		- [Setting up a Postgre database](../../../setup/deployment/databases/setting-up-PostgreSQL)
		- [Setting up an IBM database](../../../setup/deployment/databases/setting-up-IBM-DB2)

2.	Open the `deployment.toml` file (stored in the `<MI_HOME>/conf` directory).
3.	Add the following configuration to disable the default file-based user store:

	```toml
	[internal_apis.file_user_store]
	enable = false
	```

4.	Add the relevant datasource configuration and update the values for your database.

	!!! Tip
			If you are already using a JDBC user store (database) with another WSO2 product ([WSO2 API Manager](https://wso2.com/api-management/), [WSO2 Identity Server](https://wso2.com/identity-and-access-management/), or an instance of [WSO2 Enterprise Integrator 6.x.x](https://wso2.com/enterprise-integrator/6.6.0)), you can connect the same database to the Micro Integrator of WSO2 Enterprise Integrator 7 as explained below.

	```toml tab='MySQL'
	[[datasource]]
	id = "WSO2CarbonDB"
	url= "jdbc:mysql://localhost:3306/userdb"
	username="root"
	password="root"
	driver="com.mysql.jdbc.Driver"
	pool_options.maxActive=50
	pool_options.maxWait = 60000
	pool_options.testOnBorrow = true
	```

	```toml tab='MSSQL'
	[[datasource]]
	id = "WSO2CarbonDB"
	url= "jdbc:sqlserver://<IP>:1433;databaseName=userdb;SendStringParametersAsUnicode=false"
	username="root"
	password="root"
	driver="com.microsoft.sqlserver.jdbc.SQLServerDriver"
	pool_options.maxActive=50
	pool_options.maxWait = 60000
	pool_options.testOnBorrow = true
	```

	```toml tab='Oracle'
	[[datasource]]
	id = "WSO2CarbonDB"
	url= "jdbc:oracle:thin:@SERVER_NAME:PORT/SID"
	username="root"
	password="root"
	driver="oracle.jdbc.OracleDriver"
	pool_options.maxActive=50
	pool_options.maxWait = 60000
	pool_options.testOnBorrow = true
	```

	```toml tab='PostgreSQL'
	[[datasource]]
	id = "WSO2CarbonDB"
	url= "jdbc:postgresql://localhost:5432/userdb"
	username="root"
	password="root"
	driver="org.postgresql.Driver"
	pool_options.maxActive=50
	pool_options.maxWait = 60000
	pool_options.testOnBorrow = true
	```

	```toml tab='IBM DB'
	[[datasource]]
	id = "WSO2CarbonDB"
	url="jdbc:db2://SERVER_NAME:PORT/userdb"
	username="root"
	password="root"
	driver="com.ibm.db2.jcc.DB2Driver"
	pool_options.maxActive=50
	pool_options.maxWait = 60000
	pool_options.testOnBorrow = true
	```

	Parameters used above are explained below.

	<table>
		<tr>
			<th>Parameter</th>
			<th>Value</th>
		</tr>
		<tr>
			<td>
				<code>id</code>
			</td>
			<td>
				The name given to the datasource. This is required to be <b>WSO2CarbonDB</b>.</br></br>
				<b>Note</b>: If you replace 'WSO2CarbonDB' with a different id, you also need to list the id as a datasource under the <code>[realm_manager]</code> section in the <code>deployment.toml</code> file as shown below.
				<div>
					<code>
					[realm_manager]</br>
					data_source = "new_id"
					</code>
				</div>
				Otherwise the user store database id defaults to 'WSO2CarbonDB' in the realm manager configurations.
			</td>
		</tr>
		<tr>
			<td>
				<code>url</code>
			</td>
			<td>
				The URL for connecting to the database. The type of database is determined by the URL string.</a>.
			</td>
		</tr>
		<tr>
			<td>
				<code>username</code>
			</td>
			<td>
				The username used to connect to the user store and perform various operations. This user needs to be an administrator in the user store. That is, the user requires write permission to manage add, modify users and to perform search operations on the user store.
			</td>
		</tr>
		<tr>
			<td>
				<code>password</code>
			</td>
			<td>
				Password for the connection user name.
			</td>
		</tr>
		<tr>
			<td>
				<code>driver</code>
			</td>
			<td>
				The driver class specific to the JDBC user store.
			</td>
		</tr>
	</table>

	See the complete list of [database connection parameters](../../../references/config-catalog/#database-connection) and their descriptions. Also, see the recommendations for [tuning the JDBC connection pool](../../../setup/performance_tuning/jdbc_tuning).

5.	Add the JDBC user store manager under the `[user_store]` toml heading as shown below.

	!!! Tip
			If you want to be able to modify the data in your user store, be sure to enable write access to the user store.

	```toml
	[user_store]
	class = "org.wso2.micro.integrator.security.user.core.jdbc.JDBCUserStoreManager"
	type = "database"

	# Add the following parameter only if you want to disable write access to the user store.
	read_only = true
	```
	The datasource configured under the `[[datasource]]` toml heading will now be the effective user store for the Micro Integrator. 
   

## What's next?

See [Managing Users](../managing_users) for instructions on adding, deleting, or viewing users in the user store.
