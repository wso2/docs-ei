# Using HashiCorp Secrets 

!!! Info
    This feature is available as a product update from 27/10/2020 onwards. If you don't already have this update, you can [get the latest updates](https://updates.docs.wso2.com/en/latest/updates/overview/) now.

The Micro Integrator is, by default, configured to use [secure vault for encrypting secrets](encrypting_plain_text.md). However, you may encounter certain limitations if you use secrets with a large number of characters.

You can use HashiCorp secrets with the Micro Integrator if you want to handle long secret values.

!!! Note
    HashiCorp secrets are only applicable to synapse configurations. For server configurations, you can continue using secure vault.

## Before you begin

Go to your HashiCorp instance and generate the secrets. See the HashiCorp documentation for instructions.

## Connecting MI to the HashiCorp server

Once you have set up the secrets in the HashiCorp server, you can configure the Micro Integrator to connect with HashiCorp.

Follow the steps given below.

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

    <table>
        <tr>
            <th>
                Parameter
            </th>
            <th>
                Description
            </th>
        </tr>
        <tr>
            <td>
                name
            </td>
            <td>
                Specify 'hashicorp' as the external vault name.
            </td>
        </tr>
        <tr>
            <td>
                address
            </td>
            <td>
                The address URL of the HashiCorp server where the secrets are stored.
            </td>
        </tr>
        <tr>
            <td>
                rootToken
            </td>
            <td>
                Specify the root token generated from the HashiCorp server. This will be used for static authentication when connecting to the HashiCorp server.
            </td>
        </tr>
        <tr>
            <td>
                roleId
            </td>
            <td colspan="2">
                Only applies if AppRole-pull method is used for authenticating the HashiCorp server connection. Instead of specifying the rootToken, specify the role ID and secret ID generated from HashiCorp.
            </td>
        </tr>
        <tr>
            <td>
                secretId
            </td>
        </tr>
        <tr>
            <td>
                cachableDuration
            </td>
            <td>
                All resources fetched from the HashiCorp vault would be cached for this number of milliseconds.</br>
                The default value is 15000. 
            </td>
        </tr>
        <tr>
            <td>
                engineVersion
            </td>
            <td>
                The version of the HashiCorp secret engine. 
            </td>
        </tr>
        <tr>
            <td>
                namespace
            </td>
            <td>
                Namespace support is available only in the Enterprise edition of HashiCorp vault.
                The namespace value specified here can be accessed from <a href="#using-namespaces-for-the-hashicorp-connection">synapse configurations</a>. 
            </td>
        </tr>
    </table>

2.  Copy the [vault-java-driver](https://github.com/BetterCloud/vault-java-driver) to the `<MI_HOME>/lib` directory (tested with 5.1.0 version).

## Accessing HashiCorp secrets in synapse configurations

Once your Micro Integrator is connected to HashiCorp, you can point to the secrets stored in the HashiCorp vault from your synapse configurations.

Given below is a sample synapse property that uses a HashiCorp secret:

```xml
<property name="HashiCorpSecret" expression="hashicorp:vault-lookup('path-name', 'field-name') />
```

## HashiCorp server authentication methods

There are two methods that can be used for authenticating HashiCorp secrets.

### Using a static root token

To use the static root token method, you need to specify the ROOT_TOKEN that you generate from the HashiCorp server and specify it in the `deployment.toml` file under the `[[external_vault]]` configuration as [explained above](#connecting-mi-to-the-hashicorp-server).

### Using AppRole-pull method

To use the AppRole-pull method, you need to first generate a secret ID and role ID from the HashiCorp server and then specify it in the `deployment.toml` file as [explained above](#connecting-mi-to-the-hashicorp-server).
The secret ID and role ID will internally generate a token and authenticate the HashiCorp server connection.

!!! Note
    The secret ID you generate in HashiCorp may expire. If that happens, you can [renew the security token](#renewing-securing-token-approle-pull-method). 

### Renewing securing token (AppRole-pull method)

When you generate the secret ID from HashiCorp (for enabling AppRoll-pull authentication), you have the option of limiting the number of times the secret ID token can be used. This is done using the `secret_id_num_uses` parameter. 
In this case, the secret ID will expire after it is used for the specified number of times.   

In such situations, you need to regenerate a secret ID from HashiCorp and apply it to the Micro Integrator's `deployment.toml` file. However, you need to restart the Micro Integrator before
using the new secret token, which means, there will be a server downtime. 

If you want to update the secret token dynamically without restarting the server, you can use the management API of the Micro Integrator.
As shown below, you can send a request to the given URL with the new secret ID (specified in the sample payload).

-   Management API URL: `https://HOST:9164/management/external-vaults/hashicorp`
-   Payload:
    ```json
    {
      "secretId" : "new_secret_id" 
    }
    ```

## Using Namespaces for the HashiCorp connection

Namespace support is available only in the Enterprise edition of HashiCorp vault. 
You can add a global namespace value in the `deployment.toml` file as [explained above](#connecting-mi-to-the-hashicorp-server).

You can use namespace values per request in synapse configurations. Shown below is a sample property configuration that uses a namespaces.

```xml
 <property name="HashiCorpSecret" expression="hashicorp:vault-lookup('namespace', 'path-name', 'field-name') />
```