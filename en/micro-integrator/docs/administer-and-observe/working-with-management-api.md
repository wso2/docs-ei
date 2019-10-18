# Using the Management API

The Management API of WSO2 Micro Integrator is an internal REST API, which was introduced to substitute
the **admin services** that were available in WSO2 EI 6.x.x. 

The [Micro Integrator CLI](../../administer-and-observe/using-the-command-line-interface) and the [Micro Integrator dashboard](../../administer-and-observe/working-with-monitoring-dashboard) directly communicates with this service to
obtain administrative information of the server instance.

## Enabling the management API

The management API is disabled in the Micro Integrator by default. You can enable it when you
start your Micro Integrator instance by passing the following system property:

```bash
-DenableManagementApi
```

Note that the default address is **https://localhost** and the port is **9164**.

-   When you run the Micro Integrator on Docker, start your Docker
    container by passing the `enableManagementApi` system property:

    ```bash
    docker run -p 8290:8290 -p 9164:9164 -e JAVA_OPTS="-DenableManagementApi=true" <Docker_Image_Name>
    ```

-   When you run the Micro Integrator on a VM, use the following command
    to enable the `enableManagementApi` system property:

    ```bash
    sh micro-integrator.sh -DenableManagementApi
    ```

-   The Management API is enabled for the embedded Micro Integrator in WSO2 Integration Studio by default.

## Securely invoking the API
The management API is secured using JWT authentication by default. Therefore, in order to access the management API, you must first acquire a JWT token with your valid username and password.

!!! Tip
    See [Securing the Management API](../../setup/management-api/securing-management-api) on configuring users, JWT authentication, and other security options for the management API.

To acquire the JWT token, 

1.	First, encode your username:password in Basic Authorization format (encoded in base64). For example, use the default `admin:admin` credentials.
2.	Invoke the `login` resource of the API with your encoded credintials as shown below.
	```bash
	curl -X GET "https://localhost:9164/management/login" -H "accept: application/json" -H "Authorization: Basic YWRtaW46YWRtaW4=" -k -i
	```

The API will validate the authorization header and provide a response with the JWT token as follows:

```json
{ 
   "AccessToken":"%AccessToken%"
}
```

You can now use this token in subsequent API calls as shown below. 

!!! Info
     When the default JWT security handler is engaged, all the management API resources except `/login` is protected by JWT auth. Therefore, it is necessary to send the token as a bearer token when invoking the API resources.

```bash
curl -X GET "https://localhost:9164/management/inbound-endpoints" -H "accept: application/json" -H "Authorization: Bearer %AccessToken%”
```
If security is not required, user could simply remove the security handler from the management api. 

## Resources

The management API has multiple resources to provide information regarding the deployed artifacts as well as the server itself.

### GET PROXY SERVICES

-	/proxy-services

	Retrieves a list of all deployed proxy services.

	```bash
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

-	/proxy-services?proxyServiceName={proxyName}

	Retrieves information related to a specified proxy.
	```bash
	curl -X GET "https://localhost:9164/management/proxy-services?proxyServiceName=helloProxy" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
	```

### GET CARBON APPLICATIONS

-	/applications

	This operation provides you a list of available Applications.
	```bash
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

-	/applications?carbonAppName={appname}

	Retrieves information related to a specified carbon application.

	```
	curl -X GET "https://localhost:9164/management/applications?carbonAppName=HelloCApp" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
	```

### GET ENDPOINTS

-	/endpoints

	Retrieves a list of available endpoints.

	```bash
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

-	/endpoints?endpointName={endpointname}

	Retrieves information related to a specified endpoint.

### GET APIs

-	/apis

	Retrieves a list of available apis.

	```bash
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

-	/apis?apiName={api}

	Retrieves information related to a specified api.

### GET SEQUENCES

-	/sequences

	Retrieves a list of available sequences.

	```bash
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

-	/sequences?sequenceName={sequence}

	Retrieves information related to a specified sequence.


### GET LOCAL ENTRIES

-	/local-entries

	Retrieves a list of available local entries.

	```bash
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

-	/local-entries?name={entryName}

	Retrieves information related to a specified entry.

### GET TASKS

-	/tasks

	Retrieves a list of available tasks.

	```bash
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

-	/tasks?taskName={taskName}

	Retrieves information related to a specified task.

### GET MESSAGE STORES

-	/message-stores

	Retrieves a list of available message stores.

	```bash
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

-	/message-stores?name={messageStore}

	Retrieves information related to a specified message store.

### GET MESSAGE PROCESSORS   

-	/message-processors

	Retrieves a list of available message processors.

	```bash
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

-	/message-processors?name={messageProcessors}

	Retrieves information related to a specified message processor.

### POST MESSAGE PROCESSORS

-	/message-processors

	Used to activate or deactivate a specific message processor


	```bash
	curl -X POST \
	  https://localhost:9164/management/message-processors \
	  -H 'authorization: Bearer Token
	  -H 'content-type: application/json' \
	  -d '{
		"name": "testMessageProcessor",
		"status": "inactive"
	}'
	```

### GET INBOUND ENDPOINTS

-	/inbound-endpoints

	Retrieves a list of available inbound endpoints.

	```bash
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

-	/inbound-endpoints?inboundEndpointName={inboundEndpoint}

	Retrieves information related to a specified inbound endpoint.

### GET CONNECTORS

-	/connectors

	Retrieves a list of available connectors.

	```bash
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

### GET TEMPLATES

-	/templates

	Retrieves a list of available templates.

	```bash
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

-	/templates?type=TYPE

	Retrieves a list of available templates of a given type. Supported template types are as follows.
	1. endpoint
	2. sequence

	```bash
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

-	/templates?type={type}&name={template}

	Retrieves information related to a specific template. However this requires the template type to be included in the 
	request as a query parameter in addition to the template name.

	```bash
	curl -X GET "https://localhost:9164/management/templates?type=sequence&name=testSequenceTemplate" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
	```

	```json
	{
	    "Parameters": [],
	    "configuration": "<template xmlns=\"http://ws.apache.org/ns/synapse\" name=\"testSequenceTemplate\"><sequence/></template>",
	    "name": "testSequenceTemplate"
	}
	```

### GET SERVER INFORMATION

-	/server

	Retrieves information related to the micro integrator server instance.

	```bash
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

### GET DATA SERVICES

-	/data-services

	Retrieves a list of all data services deployed.

	```bash
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

-	/data-services?dataServiceName={dataservice}

	Retrieves information related to a specific data service.

	```bash
	curl -X GET "https://localhost:9164/management/data-services?dataServiceName=StudentDataService" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -k -i
	```

## Management API configuration file
All configurations related to the management API can be found in `MI_HOME/conf/internal-apis.xml`.

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

## CORS Configurations
By default the Management Api comes with CORS enabled. As shown in the above configuration, it allows all origins
and the Authorization header. User can remove the wildcard in the origin parameter and specific origins in a comma
separated as required.
Authorization header has been added by default in order to cater to the functionalities of the the monitoring dashboard.
Further values can be added in a comma separated format same as for origins. 
