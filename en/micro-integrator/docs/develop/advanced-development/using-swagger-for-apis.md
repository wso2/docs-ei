# Using Swagger Documents

API documentation is important to guide the users on what they can do using specific APIs. Follow the steps given below to use the Swagger documentation of your REST API artifacts.

## Swagger definition of a REST API

When you create a REST API artifact from WSO2 Integration Studio, a default Swagger definition is generated. You can also attach an additional custom Swagger definition for the API.

See [Creating a REST API](../../../develop/creating-artifacts/creating-an-api) for details.

## Enabling the Swagger processors

To expose the Swagger documents of REST APIs deployed in the Micro Integrator: 

1.  First, add the following server configurations to the `deployment.toml` file (stored in the `<MI_HOME>/conf` directory) of the Micro Integrator.

    ```toml
    [[server.get_request_processor]]
    item = "swagger.json"
    class = "org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerJsonProcessor"

    [[server.get_request_processor]]
    item = "swagger.yaml"
    class = "org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerYamlProcessor"
    ```

    The above configuration enables two Swagger processors:

    -   The `SwaggerJsonProcessor` provides access to the Swagger documentation as a JSON file (`swagger.json`).
    -   The `SwaggerYamlProcessor` provides access to the Swagger documentation as a YAML file (`swagger.yaml`).

2.  Restart the server to apply the configurations.

## Accessing Swagger docs

If you have enabled the Swagger processors in your server, and if your REST API is deployed, copy the following URLs (with your API details) to your browser:

!!! Note
    If you have a custom Swagger definition attached to the API, the following URLs will return the custom definition and not the default Swagger definition of the API.


-   To access the `swagger.json` file, use the following URL:

    ```bash
    http://<MI_HOST>:8290/<API_NAME>?swagger.json
    ```

    **Example**: 
    ```bash
    http://localhost:8290/HealthcareAPI?swagger.json
    ```

-   To access the `swagger.yaml` file, use the following URL:

    ```bash
    http://<MI_HOST>:8290/<API_NAME>?swagger.yaml
    ```

    **Example**: 
    ```bash
    http://localhost:8290/HealthcareAPI?swagger.yaml
    ```


!!! Tip
    -   Replace `<MI_HOST>` with `localhost`. If you are using a public IP, the respective IP address or domain needs to be specified. 
    -   Replace `<API_NAME>` with your API's name. The API name is case sensitive. Therefore, make sure to enter the API name correctly.
