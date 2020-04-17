# Securing the Management API

The Management API of WSO2 Micro Integrator is an internal REST API, which was introduced to substitute
the **admin services** that were available in WSO2 EI 6.x.x. 

The [Micro Integrator CLI](../../../administer-and-observe/using-the-command-line-interface) and the [Micro Integrator dashboard](../../../administer-and-observe/working-with-monitoring-dashboard) communicates with this service to
obtain administrative information of the server instance. If required, you can [directly access the management API](../../../administer-and-observe/working-with-management-api) without using the dashboard or CLI.

The management API of the Micro Integrator is configured using the `internal-apis.xml` file (stored in the `MI_HOME/conf/` directory).
 
## JWT-based User Authentication

JWT-based user authentication is enabled in the Micro Integrator by default. Note that only Authentication is allowed (no role-based Authorization).

If you want to update the defualt settings, open the `deployment.toml` file (stored in the `MI_HOME/conf/` directory) and add the following:

### Disable user authentication**

If security is **not required**, you can simply disable the handler for the Micro Integrator:

```toml
[management_api_token_handler]
enable = false
```

### Token store configurations**

```toml
[management_api.handler.token_store_config]
max_size = "200"
clean_up_interval = "600"
remove_oldest_token_on_overflow = true
```

<table>
    <tr>
             <th>Parameter</th>
             <th>Description</th>
    </tr>
    <tr>
          <td><code>MaxSize</code></td>
         <td>Number of tokens stored in the in memory token store. User can increase or decrease this value accordingly.</td>
    </tr>
    <tr>
          <td><code>TokenCleanupTaskInterval</code></td>
         <td>Token cleanup will be handled through a seperate thread and the frequency of the token clean up can be configured from this setting. This will clean all the expired and revoked security tokens. The thread will run only when there are tokens in the store. If it is empty, the cleanup thread will automatically stop. Interval is specified in seconds.</td>
    </tr>
    <tr>
          <td><code>RemoveOldestTokenOnOverflow</code></td>
         <td>
                  If set to <code>true</code>, this will remove the oldest accessed token when the token store is full. If it is set to <code>false</code>, the user should either wait until other tokens expire or increase the token store max size accordingly.
         </td>
    </tr>
</table>

### Token configurations**

Settings for the JWT token issued.

```toml
[management_api.handler.token_config]
expiry = "3600"
size = "2048"
```

<table>
    <tr>
             <th>Parameter</th>
             <th>Description</th>
    </tr>
    <tr>
          <td><code>expiry</code></td>
         <td>This configures the expiry time of the token (specified in seconds).</td>
    </tr>
    <tr>
          <td><code>size</code></td>
         <td>Specifies the key size of the token.</td>
    </tr>
</table>

### Adding users to the User Store

The management API uses a file-based user store by default. You can open the `deployment.toml` file and add new users as shown below. You can [encrypt the plain text](../../../setup/security/encrypting_plain_text) using **secure vault**.

```toml
[[management_api.handler.user_store.users]]
user.name = "user-1"
user.password = "pwd-1"

[[management_api.handler.user_store.users]]
user.name = "user-2"
user.password = "pwd-2"

[[management_api.handler.user_store.users]]
user.name = "user-3"
user.password = "pwd-3"
```

### Disabling the file-based user store

When the file-based user store is disabled, the [external user store](../../../setup/user_stores/setting_up_ro_ldap) 
that is configured for the Micro Integrator will function as the management API's user store.

To **disable** the file-based user store, add the below section to the `deployment.toml`. 

```toml
[management_api.handler.file_user_store]
enable = false
```

## Configuring CORS

CORS is enabled for the management API by default. The default configuration allows all origins (denoted by the `*` symbol). The `Authorization` header is also enabled by default to cater to the functionalities of the the Micro Integrator dashboard.
 
If required, you can modify this by adding the following configurations to the `deployment.toml` file.

```toml
[management_api.cors]
enabled = true
allowed_origins = "https://127.0.0.1:9743,https://wso2.com:9743"
allowed_headers = "Authorization"
```

## Securing API login/logout

JWT authentication introduces two additional resources to the management API to handle API login and logout: 

-        `/login`: This resource is used to obtain a JWT token based on the user name and password and it is protected by basic auth. The credentials for this basic authentication is set here as the users. Please refer [user store configs](#user-store-of-management-api) for more details.
-        `/logout`: This resource is used to revoke the JWT token.

When you [access the management API directly](../../../administer-and-observe/working-with-management-api), you must first acquire a JWT token with your valid username and password. To log out of the management API, this token must be revoked. See [securely invoking the management API](../../../administer-and-observe/working-with-management-api/#securely-invoking-the-api) for more information. 

When you use the [Micro Integrator Dashboard](../../../administer-and-observe/working-with-monitoring-dashboard) or the [Micro Integrator CLI](../../../administer-and-observe/using-the-command-line-interface), JWT token-based authentication is handled internally.
