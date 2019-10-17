# Introduction to Management API

Management API is an internal REST API in WSO2 Micro Integrator which was introduced to substitute
the Admin services. CLI and Monitoring dashboard distributions communicate with this service in order to
obtain administrative information of the server instance.

#Discovering Management API

Management API is disabled by default in Micro Integrator. The service can be started at the server startup 
by specifying the **-DenableManagementApi** property. Upon enabling, the Management api will start listening 
on port 9164 (default port set for all internal APIs'). The context of this service is specified as 
**management**. 

```
https://localhost:9164/management/
```

The management API has multiple resources to provide information regarding the deployed artifacts as well
as the server itself. A list of these resources would be as follows.

#Security
Management API uses JSON Web Tokens (JWT) for authentication purposes. This is implemented by JWTTokenSecurityHandler
which is delivered by default. UserStore can be found inside the handler (MI_HOME/conf/internal-apis.xml), to which 
the user can add users as required.

In order to call the Management API, first a JWT token has to be acquired by a valid username and password.
This can be done by calling the login resource of the API as follows (for default admin/admin login).

```
curl -X GET "https://localhost:9164/management/login" -H "accept: application/json" -H "Authorization: Basic YWRtaW46YWRtaW4=" -k -i
```
Note: The user name and the password has to sent to the login resource in Basic Authorization format 
(encoded in base64).
The API will validate the authorization header and provide a JWT token.

This token has to be sent to the backend with each and every request as a bearer token.
If security is not required, user could simply remove the security handler from the management api. 

#Resources

##PROXY SERVICES
### GET

####/proxy-services

##### Description
Retrieves a list of all deployed proxy services.
```
curl -X GET "https://localhost:9164/management/proxy-services" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
  "count": 1,
  "list": [
    {
      "name": "TestProxy",
      "wsdl1_1": "http://ThinkPad-X1-Carbon-3rd:8290/services/TestProxy?wsdl",
      "wsdl2_0": "http://ThinkPad-X1-Carbon-3rd:8290/services/TestProxy?wsdl2"
    }
  ]
}
```

#### /proxy-services?proxyServiceName={proxyName}

##### Description
Retrieves information related to a specified proxy.
```
curl -X GET "https://localhost:9164/management/proxy-services?proxyServiceName=helloProxy" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

## CARBON APPLICATIONS

###GET

#### /applications

#####Description 
This operation provides you a list of available Applications.
```
curl -X GET "https://localhost:9164/management/applications" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
  "count": 1,
  "list": [
    {
      "name": "SampleServicesCompositeApplication",
      "version": "1.0.0"
    }
  ]
}
```

#### /applications?carbonAppName={appname}

##### Description

Retrieves information related to a specified carbon application.

```
curl -X GET "https://localhost:9164/management/applications?carbonAppName=HelloCApp" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

## ENDPOINTS

### GET

#### /endpoints

##### Description
Retrieves a list of available endpoints.

```
curl -X GET "https://localhost:9164/management/endpoints" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
    "count": 2,
    "list": [
        {
            "name": "FailOver_EP",
            "type": "failover"
        },
        {
            "name": "WSDL_EP",
            "type": "wsdl"
        }
    ]
}
```

#### /endpoints?endpointName={endpointname}

##### Description
Retrieves information related to a specified endpoint.



## API

### GET

#### /apis

##### Description
Retrieves a list of available apis.

```
curl -X GET "https://localhost:9164/management/apis" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
    "count": 2,
    "list": [
        {
            "name": "api",
            "url": "http://localhost:8290/test"
        },
        {
            "name": "helloApi",
            "url": "http://localhost:8290/api"
        }
    ]
}
```

#### /apis?apiName={api}

##### Description
Retrieves information related to a specified api.



## SEQUENCES

### GET

#### /sequences

##### Description
Retrieves a list of available sequences.

