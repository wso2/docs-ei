# Securing the Management API
<!--
This section focuses on securing internal API. Above diagram depicts the implementation architecture of the internal API. 
Current available internal APIs:
1. Management API
2. Prometheus API (Unsecured)
-->

There are two handlers implemented for securing the management API of WSO2 Micro Integrator:

1. JWT Authentication handler (Default)
2. Basic Authentication handler

Note that the above handlers only perform Authentication (no role based Authorization).

## JWT Authentication handler (default)

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

### Token store configurations

This config section specifies the token store maintained inside the Micro Integrator.
 
-        `MaxSize`: Number of tokens stored in the in memory token store. User can increase or decrease this value accordingly.
-        `TokenCleanupTaskInterval`: Token cleanup will be handled through a seperate thread and the frequency of the token clean up can be configured from this setting. This will clean all the expired and revoked security tokens.  The thread will run only when there are tokens in the store. If it is empty, the cleanup thread will automatically stop. Interval is specified in seconds. 
-        `RemoveOldestTokenOnOverflow`: If set to `true`, this will remove the oldest accessed token when the token store is full. If it is set to `false`, the user should either wait until other tokens expire or increase the token store max size accordingly. 

### Token configurations

Settings for the JWT token issued. 

-        `expiry`: This configures the expiry time of the token (specified in seconds). 
-        `size`: Specifies the key size of the token.

## Basic authentication handler

!!! Warning
    The Micro Integrator Dashboard and the Micro Integrator CLI will only work with [JWT-based authentication](#jwt-authentication-handler-default). Basic authentication can only be used if you are directly invoking the management API.

If required, you can engage the basic auth handler as shown below. Note that only one security handler can be engaged.

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

## User Store

A file-based user store is available in the default management API configuration as shown below. This file-based user store can be used for both security handler configurations exaplained above.

Update the below configuration to add/remove users for the management API.

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

## Secure Vault Support

Secure vault support is available for file based users store securing user passwords.

## Securing API login/logout

If you are accessing the management API directly, you must first acquire a JWT token with your valid username and password before invoking any of the API resources. This token needs to be revoked in order to log out of the management API.

!!! Info
    Note that this step is not required when you use the [Micro Integrator CLI](../../administer-and-observe/using-the-command-line-interface) or the [Micro Integrator dashboard](../../administer-and-observe/working-with-monitoring-dashboard).

JWT authentication introduces two additional resources to the management API to handle this requirement. 

-        `/login`: This resource is used to obtain a JWT token based on the user name and password and it is protected by basic auth. The credentials for this basic authentication is set here as the users. Please refer user store configs for more details.
-        `/logout`: This resource is used to revoke the JWT token.

### Getting a JWT token

Follow the steps given below to acquire the JWT token. 

1.	First, encode your username:password in Basic Authorization format (encoded in base64). For example, use the default `admin:admin` credentials.
2.	Invoke the `login` resource of the API with your encoded credintials as shown below.
	```bash
	curl -X GET "https://localhost:9164/management/login" -H "accept: application/json" -H "Authorization: Basic YWRtaW46YWRtaW4=" -k -i
	```
3.	The API will validate the authorization header and provide a response with the JWT token as follows:
	```json
	{ 
	   "AccessToken":"%AccessToken%"
	}
	```

### Invoking an API resource

You can now use this token in subsequent API calls as shown below. 

!!! Info
     When the default JWT security handler is engaged, all the management API resources except `/login` is protected by JWT auth. Therefore, it is necessary to send the token as a bearer token when invoking the API resources.

```bash
curl -X GET "https://localhost:9164/management/inbound-endpoints" -H "accept: application/json" -H "Authorization: Bearer %AccessToken%”
```
If security is not required, user could simply remove the security handler from the management api.

### Log out from management API

Invoke the `/logout` resource to revoke the JWT token you used for [invoking the api resource](#invoking-an-api-resource).

```bash
curl -X GET "https://localhost:9164/management/logout” -H "accept: application/json" -H "Authorization: Bearer %AccessToken%”
```
