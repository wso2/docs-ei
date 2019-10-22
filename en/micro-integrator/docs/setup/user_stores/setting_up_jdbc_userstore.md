# Setting up a JDBC User Store

Follow the steps given below to set up a JDBC user store:

## Set up a database (user store)

1. Create a database.
2. Create the database tables for the user store by running the database scripts
   stored in the MI_HOME/dbscripts/ directory.
3. You need to add correct JDBC driver to <MI_HOME>/lib

## Connect the server to the user store

Open the deployment.toml file and apply  the following updates:

```java
[user_store]
class = "org.wso2.micro.integrator.security.user.core.jdbc.JDBCUserStoreManager"
tenant_manager  =  "org.wso2.carbon.user.core.tenant.JDBCTenantManager"
read_only  =  false
read_groups  =  true
write_groups  =  true
username_java_regex  =  "^[\\S]{3,30}$"
username_javascript_regex  =  "^[\\S]{3,30}$"
username_java_regex_violation_error_msg  =  "Username pattern policy violated"
password_java_regex  =  "^[\\S]{5,30}$"
password_javascript_regex  =  "^[\\S]{5,30}$"
password_java_regex_violation_error_msg  =  "Password length should be within 5 to 30 characters"
rolename_java_regex  =  "^[\\S]{3,30}$"
rolename_javascript_regex  =  "^[\\S]{3,30}$" 	
case_insensitive_username  =  true
scim_enabled  =  false
is_bulk_import_supported  =  true
password_digest  =  "SHA-256"
store_salted_password  =  true
multi_attribute_separator  = ","
max_user_name_list_length  =  100
max_role_name_list_length  =  100
user_roles_cache_enabled  =  true
user_name_unique_across_tenants  =  false

```
Find more parameters to configure this user store.

### Properties used in JDBC userstore manager

Following are the properties used in JDBC user store manager:

