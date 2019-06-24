# Configuration Catalog

This document describes all the configuration parameters that are used in WSO2 API Manager. 

## Instructions for use

> Select the configuration sections, parameters, and values that are required for your use and add them to the .toml file. See the example .toml file given below.

```
## This is an example .toml file.

[deployment]             ##Config section.
pattern="value"          ##Parameter-value pair.
node_act_as="value"      ##Parameter-value pair.

[key_mgr_node]           ##Config section.
endpoints="value"        ##Parameter-value pair.

[gateway].               ##Config section.
gateway_environments=["dev","test"]   ##Parameter-value pair.

[[database]]               ##Config section
pool_options.maxActiv="5"

```

## Configuring the default deployment settings

<table>
	<tr>
		<th colspan="2"> Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[server]</code></b></td>
		<td>
			This toml header groups the parameters that define the server node details.
			<ul>
				<li><b>Default</b> Enabled with default settings.</li>
				<li><b>Mandatory</b> </li>
			</ul>			
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>hostname</code></b></td>
		<td>
			The hostname of the computer on which the product server is running.
			<ul>
				<li><b>Default</b> <code>"localhost"</code></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>node_ip</code></b></td>
		<td>
			The IP address of the computer on which the product server is running.
			<ul>
				<li><b>Default</b> <code>"127.0.0.1"</code></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>enable_mtom</code></b></td>
		<td>
			Use this paramater to enable MTOM (Message Transmission Optimization Mechanism) for the product server. Read more about MTOM messaging.
			<table id="table_inner">
				<tr>
					<td><b>Optional</b></td>
					<td><b>Default</b></br>MTOM is disabled.</td>
					<td><b>Possible Values</b></br><code>"true"</code> to enable MTOM.</td>
				</tr>
			</table>
			<!--
			<ul>
				<li><b>Default</b> MTOM is disabled.</li>
				<li><b>Possible Values</b>  Use <code>"true"</code> to enable MTOM.</li>
				<li><b>Optional</b></li>
			</ul>
			-->
		</td>
	</tr>
	<tr>
		<td><b><code>enable_swa</code></b></td>
		<td>
			Use this paramater to enable SwA (SOAP with Attachments) for the product server. When SwA is enabled, the ESB will process the files attached to SOAP messages. Read more about SwA messaging.
			<ul>
				<li><b>Default</b> SwA is disabled.</li>
				<li><b>Possible Values</b> Use "true" to enable SwA.</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
</table>

## Connecting to the primary data store

<table>
	<tr>
		<th colspan="2"> Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[database.shared_db]</code></b></td>
		<td>
			This config heading that groups the parameters connecting the server to the primary database. This database stores the user <b>permissions</b> that apply to user roles. This also stores the <b>users</b> and <b>roles</b> unless a separate user store is configured with the [user_store] config section. Read more about setting up databases for WSO2 EI.
			<ul>
				<li><b>Default</b> The embedded H2 database (stored in the EI_HOME/repository/database/ directory) is enabled.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>type</code></b></td>
		<td>
			The type of database that is used for the primary database. All the database types that are supported by this product is listed below as possible values.
			<table id="table_inner">
				<tr>
					<td><b>Optional</b></td>
					<td><b>Default</b></br><code>"H2"</code></td>
					<td><b>Possible Values</b></br></b> "H2" (not recommended for production environments), </br>"MySql", "Oracle", "Postgre"</td>
				</tr>
			</table>
			<!--
			<ul>
				<li><b>Default</b>: <code>"H2"</code></li>
				<li><b>Possible Values</b> "H2" (not recommended for production environments), </br>"MySql", "Oracle", "Postgre"</li>
				<li><b>Mandatory</b></li>
			</ul>
		-->
		</td>
	</tr>
	<tr>
		<td><b><code>url</code></b></td>
		<td>
			The connection URL of the database. Note that this is specific to the database type you are using.
			<ul>
				<li><b>Default</b> </li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>username</code></b></td>
		<td>
			The user name for connecting to the primary database.
			<ul>
				<li><b>Default</b> <code>"wso2carbon"</code></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>password</code></b></td>
		<td>
			The password for connecting to the database.
			<ul>
				<li><b>Default</b> <code>"wso2carbon"</code></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
