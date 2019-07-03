# Using JSON with Data Services

You can send and receive JSON messages by default via WSO2 Enterprise
Integrator's (WSO2 EI) ESB profile. See the topics given below to
understand how data can be exposed in the JSON format, and how data can
be changed by sending JSON payloads. In this tutorial, you will use a
data service that exposes RDBMS data.

!!! tip

Before you begin!

If you have not tried the [Exposing a Datasource as a Data
Service](_Exposing_a_Datasource_as_a_Data_Service_) tutorial previously
follow the steps given below:

1.  Download the **RDBMSDataService** from
    [here](attachments/119133319/119133320.dbs) . Be sure to update your
    MySQL credentials in the dataservice file.
2.  Download the product installer from
    [here](http://wso2.com/integration/) , and run the installer.  
    Let's call the installation location of your product the
    **\<EI\_HOME\>** directory.

    If you installed the product using the **installer** , this is
    located in a place specific to your OS as shown below:

    <table style="width:100%;">
    <colgroup>
    <col style="width: 9%" />
    <col style="width: 90%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>OS</th>
    <th>Home directory</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Mac OS</td>
    <td><code>               /Library/WSO2/EnterpriseIntegrator/6.5.0              </code></td>
    </tr>
    <tr class="even">
    <td>Windows</td>
    <td><code>               C:\Program Files\WSO2\EnterpriseIntegrator\6.5.0\              </code></td>
    </tr>
    <tr class="odd">
    <td>Ubuntu</td>
    <td><code>               /usr/lib/wso2/EnterpriseIntegrator/6.5.0              </code></td>
    </tr>
    <tr class="even">
    <td>CentOS</td>
    <td><code>               /usr/lib64/EnterpriseIntegrator/6.5.0              </code></td>
    </tr>
    </tbody>
    </table>

3.  Download the JDBC driver for MySQL from
    [here](http://dev.mysql.com/downloads/connector/j/) . Unzip it, get
    the
    `           <MySQL_HOME>/mysql-connector-java-8.0.16.jar          `
    JAR, and place it in the `           <EI_HOME>/lib          `
    directory.

    !!! note

    If the driver class does not exist in the relevant folders when you
    create the datasource, you will get an exception, such as '
    `           Cannot load JDBC driver class com.mysql.jdbc.Driver          `
    '.


4.  Start the WSO2 ESB profile.

    -   [**On MacOS/Linux/CentOS**](#f5ecdeddd5744a68ac9e526e4ff0f791)
    -   [**On Windows**](#3e22f7fa7a604acca3d17e0aa7b3bd9f)

    Open a terminal and execute the following command:

    ``` java
        sudo wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

5.  Go to product's management console:
    `          https://localhost:9443/carbon         `
6.  Enter `          admin         ` as the username and password.
7.  Click **Add \> Data Service \> Upload** .
8.  Browse and add the `          RDBMSDataService.dbs         `
    file you downloaded.


-   [Map the output type to
    JSON](#UsingJSONwithDataServices-MaptheoutputtypetoJSON)
-   [GET data in JSON](#UsingJSONwithDataServices-GETdatainJSON)
-   [POST/UPDATE data using
    JSON](#UsingJSONwithDataServices-POST/UPDATEdatausingJSON)
    -   [Post data](#UsingJSONwithDataServices-Postdata)
    -   [Post data in
        batches](#UsingJSONwithDataServices-Postdatainbatches)
    -   [Update data](#UsingJSONwithDataServices-Updatedata)
    -   [Post data using Request
        Box](#UsingJSONwithDataServices-PostdatausingRequestBox)

A data service can expose data in one of the following formats: XML,
RDF, or JSON. You can select the required format by specifying
the output type for the data service query. To expose data in JSON, you
need to select JSON as the output type, and map the output to a JSON
template.

### Map the output type to JSON

The data service we uploaded or created previously, maps the output type
to XML. Follow the steps given below to change it to JSON.

1.  Open the ESB profile's Management Console using
    `                       https://localhost:9443/carbon                     `
    , and log in using `           admin          ` as the username and
    the password.

2.  Click **List** under **Main \> Services** . The **RDBMS** data
    service should be listed.
3.  Click the data service to open the **Service Dashboard** .
4.  Click **Edit Data Service (Wizard)** to open the data service using
    the **Create Data Service** wizard.
5.  Click **Next** until you get to the **Queries** screen.
6.  Edit the **GetEmployeeDetails** query.
7.  Change the Output Type to **JSON** .
8.  You can now start defining the JSON template for the output. Listed
    below are a few sample templates that you can use for this query.

    -   [**Simple JSON template**](#d9b86d0af47f4b20ad8a224f40202090)
    -   [**Define data types**](#69b16d0b86ce4949a97c8bcc3d7f8f5f)
    -   [**Nested queries**](#5351e44b06014c109f815827e1208f57)

    Shown below is a basic mapping.

    **Sample JSON Mapping**

    ``` javascript
        { "Employees":
              {"Employee":[
                {"EmployeeNumber":"$EmployeeNumber",                        
                 "Details": {
                  "FirstName":"$FirstName",
                  "LastName":"$LastName",
                  "Email":"$Email",
                  "Salary":"$Salary"
                 }
                }                  
              ]
            }            
        } 
    ```

        !!! tip
        **Note the following:**
    
        As shown in the sample given above, the column name values that are
        expected in the query result should be referred to by the column
        name with the "$" prefix. **E.g.** "$EmployeeNumber".
    
        Also, the structure of the JSON template should follow some
        guidelines in order to be compatible with the result. These
        guidelines are:
    
        -   The top most item should be a JSON object. It cannot be a JSON
            array.
        -   For handling multiple records from the result set, the immediate
            child of the top most object can be a JSON array, and the array
            should contain only a single object.
        -   If only a single result is returned, the immediate child of
            the top most object can be a single JSON object.
        -   After the immediate child of the top most object, there cannot
            be other JSON arrays in the mapping.
    
        All JSON responses are returned as an array.
    

    In a basic JSON output mapping, we specify the field values that we
    expect in the query result. You can give additional properties to
    this field mapping such as data type of the field, the possible
    content filtering user roles etc. These extended properties for the
    fields are given in parentheses <span class="underline">,</span>
    with a list of string tokens providing the additional properties,
    separated by a semicolon (";"). See the sample below.

    **Sample JSON Mapping**

    ``` javascript
        { "Employees":
              {"Employee":[
                {"EmployeeNumber":"$EmployeeNumber(type:integer)",                        
                 "Details": {
                  "FirstName":"$FirstName",
                  "LastName":"$LastName",
                  "Email":"$Email",
                  "Salary":"$Salary(requiredRoles:hr,admin)"
                 }
                }                  
              ]
            }            
        }
    ```

        !!! tip
        ##### Setting data types
    
        -   The 'data type' property is added to the
            `               EmployeeNumber              ` field using
            parentheses `               ()              ` .
        -   The extended property 'type' is given along with the value
            'integer'.
        -   The property and the value are separated by a colon.
        -   Note that the possible values for data type are "integer",
            "long", "double", and "boolean".
    
        ##### Content filtering (Required Roles)
    
        -   The 'data type' extended property, as well as the 'required
            roles' extended property, is added to the 'Salary' field using
            parentheses "()".
        -   The `               requiredRoles              ` property is
            added along with the values 'hr' and 'admin'.
        -   Note that the two values for the
            `               requiredRoles              ` property are
            separated by a comma.
        -   Also, the two extended properties are separated by a semicolon.
    

    If you want to write a **nested query** using JSON, see the tutorial
    on [working with nested queries](_Defining_Nested_Queries_) .

9.  Save the query.

### GET data in JSON

The RDBMSDataService that you are using already contains the following
resource:

|                 |                                                      |
|-----------------|------------------------------------------------------|
| Resource Path   | `             Employee/{EmployeeNumber}            ` |
| Resource Method | `             GET            `                       |
| Query ID        | `             GetEmployeeDetails            `        |

You can now RESTfully invoke the above resource. To send a JSON message
to a RESTful resource, you can simply add the “
`         Accept        ` : `         Application/json        ` ” to the
request header when you send the request. The service can be invoked in
REST-style via [curl](http://curl.haxx.se/) .  
Shown below is the curl command to invoke the GET resource:

``` java
    curl -X GET -H "Accept: application/json" http://localhost:8280/services/RDBMSDataService/Employee/{EmployeeNumber}
```

Example:

``` java
    curl -X GET -H "Accept: application/json" http://localhost:8280/services/RDBMSDataService/Employee/1
```

As a result, you receive the response in JSON format as shown below.

``` js
    {"Employees":{"Employee":[{"EmployeeNumber":"1","FirstName":"John","LastName":"Doe","Email":"JohnDoe@gmail.com","Salary":"10000"},{"EmployeeNumber":"1","FirstName":"John","LastName":"Doe","Email":"JohnDoe@gmail.com","Salary":"20000"}]}
```

### POST/UPDATE data using JSON

When a client sends a request to change data (POST/PUT/DELETE) in the
datasource, the HTTP header `         Accept        ` should be set to
`         application/json        ` .  Also, if the data is sent as a
JSON payload, the HTTP header `         Content-Type        ` should be
set to `         application/json        ` .

The RDBMSDataService that you are using already contains the following
resources for adding and updating data.

-   Resource for adding employee information:

    |                 |                                                   |
    |-----------------|---------------------------------------------------|
    | Resource Path   | `               Employee              `           |
    | Resource Method | `               POST              `               |
    | Query ID        | `               AddEmployeeDetails              ` |

-   Resource for updating employee information:

    |                 |                                                      |
    |-----------------|------------------------------------------------------|
    | Resource Path   | `               Employee              `              |
    | Resource Method | `               PUT              `                   |
    | Query ID        | `               UpdateEmployeeDetails              ` |

You can RESTfully invoke the above resource by sending HTTP requests as
explained below.

#### Post data

To post new employee information, you need to invoke the resource with
the POST method.

1.  First, create a file named
    `           employee-payload.json          ` , and define the JSON
    payload for posting new data as shown below.

    ``` java
            {
              "user_defined_value": {
                "EmployeeNumber" : "14001",
                "LastName": "Smith",
                "FirstName": "Will",
                "Email": "will@google.com",
                "Salary": "15500.0"
              }
            }
    ```

2.  On the terminal, navigate to the location where the
    **`            employee-payload.json           `** file is stored,
    and execute the following HTTP request:

    ``` java
            curl -X POST -H 'Accept: application/json'  -H 'Content-Type: application/json' --data "@employee-payload.json" -k -v http://localhost:8280/services/RDBMSDataService/Employee
    ```

#### Post data in batches

You are able to post JSON data in batches using the
`         RDBMSDataService        ` that you created or uploaded.

!!! info

To verify that batch requesting is enabled:

1.  Log in to the EI Management Console.
2.  Click **List** under **Main \> Services** and select the
    **RDBMSDataService** .
3.  Click the data service to open the **Service Dashboard** .
4.  Click **Edit Data Service (Wizard)** to open the data service using
    the **Create Data Service** wizard.
5.  See that the **Batch Requesting** check box is selected.


1.  First, create a file named
    **`            employee-batch-payload.json           `** , and
    define the JSON payload for posting multiple employee records
    (batch) as shown below.

    ``` java
        {
            "user_defined_value": {
                "user_defined_value": [
                    {
                        "EmployeeNumber": "5012",
                        "FirstName": "Will",
                        "LastName": "Smith",
                        "Email": "will@smith.com",
                        "Salary": "13500.0"
                    },
                    {
                        "EmployeeNumber": "5013",
                        "FirstName": "Parker",
                        "LastName": "Peter",
                        "Email": "peter@parker.com",
                        "Salary": "15500.0"
                    }
                ]
            }
        }
    ```

2.  On the terminal, navigate to the location where the
    **`            employee-batch-payload.json           `** file is
    stored, and execute the following HTTP request:

    ``` java
            curl -X POST -H 'Accept: application/json'  -H 'Content-Type: application/json' --data "@employee-batch-payload.json" -k -v http://localhost:8280/services/RDBMSDataService/Employee_batch_req
    ```

#### Update data

To update the existing employee records, you need to invoke the resource
with the PUT method.

1.  First, create a file named
    **`            employee-upload-update.json           `** , and
    define the JSON payload for updating an existing employee record as
    shown below.  
    For example, change the salary amount. Make sure that the employee
    number already exists in the database.

    ``` java
            {
              "user_defined_value": {
                "EmployeeNumber" : "1",
                "FirstName": "Will",
                "LastName": "Smith",
                "Email": "will@smith.com",
                "Salary": "78500.0"
              }
            }
    ```

2.  On the terminal, navigate to the location where the
    **`            employee-upload-update.json           `** file is
    stored, and execute the following HTTP request:

    ``` java
            curl -X PUT -H 'Accept: application/json'  -H 'Content-Type: application/json' --data "@employee-upload-update.json" -k -v http://localhost:8280/services/RDBMSDataService/Employee
    ```

#### Post data using Request Box

When the Request Box feature is enabled, you can invoke multiple
operations (consecutively) using one single operation. The process of
posting a JSON payload through a request box transaction is explained
below.

!!! info

To verify that batch requesting is enabled:

1.  Log in to the EI Management Console.
2.  Click **List** under **Main \> Services** and select the
    **RDBMSDataService** .
3.  Click the data service to open the **Service Dashboard** .
4.  Click **Edit Data Service (Wizard)** to open the data service using
    the **Create Data Service** wizard.
5.  See that the **Enable Boxcarring** check box is selected.


1.  First, create a file named
    **`            employee-request-box-payload           `**
    **`            .json           `** , and define the JSON payload for
    posting multiple employee records (batch) as shown below.

        !!! tip
    
        The following payload works for this use case. When you create
        payloads for different use cases, be mindful of the tips [given
        here](#UsingJSONwithDataServices-JSON_payloads) .
    

    ``` java
        {
         "request_box"  : { 
              "_postemployee" : {
                        "EmployeeNumber"  : "14005", 
                        "LastName" :  "Smith" ,
                        "FirstName" :  "Will" , 
                        "Email" :  "will@google.com" ,
                        "Salary" : "15500.0"
                                },
              "_getemployee_employeenumber":{
                        "EmployeeNumber"  : "14005"
                   }
            }
        }
    ```

2.  On the terminal, navigate to the location where the
    **`            employee-request-box-payload.json           `** file
    is stored, and execute the following HTTP request:

    ``` java
            curl -X POST -H 'Accept: application/json'  -H 'Content-Type: application/json' --data "@employee-request-box-payload.json" http://localhost:8280/services/RDBMSDataService/request_box
    ```

!!! tip

Creating JSON payloads for Request Box transactions

Note the following when you define a JSON payload for a request box
transaction: The object name specified in the payload must be in the
following format: " `         _<HTTP_METHOD><RESOURCE_PATH>        `
" where `         RESOURCE_PATH        ` represents the path value
specified in the data service resource. For example, if the
`         RESOURCE_PATH        ` is "employee", the payload object name
should be as follows:

-   For HTTP POST requests: `          _postemployee         `
-   For HTTP PUT requests: `          _putemployee         `

The child name/values of the child fields in the payload should be the
names and values of the input parameters in the target query.

**Handling a resource path with the "/" symbol**

If the `         RESOURCE_PATH        ` specified in the data service
contains the "/" symbol, be sure to replace the "/" symbol with the
underscore symbol ("\_") in the payload object name.

!!! note

**Important!** In this scenario, the `         RESOURCE_PATH        `
value should only contain simple letters. For example, the value can be
" `         /employee/add"        ` but not "
`         /Employee/Add"        ` .


For example, if the `         RESOURCE_PATH        ` is
`         /employee/add        ` , the payload object name should be as
follows:

-   For HTTP POST requests: `          _post_employee_add         `
-   For HTTP PUT requests: `          _put_employee_add         `

