# Amazon Lambda Connector Configuration

The following operations allow you to work with the Amazon Lambda Connector. Click an operation name to see parameter details and samples on how to use it.

### Working with Amazon Lambda Accounts

??? note "getAccountSettings"
    The getAccountSettings operation retrieves details about your account's limits and usage in an AWS Region. See the [related API documentation](https://docs.aws.amazon.com/lambda/latest/dg/API_GetAccountSettings.html).
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>apiVersionGetAccountSettings</td>
            <td>API version for GetAccountSettings method.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazonlambda.getAccountSettings>
        <apiVersionGetAccountSettings>{$ctx:apiVersionGetAccountSettings}</apiVersionGetAccountSettings>
    </amazonlambda.getAccountSettings>
    ```
    
    **Sample request**
    
    ```json
    {
        "secretAccessKey":"0b+fcboKq87Nf7mH6M**********************",
        "accessKeyId":"AKIAJHJ*************",
        "region":"us-east-2",
        "blocking":"false",
        "apiVersionGetAccountSettings": "2016-08-19"
    }
    ```

    **Sample response**

    ```json
    {
        "AccountLimit": {
            "CodeSizeUnzipped": 262144000,
            "CodeSizeZipped": 52428800,
            "ConcurrentExecutions": 1000,
            "TotalCodeSize": 80530636800,
            "UnreservedConcurrentExecutions": 1000,
            "UnreservedConcurrentExecutionsMinimum": null
        },
        "AccountUsage": {
            "FunctionCount": 1,
            "TotalCodeSize": 176268666
        },
        "DeprecatedFeaturesAccess": null,
        "HasFunctionWithDeprecatedRuntime": false,
        "PreviewFeatures": null
    }
    ```

### Working with Amazon Lambda Aliases

??? note "createAlias"
    The createAlias implementation of the POST operation creates an alias for a Lambda function version. Use aliases to provide clients with a function identifier that you can update to invoke a different version. You can also map an alias to split invocation requests between two versions. Use the RoutingConfig parameter to specify a second version and the percentage of invocation requests that it receives. See the [related API documentation](https://docs.aws.amazon.com/lambda/latest/dg/API_CreateAlias.html).
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>apiVersionCreateAlias</td>
            <td>API version for CreateAlias method.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>functionName</td>
            <td>The name of the Lambda function that the alias invokes.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>createAliasDescription</td>
            <td>The description of the alias.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>functionVersion</td>
            <td>The function version that the alias invokes.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>aliasName</td>
            <td>The name of the alias.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>aliasAdditionalVersionWeights</td>
            <td>The name of second alias, and the percentage of traffic that's routed to it.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazonlambda.createAlias>
        <functionName>{$ctx:functionName}</functionName>
        <createAliasDescription>{$ctx:createAliasDescription}</createAliasDescription>
        <functionVersion>{$ctx:functionVersion}</functionVersion>
        <aliasName>{$ctx:aliasName}</aliasName>
        <aliasAdditionalVersionWeights>{$ctx:aliasAdditionalVersionWeights}</aliasAdditionalVersionWeights>
        <apiVersionCreateAlias>{$ctx:apiVersionCreateAlias}</apiVersionCreateAlias>
    </amazonlambda.createAlias>
    ```
    
    **Sample request**
    
    ```json
    {
        "secretAccessKey":"0b+fcboKq87Nf7mH6M**********************",
        "accessKeyId":"AKIAJHJ*************",
        "region":"us-east-2",
        "blocking":"false",
        "functionName":"test",
        "functionVersion":"$LATEST",
        "aliasName":"alias2",
        "apiVersionCreateAlias":"2015-03-31"
    }
    ```

    **Sample response**

    ```json
    {
        "AliasArn": "arn:aws:lambda:us-east-2:********:function:test:alias2",
        "Description": "",
        "FunctionVersion": "$LATEST",
        "Name": "alias2",
        "RevisionId": "be8925ae-a634-4303-92e2-5364d0724406",
        "RoutingConfig": null
    }
    ```