</table>

## Tuning the primary data store connection

<table>
	<tr>
		<th colspan="2">Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[database.shared_db.pool_options]</code></b></td>
		<td>
			This config heading in the ei.toml file groups the performance tuning parameters of the primary database that you configured using the [database.shared_db] section. Read more about tuning database performance in WSO2 EI.
			<ul>
				<li><b>Default</b> Enabled with default settings.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>maxActive</code></b></td>
		<td>
			The maximum number of active connections that can be allocated at the same time from the connection pool. If there are too many active connections in the connection pool, some of them would be idle, which incurs an unnecessary system overhead.
			<ul>
				<li><b>Default</b> "50"</li>
				<li><b>Possible Values</b> Depends on environment. Enter any negative value to denote an unlimited number of active connections.</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>maxWait</code></b></td>
		<td>
			The maximum number of milliseconds that the pool will wait (when there are no available connections) for a connection to be returned before throwing an exception.
			<ul>
				<li><b>Default</b> "6000"</li>
				<li><b>Possible Values</b> Depends on environment. Enter zero or a negative value to wait indefinitely.</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>testOnBorrow</code></b></td>
		<td>
			Determines whether objects are validated before being borrowed from the pool. If the object fails to validate, it will be dropped from the pool, and another attempt will be made to borrow another.
			<ul>
				<li><b>Default</b> Enabled</li>
				<li><b>Possible Value</b> Use <code>"false"</code> to disable testOnBorrow.</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>validationInterval</code></b></td>
		<td>
			Connections are validated (at the most) at this frequency (time in milliseconds). If a connection is due for validation but has been validated previously within this interval, it will not be validated again. This avoids access validation.
			<ul>
				<li><b>Default</b> "30000"</li>
				<li><b>Possible Values</b> Depends on environment.</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>defaultAutoCommit</code></b></td>
		<td>
			Specifies whether each SQL statement should be automatically committed when the operation is completed. It is recommended to disable auto committing.
			<ul>
				<li><b>Default</b> Enabled</li>
				<li><b>Possible Values</b> Use "false" to disable default auto commit.</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
</table>

## Connecting to the user stores

<table>
	<tr>
		<th colspan="2"> Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[user_store]</code></b></td>
		<td>
			This config heading in the ei.toml connects the server to the user store. The user store holds the users and roles defined for the system. Read more about user management in WSO2 EI.
			<ul>
				<li><b>Default</b> The primary database is enabled as the user store.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>type</code></b></td>
		<td>
			The type of user store.
			<ul>
				<li><b>Default</b> <code>"database"</code></li>
				<li><b>Possible Values</b> </br> <code>"database"</code> refers the embedded H2 database with the settings defined under [database.shared_db], </br><code>"JDBC"</code> refers an external RDBMS. </br><code>"RW LDAP"</code> refers an external LDAP with both read/write access, </br><code>"RO LDAP"</code> refers an external LDAP with read only access, </br><code>"AD"</code> refers an active directory user store, </br><code>"Custom"</code> refers a custom user store implementation.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>connection_url</code></b></td>
		<td>
			The connection URL of the LDAP or AD user store.
			<ul>
				<li><b>Default</b> Not used.</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>connection_name</code></b></td>
		<td>
			The connection name of an LDAP/AD user store.
			<ul>
				<li><b>Default</b> Not used.</li>
				<li><b>Possible Values</b><code>"uid=admin,ou=wso2ei"</code></li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>connection_password</code></b></td>
		<td>
			The password for connecting to an LDAP/AD user store.
			<ul>
				<li><b>Default</b> Not used.</li>
				<li><b>Possible Values</b> <code>"$secret{ldap_password}"</code> is used to refer the .......</li>
				<li><b><code>Optional</code></b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>base_dn</code></b></td>
		<td>
			The ........
			<ul>
				<li><b>Default</b> Not used.</li>
				<li><b>Possible Values</b> <code>"dc=example,dc=com"</code></li>
			</ul>
		</td>
	</tr>