<table>
<thead>
<tr class="header">
<th>Property Name</th>
<th>Display Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td>url</td>
<td>Connection URL</td>
<td>Connection URL to the database which can include additional connection parameters as well<br />
Sample values: jdbc: <a href="mysql://localhost:3306/db_name">mysql://localhost:3306/db_name</a></td>
</tr>
<tr class="odd">
<td>username</td>
<td>Connection Name</td>
<td>The username used to connect to database and perform various operations. This user does not have to be an administrator in the database or have an administrator role in the WSO2 product that you are using, but this user MUST have privileges to do required operation.</td>
</tr>
<tr class="even">
<td>password</td>
<td>Connection Password</td>
<td>Password for the ConnectionName user.</td>
</tr>
<tr class="odd">
<td>driverName</td>
<td>Driver Name</td>
<td>JDBC driver name which used to connect to the database. This driver should be available in the &lt;PRODUCT_HOME&gt;/lib folder.</td>
</tr>
<tr class="odd">
<td>read_only </td>
<td>Read-Only</td>
<td>Indicates whether user store operates in the read-only mode or not.<br />
Possible values:<br />
true: Operates in read-only mode<br />
false: Operates in read-write mode</td>
</tr>
<tr class="even">
<td>read_groups</td>
<td>ReadGroups</td>
<td>When WriteGroups is set to false, it Indicates whether groups should be read from the user store. If this is disabled by setting it to false, none of the groups in the user store can be read, and the following group configurations are NOT mandatory: GroupSearchBase, GroupNameListFilter, or GroupNameAttribute.<br />
<br />
Possible values:<br />
true: Read groups from user store<br />
false: Do not read groups from user store</td>
</tr>
<tr class="odd">
<td>write_groups</td>
<td>WriteGroups</td>
<td>Indicates whether groups should be written to the user store.<br />
<br />
Possible values:<br />
true : Write groups to user store<br />
false : Do not write groups to user store, so only internal roles can be created. Depending on the value of ReadGroups property, it will read existing groups from user store or not</td>
</tr>
<tr class="even">
<td>username_java_regex  </td>
<td>Username RegEx (Java)</td>
<td>The regular expression used by the back-end components for username validation. By default, strings with non-empty characters have a length of 3 to 30 are allowed. You can provide ranges of alphabets, numbers and also ranges of ASCII values in the RegEx properties.<br />
Default: [a-zA-Z0-9._-|//]{3,30}$</td>
</tr>
<tr class="odd">
<td>username_javascript_regex</td>
<td>Username RegEx (Javascript)</td>
<td>The regular expression used by the front-end components for username validation. Default: ^[\S]{3,30}$</td>
</tr>
<tr class="even">
<td>username_java_regex_violation_error_msg</td>
<td>Username RegEx Violation Error Message</td>
<td>Error message when the Username is not matched with username_java_regex </td>
</tr>
<tr class="odd">
<td>password_java_regex</td>
<td>Password RegEx (Java)</td>
<td>The regular expression used by the back-end components for password validation. By default, strings with non-empty characters have a length of 5 to 30 are allowed. You can provide ranges of alphabets, numbers and also ranges of ASCII values in the RegEx properties.<br />
Default: ^[\S]{5,30}$</td>
</tr>
<tr class="even">
<td>password_java_regex  </td>
<td>Password RegEx (Javascript)</td>
<td>The regular expression used by the front-end components for password validation.<br />
Default: ^[\S]{5,30}$</td>
</tr>
<tr class="odd">
<td>password_java_regex_violation_error_msg</td>
<td>Password RegEx Violation Error Message</td>
<td>Error message when the Password is not matched with passwordJavaRegEx</td>
</tr>
<tr class="even">
<td>rolename_java_regex</td>
<td>Role Name RegEx (Java)</td>
<td>The regular expression used by the back-end components for role name validation. By default, strings with non-empty characters have a length of 3 to 30 are allowed. You can provide ranges of alphabets, numbers and also ranges of ASCII values in the RegEx properties.<br />
Default: [a-zA-Z0-9._-|//]{3,30}$</td>
</tr>
<tr class="odd">
<td>rolename_javascript_regex</td>
<td>Role Name RegEx (Javascript)</td>
<td>The regular expression used by the front-end components for role name validation. Default: ^[\S]{3,30}$</td>
</tr>
<tr class="even">
<td>case_insensitive_username</td>
<td>Case Insensitive Username</td>
<td><p>Indicates whether the user name should be case insensitive or not.<br />
Default: false<br />
<br />
Possible values:<br />
true: If you are not using case-sensitive usernames better to configure this. Please note that enabling this could lead to performance degradation when searching for users as the number of users increases.</p></td>
</tr>
<tr class="odd">
<td>scim_enabled </td>
<td>Enable SCIM</td>
<td>This is to configure whether user store is supported for SCIM provisioning.<br />
<br />
Possible values:<br />
True : User store support for SCIM provisioning.<br />
False : User does not store support for SCIM provisioning.</td>
</tr>
<tr class="even">
<td>is_bulk_import_supported </td>
<td>Bulk Import Support</td>
<td>Define whether the userstore support for bulk user import operation</td>
</tr>
<tr class="odd">
<td>password_digest</td>
<td>Password Hashing Algorithm</td>
<td><div class="content-wrapper">
<p>Specifies the Password Hashing Algorithm used the hash the password before storing in the user store.<br />
Possible values:<br />
SHA - Uses SHA digest method. SHA-1, SHA-256<br />
MD5 - Uses MD 5 digest method.<br />
PLAIN_TEXT - Plain text passwords.</p>
<div class="admonition note">
<p class="admonition-title">Note</p>
    <p>If you enter SHA as the value, it is considered as SHA-1. It is always better to configure an algorithm with a higher bit value so that the digest bit size is higher.</p></div>
</div></td>
</tr>
<tr class="even">
<td>multi_attribute_separator</td>
<td>Multiple Attribute Separator</td>
<td>This property is used to define a character to separate multiple attributes. This ensures that it will not appear as part of a claim value. Normally “,” is used to separate multiple attributes, but you can define ",,," or "..." or a similar character sequence<br />
Default: “,”</td>
</tr>
<tr class="odd">
<td>store_salted_password  </td>
<td>Enable Salted Passwords</td>
<td><div class="content-wrapper">
Indicates whether to stores the password with salted value<br />
Default: true<br />
Possible values: false<br />

<p>By default MI stores the password with a salted value. The recommended way to protect passwords is to use salted password hashing. Once it is salted, the passwords are less vulnerable to dictionary and brute force attacks.</p>
<p>Setting this property to false causes passwords to be stored without a salted value. This means that if two users (Bob and Alice) have the same password, it is stored as the same hash value.</p>
<p></p>
<p>However, if salted passwords are used, MI adds a random value to the password and then generates the hash of the password. Therefore if two users have the same password, they would be stored as different hashed values. This is a more secure method of storing passwords.</p>
</div></td>
</tr>
<tr class="even">
<td>max_user_name_list_length</td>
<td>Maximum User List Length</td>
<td>Controls the number of users listed in the user store of a WSO2 product. This is useful when you have a large number of users and do not want to list them all. Setting this property to 0 displays all users. (Default: 100)<br />
<br />
In some user stores, there are policies to limit the number of records that can be returned from a query. By setting the value to 0, it will list the maximum results returned by the user store. If you need to increase this number, you need to set it in the user store level.<br />
Eg: Active directory has the MaxPageSize property with the default value of 1000.</td>
</tr>
<tr class="odd">
<td>max_role_name_list_length</td>
<td>Maximum Role List Length</td>
<td>Controls the number of roles listed in the user store of a WSO2 product. This is useful when you have a large number of roles and do not want to list them all. Setting this property to 0 displays all roles. (Default: 100)<br />
<br />
In some user stores, there are policies to limit the number of records that can be returned from a query. By setting the value to 0, it will list the maximum results returned by the user store. If you need to increase this number, you need to set it in the user store level.<br />
Eg: Active directory has the MaxPageSize property with the default value of 1000.</td>
</tr>
<tr class="even">
<td>user_roles_cache_enabled</td>
<td>Enable User Role Cache</td>
<td>This is to indicate whether to cache the role list of a user. (Default: true)<br />
<br />
Possible values:<br />
false: Set it to false if the user roles are changed by external means and those changes should be instantly reflected in the Carbon instance.</td>
</tr>
<tr class="odd">
<td>tenant_manager</td>
<td><br />
</td>
<td>Define the tenant manager class specific to each user store type. This is only used in primary user store since its shared among tenants.<br />
JDBC : <code>             org.wso2.carbon.user.core.tenant.JDBCTenantManager            </code><br />
LDAP / AD : <code>             org.wso2.carbon.user.core.tenant.CommonHybridLDAPTenantManager            </code></td>
</tr>
</tbody>
</table>

### Configure Primary user store with datasource

When configuring a JDBC user store as a primary user store, you can use
a datasource to configure database connection configurations and point
that datasource from user store manager configurations. This is a much
cleaner way to configure primary user store with a JDBC user store.

To define a datasource, you can use
` <MI_HOME>/conf/deployment.toml           ` file,
 For detailed information on setting up databases, see
[Setting Up the Physical
Database](../../administer/setting-up-the-physical-database)
, and for information on the purpose of defining datasources and how
they are configured for a product, see [Managing
Datasources](../../administer/managing-datasources)
.

1.  You can add the following in the ` deployment.toml           ` file in order to setup a new datasource.

    ``` toml
    [[datasource]]
    id = "WSO2CarbonDB"
    url= "jdbc:mysql://localhost:3306/WSO2_CARBON_DB"
    username="wso2carbon"
    password="wso2carbon"
    driver="com.mysql.cj.jdbc.Driver"
    pool_options.maxActive=50
    pool_options.maxWait = 60000 # wait in milliseconds
    pool_options.testOnBorrow = true
    ```
    Edit the values accordingly.

    If you are using the same RDBMS as the user store in your system,
    this method would suffice. However, if you have set up
    a separate RDBMS as the user store, instead of using a common RDBMS
    for authorization information as well as the user store, you must
    refer to the datasource configuration from the `  deployment.toml       `
    file by adding the data_source property like below.

    ```toml
    [realm_manager]
    data_source = "jdbc/WSO2CarbonDB"
    ```

## Create admin credentials in the user store

Be sure that the user administrator is updated:

1. Get an encrypted password for the administrator.
2. Open the deployment.toml file and update the following:
    ```java
    [secrets]
    db_password = "encrypted_value_1"
    ```
3. Update the administrator credentials in the deployment.toml file:
   ```java
   [super_admin]
   username = "admin"
   password = "admin"
   create_super_admin = true

   ```