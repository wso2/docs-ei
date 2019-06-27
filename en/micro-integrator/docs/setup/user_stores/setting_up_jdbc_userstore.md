# Setting up a JDBC User Store

Follow the steps given below to set up a JDBC user store:

## Set up a database (user store)

1. Create a database.
2. Create the database tables for the user store by running the database scripts
   stored in the EI_HOME/dbscripts/ directory.

## Connect the server to the user store

Open the esb.toml file and apply  the following updates:

```java
[user_store]
type = "database"
class = "org.wso2.carbon.user.core.jdbc.JDBCUserStoreManager"
tenant_manager  =  "org.wso2.carbon.user.core.tenant.JDBCTenantManager"
read_only  =  false
read_groups  =  true
write_groups  =  true
username_java_regex  =  "^[\S]{3,30}$"
username_javascript_regex  =  "^[\S]{3,30}$"
username_java_regex_violation_error_msg  =  "Username pattern policy violated"
password_java_regex  =  "^[\S]{5,30}$"
password_javascript_regex  =  "^[\S]{5,30}$"
password_java_regex_violation_error_msg  =  "Password length should be within 5 to 30 characters"
rolename_java_regex  =  "^[\S]{3,30}$"
rolename_javascript_regex  =  "^[\S]{3,30}$"
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

## Create admin credentials in the user store

Be sure that the user administrator is updated:

1. Get an encrypted password for the administrator.
2. Open the esb.toml file and update the following:
    ```java
    [secrets]
    db_password = "encrypted_value_1"
    ```
3. Update the administrator credentials in the esb.toml file:
   ```java
   [super_admin]
   username = "admin"
   password = "admin"
   create_super_admin = true

   ```