</table>

## Configuring the system administrator

<table>
	<tr>
		<th colspan="2">Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[super_admin]</code></b>:</td>
		<td>
			The config heading in the ei.toml file that groups the parameters defining the system adminsitrator.
			<ul>
				<li><b>Default</b> Enabled with the default parameters.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>username</code></b></td>
		<td>
			The user name of the system administrator. This user has all permissions enabled by default.
			<ul>
				<li><b>Default</b> <code>"admin"</code></li>
				<li><b>Possible Values</b> Any alpha numeric value.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>password</code></b></td>
		<td>
			The password of the system adminsitrator.
			<ul>
				<li><b>Default</b> <code>"admin"</code></li>
				<li><b>Possible Values</b> Any alpha numeric value.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>create_admin_account</code></b></td>
		<td>
			Sets whether the system administrator credentials should be created in the system at the time of starting the server.
			<ul>
				<li><b>Default</b> Enabled</li>
				<li><b>Possible Values</b> Use <code>"false"</code> to disable.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
</table>

## Configuring the keystore

<table>
	<tr>
		<th colspan="2">Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[keystore.tls]</code></b></td>
		<td>
			This config heading in the ei.toml file groups the parameters that connect the server to the keystore used for SSL handshaking when the server communicates with another server. All keystore files used by this product should be stored in the EI_HOME/repository/resources/security/ directory. The product is configured to use the default keystore (wso2carbon.jks), which contains the key pair that is used for all encryption/decryption purposes. Read more about asymetric encryption in WSO2 EI.
			<ul>
				<li><b>Default</b> Enabled with default parameters.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>file_name</code></b></td>
		<td>
			The name of the keystore file that is used for SSL communication.
			<ul>
				<li><b>Default</b> <code>"wso2carbon.jks"</code></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>type</code></b></td>
		<td>
			The type of the keystore file.
			<ul>
				<li><b>Default</b> <code>"JKS"</code></li>
				<li><b>Possible Values</b> <code>"JKS"</code> or <code>"PKCS12"</code></li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>password</code></b></td>
		<td>
			The password of the keystore file that is used for SSL communication. The keystore password is used when accessing the keys in the keystore.
			<ul>
				<li><b>Default</b> <code>"wso2carbon"</code></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>alias</code></b></td>
		<td>
			The alias of the public key corresponding to the private key that is included in the keystore. The public key is used for encrypting data in the ESB server, which only the corresponding private key can decrypt. The public key is embedded in a digital certificate, and this certificate can be shared over the internet by storing it in a separate trust store file. 
			<ul>
				<li><b>Default</b> <code>"wso2carbon"</code></li>
				<li><b>Mandatory</b></li>
			</ul> 
		</td>
	</tr>
	<tr>
		<td><b><code>key_password</code></b></td>
		<td>
			The password of the private key that is included in the keystore. The private key is used to decrypt the data that has been encrypted using the keystore's public key.
			<ul>
				<li><b>Default</b> <code>"wso2carbon"</code></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
</table>

## Configuring the truststore

