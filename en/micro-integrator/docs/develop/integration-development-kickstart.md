# Developing Your First Integration Solution

Integration developers need efficient tools to build and test all the integration use cases required by the enterprise 
before pushing them into a production environment. 
The following topics will guide you through the process of building and running an example 
integration use case using WSO2 Integration Studio. 
This tool contains an embedded WSO2 Micro Integrator instance as well as other capabilities 
that allows you to conveniently design, develop, and test your integration artifacts before 
deploying them in your production environment.

## Use case

We are going to use the same use case we consired in the Quick Start Guide. 
In the Quick Start Guide we just run and executed the already built integration scenario. 
Here we are going to build the integration scenario from the scratch. Let’s recall the 
business scenario we are building.

![Integration Scenario](../assets/img/developing-first-integration/dev-first-integration-0.png)

The scenario is about a basic Health Care system where WSO2 Micro Integrator is used as the integration middleware. 
Most of the healthcare centers has a system which is used to make doctor appointments. 
To check the availability of the doctors for a particular time, 
users need to visit the hospitals or use each and every online system which is dedicated for a particular 
healthcare center. 
Here we are making the life of the patients easier by orchestrating those isolated systems in 
each healthcare providers and exposing a single interface to the users. 

Both Grand Oak service and Pine Valley service is exposed over HTTP protocol.
Grand Oak service accepts a GET request in following service endpoint url.


```bash
http://<HOST_NAME>:<PORT>/grandOak/doctors/<DOCTOR_TYPE>
```

Pine Vallery service accepts a POST request in the following service endpoint url. 
```bash
http://<HOST_NAME>:<PORT>/pineValley/doctors
```

and the expected payload should be in the following json format.
```bash
{
        "doctorType": "<DOCTOR_TYPE>"
}
```

Let’s implement a simple Rest API that can be used to query for availability of doctors for a particular category 
from all the available healthcare centers.



## Set up the workspace

In this guide, we will see how we can build integration solution using the Integration Studio and run it on a VM 
or a local machine.

