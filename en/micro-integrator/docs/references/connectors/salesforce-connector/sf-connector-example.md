# Salesforce Connector Example

In this use-case, we are going to receive the details about the Salesforces themes.

1. Open WSO2 Integration Studio and create an ESB Solution Project.
2. Our API would be looking as below(source view).

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <api context="/salesforcerest" name="salesforcerest" xmlns="http://ws.apache.org/ns/synapse">
        <resource methods="POST" uri-template="/themecolor">
            <inSequence>
                <property expression="json-eval($.accessToken)" name="accessToken" scope="default" type="STRING"/>
                <property expression="json-eval($.apiUrl)" name="apiUrl" scope="default" type="STRING"/>
                <property expression="json-eval($.clientId)" name="clientId" scope="default" type="STRING"/>
                <property expression="json-eval($.refreshToken)" name="refreshToken" scope="default" type="STRING"/>
                <property expression="json-eval($.clientSecret)" name="clientSecret" scope="default" type="STRING"/>
                <property expression="json-eval($.hostName)" name="hostName" scope="default" type="STRING"/>
                <property expression="json-eval($.apiVersion)" name="apiVersion" scope="default" type="STRING"/>
                <property expression="json-eval($.registryPath)" name="registryPath" scope="default" type="STRING"/>
                <property expression="json-eval($.intervalTime)" name="intervalTime" scope="default" type="STRING"/>
                <salesforcerest.init>
                    <accessToken>{$ctx:accessToken}</accessToken>
                    <apiVersion>{$ctx:apiVersion}</apiVersion>
                    <hostName>{$ctx:hostName}</hostName>
                    <refreshToken>{$ctx:refreshToken}</refreshToken>
                    <clientSecret>{$ctx:clientSecret}</clientSecret>
                    <clientId>{$ctx:clientId}</clientId>
                    <apiUrl>{$ctx:apiUrl}</apiUrl>
                    <registryPath>{$ctx:registryPath}</registryPath>
                    <intervalTime>{$ctx:intervalTime}</intervalTime>
                </salesforcerest.init>
                <log level="full"/>
                <salesforcerest.themes/>
                <respond/>
            </inSequence>
            <outSequence/>
            <faultSequence/>
        </resource>
    </api>

    ```

3. Right-click on the Composite Application Project and click on **Export Project Artifacts and Run**. Select **Run on Micro Integrator**.
4. Micro Integrator will be started and the composite application will be deployed. You can further refer to the application deployed through the CLI tool. Make sure you first export the PATH as below.

    ```
    $ export PATH=/path/to/mi/cli/directory/bin:$PATH\
    ```

5. Login.

    ```
    ./mi remote login
    ```

6. Provide default credentials admin for both username and password.
7. In order to view the APIs deployed, execute the following command.

    ```
    ./mi api show
    ```