<table>
	<tr>
		<th colspan="2">Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[truststore]</code></b></td>
		<td>
			Add this config heading to the ei.toml file to group the parameters that connect the server to the keystore file (trust store) that is used to store the digital certificates that the server trusts for SSL communication. All keystore files used by this product should be stored in the EI_HOME/repository/resources/security/ directory. The product is configured to use the default trust store (wso2truststore.jks), which contains the self-signed digital certificate of the default keystore. Read more about asymetric encryption in WSO2 EI.
			<ul>
				<li><b>Default</b> Enabled with default parameters.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>file_name</code></b></td>
		<td>
			The name of the keystore file that is used for storing the trusted digital certificates.
			<ul>
				<li><b>Default</b> <code>"wso2truststore.jks"</code></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>type</code></b></td>
		<td>
			The type of the keystore file that is used as the trust store.
			<ul>
				<li><b>Default</b> <code>"JKS"</code></li>
				<li><b>Possible Values</b> <code>"JKS"</code> or <code>"PKCS12"</code></li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>password</code></b></td>
		<td>
			The password of the keystore file that is used as the trust store.
			<ul>
				<li><b>Default</b> <code>"wso2carbon"</code></li>
				<li><b>Possible Values</b> See the content on generating keystores.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>alias</code></b></td>
		<td>
			The alias is the password of the digital certificate (which holds the public key) that is included in the trustore.
			<ul>
				<li><b>Default</b> <code>"symmetric.key.value"</code></li>
				<li><b>Possible Values</b> See the content on generating digital certificates.</li>
				<li><b>Mandatory</b></li>
			</ul> 
		</td>
	</tr>
	<tr>
		<td><b><code>algorithm</code></b></td>
		<td>
			The password of the ...
			<ul>
				<li><b>Default</b> <code>"AES"</code></li>
				<li><b>Possible Value</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
</table>

## Configuring the HTTP transport

<table>
	<tr>
		<th colspan="2">Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[transport.http]</code></b></td>
		<td>
			Add this config heading to the ei.toml file to group the parameters for configuring the HTTP/S transports in the product.
			<ul>
				<li><b>Default</b> Disabled. </li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>socket_timeout_millis</code></b></td>
		<td>
			................
			<ul>
				<li><b>Default</b> <code>"180000"</code></li>
				<li><b>Possible Values</b> </li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>listener.port</code></b></td>
		<td>
			The port on which this transport receiver should listen for incoming messages.
			<ul>
				<li><b>Default</b> <code>"8280"</code></li>
				<li><b>Possible Values</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>listener.wsdl_epr_prefix</code></b></td>
		<td>
			A URL prefix which will be added to all service EPRs and EPRs in WSDLs etc.
			<ul>
				<li><b>Default</b> <code>"https://apachehost:port/somepath"</code></li>
				<li><b>Possible Values</b> </li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>listener.secured_port</code></b></td>
		<td>
			The port on which this transport receiver should listen for incoming messages.
			<ul>
				<li><b>Default</b> <code>"8280"</code></li>
				<li><b>Possible Values</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>listener.secured_wsdl_epr_prefix</code></b></td>
		<td>
			A URL prefix which will be added to all service EPRs and EPRs in WSDLs etc.
			<ul>
				<li><b>Default</b> <code>"https://apachehost:port/somepath"</code></li>
				<li><b>Possible Values</b> </li>
				<li><b>Mandatory</b></li>
			</ul> 
		</td>
	</tr>
	<tr>
		<td><b><code>sender.warn_on_http_500</code></b></td>
		<td>
			........
			<ul>
				<li><b>Default</b> <code>"*"</code></li>
				<li><b>Possible Value</b> </li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.proxy_hostname</code></b></td>
		<td>
			If the outgoing messages should be sent through an HTTP proxy server, use this parameter to specify the target proxy.
			<ul>
				<li><b>Default</b> <code>"${deployement.node_ip}"</code></li>
				<li><b>Possible Value</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.proxy_port</code></b></td>
		<td>
			The port through which the target proxy accepts HTTP traffic.
			<ul>
				<li><b>Default</b> <code>"3128"</code></li>
				<li><b>Possible Value</b> </li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.non_proxy_hostnames</code></b></td>
		<td>
			The list of hosts to which the HTTP traffic should be sent directly without going through the proxy. When trying to add multiple hostnames along with an asterisk in order to define a set of sub-domains for non-proxy hosts, you need to add a period before the asterisk when configuring proxy server. ".*.abc.my.com|localhost"
			<ul>
				<li><b>Default</b> <code>"localhost|moon|sun"</code></li>
				<li><b>Possible Value</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>blocking_sender.enable_client_caching</code></b></td>
		<td>
			This parameter is used to specify whether the HTTP client should save cache entries and the cached responses in the JVM memory or not.
			<ul>
				<li><b>Default</b> Enabled</li>
				<li><b>Possible Value</b> Use <code>"false"</code> to disable. </li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>blocking_sender.transfer_encoding</code></b></td>
		<td>
			This parameter enables you to specify whether the data sent should be chunked. It can be used instead of the Content-Length header if you want to upload data without having to know the amount of data to be uploaded in advance.
			<ul>
				<li><b>Default</b> <code>"chunked"</code></li>
				<li><b>Possible Value</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>blocking_sender.default_connections_per_host</code></b></td>
		<td>
			The maximum number of connections that will be created per host server by the client. If the backend server is slow, the connections in use at a given time will take a long time to be released and added back to the connection pool. As a result, connections may not be available for some requests and you may get the org.apache.commons.httpclient.ConnectionPoolTimeoutException: Timeout waiting for connection error. In such situations, it is recommended to increase the value for this parameter.
			<ul>
				<li><b>Default</b> <code>"200"</code></li>
				<li><b>Possible Value</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>blocking_sender.omit_soap12_action</code></b></td>
		<td>
			If following is set to 'true', optional action part of the Content-Type will not be added to the SOAP 1.2 messages.
			<ul>
				<li><b>Default</b> Enabled</li>
				<li><b>Possible Value</b> Use <code>"fales"</code> to disable.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