```
curl -X GET "https://localhost:9164/management/sequences" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
    "count": 3,
    "list": [
        {
            "tracing": "disabled",
            "stats": "disabled",
            "name": "fault"
        },
        {
            "container": "[ Deployed From Artifact Container: helloCompositeApplication ] ",
            "tracing": "disabled",
            "stats": "disabled",
            "name": "sequenceForSampler"
        },
        {
            "tracing": "disabled",
            "stats": "disabled",
            "name": "main"
        }
    ]
}
```

#### /sequences?sequenceName={sequence}

##### Description
Retrieves information related to a specified sequence.



## LOCAL ENTRIES

### GET

#### /local-entries

##### Description
Retrieves a list of available local entries.

```
curl -X GET "https://localhost:9164/management/local-entries" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
    "count": 2,
    "list": [
        {
            "name": "testEntry1",
            "type": "Inline Text"
        },
        {
            "name": "testentry2",
            "type": "Inline XML"
        }
    ]
}
```

#### /local-entries?name={entryName}

##### Description
Retrieves information related to a specified entry.




## TASKS

### GET

#### /tasks

##### Description
Retrieves a list of available tasks.

```
curl -X GET "https://localhost:9164/management/tasks" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
    "count": 1,
    "list": [
        {
            "name": "testTask"
        }
    ]
}
```

#### /tasks?taskName={taskName}

##### Description
Retrieves information related to a specified task.



## MESSAGE STORES

### GET

#### /message-stores

##### Description
Retrieves a list of available message stores.

```
curl -X GET "https://localhost:9164/management/message-stores" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
    "count": 2,
    "list": [
        {
            "size": 0,
            "name": "testMessageStore",
            "type": "in-memory-message-store"
        },
        {
            "size": 0,
            "name": "jdbc_sample_store",
            "type": "jdbc-message-store"
        }
    ]
}
```

#### /message-stores?name={messageStore}

##### Description
Retrieves information related to a specified message store.




## MESSAGE PROCESSORS   

### GET

#### /message-processors

##### Description
Retrieves a list of available message processors.

```
curl -X GET "https://localhost:9164/management/message-processors" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
    "count": 2,
    "list": [
        {
            "name": "testMessageProcessor",
            "type": "Scheduled-message-forwarding-processor",
            "status": "active"
        },
        {
            "name": "TestSamplingProcessor",
            "type": "Sampling-processor",
            "status": "active"
        }
    ]
}
```

#### /message-processors?name={messageProcessors}

##### Description
Retrieves information related to a specified message processor.


### POST

#### /message-processors

##### Description
Used to activate or deactivate a specific message processor


```
curl -X POST \
  https://localhost:9164/management/message-processors \
  -H 'authorization: Bearer Token
  -H 'content-type: application/json' \
  -d '{
	"name": "testMessageProcessor",
	"status": "inactive"
}'
```



## INBOUND ENDPOINTS

### GET

#### /inbound-endpoints

##### Description
Retrieves a list of available inbound endpoints.

```
curl -X GET "https://localhost:9164/management/inbound-endpoints" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
    "count": 1,
    "list": [
        {
            "protocol": "http",
            "name": "HTTPIEP"
        }
    ]
}
```

#### /inbound-endpoints?inboundEndpointName={inboundEndpoint}

##### Description
Retrieves information related to a specified inbound endpoint.


## CONNECTORS

### GET

#### /connectors

##### Description
Retrieves a list of available connectors.

```
curl -X GET "https://localhost:9164/management/connectors" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
    "count": 2,
    "list": [
        {
            "package": "org.wso2.carbon.connector",
            "name": "fileconnector",
            "description": "wso2 file connector",
            "status": "enabled"
        },
        {
            "package": "org.wso2.carbon.connector",
            "name": "gmail",
            "description": "WSO2 Gmail connector library",
            "status": "enabled"
        }
    ]
}
```





## TEMPLATES

### GET

#### /templates

##### Description
Retrieves a list of available templates.

```
curl -X GET "https://localhost:9164/management/templates" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
    "sequenceTemplateList": [
        {
            "name": "testSequenceTemplate"
        }
    ],
    "endpointTemplateList": [
        {
            "name": "endpointTemplate"
        }
    ]
}
```