??? note "deleteAlias"
    The deleteAlias implementation deletes a Lambda function alias. See the [related API documentation](https://docs.aws.amazon.com/lambda/latest/dg/API_DeleteAlias.html).
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>apiVersionDeleteAlias</td>
            <td>API version for DeleteAlias method.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>functionName</td>
            <td>The name of the Lambda function that the alias invokes.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>aliasName</td>
            <td>The name of the alias.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazonlambda.deleteAlias>
        <functionName>{$ctx:functionName}</functionName>
        <aliasName>{$ctx:aliasName}</aliasName>
        <apiVersionDeleteAlias>{$ctx:apiVersionDeleteAlias}</apiVersionDeleteAlias>
    </amazonlambda.deleteAlias>
    ```
    
    **Sample request**
    
    ```json
    {
        "secretAccessKey":"0b+fcboKq87Nf7mH6M**********************",
        "accessKeyId":"AKIAJHJ*************",
        "region":"us-east-2",
        "blocking":"false",
        "functionName":"test",
        "aliasName":"alias2",
        "apiVersionDeleteAlias":"2015-03-31"
    }
    ```

    **Sample response**

    ```
    Status: 204 No Content
    ```

??? note "getAlias"
    The getAlias implementation of the GET operation returns details about a Lambda function alias. See the [related API documentation](https://docs.aws.amazon.com/lambda/latest/dg/API_GetAlias.html).
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>apiVersionGetAlias</td>
            <td>API version for getAlias method.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>functionName</td>
            <td>The name of the Lambda function that the alias invokes.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>aliasName</td>
            <td>The name of the alias.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazonlambda.getAlias>
        <functionName>{$ctx:functionName}</functionName>
        <aliasName>{$ctx:aliasName}</aliasName>
        <apiVersionGetAlias>{$ctx:apiVersionGetAlias}</apiVersionGetAlias>    
    </amazonlambda.getAlias>
    ```
    
    **Sample request**
    
    ```json
    {
        "secretAccessKey":"0b+fcboKq87Nf7mH6M**********************",
        "accessKeyId":"AKIAJHJ*************",
        "region":"us-east-2",
        "blocking":"false",
        "functionName":"test",
        "aliasName":"alias2",
        "apiVersionGetAlias":"2015-03-31"
    }
    ```

    **Sample response**

    ```
    Status: 204 No Content
    ```

    ```json
    {
        "AliasArn": "arn:aws:lambda:us-east-2:********:function:test:alias2",
        "Description": "",
        "FunctionVersion": "$LATEST",
        "Name": "alias2",
        "RevisionId": "be8925ae-a634-4303-92e2-5364d0724406",
        "RoutingConfig": null
    }
    ```

??? note "updateAlias"
    The updateAlias method implementation updates the configuration of a Lambda function alias. See the [related API documentation](https://docs.aws.amazon.com/lambda/latest/dg/API_UpdateAlias.html).
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>apiVersionUpdateAlias</td>
            <td>API version for updateAlias method.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>functionName</td>
            <td>The name of the Lambda function that the alias invokes.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>aliasName</td>
            <td>The name of the alias.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>updatedAliasDescription</td>
            <td>The description of the alias.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>updatedAliasAdditionalVersionWeight</td>
            <td>The name of second alias, and the percentage of traffic that's routed to it.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>functionVersion</td>
            <td>The function version that the alias invokes.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazonlambda.updateAlias>
        <functionName>{$ctx:functionName}</functionName>
        <updatedAliasDescription>{$ctx:updatedAliasDescription}</updatedAliasDescription>
        <functionVersion>{$ctx:functionVersion}</functionVersion>
        <aliasName>{$ctx:aliasName}</aliasName>
        <updatedAliasAdditionalVersionWeight>{$ctx:updatedAliasAdditionalVersionWeight}</updatedAliasAdditionalVersionWeight>
        <apiVersionUpdateAlias>{$ctx:apiVersionUpdateAlias}</apiVersionUpdateAlias>
    </amazonlambda.updateAlias>
    ```
    
    **Sample request**
    
    ```json
    {
        "secretAccessKey":"0b+fcboKq87Nf7mH6M**********************",
        "accessKeyId":"AKIAJHJ*************",
        "region":"us-east-1",
        "blocking":"false",
        "functionName":"test",
        "aliasName":"alias2",
        "functionVersion":"$LATEST",
        "apiVersionUpdateAlias":"2015-03-31"
    }
    ```

    **Sample response**

    ```
    Status: 200 OK
    ```

    ```json
    {
        "AliasArn": "arn:aws:lambda:us-east-2:*********:function:test:alias2",
        "Description": "",
        "FunctionVersion": "$LATEST",
        "Name": "alias2",
        "RevisionId": "6d8d089b-c632-4a4b-91ba-ee1ce706c50a",
        "RoutingConfig": null
    }
    ```