</table>

## Configuring the Local transport

<table>
	<tr>
		<th colspan="2">Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[transport.local]</code></b></td>
		<td>
			This parameter is used to enable the listeners and senders when the ESB server communicates through the local transport.
			<ul>
				<li><b>Default</b> Disabled</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>listener.enable</code></b></td>
		<td>
			The parameter for enabling the local transport listener.
			<ul>
				<li><b>Default</b> Disabled </li>
				<li><b>Possible Values</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.enable</code></b></td>
		<td>
			The parameter for enabling the local transport sender.
			<ul>
				<li><b>Default</b> Disabled </li>
				<li><b>Possible Values</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
</table>

## Configuring the VFS transport

<table>
	<tr>
		<th colspan="2">Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[transport.vfs]</code></b></td>
		<td>
		Add this config heading to the ei.toml file to group the parameters that configure the ESB server to communicate through the VFS transport. Read more about file transfering in the ESB.
			<ul>
				<li><b>Default</b> Enabled</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>listener.enable</code></b></td>
		<td>
			The parameter for enabling the VFS listener in the ESB.
			<ul>
				<li><b>Default</b> Disabled</li>
				<li><b>Possible Values</b> Use <code>"true"</code> to enable.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.enable</code></b></td>
		<td>
			The parameter for enabling the VFS sender in the ESB.
			<ul>
				<li><b>Default</b> Disabled </li>
				<li><b>Possible Values</b>Use <code>"true"</code> to enable.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
</table>

## Configuring the MAIL transport