#### /templates?type=TYPE

##### Description
Retrieves a list of available templates of a given type. Supported template types are as follows.
1. endpoint
2. sequence

```
curl -X GET "https://localhost:9164/management/templates?type=sequence" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```


```json
{
    "count": 1,
    "list": [
        {
            "name": "testSequenceTemplate"
        }
    ]
}
```

#### /templates?type={type}&name={template}

##### Description
Retrieves information related to a specific template. However this requires the template type to be included in the 
request as a query parameter in addition to the template name.


```
curl -X GET "https://localhost:9164/management/templates?type=sequence&name=testSequenceTemplate" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```


```json
{
    "Parameters": [],
    "configuration": "<template xmlns=\"http://ws.apache.org/ns/synapse\" name=\"testSequenceTemplate\"><sequence/></template>",
    "name": "testSequenceTemplate"
}
```


## SERVER INFORMATION

### GET

#### /server

##### Description
Retrieves information related to the micro integrator server instance.

```
curl -X GET "https://localhost:9164/management/server" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
    "productVersion": "1.1.0",
    "repositoryLocation": "/Users/Sachith/IdeaProjects/micro-integrator-public/distribution/target/wso2mi-1.1.0-SNAPSHOT/repository/deployment/client/",
    "osVersion": "10.14",
    "javaVersion": "1.8.0_171",
    "workDirectory": "/Users/Sachith/IdeaProjects/micro-integrator-public/distribution/target/wso2mi-1.1.0-SNAPSHOT/tmp/work",
    "carbonHome": "/Users/Sachith/IdeaProjects/micro-integrator-public/distribution/target/wso2mi-1.1.0-SNAPSHOT",
    "javaVendor": "Oracle Corporation",
    "osName": "Mac OS X",
    "productName": "WSO2 Micro Integrator",
    "javaHome": "/Library/Java/JavaVirtualMachines/jdk1.8.0_171.jdk/Contents/Home/jre"
}
```





## DATA SERVICES

### GET

#### /data-services

##### Description
Retrieves a list of all data services deployed.

```
curl -X GET "https://localhost:9164/management/data-services" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```

```json
{
    "count": 1,
    "list": [
        {
            "name": "StudentDataService",
            "wsdl1_1": "http://Sachiths-MacBook-Pro.local:8290/services/StudentDataService?wsdl",
            "wsdl2_0": "http://Sachiths-MacBook-Pro.local:8290/services/StudentDataService?wsdl2"
        }
    ]
}
```

#### /data-services?dataServiceName={dataservice}

##### Description
Retrieves information related to a specific data service.

```
curl -X GET "https://localhost:9164/management/data-services?dataServiceName=StudentDataService" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
```


#Configurations
All configurations related to the management API can be found in MI_HOME/conf/internal-apis.xml.

```xml
<api class="org.wso2.micro.integrator.management.apis.ManagementInternalApi" name="ManagementApi"
             protocol="https">
            <handlers>
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
                <!--handler class="org.wso2.micro.integrator.management.apis.security.handler.BasicSecurityHandler"
                         name="BasicSecurityHandler">
                    <UserStore>
                        <users>
                            <user>
                                <username>admin</username>
                                <password>admin</password>
                            </user>
                        </users>
                    </UserStore>
                </handler-->
            </handlers>
            <cors>
                <enabled>true</enabled>
                <allowedOrigins>*</allowedOrigins>
                <!-- comma separated values-->
                <allowedHeaders>Authorization</allowedHeaders>
            </cors>
        </api>
```

###Security Configurations
All security related aspects of the API can be found in JWTTokenSecurityHandler. More information regarding security cab be 
found in [https://github.com/wso2/docs-ei/pull/814/files]

###CORS Configurations
By default the Management Api comes with CORS enabled. As shown in the above configuration, it allows all origins
and the Authorization header. User can remove the wildcard in the origin parameter and specific origins in a comma
separated as required.
Authorization header has been added by default in order to cater to the functionalities of the the monitoring dashboard.
Further values can be added in a comma separated format same as for origins. 
