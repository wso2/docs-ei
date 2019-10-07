#Internal API Security

This section focuses on securing internal API



Above diagram depicts the implementation architecture of the internal API. 
Current available internal APIs:
1. Management API
2. Prometheus API (Unsecured)

An API Handler is added to engage security for the Management API 

There are two handlers implemented for Management Dashboard API

1. JWT token generation/validation handler (Default)
2. Basic Authentication handler



Note : 
Internal API security only perform Authentication (no role based Authorization)

##JWT token generation/validation handler

In order to enable JWT based authorization we need to enable the JWT based security handler in `internal-apis.xml` as below.

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

###Token store config 

This config sets configurations of issued token store maintained inside the MI.
 
MaxSize - Number of tokens stored in the in memory token store. User can increase or decrease this value accordingly.

TokenCleanupTaskInterval -  Token cleanup will be handled through a seperate thread and the frequency of the token clean up can be setup using this setting. This will clean all the expired and revoked security tokens.  The thread will run only when there are tokens in the store. If it’s empty cleanup thread will automatically stop. Interval is specified in seconds. 

RemoveOldestTokenOnOverflow - If set to true, this will remove the oldest accessed token if the token store is full . If it’s set to false, either user have to wait until other tokens to expire or increase the token store max size accordingly. 

###Token config

Settings for the JWT token issued. 

expiry - This configures the expiry time of the token. This is specified in seconds. 

size - User can specify the key size of the token through this.

###User store 

/Login resource is used to get the token and it is protected by basic auth. The credentials for this basic authentication is set here as the users. Please refer user store configs for more details.


JWT token based authentication introduces two new resources in management API. 

1. /login
2. /logout

/Login resource is used to obtain a JWT token based on the user name and password. A sample request will be as below.

```java
curl -X GET "https://localhost:9164/management/login” -H "accept: application/json" -H "Authorization: Basic YWRtaW46YWRtaW4=“ 
```

Then user will get a response with the access token as follows.

```json
{ 
   "AccessToken":"%AccessToken%"
}
```

Users can use this token in subsequent api calls as below. Please note if the JWT security handler is engaged all the other resources except /login is protected by JWT auth. So we need to send the token as a bearer token.

```java
curl -X GET "https://localhost:9164/management/inbound-endpoints" -H "accept: application/json" -H "Authorization: Bearer %AccessToken%”
```

User can use /logout resource to revoke the token as below.

```java
curl -X GET "https://localhost:9164/management/logout” -H "accept: application/json" -H "Authorization: Bearer %AccessToken%”
```


##Basic Authentication handler

Users can engage basic auth handler as below. Note that user should engage only one security handler.

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

##User Store

Currently file based user store is available for internal APIs defined under both security handler configurations.

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

##Secure Vault Support

Secure vault support is available for file based users store securing user passwords.