<table>
	<tr>
		<th colspan="2">Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[transport.mail]</code></b></td>
		<td>
			Add this config heading to the ei.toml file to group the parameters that configure the ESB server to communicate through the MAIL transport. Read more about using the MAIL transport.
			<ul>
				<li><b>Default</b> Disabled</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>listener.enable</code></b></td>
		<td>
			The parameter for enabling the MAIL transport listener in the ESB.
			<ul>
				<li><b>Default</b> Disabled</li>
				<li><b>Possible Values</b> Use <code>"true"</code> to enable.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.hostname</code></b></td>
		<td>
			.....
			<ul>
				<li><b>Default</b> <code>"smtp.gmail.com"</code> </li>
				<li><b>Possible Values</b> </li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.port</code></b></td>
		<td>
			......
			<ul>
				<li><b>Default</b> <code>587</code></li>
				<li><b>Possible Values</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.enable_tls</code></b></td>
		<td>
			Specifies whether TLS should be enabled.
			<ul>
				<li><b>Default</b> Enabled. </li>
				<li><b>Possible Values</b> Use <code>"true"</code> to enable.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.auth</code></b></td>
		<td>
			Specifies whether the user should be authenticated.
			<ul>
				<li><b>Default</b> Enabled. </li>
				<li><b>Possible Values</b> Use <code>"true"</code> to enable.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.username</code></b></td>
		<td>
			The user name of the email account (mail sender). Note that in some email service providers, the user name is the same as the email address specified for the 'From' parameter.
			<ul>
				<li><b>Default</b> <code>"demo_user"</code></li>
				<li><b>Possible Values</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.password</code></b></td>
		<td>
			The password of the email account (mail sender).
			<ul>
				<li><b>Default</b> <code>"mailpassword"</code> </li>
				<li><b>Possible Values</b> </li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.from</code></b></td>
		<td>
			The email address from which mails will be sent.
			<ul>
				<li><b>Default</b> <code>"demo_user@wso2.com"</code> </li>
				<li><b>Possible Values</b> </li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
</table>

## Configuring the FIX transport

<table>
	<tr>
		<th colspan="2">Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[transport.fix]</code></b></td>
		<td>
			Add this config heading to the ei.toml to group the parameters that configure the ESB server to communicate through the FIX transport. Read more about how the ESB uses FIX communication.
			<ul>
				<li><b>Default</b> Disabled</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>listener.enable</code></b></td>
		<td>
			The parameter for enabling the FIX listener in the ESB.
			<ul>
				<li><b>Default</b> Disabled</li>
				<li><b>Possible Values</b> Use <code>"true"</code> to enable.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.enable</code></b></td>
		<td>
			The parameter for enabling the FIX sender in the ESB.
			<ul>
				<li><b>Default</b> Disabled </li>
				<li><b>Possible Values</b> Use <code>"true"</code> to enable.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
</table>

## Configuring the HL7 transport

<table>
	<tr>
		<th colspan="2">Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[transport.hl7]</code></b></td>
		<td>
			Add this config heading to group the parameters that configure the ESB server communicate through the HL7 transport. Read more about HL7 communication in the ESB.
			<ul>
				<li><b>Default</b> Disabled</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>listener.enable</code></b></td>
		<td>
			The parameter for enabling the HL7 listener in the ESB.
			<ul>
				<li><b>Default</b> Disabled </li>
				<li><b>Possible Values</b> Use <code>"true"</code> to enable.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.enable</code></b></td>
		<td>
			The parameter for enabling the HL7 sender in the ESB.
			<ul>
				<li><b>Default</b> Disabled </li>
				<li><b>Possible Values</b> Use <code>"true"</code> to enable.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
</table>

## Configuring the SAP transport

<table>
	<tr>
		<th colspan="2">Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[transport.sap]</code></b></td>
		<td>
			Add this config heading to the ei.toml file to group the parameters that configure the ESB to communicate with a SAP system. Read more about SAP integration of the ESB.
			<ul>
				<li><b>Default</b> Disabled</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>listener.enable</code></b></td>
		<td>
			The parameter for enabling the SAP listener in the ESB.
			<ul>
				<li><b>Default</b> Disabled </li>
				<li><b>Possible Values</b> Use <code>"true"</code> to enable.</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>sender.enable</code></b></td>
		<td>
			The parameter for enabling the SAP sender in the ESB.
			<ul>
				<li><b>Default</b> Disabled </li>
				<li><b>Possible Values</b> Use <code>"true"</code> to enable.</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
