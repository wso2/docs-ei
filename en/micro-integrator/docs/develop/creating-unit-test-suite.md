# Creating Unit Test Suite

Once you have developed an integration solution, WSO2 Integration Studio allows you to build unit tests for your integration configurations.

With Unit Test you can do the following things:
- Test Sequence artifacts with multiple test cases
- Test Proxy-Service artifacts with multiple test cases
- Test API artifacts with multiple test cases
- Create mock-services for actual endpoints
- Test artifacts with Registry resources
- Test artifacts with Connector resources

## Create Unit Test Suite

1.  Open the WSO2 Integration Studio interface.
2.  Open an existing project which has your integration solution.
3.  Right-click on **test** folder parallel to the src folder and navigate to **New** and click **Unit Test Suite** as shown in below.

    ![Create Unit Test Suite](../../assets/img/create_project/synapse_unit_test/create-test-suite.png)

    The **New Unit Test Suite** wizard opens.
    
4.  Select the **Create a New Unit Test Suite** and click **Next**.

    ![Select Create Method](../../assets/img/create_project/synapse_unit_test/select-create-method.png)
    
5.  Specify a name for the unit test suite and select the artifact file which you want to test from the tree-based file list and click **Next**. (You can only select one sequence, proxy service or API artifact file per unit test suite).

    ![Fill Unit Test Suite Details](../../assets/img/create_project/synapse_unit_test/select-main-artifact.png)
    
6.  Select supportive artifact files from the list which are using in the previously selected artifact file and click **Next**.

    ![Select Supportive Artifacts](../../assets/img/create_project/synapse_unit_test/select-supportives.png)
    
