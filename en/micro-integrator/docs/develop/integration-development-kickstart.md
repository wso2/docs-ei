# Developing Your First Integration Solution

Integration developers need efficient tools to build and test all the integration use cases required by the enterprise before pushing them into a production environment. The following topics will guide you through the process of building and running an example integration use case using WSO2 Integration Studio. This tool contains an embedded WSO2 Micro Integrator instance as well as other capabilities that allows you to conveniently design, develop, and test your integration artifacts before deploying them in your production environment.

We are going to use the same use case we used in the Quick Start Guide. In the Quick Start Guide we just run and execute the already built integration scenario. Here we are going to build the integration scenario from the scratch. Let’s recall the business scenario we are building.

![alt text](../../assets/img/quick-start-guide/MI-quick-start-guide.png)

The scenario is about a basic healthcare system where WSO2 Micro Integrator is used as the integration middleware. Most of the healthcare centers have a system that is used to make doctor appointments. To check the availability of the doctors for a particular time, users need to visit the hospitals or use each and every online system that is dedicated for a particular healthcare center. Here we are making the life of the patients easier by orchestrating those isolated systems in each healthcare providers and exposing a single interface to the users. 

Both Grand Oak service and Pine Valley service is exposed over HTTP protocol.

Grand Oak service accepts a GET request in following service endpoint URL.

```bash
http://<HOST_NAME>:<PORT>/grandOak/doctors/<DOCTOR_TYPE>
```

Pine Valley service accepts a POST request in the following service endpoint URL. 

```bash
http://<HOST_NAME>:<PORT>/pineValley/doctors
```

The expected payload should be in the following JSON format.

```json
{
        "doctorType": "<DOCTOR_TYPE>"
}
```

Let’s implement a simple Rest API that can be used to query for availability of doctors for a particular category from all the available healthcare centers.

## Set up the workspace

In this guide, we will see how we can build an integration solution using the Integration Studio and run it on a VM or a local machine.

- Install [WSO2 Integration Studio](https://wso2.com/integration/tooling/)

- Install [curl](https://curl.haxx.se/)

## Start backend mock services

Two mock hospital information services are available in the DoctorInfo.jar file located [here](https://github.com/samgnaniah/docs-ei/tree/master/en/micro-integrator/docs/assets/attach/quick-start-guide). 

Open a terminal window and use the following command to start the services.


java -jar DoctorInfo.jar


You will see following get printed in the terminal


[ballerina/http] started HTTP/WS listener 0.0.0.0:9090
[ballerina/http] started HTTP/WS listener 0.0.0.0:9091

Develop the integration artifacts

We use WSO2 Integration Studio to develop the integration artifacts. 

Step 1: Create projects
 
In this step we create the project directories that are required for storing the various synapse artifacts that build our integration use case.

ESB Solution project
An ESB solution consists of one or several project directories. These directories store the various ESB artifacts that you create for your integration solution.

Open WSO2 Integration Studio and click ESB Project → Create New in the Getting Started view as shown below.

### What's next?

-   Use [WSO2 Micro Integrator's CLI Tool](../administer-and-observe/using-the-command-line-interface.md) to check the integration artifacts deployed in the Micro Integrator instance.
-   Use **Prometheus** to [monitor your Micro Integrator](../administer-and-observe/monitoring_with_prometheus.md) instance.
