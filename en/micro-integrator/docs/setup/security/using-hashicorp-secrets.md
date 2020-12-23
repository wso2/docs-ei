# Using HashiCorp Secrets 

The Micro Integrator is by default configured to use secure vault for encrypting secrets. 
However, you may encounter certain limitations if you use secrets with a large number of characters.

Therefore, you can use HashiCorp secrets with the Micro Integrator if you want to handle long secret values.

!!! Note
    HashiCorp secrets are only applicable to synapse configurations. For server configurations, you can continue using secure vault.

## Connecting MI to the HashiCorp server

1.  Open the `deployment.toml` file (stored in the `<MI_HOME>/conf` folder) and add the following configuration. 

    ```toml
    [[external_vault]]
    name = "hashicorp"
    address = "http://127.0.0.1:8200"
    # When static authentication is used, apply the rootToken:
    rootToken = "ROOT_TOKEN"
    # When AppRole-pull method is used for authentication, apply the roleId and secretId:
    roleId = "ROLE_ID"
    secretId = "SECRET_ID"
    cachableDuration = 15000
    engineVersion = 2
    namespace = "NAMESPACE"
    trustStoreFile = "${carbon.home}/repository/resources/security/client-truststore.jks"
    keyStoreFile = "${carbon.home}/repository/resources/security/wso2carbon.jks"
    keyStorePassword = "KEY_STORE_PASSWORD"
    ```

2.  Copy the [vault-java-driver](https://github.com/BetterCloud/vault-java-driver) to the `<MI_HOME>/lib` directory (tested with 5.1.0 version).

## Accessing HashiCorp secrets in synapse configurations

Once your Micro Integrator is connected to the HashiCorp, you can point to the secrets stored in the HashiCorp server from your synapse configurations.

Given below is a sample synapse property that uses a hashiCorp secret:

```xml
<property name="HashiCorpSecret" expression="hashicorp:vault-lookup('path-name', 'field-name') />
```

## HashiCorp server authentication methods

There are two methods that can be used for authenticating HashiCorp secrets.

### Using a static root token

To use the static root token method, you need to specify the ROOT_TOKEN that you generate from the HashiCorp server and specify it in the `deployment.toml` file as shown below.

```toml
[[external_vault]]
name = "hashicorp"
address = "http://127.0.0.1:8200"
rootToken = "ROOT_TOKEN"
cachableDuration = 15000
engineVersion = 2
trustStoreFile = "${carbon.home}/repository/resources/security/client-truststore.jks"
keyStoreFile = "${carbon.home}/repository/resources/security/wso2carbon.jks"
keyStorePassword = "KEY_STORE_PASSWORD"
``` 

### Using AppRole-pull method

To use the AppRole-pull method, you need to first generate a secret ID and role ID from HashiCorp server and then specify it in the `deployment.toml` file as shown below.
The secret ID an role ID will internally generate a token and authenticate the HashiCorp server connection.

!!! Note
    The secret ID you generate in HashiCorp may expire. If that happens, you can [renew the security token](#renewing-securing-token-approle-pull-method).  

```toml
[[external_vault]]
name = "hashicorp"
address = "http://127.0.0.1:8200"
roleId = "ROLE_ID"
secretId = "SECRET_ID"
cachableDuration = 15000
engineVersion = 2
trustStoreFile = "${carbon.home}/repository/resources/security/client-truststore.jks"
keyStoreFile = "${carbon.home}/repository/resources/security/wso2carbon.jks"
keyStorePassword = "KEY_STORE_PASSWORD"
```

### Renewing securing token (AppRole-pull method)

When you use the AppRole-pull method for authenticating the HashiCorp connection, there can be situation where the secret ID expires. 
When you generate the secret ID from HashiCorp, you have the option of limiting the number of time the secret ID can be used (with the `secret_id_num_uses` parameters). 
In this case, the secret ID will expire after it is used for the specified number of times.   

In such situations, you need to regenerate a secret ID from HashiCorp and apply it to the `deployment.toml` file. However, you need to restart the Micro Integrator before
using the new secret token. This means, there will be a server downtime. 

If you want to update the secret token dynamically without restarting the server, you can use the management API of the Micro Integrator.
As shown below, you can send a request to the given URL with the new secret ID (specified in the sample payload).

-   Management API URL: `https://HOST:9164/management/external-vaults/hashicorp`
-   Payload:
    ```json
    {
      "secretId" : "new_secret_id" 
    }
    ```

## Using Namespaces for HashiCorp connection

Namespace support is only available only in the Enterprise edition of HashiCorp vault. 
You can add a global namespace value in the `deployment.toml` file or the `external-vault.xml` file.
Can use namespace value per request in Synapse configurations as shown below.

```xml
 <property name="HashiCorpSecret" expression="hashicorp:vault-lookup('namespace', 'path-name', 'field-name') />
```