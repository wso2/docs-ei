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

See the documentation of your LDAP provider for instructions on setting up the LDAP, and for managing users and roles.

!!! Note
	The current release of the Micro Integrator does not offer user management functionality. Therefore, you must manage users and roles from your LDAP and then [connect it to the Micro Integrator](#step-2-connecting-to-the-ldap).

### Step 2: Connecting to the LDAP 

Follow the steps given below to connect the Micro Integrator to the LDAP user store.

!!! Note
	The following configuration defines **read-only** access to the LDAP from the Micro Integrator. The Micro Integrator does not require write access since it will not manage the user data in the LDAP.

1.	Open the `deployment.toml` file stored in the `<MI_HOME>/conf/` directory.
2.	Add the following configurations and update the required values. 

	```toml
	[user_store]
	connection_url = "ldap://localhost:10389"  
	connection_name = "uid=admin,ou=system" 
	connection_password = "admin"  
	user_search_base = "ou=system"   
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
				The username used to connect to the user store and perform various operations. This user does not need to be an administrator in the user store. However, the user requires permission to read the user list and user attributes, and to perform search operations on the user store. The value you specify is used as the DN (Distinguish Name) attribute of the user who has sufficient permissions to perform operations on users and roles in LDAP.
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
	</table>

See the [complete list of parameters](../../../references/config-catalog/#ldap-user-store) you can configure for the ldap user store.

## Configuring an RDBMS user store (Optional)

If you are already using a JDBC user store (database) with another WSO2 product (WSO2 API Manager or WSO2 Identity Server), you can connect the same database to the Micro Integrator of WSO2 Enterprise Integrator 7 as explained below.

!!! Warning
	You cannot manage users and roles when you use a JDBC user store with the Micro Integrator. Therefore, be sure that your database is already up-to-date before connecting it to the Micro Integrator. Alternatively, you can shift to an [LDAP user store](#configuring-an-ldap-user-store).

1.	Open the `deployment.toml` file (stored in the `<MI_HOME>/conf` directory).
2.	Add the following datasource configuration and update the values for your database.

	```toml
	[[datasource]]
	id = "WSO2_CARBON_DB"
	url= "jdbc:h2:./repository/database/WSO2CARBON_DB;DB_CLOSE_ON_EXIT=FALSE;LOCK_TIMEOUT=60000"
	username="username"
	password="password"
	driver="org.h2.Driver"
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
				The name given to the datasource.
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
				The username used to connect to the user store and perform various operations. This user does not need to be an administrator in the user store. However, the user requires permission to read the user list and user attributes, and to perform search operations on the user store.
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

3.	Add the JDBC user store manager under the `[user_store]` toml heading as shown below.

	```toml
	[user_store]
	class = "org.wso2.micro.integrator.security.user.core.jdbc.JDBCUserStoreManager"
	```
	The datasource configured under the `[[datasource]]` toml heading will now be the effective user store for the Micro Integrator.