</table>

## Configuring a custom transport

<table>
	<tr>
		<th colspan="2">Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[[custom.transport]]</code></b></td>
		<td>
			This config heading is used to group the parameters for a custom transport implementation that you want to use in your product.
			<ul>
				<li><b>Default</b> Disabled</li>
				<li><b>Optional</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>class</code></b></td>
		<td>
			The transport class implementation.
			<ul>
				<li><b>Default</b></li>
				<li><b>Possible Values</b> A qualified class name.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>protocol</code></b></td>
		<td>
			........
			<ul>
				<li><b>Default</b> <code>"ISO8583"</code> </li>
				<li><b>Possible Values</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>parameter.port</code></b></td>
		<td>
			........
			<ul>
				<li><b>Default</b> <code>"8081"</code> </li>
				<li><b>Possible Values</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>parameter.hostname</code></b></td>
		<td>
			........
			<ul>
				<li><b>Default</b> <code>"$conf{deployment.node_ip}"</code> </li>
				<li><b>Possible Values</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
</table>

## Configuring the mediation process

<table>
	<tr>
		<th colspan="2">Config Heading</th>
	</tr>
	<tr>
		<td style="width: 20%"><b><code>[mediation]</code></b></td>
		<td>
			Add this config heading to the ei.toml file to group the parameters for tuning the mediation process (Synapse engine) of the ESB. These parameters are mainly used when mediators such as Iterate and Clone (which leverage on internal thread pools) are used.
			<ul>
				<li><b>Default</b> Enabled with default parameters. </li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<th colspan="2">Parameters</th>
	</tr>
	<tr>
		<td><b><code>synapse.core_threads</code></b></td>
		<td>
			The initial number of synapse threads in the pool. This parameter is applicable only if the Iterate or the Clone mediator is used to handle a higher load. The number of threads specified for this parameter should be increased as required to balance an increased load.
			<ul>
				<li><b>Default</b> <code>"20"</code></li>
				<li><b>Possible Values</b> Depends on the environment.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>synapse.max_threads</code></b></td>
		<td>
			The maximum number of synapse threads in the pool. This parameter is applicable only if the Iterate or the Clone mediator is used to handle a higher load. The number of threads specified for this parameter should be increased as required to balance an increased load.
			<ul>
				<li><b>Default</b> <code>"100"</code></li>
				<li><b>Possible Values</b> Depends on the environment.</li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>synapse.threads_queue_length</code></b></td>
		<td>
			The length of the queue that is used to hold the runnable tasks to be executed by the pool. This parameter is applicable only if the Iterate or the Clone mediator is used to handle a higher load.
			<ul>
				<li><b>Default</b> <code>"10"</code></li>
				<li><b>Possible Values</b> Depends on the environment. </li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>synapse.global_timeout_interval_millis</code></b></td>
		<td>
			The maximum number of milliseconds within which a response for the request should be received. A response which arrives after the specified number of seconds cannot be correlated with the request. Hence, a warning all be logged and the request will be dropped. This parameter is also referred to as the time-out handler.
			<ul>
				<li><b>Default</b> <code>"120000"</code></li>
				<li><b>Possible Values</b> Depends on the environment. </li>
				<li><b>Mandatory</b></li>
			</ul> 
		</td>
	</tr>
	<tr>
		<td><b><code>synapse.preserve_namespace_on_xml_to_json</code></b></td>
		<td>
			...........
			<ul>
				<li><b>Default</b></li>
				<li><b>Possible Value</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>statistics.enable_clean</code></b></td>
		<td>
			...........
			<ul>
				<li><b>Default</b></li>
				<li><b>Possible Value</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b><code>statistics.clean_interval_millis</code></b></td>
		<td>
			...........
			<ul>
				<li><b>Default</b></li>
				<li><b>Possible Value</b></li>
				<li><b>Mandatory</b></li>
			</ul>
		</td>
	</tr>
</table>
