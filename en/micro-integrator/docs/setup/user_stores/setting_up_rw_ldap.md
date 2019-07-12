# Setting up a Read Write LDAP

Follow the steps given below to set up a read-write LDAP user store:

## Set up a read-only LDAP

Set up your read-write LDAP.

## Connect the server to the user store

Open the esb.toml file and apply  the following updates:

```java
[user_store]
type = "read_write_ldap"
connection_url = "ldap://ldap.example.com:389"
connection_name = "uid=admin,ou=wso2is"
connection_password = "$secret{ldap_password}"
base_dn = "dc=example,dc=com"

```
Find more parameters to configure this user store.

## Create admin credentials in the user store

Be sure that the user administrator is updated:

1. Get an encrypted password for the administrator.
2. Open the esb.toml file and update the following:
    ```java
    [secrets]
    ldap_password = "encrypted_value_1"
    ```
3. Update the administrator credentials in the esb.toml file:
   ```java
   [super_admin]
   username = "admin"
   password = "admin"
   create_super_admin = true

   ```