7.  Select created mock-service files from the list, if you want to mock an existing endpoint with a created mock service. Then click **Finish**. (Click Create a New Mock Service link to create a new Mock Service or see [Create Mock Service](#create-mock-service) section)

    ![Select Mock Services](../../assets/img/create_project/synapse_unit_test/select-mock-services.png)

    Once you have created a Unit Test Suite in WSO2 Integration Studio, you can find it inside the test folder with your given name. You can update the Unit Test Suite by adding test cases and changing the existing supportive artifacts and mock-services.

Follow the steps given below,

1.  First, open Unit Test Suite from the project explorer (You can use either the design view or the source view to update the unit test suite).

    ![Unit Test Form](../../assets/img/create_project/synapse_unit_test/unit-test-form.png)
    
2.  In design view, press (+) button under the **Test Artifact, Test Cases and Assertion Details** section to add a new **test case** to the unit test suite. 

    ![Add Test Case](../../assets/img/create_project/synapse_unit_test/add-test-case.png)
    
3.  Enter information in the wizard as follows,
    
    1.  Enter a name for the test case.
    2.  In **Input Payload and Properties** section, enter the following details:
        -   **Input Payload**: Input payload of the test case. This can be a type of JSON, XML or plain text.
        -   **Input properties**: Input properties of the test case. There are three types of properties allowed in Unit Testing to test. **Synapse($ctx)**, **Axis2($axis2)**, and **Transport($trp)** properties. For the sequences, the suite allows to add all type of properties with the value and for the APIs and Proxy-services, it only allows to add transport properties.
        -   For the APIs, can see an extra two compulsory fields called **Request Path** and **Request Method** in the this section. **Request Path** indicates the URL-mapping of the API resource and **Request Method** indicates the REST method type of the resource.   
    
    3.  In **Assertions** section, can add multiple assertion to the test case with two types of assertions. **AssertEquals** check whether the equality of mediated result and expected value's and **AssertNotNull** check whether the mediated result is not null.
    
        ![Add Assertions](../../assets/img/create_project/synapse_unit_test/add-assertion.png)
    
        -   **Assertion Type**: Type of the assertion.
        -   **Actual Expression**: Expression that you want to assert.
            -   **$body** - assert the payload<br/>
            -   **$ctx:<property_name>** - assert synapse property
            -   **$axis2:<property_name>** - assert axis2 property
            -   **$trp:<property_name>** - assert transport property
                
        -   **Expected Value**: Expected value for the actual expression. Type can be a JSON, XML or a plain text.  
        -   **Error Message**: Error message to print when the assertion is failed.
    4.  Once you have added at least one assertion required details, click **Add**. Then save the unit test suite.
    
## Run Unit Test Suites

You can run the created Unit Test Suites using embedded unit testing server in Micro Integrator runtime packed inside the WSO2 Integration Studio. You can right-click on the **test** directory and click **Run Unit Test** to run all the unit test suites at once or right-click on the particular unit test suite and click **Run Unit Test** to run that unit test suite alone.

The **Unit Test Run Configuration** wizard opens. Select one of server run configuration method you want to proceed with Unit Test.

![Run Unit Test Suite](../../assets/img/create_project/synapse_unit_test/run-test.png)

1.  **Local Server Configuration** - Run the Unit Test Suite(s) in embedded unit testing Server in Micro Integrator or local unit testing server.
    -   **Executable Path**: Path to the unit testing server.
    -   **Server Test Port**: Port of the unit testing server.
    
2.  **Remote Server Configuration** - Run the Unit Test Suite(s) in remote unit testing server.
    -   **Server Remote Host**: Host IP of the remote unit testing server.
    -   **Server Test Port**: Port of the remote unit testing server.
    
    Once you entered the required details, click **Run**. It will start the unit testing server in the console and prints the summary report for the given unit test suite(s) using the response from the unit testing server.

    ![Output Console](../../assets/img/create_project/synapse_unit_test/console-log.png)    
## Create Mock Service

Mock services give the opportunity to simulate the actual endpoint behaviors. If you want to mock an particular endpoint follow the steps given below.

1.  Open an existing project which has your integration solution.
2.  Right-click on **test** folder parallel to the src folder and navigate to **New** and click **Mock Service** as shown in below. 

    ![Create Mock Service](../../assets/img/create_project/synapse_unit_test/create-mock.png) 
    
3.  Select the **Create a New Mock Service** and click **Next**.

    ![Select Create Mock Service Method](../../assets/img/create_project/synapse_unit_test/select-mock-method.png) 
    
4.  In the **Create a new Mock Service** page, enter the following details:
    
    ![Mock Service Details](../../assets/img/create_project/synapse_unit_test/mock-details.png) 
    
    -   **Name of the Mock Service**: A name for the mock service.
    -   **Mocking Endpoint Name**: Endpoint name which wants to mock.
    -   **Mock Service Port**: Port for the mock service.
    -   **Mock Service Context**: Main context for the service starts with '/'.

5.  Add multiple resources for the mock service as needed. To add multiple resources click (+) icon on top of the resources table.   
    -   **Service Sub Context**: Sub context of the resource starts with '/'.
    -   **Service Method**: REST method type of the resource.
    
6.  Fill the **Expected Request to the Mock Service Resource** section if you want to mock an endpoint based on the coming request headers or payload.
    
    ![Mock Service Resource Request Details](../../assets/img/create_project/synapse_unit_test/resource-request.png) 
    
    -   **Header Name**: Expected request header name.
    -   **Header Value**: Expected request header value.
    -   **Expected Request Payload**: Expected request payload to the service.
    
    **Note**: Entered request headers/payload must me matched with the request to send the response a this mock service.
    
7.  Fill the **Response Send Out from the Mock Service Resource** section to get a response from the service.

    ![Mock Service Resource Response Details](../../assets/img/create_project/synapse_unit_test/resource-response.png) 
    
    -   **Response Status Code**: Response status code of the mock service.
    -   **Header Name**: Response request header name.
    -   **Header Value**: Response request header value.
    -   **Send Out Response Payload**: Expected response payload from the service.   
  
Once you have entered the required details, click **Add**. It will list the resource under the **Add Service Resource** table with Sub Context and Method. After that click **Finish** to create a Mock Service.
It will locate under the ```test → resources → mock-services``` directory.

![Mock Service Form](../../assets/img/create_project/synapse_unit_test/mock-service-form.png)     