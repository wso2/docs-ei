# Amazon Lambda Connector Example

AWS Lambda is a compute service that lets you run code without provisioning or managing servers. AWS Lambda executes your code only when needed and scales automatically, from a few requests per day to thousands per second. 

Given below is a sample scenario that demonstrates how to create an Amazon Lambda function in the AWS Lambda Service using the WSO2 Amazon Lambda Connector.

## What you'll build
To use the Amazon Lambda connector, add the <amazonlambda.init> element in your configuration before carrying out any Amazon Lambda operations. This Amazon Lambda configuration authenticates with Amazon Lambda by specifying the AWS access key ID and secret access key ID, which are used for every operation. The signature is used with every request and thus differs based on the request the user makes.

**initialize the connector**

You can add the parameter as follows in the init operation:

````xml
<amazonlambda.init>
    <region>{$ctx:region}</region>
    <secretAccessKey>{$ctx:secretAccessKey}</secretAccessKey>
    <accessKeyId>{$ctx:accessKeyId}</accessKeyId>
    <blocking>{$ctx:blocking}</blocking>
</amazonlambda.init>
````
**createFunction**

The `createFunction` operation creates a Lambda function. To create a function, you need a deployment package and an execution role. The deployment package contains your function code. The execution role grants the function permission to use AWS services.

You can add the parameter as follows in the `createFunction` operation:

````xml
<amazonlambda.createFunction>
    <apiVersionCreateFunction>{$ctx:apiVersionCreateFunction}</apiVersionCreateFunction>
    <functionName>{$ctx:functionName}</functionName>
    <functionDescription>{$ctx:functionDescription}</functionDescription>
    <s3Bucket>{$ctx:s3Bucket}</s3Bucket>
    <s3Key>{$ctx:s3Key}</s3Key>
    <s3ObjectVersion>{$ctx:s3ObjectVersion}</s3ObjectVersion>
    <zipFile>{$ctx:zipFile}</zipFile>
    <targetArn>{$ctx:targetArn}</targetArn>
    <environmentVariables>{$ctx:environmentVariables}</environmentVariables>
    <handler>{$ctx:handler}</handler>
    <kmsKeyArn>{$ctx:kmsKeyArn}</kmsKeyArn>
    <layers>{$ctx:layers}</layers>
    <memorySize>{$ctx:memorySize}</memorySize>
    <publish>{$ctx:publish}</publish>
    <role>{$ctx:role}</role>
    <runtime>{$ctx:runtime}</runtime>
    <tags>{$ctx:tags}</tags>
    <timeout>{$ctx:timeout}</timeout>
    <mode>{$ctx:mode}</mode>
    <securityGroupIds>{$ctx:securityGroupIds}</securityGroupIds>
    <subnetIds>{$ctx:subnetIds}</subnetIds>
</amazonlambda.createFunction>
````
The following diagram illustrates all the required functionality of the Amazon Lambda Service that you are going to build.

<img src="/assets/img/connectors/AmazonLambdaConnectorSample.png" title="Amazon Lambda Connector" width="800" alt="Amazon Lambda Connector"/>

This example demonstrates, how to create an Amazon Lambda function easily using the WSO2 Amazon Lambda Connector. Before creating an Amazon Lambda function inside the AWS Lambda service, you need to implement the required deployment package (ZIP Archive) locally.

As a next step, simply create an AWS S3 bucket and the deployment package should be uploaded into that bucket. This sample API contains a service that can be invoked through an HTTP POST request. Once the service is invoked, it creates a Lambda function inside the AWS Lambda service. When the created Lambda function is invoked, it is able to run without provisioning or managing servers.

## Configure the connector in WSO2 Integration Studio
Follow these steps to set up the ESB Solution Project and the Connector Exporter Project.

{!references/connectors/importing-connector-to-integration-studio.md!}