1. [Download Micro Integrator](https://www.wso2.com/integration/micro-integrator) for your Operating System.

2. Download [curl](https://curl.haxx.se/) or a similar tool that can call an HTTP endpoint.


## Start backend mock services

Two mock hospital information services are available in the DoctorInfo.jar file located here. 

Open a terminal window and use the following command to start the services.

```bash
java -jar DoctorInfo.jar
```

You will see following get printed in the terminal

```bash
[ballerina/http] started HTTP/WS listener 0.0.0.0:9090
[ballerina/http] started HTTP/WS listener 0.0.0.0:9091
```

## Develop the integration artifacts

We use WSO2 Integration Studio to develop the integration artifacts. 

### Step 1: Create projects
 
In this step we create the project directories that are required for storing the various synapse artifacts that 
build our integration use case.

**ESB Solution project**

An ESB solution consists of one or several project directories. 
These directories store the various ESB artifacts that you create for your integration solution.

1. Open **WSO2 Integration Studio** and click **ESB Project** → **Create New** in the Getting Started view as shown below.

    ![Getting Started Dashboard](../assets/img/developing-first-integration/dev-first-integration-1.png)

2. In the **New ESB Solution Project** dialog that opens, enter a name for the ESB config project. 
Make sure to leave the check box for ‘Create Composite Application Project’ selected.

    ![ESB Solution Project Wizard](../assets/img/developing-first-integration/dev-first-integration-2.png)

3. Click **Finish** to save the projects. The ESB projects are listed in the project explorer as shown below.

    ![Project Explorer](../assets/img/developing-first-integration/dev-first-integration-3.png)

    The HealthcareConfigProject is the folder for storing integration (synapse) artifacts and the 
    HealthcareConfigProjectCompositeApplication folder is for the composite application project that is used for 
    packaging the integration artifacts. 

### Step 2: Create Endpoints

Endpoints are the logical representation in integration configuration for actual backend services.

1. Right click on the ‘HealthcareConfigProject’ and go to **New** → **Endpoint** to open the New Endpoint Artifact dialog.
    ![New Endpoint](../assets/img/developing-first-integration/dev-first-integration-4.png)
    
    Select **Create a New Endpoint** and click **Next**.

2.  Type a unique name for the endpoint, and then select the type to **HTTP Endpoint**.
    
    For ‘Grand Oak hospital service’ let’s use following configuration parameters 

    <table>
      <tr>
         <th>Parameter</th>
         <th>Value</th>
      </tr>
    
      <tr>
        <td>Endpoint Name</td>
        <td>GrandOakEndpoint</td>
      </tr>
    
      <tr>
        <td>Endpoint Type</td>
        <td>HTTP Endpoint</td>
      </tr>
    
      <tr>
        <td>URI Template</td>
        <td>http://localhost:9090/grandOak/doctors/{uri.var.doctorType}</td>
      </tr>
    
      <tr>
        <td>Method</td>
        <td>GET</td>
      </tr>
    </table>
   
    ![New Endpoint Wizard](../assets/img/developing-first-integration/dev-first-integration-5.png)

    Click **Finish** to save the endpoint configuration.

3. Do the same steps to create an endpoint for ‘Pine Valley Hospital’. Use following parameters.
   
    <table>
      <tr>
         <th>Parameter</th>
         <th>Value</th>
      </tr>
    
      <tr>
        <td>Endpoint Name</td>
        <td>PineValleyEndpoint</td>
      </tr>
    
      <tr>
        <td>Endpoint Type</td>
        <td>HTTP Endpoint</td>
      </tr>
    
      <tr>
        <td>URI Template</td>
        <td>http://localhost:9091/pineValley/doctors</td>
      </tr>
    
      <tr>
        <td>Method</td>
        <td>POST</td>
      </tr>
    </table>  
    
### Step 3: Create REST API

Here we are orchestrating multiple services and exposing a single API to the clients, 
the main integration artifact is going to be a REST API. 

1. Right-click the ‘HealthcareConfigProject’ project in the navigator and 
go to **New** → **REST API** to open the API Artifact Creation Options dialog box.
2. Select the **Create A New API** Artifact and click **Next**.
3. Specify values for the required REST API properties: API name, and Context.

    <table>
      <tr>
         <th>Parameter</th>
         <th>Value</th>
      </tr>
    
      <tr>
        <td>Name</td>
        <td>HealthcareAPI</td>
      </tr>
    
      <tr>
        <td>Context</td>
        <td>/healthcare</td>
      </tr>
    
    </table> 
    
    ![New API Wizard](../assets/img/developing-first-integration/dev-first-integration-6.png)
    
4. Click **Finish**. The REST API is created inside the src/main/synapse-config/api folder under the HealthcareConfigProject.
5. Open the new artifact from the project explorer. You will see the graphical representation of HealthcareAPI in the editor.

    ![API Resource](../assets/img/developing-first-integration/dev-first-integration-7.png)
    
    On the left of the editor you will see the mediator palette contains various kind of mediators 
    that can be dragged and dropped into the canvas of the Resource. 

6. Double click on the Resource. You will see the Properties view of the API Resource.

    ![Resource Properties](../assets/img/developing-first-integration/dev-first-integration-8.png)
    
    Specify values for the required Resource properties.

    <table>
      <tr>
         <th>Parameter</th>
         <th>Value</th>
      </tr>
    
      <tr>
        <td>Url Style</td>
        <td>URL_TEMPLATE</td>
      </tr>
    
      <tr>
        <td>Uri Template</td>
        <td>/doctor/{doctorType}</td>
      </tr>
      
      <tr>
        <td>Methods</td>
        <td>Get</td>
      </tr>
    
    </table>     

    Here in the **Uri Template**, {doctorType} is a uri variable that get evaluated to the path parameter value in the runtime. 
    We can access the value of the uri variable in the mediation flow using the variable (property) called ‘uri.var.doctorType’.

### Step 4: Build the mediation logic

1. Create two parallel message flows
In this scenario the Healthcare API receive a HTTP GET request and it is required to deliver into two different backend 
services. So we need to clone the message into two branches and process them in parallel. 
To do that we can use the **Clone Mediator**.

    Drag the **Clone Mediator** from the mediator palette and drop it into the request path (inSequence) of Resource canvas. 

   ![Clone](../assets/img/developing-first-integration/dev-first-integration-9.png)

    Right click on the Clone Mediator and select **Add/Remove Target..**. 
    In the **Add Target Branches** window, set Number of Branches to 2. 
    You will see two branches inside the Clone Mediator now.

   ![Clone Branches](../assets/img/developing-first-integration/dev-first-integration-10.png)

2. Invoke GrandOak Endpoint. 

    The **Call Mediator** is used to invoke a backend service. In the ESB configuration, 
    the backend service is represented using an Endpoint. In Step 2 we have already created an Endpoint to 
    represent the GrandOak Endpoint.

    Drag the Call Mediator from the mediator palette, into one branch of the Clone Mediator. 

   ![Call Mediator](../assets/img/developing-first-integration/dev-first-integration-11.png)
   
    Then Drag the already defined GrandOak Endpoint available under Defined Endpoints section of the palette into 
    the Call mediator area.

   ![Call Mediator EP](../assets/img/developing-first-integration/dev-first-integration-12.png)
   
3. Construct message payload for PineValley Endpoint

    Unlike the GranOAK endpoint which accept a simple GET request, the PineValley requires a POST request with 
    following JSON message. 

    ```bash
    {
        "doctorType": "<DOCTOR_TYPE>"
    }
    ```

    So we need to first construct the required message payload. To construct the messages there are several 
    Transformation mediators available in EI. Here we are going to use the PayloadFactory mediator.
    Drag the PayloadFactory mediator into the 2nd branch of the Clone Mediator. 

   ![PayloadFactory Mediator](../assets/img/developing-first-integration/dev-first-integration-13.png)

    Specify values for the required PayloadFactory properties.

    <table>
      <tr>
         <th>Parameter</th>
         <th>Value</th>
      </tr>
    
      <tr>
        <td>Payload Format</td>
        <td>Inline</td>
      </tr>
    
      <tr>
        <td>Media Type</td>
        <td>json</td>
      </tr>
      
      <tr>
        <td>Payload</td>
        <td>{
                  "doctorType": "$1"
            }
        </td>
      </tr>
      
      <tr>
        <td>Args</td>
        <td>$ctx:uri.var.doctorType</td>
      </tr>
    
    </table>

    Note the $1 in the Payload format. It denotes a parameter that can get a value assigned dynamically. 
    Value for the parameters need to assigned using Arguments **(Args)**. 
    **Args** can be added using the **PayloadFactoryArgument** dialog box which get appears when you click on the + sign.
    
    ![PayloadFactory Mediator Args](../assets/img/developing-first-integration/dev-first-integration-14.png)
    
    In the PayloadFactoryArgument dialog box select **Argument Type** to Expression and click on **Argument Expression**. 
    Then you will see the **Expression Selector** dialog box. 
    Enter **$ctx:uri.var.doctorType** as the value for the expression.
    
4. Invoke PineValley Endpoint
    
     Follow the same steps mentioned in ‘Invoke GrandOak Endpoint’ section to use Call mediator to invoke the PineVallery Endpoint.
    
5. Aggregating response messages
    
     Since we are cloning the messages and delivering into two different services, we will receive two responses. 
     So we need to aggregate those two responses and construct a single response. To do that we can use the Aggregate Mediator.
    
     Drag the Aggregate Mediator and drop it over right after the Clone Mediator like follows.
   
    ![Aggregate Mediator](../assets/img/developing-first-integration/dev-first-integration-15.png)
   
    Specify values for the required Aggregate Mediator properties.

    <table>
      <tr>
         <th>Parameter</th>
         <th>Value</th>
      </tr>
    
      <tr>
        <td>Aggregation Expression</td>
        <td>json-eval($.doctors.doctor)</td>
      </tr>
    
    </table>

6. Send a response back to the client 

    To send the response back to the client we can use the Respond Mediator.  
    Place the Respond Mediator inside the Aggregate mediator. 

    ![Respond Mediator](../assets/img/developing-first-integration/dev-first-integration-16.png)
    
The final mediation configuration looks similar to the above diagram.     
Following is what you will see in the Source View.
    
    
<details>
        <summary>HealthcareAPI</summary>
	    ```xml
            <?xml version="1.0" encoding="UTF-8"?>
            <api context="/healthcare" name="HealthcareAPI" xmlns="http://ws.apache.org/ns/synapse">
                <resource methods="GET" uri-template="/doctor/{doctorType}">
                    <inSequence>
                        <clone>
                            <target>
                                <sequence>
                                    <call>
                                        <endpoint key="GrandOakEndpoint"/>
                                    </call>
                                </sequence>
                            </target>
                            <target>
                                <sequence>
                                    <payloadFactory media-type="json">
                                        <format>{
                                                          "doctorType": "$1"
                                                       }
                                        </format>
                                        <args>
                                            <arg evaluator="xml" expression="$ctx:uri.var.doctorType"/>
                                        </args>
                                    </payloadFactory>
                                    <call>
                                        <endpoint key="PineValleyEndpoint"/>
                                    </call>
                                </sequence>
                            </target>
                        </clone>
                        <aggregate>
                            <completeCondition>
                                <messageCount max="-1" min="-1"/>
                            </completeCondition>
                            <onComplete expression="json-eval($.doctors.doctor)">
                                <respond/>
                            </onComplete>
                        </aggregate>
                    </inSequence>
                    <outSequence/>
                    <faultSequence/>
                </resource>
            </api>
	    ```    
</details>    
    
## Test the integration scenario

There are several ways that you can test the integration scenario. 

### Deploy integration artifacts and run 

#### Option1: Run from the Integration Studio

1. Right click on the HealthcareConfigProject goto **Run As** → **Run on Micro Integrator**.

    ![Run in built-in MI](../assets/img/developing-first-integration/dev-first-integration-17.png)

2. You will see the following dialog box. 
   Select the HealthcareConfigProject in the Artifact list and click **Finish**.

   ![Run in built-in MI 2](../assets/img/developing-first-integration/dev-first-integration-18.png)

    You will see the MicroIntegrator get started and several logs get printed in the Integration Studio console.
  
#### Option 2: Export artifacts, deploy and run the Micro Integrator

##### Export artifacts
To export the artifacts as a deployable CAR file, Right click on the the 
HealthcareConfigProjectProjectCompositeApplication and select Export Composite Application Project.

   ![Export CAR](../assets/img/developing-first-integration/dev-first-integration-19.png)
   
#### Deploy the Healthcare service

Copy the above exported CAR file of the Healthcare service to the MI_HOME/repository/deployment/server/carbonapps directory.

#### Start the Micro Integrator
Follow the steps relevant to your OS:

On MacOS/Linux/CentOS, open a terminal and execute the following commands:
sudo wso2mi-1.1.0

On Windows, go to Start Menu -> Programs -> WSO2 -> Micro Integrator. 
This will open a terminal and start the relevant profile.
   
### Invoke the Healthcare service

Open a terminal and execute the following curl command to invoke the service:
```bash
curl -v http://localhost:8290/healthcare/doctor/Ophthalmologist
```

Upon invocation, you should be able to observe the following response:

```bash
[
    [
        {
            "name": "Geln Ivan",
            "time": "05:30 PM",
            "hospital": "pineValley"
        },
        {
            "name": "Daniel Lewis",
            "time": "05:30 PM",
            "hospital": "pineValley"
        }
    ],
    [
        {
            "name": "Shane Martin",
            "time": "07:30 AM",
            "hospital": "Grand Oak"
        },
        {
            "name": "Geln Ivan",
            "time": "08:30 AM",
            "hospital": "Grand Oak"
        }
    ]
]
```

## What's Next


- [Running on Docker](../page-not-found).
- [Running on Kubernetes](../page-not-found).
- [Writing an unit test for integration artifacts](creating-unit-test-suite).
