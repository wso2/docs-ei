# Creating an API

Follow the instructions given below to create a new [REST API](../../../references/synapse-properties/rest-api-properties) artifact in WSO2 Integration Studio.

## Instructions

### Step 1: Create new API

1. Right-click the project in the navigator and go to **New → REST API** to open the **API Artifact Creation Options** dialog box.

    <img src="../../../assets/img/create_artifacts/new-artifact.png">

2.  Select the **Create A New API Artifact** and click **Next**.

    <img src="../../../assets/img/create_artifacts/new_api/create-new-api-option.png" width="500">

3.  Specify values for the [required REST API properties](../../../references/synapse-properties/rest-api-properties/#rest-api-properties-required): **API name**, and **Context**.

    <img src="../../../assets/img/create_artifacts/new_api/new-api-artifact-dialog.png" width="500">

4. Do one of the following to save the API:  
    - To save the API in an existing ESB Config project in your workspace, click **Browse** and select that project.
    - To save the API in a new ESB Config project, click **Create new Project** and create the new project.
5.  Click **Finish**. The REST API is created inside the `src/main/synapse-config/api` folder under the ESB Config project you specified.
6. Open the new artifact from the project explorer. Select the **Design View** to view the API artifact with the default **Resource**.

    <img src="../../../assets/img/create_artifacts/new_api/api-artifact-design-view.png" width="500">

7. Click the API to open the <b>Properties</b> tab and update any [optional REST API properties](../../../references/synapse-properties/rest-api-properties/#rest-api-properties-optional).

    <img src="../../../assets/img/create_artifacts/new_api/api-properties.png" width="700">

### Step 2: Add new API Resources (Optional)

When you created the API, an API resource is created by default. If you want to add a new resource, click **API Resource** in the Tool pallet of the API section and simply drag and drop the resource to the REST API.

!!! Info
    **About the default API Resource**
    
    Each API can have at most one default resource. Any request received
        by the API but does not match any of the enclosed resource
        definitions will be dispatched to the default resource of the API.
        In case of API\_3, a DELETE request on the URL “/payments” will be
        dispatched to the default resource as none of the other resources in
        API\_3 are configured to handle DELETE requests. If you go to the
        Source view, the default resource will be as follows: 

    ```xml
    <api context="/healthcare" name="HealthcareAPI" xmlns="http://ws.apache.org/ns/synapse">
        <resource methods="GET">
            <inSequence/>
            <outSequence/>
            <faultSequence/>
        </resource>
    </api>
    ```    

### Step 3: Update API Resource Properties

Open the REST API artifact and go to the **Design** view of the API Resource, click the **Resource** icon to enable the **Properties** tab. 

<img src="../../../assets/img/create_artifacts/new_api/api-resource-properties.png" width="700">

You can now update the [API Resource properties](../../../references/synapse-properties/rest-api-properties/#rest-api-resource-properties).

## Examples

-   [Using a Simple Rest API](../../../use-cases/examples/rest_api_examples/introduction-rest-api)
-   [Working with Query Parameters](../../../use-cases/examples/rest_api_examples/setting-query-params-outgoing-messages)
-   [Exposing a SOAP Endpoint as a RESTful API](../../../use-cases/examples/rest_api_examples/enabling-rest-to-soap)
-   [Exposing Non-HTTP Services as RESTful APIs](../../../use-cases/examples/rest_api_examples/configuring-non-http-endpoints)
-   [Handling Non Matching Resources](../../../use-cases/examples/rest_api_examples/handling-non-matching-resources)
-   [Handling HTTP Status Codes](../../../use-cases/examples/rest_api_examples/setting-https-status-codes)
-   [Manipulating Content Types](../../../use-cases/examples/rest_api_examples/transforming-content-type)
-   [Securing a REST API](../../../use-cases/examples/rest_api_examples/securing-rest-apis)
-   [Using GET with a Message Body](../../../use-cases/examples/rest_api_examples/using-get-with-a-body)
-   [Using POST with Empty Message Body](../../../use-cases/examples/rest_api_examples/using-post-with-no-body)
-   [Using POST with Query Parameters](../../../use-cases/examples/rest_api_examples/using-post-with-query-param)