1. Our project would look similar to the following (source view).

   ```
    <?xml version="1.0" encoding="UTF-8"?>
    <api context="/createFunction" name="createFunction" xmlns="http://ws.apache.org/ns/synapse">
        <resource methods="POST">
            <inSequence>
                <property expression="json-eval($.secretAccessKey)" name="secretAccessKey" scope="default" type="STRING"/>
                <property expression="json-eval($.accessKeyId)" name="accessKeyId" scope="default" type="STRING"/>
                <property expression="json-eval($.region)" name="region" scope="default" type="STRING"/>
                <property expression="json-eval($.blocking)" name="blocking" scope="default" type="STRING"/>
                <property expression="json-eval($.functionName)" name="functionName" scope="default" type="STRING"/>
                <property expression="json-eval($.functionDescription)" name="functionDescription" scope="default" type="STRING"/>
                <property expression="json-eval($.apiVersionCreateFunction)" name="apiVersionCreateFunction" scope="default" type="STRING"/>
                <property expression="json-eval($.s3Bucket)" name="s3Bucket" scope="default" type="STRING"/>
                <property expression="json-eval($.s3Key)" name="s3Key" scope="default" type="STRING"/>
                <property expression="json-eval($.s3ObjectVersion)" name="s3ObjectVersion" scope="default" type="STRING"/>
                <property expression="json-eval($.zipFile)" name="zipFile" scope="default" type="STRING"/>
                <property expression="json-eval($.targetArn)" name="targetArn" scope="default" type="STRING"/>
                <property expression="json-eval($.environmentVariables)" name="environmentVariables" scope="default" type="STRING"/>
                <property expression="json-eval($.handler)" name="handler" scope="default" type="STRING"/>
                <property expression="json-eval($.kmsKeyArn)" name="kmsKeyArn" scope="default" type="STRING"/>
                <property expression="json-eval($.layers)" name="layers" scope="default" type="STRING"/>
                <property expression="json-eval($.memorySize)" name="memorySize" scope="default" type="STRING"/>
                <property expression="json-eval($.publish)" name="publish" scope="default" type="STRING"/>
                <property expression="json-eval($.role)" name="role" scope="default" type="STRING"/>
                <property expression="json-eval($.runtime)" name="runtime" scope="default" type="STRING"/>
                <property expression="json-eval($.tags)" name="tags" scope="default" type="STRING"/>
                <property expression="json-eval($.timeout)" name="timeout" scope="default" type="STRING"/>
                <property expression="json-eval($.mode)" name="mode" scope="default" type="STRING"/>
                <property expression="json-eval($.securityGroupIds)" name="securityGroupIds" scope="default" type="STRING"/>
                <property expression="json-eval($.subnetIds)" name="subnetIds" scope="default" type="STRING"/>
                <amazonlambda.init>
                    <region>{$ctx:region}</region>
                    <accessKeyId>{$ctx:accessKeyId}</accessKeyId>
                    <secretAccessKey>{$ctx:secretAccessKey}</secretAccessKey>
                    <blocking>{$ctx:blocking}</blocking>
                </amazonlambda.init>
                <amazonlambda.createFunction>
                    <functionName>{$ctx:functionName}</functionName>
                    <functionDescription>{$ctx:functionDescription}</functionDescription>
                    <apiVersionCreateFunction>{$ctx:apiVersionCreateFunction}</apiVersionCreateFunction>
                    <s3Bucket>{$ctx:s3Bucket}</s3Bucket>
                    <s3Key>{$ctx:s3Key}</s3Key>
                    <s3ObjectVersion>{$ctx:s3ObjectVersion}</s3ObjectVersion>
                    <zipFile>{$ctx:zipFile}</zipFile>
                    <targetArn>{$ctx:targetArn}</targetArn>
                    <environmentVariables>{$ctx:environmentVariables}</environmentVariables>
                    <handler>{$ctx:handler}</handler>
                    <kmsKeyArn>{$ctx:kmsKeyArn}</kmsKeyArn>
                    <layers>{$ctx:layers}</layers>
                    <memorySize>{$ctx:memorySize}</memorySize>
                    <publish>{$ctx:publish}</publish>
                    <role>{$ctx:role}</role>
                    <runtime>{$ctx:runtime}</runtime>
                    <tags>{$ctx:tags}</tags>
                    <timeout>{$ctx:timeout}</timeout>
                    <mode>{$ctx:mode}</mode>
                    <securityGroupIds>{$ctx:securityGroupIds}</securityGroupIds>
                    <subnetIds>{$ctx:subnetIds}</subnetIds>
                </amazonlambda.createFunction>
                <respond/>
            </inSequence>
            <outSequence/>
            <faultSequence/>
        </resource>
    </api> 
    ```
        
2. Right-click on the Composite Application Project and click on **Export Project Artifacts and Run**. Select **Run on Micro Integrator**.
3. Micro Integrator will be started and the composite application will be deployed. You can further refer to the application deployed through the CLI tool. Make sure you first export the PATH as below.

    ```
    $ export PATH=/path/to/mi/cli/directory/bin:$PATH\
    ```
{!/references/connectors/exporting-artifacts.md!} 

## Create Amazon Lambda Deployment Package (Lambda function) 
In this scenario we created sample AWS Deployment Package (Lambda function) in Python.

