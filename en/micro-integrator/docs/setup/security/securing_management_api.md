# Securing the Management API

The Management API of WSO2 Micro Integrator is an internal REST API, which was introduced to substitute
the **admin services** that were available in WSO2 EI 6.x.x. 

The [Micro Integrator CLI](../../../administer-and-observe/using-the-command-line-interface) and the [Micro Integrator dashboard](../../../administer-and-observe/working-with-monitoring-dashboard) communicates with this service to
obtain administrative information of the server instance. If required, you can [directly access the management API](../../../administer-and-observe/working-with-management-api) without using the dashboard or CLI.

The management API of the Micro Integrator is configured using the `internal-apis.xml` file (stored in the `MI_HOME/conf/` directory).

## User authentication

There are two handlers implemented for securing the management API of WSO2 Micro Integrator:

1. [JWT Authentication](#jwt-authentication-default) handler (default)
2. [Basic Authentication](#basic-authentication-optional) handler

Note that the above handlers only perform Authentication (no role based Authorization). If security is **not required**, user could simply remove the security handler from the management API.

### JWT Authentication (default)

!!! Note
    The following settings have been improved by a product update. See [TOML configurations for the Management API](#toml-configurations-for-the-management-api) for details.

JWT-based authentication is enabled in the Micro Integrator by default. You can find the JWT security handler enabled in the `internal-apis.xml` file (stored in the `MI_HOME/conf` directory as shown below.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<handler class="org.wso2.micro.integrator.management.apis.security.handler.JWTTokenSecurityHandler"
         name="JWTTokenSecurityHandler">
    <TokenStoreConfig>
        <MaxSize>200</MaxSize>
        <TokenCleanupTaskInterval>600</TokenCleanupTaskInterval><!--Seconds /-->
        <RemoveOldestTokenOnOverflow>true</RemoveOldestTokenOnOverflow>
    </TokenStoreConfig>
    <TokenConfig>
        <expiry>3600</expiry><!--Seconds /-->
        <size>2048</size>
    </TokenConfig>
    <UserStore>
        <users>
            <user>
                <username>admin</username>
                <password>admin</password>
            </user>
        </users>
    </UserStore>
</handler>
```

**Token store configurations**

This config section specifies the token store maintained inside the Micro Integrator.
 
-        `MaxSize`: Number of tokens stored in the in memory token store. User can increase or decrease this value accordingly.
-        `TokenCleanupTaskInterval`: Token cleanup will be handled through a seperate thread and the frequency of the token clean up can be configured from this setting. This will clean all the expired and revoked security tokens.  The thread will run only when there are tokens in the store. If it is empty, the cleanup thread will automatically stop. Interval is specified in seconds. 
-        `RemoveOldestTokenOnOverflow`: If set to `true`, this will remove the oldest accessed token when the token store is full. If it is set to `false`, the user should either wait until other tokens expire or increase the token store max size accordingly. 

**Token configurations**

Settings for the JWT token issued. 

-        `expiry`: This configures the expiry time of the token (specified in seconds). 
-        `size`: Specifies the key size of the token.

**User store**

See the section on [user store of the management API](#user-store-of-management-api).

### Basic Authentication (optional)

!!! Warning
    The Micro Integrator Dashboard and the Micro Integrator CLI will only work with [JWT-based authentication](#jwt-authentication-default). Basic authentication can only be used if you are directly invoking the management API.

If required, you can engage the basic auth handler as shown below. Note that only one security handler can be engaged. Once you have engaged the basic auth handler, you can configure the [user store](#user-store-of-management-api) as well as [CORS](#cors-configurations)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<handlers>
   <handler class="org.wso2.micro.integrator.management.apis.security.handler.BasicSecurityHandler" name="BasicSecurityHandler">
      <UserStore>
         <users>
            <user>
               <username>admin</username>
               <password>admin</password>
            </user>
         </users>
      </UserStore>
   </handler>
</handlers>
```

## User store of Management API

!!! Note
    The following settings have been improved by a product update. See [TOML configurations for the Management API](#toml-configurations-for-the-management-api) for details.

A file-based user store is included for the [authentication handler](#user-authentication) that is enabled for the Micro Integrator. Open the `internal-apis.xml` file (stored in the `MI_HOME/conf` directory and see that the `<UserStore>` section is configured as shown below. 

```xml
<UserStore>
    <users>
        <user>
            <username>admin</username>
            <password>admin</password>
        </user>
        <user>
            <username>admin2</username>
            <password>admin2Password@123</password>
        </user>
    </users>
</UserStore>
```

Update this configuration to add/remove users for the management API. You can [encrypt the plain text passwords](../../../setup/security/encrypting_plain_text) using **secure vault**.

To **disable** the file-based user store, remove this section from the `internal-apis.xml` file. When the file-based user store is disabled, the [external user store](../../../setup/user_stores/setting_up_ro_ldap) that is configured for the Micro Integrator will function as the management API's user store.

## CORS Configurations

!!! Note
    The following settings have been improved by a product update. See [TOML configurations for the Management API](#toml-configurations-for-the-management-api) for details.

CORS is enabled for the management API by default. Open the `internal-apis.xml` file (stored in the `MI_HOME/conf` directory and see that the `<cors>` section is configured for the [authentication handler](#user-authentication). 

The default configuration allows all origins (denoted by the `*` symbol) as shown below. The `Authorization` header is also enabled by default to cater to the functionalities of the the Micro Integrator dashboard.
  
```xml
<cors>
       <enabled>true</enabled>
       <allowedOrigins>*</allowedOrigins>
       <allowedHeaders>Authorization</allowedHeaders>
 </cors>
```
If required, you can remove the wild card and add specific origins as a comma-separated list. You can also add more header values in a comma separated list (same as for origins).

```xml
<cors>
       <enabled>true</enabled>
       <allowedOrigins>https://127.0.0.1:9743</allowedOrigins>
       <allowedHeaders>Authorization</allowedHeaders>
 </cors>
```

## Securing API login/logout

!!! Tip
    This section only applies when the default [JWT authentication handler](#jwt-authentication-default) is engaged. If you have switched to [basic authentication](#basic-authentication-optional), you can secure the login by simply passing the base64 encoded user credentials when you send the invocation request.

JWT authentication introduces two additional resources to the management API to handle API login and logout: 

-        `/login`: This resource is used to obtain a JWT token based on the user name and password and it is protected by basic auth. The credentials for this basic authentication is set here as the users. Please refer [user store configs](#user-store-of-management-api) for more details.
-        `/logout`: This resource is used to revoke the JWT token.

When you [access the management API directly](../../../administer-and-observe/working-with-management-api), you must first acquire a JWT token with your valid username and password. To log out of the management API, this token must be revoked. See [securely invoking the management API](../../../administer-and-observe/working-with-management-api/#securely-invoking-the-api) for more information. 

When you use the [Micro Integrator Dashboard](../../../administer-and-observe/working-with-monitoring-dashboard) or the [Micro Integrator CLI](../../../administer-and-observe/using-the-command-line-interface), JWT token-based authentication is handled internally.

## TOML configurations for the Management API

!!! Note
    This capability of configuring the management API using the `deployment.toml` file is released as a product update on the **26th March, 2020**.  You can get the latest updates using [WSO2 Update Manager](https://docs.wso2.com/display/updates/Getting+Started)

If your product instance has the latest updates, open the `deployment.toml` file (stored in the `<MI_HOME>/conf` directory) and configure the management API instead of using the `internal-apis.xml` file.

- Updating token store configurations for JWT authentication:

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

- Updating token configurations for JWT authentication:

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

- Adding users to the default file-based user store:

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

- Configuring CORS for the management API:

    ```toml
    [management_api.cors]
    enabled = true
    allowed_origins = "https://127.0.0.1:9743,https://wso2.com:9743"
    allowed_headers = "Authorization"
    ```
