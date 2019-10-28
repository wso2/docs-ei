# Generating Swagger Documents for REST APIs

API documentation is important to guide the users on what they can do using specific APIs. Follow the steps given below to generate Swagger documentation for the REST APIs you created in the Micro Integrator.

!!! Info
    You need to have created a REST API to view the documentation for that API.

## Step 1: Configure the Micro Integrator

1.  Add the following configuration in `MI_HOME/conf/deployment.toml` file to generate the Swagger JSON or YAML files.
    ```toml
    [[server.get_request_processor]]
    item = "swagger.json"
    class = "org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerJsonProcessor"

    [[server.get_request_processor]]
    item = "swagger.yaml"
    class = "org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerYamlProcessor"
    ```
2.  Open the terminal, navigate to the `<MI_HOME>/bin` directory and start the Micro Integrator.

## Step 2: Generate the Swagger docs

### JSON

Enter the following on the browser and the JSON content will appear on the browser screen: http://<EI_HOST>:8280/<API_NAME>?swagger.json

- By default, <MI_HOST> is `localhost`. However, if you are using a public IP, the respective IP address or domain needs to be specified.
- Enter the APIs name for <API_NAME>.

!!! Note
    The API name is case sensitive. Therefore, make sure to the enter the API name correctly. Else, the JSON file will not download.

Example: http://localhost:8290/HealthcareAPI?swagger.json

### YAML

Enter the following on the browser and the YAML file will download to your machine: http://<MI_HOST>:8290/<API_NAME>?swagger.yaml

- By default, <MI_HOST> is localhost. However, if you are using a public IP, the respective IP address or domain needs to be specified.
- Enter the APIs name for <API_NAME>.

!!! Note
    The API name is case sensitive. Therefore, make sure to the enter the API name correctly. Else, the YAML file will not download.

Example: http://localhost:8290/HealthcareAPI?swagger.yaml

## Step 3: Copy JSON/YAML to Swagger editor

Copy the content of the JSON or YAML file to the Swagger Editor or any other tool and check out the documentation of the API.