1. Our sample Deployment Package would look similar to the following (source view : addingNumbers.py).
    ```
    import json
       
       print('Loading function')
       
       def addingNumbers(event, context):
           #print("Received event: " + json.dumps(event, indent=2))
           value1 = event['key1']
           value2 = event['key2']
           print("value1 = " + value1)
           print("value2 = " + value2)
           return float(value1) + float(value2) # Echo back the addition of two keys
           #raise Exception('Something went wrong')
    ```

2. Create a ZIP archive.

Please use command line terminal or shell to run following commands. Commands are shown in listings preceded by a prompt symbol ($) and the name of the current directory, when appropriate:
```
~/Documents$ zip addingNumbers.zip addingNumbers.py
 adding: addingNumbers.py.py (deflated 17%)
```
## Upload Amazon Lambda Deployment Package (ZIP archive) in to the AWS S3 bucket
1. Log in to the AWS Management Console.
2. Navigate to the created S3 bucket (e.g., eiconnectortest).
3. Click **Upload**.
4. Select created Amazon Lambda Deployment Package (ZIP archive) and Upload.

## Create Execution Role
You need to create an Execution Role using [Setting up the Amazon Lambda Environment](../amazonlambda-configuration.md) documentation.  

## Deployment
Follow these steps to deploy the exported CApp in the Enterprise Integrator Runtime. 

{!references/connectors/deploy-capp.md!}

## Testing
1. Log in to the Micro Integrator CLI tool.

    ```
    ./mi remote login
    ```
2. Provide default credentials admin for both username and password.

3. In order to view the proxy services deployed, execute the following command.

    ```
    ./mi api show
    ```
4. Send a POST request using a CURL command or sample client.
  ``` 
  curl -v POST -d 
  '{
    "secretAccessKey":"xxxx",
    "accessKeyId":"xxxx",
    "region":"us-east-2",
    "blocking":"false",
    "s3Bucket":"eiconnectortest",
    "s3Key":"addingNumbers.zip",
    "s3ObjectVersion":"null",
    "functionName":"eiLambdaConnector",
    "handler":"addingNumbers.addingNumbers",
    "role":"arn:aws:iam::610968236798:role/EIConnectorTestRole",
    "runtime":"python3.7",
    "apiVersionCreateFunction":"2015-03-31"
    }' "http://localhost:8290/createFunction" -H "Content-Type:application/json"  
 ```
5. See the following message content.

 ```
 {
        "Description": "",
        "TracingConfig": {
            "Mode": "PassThrough"
        },
        "VpcConfig": null,
        "RevisionId": "4b6e5fdd-cbfa-4ba2-9f6e-528cccdb333f",
        "LastModified": "2020-03-13T05:33:54.900+0000",
        "FunctionName": "eiLambdaConnector",
        "Runtime": "python3.7",
        "Version": "$LATEST",
        "LastUpdateStatus": "Successful",
        "Layers": null,
        "FunctionArn": "arn:aws:lambda:us-east-2:610968236798:function:eiLambdaConnector",
        "KMSKeyArn": null,
        "MemorySize": 128,
        "LastUpdateStatusReason": null,
        "DeadLetterConfig": null,
        "Timeout": 3,
        "Handler": "addingNumbers.addingNumbers",
        "CodeSha256": "VAISY9lY/a7DvxZNOSKCj+q/fsbfUaJjKhCsCVG3yzU=",
        "Role": "arn:aws:iam::610968236798:role/EIConnectorTestRole",
        "MasterArn": null,
        "CodeSize": 405,
        "State": "Active",
        "StateReason": null,
        "Environment": null,
        "StateReasonCode": null,
        "LastUpdateStatusReasonCode": null
    }
```
6. Log in to the AWS Management Console.

7. Navigate to the AWS Lambda and Functions tab.
   <img src="/assets/img/connectors/AWSLambdaFunction.png" title="Amazon Lambda Function" width="800" alt="Amazon Lambda Function"/>
   
8. Next you need to execute the function. Navigate to **Configure test events**.
   
   <img src="/assets/img/connectors/configureTestEvent.png" title="Configure Test Event" width="800" alt="Configure Test Event"/>
   
9. Click **Create new test event**.
   <img src="/assets/img/connectors/createTestEvent.png" title="Create Test Event" width="800" alt="Create Test Event"/>
   
10. Navigate and select the created test event from the dropdown in the top right corner. Click the **Test** button and execute the test event.
   <img src="/assets/img/connectors/executeCreatedEvent.png" title="Execute Test Event" width="800" alt="Execute Test Event"/>
 
## What's next
* You can deploy and run your project on [Docker](../../../setup/installation/run_in_docker.md) or [Kubernetes](../../../setup/installation/run_in_kubernetes.md).
* To customize this example for your own scenario, see [Amazon Lambda Connector Configuration](../amazonlambda-configuration.md) documentation.