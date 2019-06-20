# Quick Start Guide

WSO2 Enterprise Integrator (WSO2 EI) is a comprehensive solution that allows you to seamlessly integrate applications, services, data, and business processes to support modern enterprise integration requirements. 

For this quick start guide, let's consider a basic Health Care System where WSO2 EI is used as the integration software. In this guide, an external party (a patient) wants to make a doctor's reservation at a given hospital.

> Before you begin,
>   1. Get a [free trial subscription](#https://wso2.com/subscription/free-trial) that enables you to download the product with the latest updates. Then, download the product installer from [here](#https://wso2.com/integration/install/download/), and run the installer. 
>
>   2. Download the [back-end service](#https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0.jar) and copy it to the `<EI_HOME>/wso2/msf4j/deployment/microservices` directory. 
>      The back-end service is now deployed in the MSF4J profile, which will run microservices for your integration flows. 
>   3. Start the MSF4J profile:
>      1.  Open a terminal and execute the following command:
>      `wso2ei-6.4.0-msf4j`
>      2. Go to **Start Menu** -> **Programs** -> **WSO2** -> **Enterprise Integrator 6.4.0 MSF4J**. This will open a terminal and start the MSF4J profile.
>   4. If you are on a Windows OS, install cURL. For more information, see the [cURL Releases and Downloads](#https://curl.haxx.se/download.html).

Let's get started!

## Routing requests based on message content

This is a 5-minute guide to give you a quick overview of how WSO2 EI mediates and routes messages from a front-end service (client) to a back-end service.

Start the ESB profile:
The WSO2 EI product consists of many profiles. The ESB profile is used to manage short-running and stateless integration. For more information, see the [WSO2 EI Introduction](Overview/Introduction).

Open a terminal and execute the following command:
wso2ei-6.4.0-integrator


Go to Start Menu -> Programs -> WSO2 -> Enterprise Integrator 6.4.0 Integrator. This will open a terminal and start the ESB profile.



Open the ESB profile Management Console using https://localhost:9443/carbon, and log in using admin as the username and the password.
The Management Console provides a UI to configure WSO2 EI. WSO2 Carbon is the core platform on which WSO2 middleware products are built. 

Download the SampleServicesCompositeApplication_1.0.0.car file from GitHub.


This file is a Carbon Application Archive (CAR file) containing the integration artifacts you will use in this tutorial, including:
An API resource, which acts as the endpoint that accepts incoming requests from a client, routes them to the back-end service for processing, receives a response from the service, and sends the response back to the client.
A Switch Mediator to route messages based on the message content to the relevant HTTP Endpoint defined in the ESB.
A Log Mediator to log the message that the switch mediator is routing the message to the correct endpoint.
A Send Mediator to send the appointment request to the correct hospital endpoint.
WSO2 Enterprise Integrator 6.4.0 > Quick Start Guide > in-sequence.png
If you want to try out this same guide by configuring the artifacts, try out the Routing Requests Based on Message Content tutorial.



 Deploy the SampleServicesCompositeApplication_1.0.0.car as follows:
On the Main tab of the Management Console, go to Manage > Carbon Applications and click Add. 
Click Choose File, select the SampleServicesCompositeApplication_1.0.0.car file that you downloaded, and click Upload. 
Refresh the page to see the Carbon application you just added in the Carbon Applications List screen.
 WSO2 Enterprise Integrator 6.4.0 > Quick Start Guide > carbon-app.png
Sending requests to WSO2 EI
We are now ready to request a doctor's appointment at Grand Oak Community Hospital.  
Create a JSON file named request.json with the following payload to specify the details the back-end service needs to make the appointment: patient information, doctor name, hospital name, and appointment date. 
{
  "patient": {
    "name": "John Doe",
    "dob": "1940-03-19",
    "ssn": "234-23-525",
    "address": "California",
    "phone": "8770586755",
    "email": "johndoe@gmail.com"
  },
  "doctor": "thomas collins",
  "hospital": "grand oak community hospital",
  "appointment_date": "2025-04-02"
}

If you want to request a different hospital, you can specify one of the following hospital names instead.
clemency medical center
pine valley community hospital

Open a terminal, navigate to the directory where you have saved the request.json file, and execute the following cURL command.
curl -v -X POST --data @request.json http://localhost:8280/healthcare/categories/surgery/reserve --header "Content-Type:application/json"

This command sends the JSON payload you created in the previous step to the API resource http://localhost:8280/healthcare/categories/surgery/reserve, which was included in the CAR file you uploaded. The API resource contains the logic for routing appointment requests to the back-end service you deployed in the microservices directory. 
You get the following response:
{"appointmentNumber":1,"doctor":{"name":"thomas collins","hospital":"grand oak community hospital","category":"surgery","availability":"9.00 a.m - 11.00 a.m","fee":7000.0},"patient":{"name":"John Doe","dob":"1940-03-19","ssn":"234-23-525","address":"California","phone":"8770586755","email":"johndoe@gmail.com"},"fee":7000.0,"confirmed":false,"appointmentDate":"2025-04-02"}

Now check the terminal window and you see the following message: INFO - LogMediator message = Routing to grand oak community hospital
Congratulations, you have successfully completed this guide! 
In this tutorial, you have seen how you can create a request payload and send it to an endpoint in WSO2 EI, which routes the message to a back-end service and then sends a response back to the client. 
For more information on the artifacts used in the section, try out the Routing Requests Based on Message Content tutorial.
Want to know more and evaluate WSO2 EI further? See the Other capabilities of WSO2 EI section given below.

Other capabilities of WSO2 EI
Try out the following use cases to understand other capabilites of WSO2 EI.
Exposing a datasource as a service

Let's look at how we can check the availability of doctors in the healthcare service by sending a request. To make things simple, you set up an already configured database that has the doctors details and exposethisdatabase as a data service using WSO2 EI. 
Follow the steps given below to check the doctors that are available without interacting with the database itself:
Set up the back-end database
First, let's set up a back-end database for our healthcare service. Follow the steps below to create the database.
Download the dataServiceSample.zip file from GitHub and extract it to a location on your computer.
Let's refer to the extracted  dataServiceSample  directory as <QSG_HOME>, which contains the following:
A DB script to create the back-end database (DATA_SERV_QSG) with the channeling information of the healthcare service.
A pre-packaged data service (DOCTORS_DataService.dbs file), which can expose the back-end database as a service.

Open a terminal, navigate to the <QSG_HOME> directory, and execute the following command:
ant -Ddshome=PATH_TO_EI_HOME

The  DATA_SERV_QSG  database is now created in the <EI_HOME>/samples/data-services/database directory with information of all available doctors in the healthcare service.
Expose the database as a data service
Now, let's start the ESB profile and upload the sample data service:
Start the ESB profile:
If you have started the ESB profile previously, stop the server by pressing  Ctrl+C and restart it using the following commands.



Open a terminal and execute the following command:
wso2ei-6.4.0-integrator


Go to Start Menu -> Programs -> WSO2 -> Enterprise Integrator 6.4.0 Integrator. This will open a terminal and start the ESB profile.



In your Web browser, navigate to the WSO2 EI management console using the following URL: https://localhost:9443/carbon/        
Log in to the Management Console using the following credentials:
Username: admin
Password: admin
Go to the Main tab and click Data Service > Upload. 
 WSO2 Enterprise Integrator 6.4.0 > Quick Start Guide > upload.png
Upload the DOCTORS_DataService.dbs file from the <QSG_HOME> directory.
Refresh the page to see the deployed data service in the Deployed Services screen.
WSO2 Enterprise Integrator 6.4.0 > Quick Start Guide > deployed-doctors-service.png
The database of the channeling service is now exposed through the DOCTORS_DataService data service, which we just deployed in WSO2 EI.
Request doctors' information
Assume that you want information on the availability of all surgeons. Open a terminal and execute the following command. Note that we are specifying 'surgery' as the specialty.
curl -v http://localhost:8280/services/DOCTORS_DataService/getDoctors?SPECIALITY=surgery
The information about the availability of all surgeons will be published on your terminal:
<DOCTORSLIST xmlns="http://ws.wso2.org/dataservice">
<DOCTOR>
    <NAME>thomas collins</NAME>
    <HOSPITAL>grand oak community hospital</HOSPITAL>
    <SPECIALITY>surgery</SPECIALITY>
    <AVAILABILITY>9.00 a.m - 11.00 a.m</AVAILABILITY>
    <CHARGE>7000</CHARGE>
</DOCTOR>
<DOCTOR>
    <NAME>anne clement</NAME>
    <HOSPITAL>clemency medical center</HOSPITAL>
    <SPECIALITY>surgery</SPECIALITY>
    <AVAILABILITY>8.00 a.m - 10.00 a.m</AVAILABILITY>
    <CHARGE>12000</CHARGE>
</DOCTOR>
<DOCTOR>
    <NAME>seth mears</NAME>
    <HOSPITAL>pine valley community hospital</HOSPITAL>
    <SPECIALITY>surgery</SPECIALITY>
    <AVAILABILITY>3.00 p.m - 5.00 p.m</AVAILABILITY>
    <CHARGE>8000</CHARGE>
</DOCTOR>
</DOCTORSLIST>
Congratulations, you have successfully sent a request to the data service!
Want to know more and evaluate WSO2 EI further? See the other tutorials under the Other capabilities of WSO2 EI section.



Guaranteeing message delivery

Now, instead of sending the request directly to the back-end service, you store the request message in the Message Broker profile of WSO2 EI. You use a Message Processor to retrieve the message from the store and then deliver the message to the back-end service. Store and forward messaging is used for serving traffic to back-end services that can accept request messages only at a given rate. This is also used for guaranteed delivery to ensure that request received never gets lost since they are stored in the message store and also available for future reference. 
Let's get started!
Configuring WSO2 EI
Open the <EI_HOME>/conf/jndi.properties file and add the following line after the queue.MyQueue = example.MyQueue line:
queue.PaymentRequestJMSMessageStore=PaymentRequestJMSMessageStore

Start the Message broker profile:


Open a terminal and execute the following command:
wso2ei-6.4.0-broker


Go to Start Menu -> Programs -> WSO2 -> Enterprise Integrator 6.4.0 Broker. This will open a terminal and start the Message Broker profile.



Download the back-end service from GitHub. This will process appointment requests and copy it to the <EI_HOME>/wso2/msf4j/deployment/microservices directory. The back-end service is now deployed in the MSF4J profile, which will run microservices for your integration flows. 
Start the MSF4J profile to send requests to the back-end service:
If you have started the MSF4J profile previously, skip this step.



Open a terminal and execute the following command:
wso2ei-6.4.0-msf4j


Go to Start Menu -> Programs -> WSO2 -> Enterprise Integrator 6.4.0 MSF4J. This will open a terminal and start the MSF4J profile.


The Healthcare service is now active and you can start sending requests to the service.
Start the ESB profile:
If you have started the ESB profile previously, skip this step.



Open a terminal and execute the following command:
wso2ei-6.4.0-integrator


Go to Start Menu -> Programs -> WSO2 -> Enterprise Integrator 6.4.0 Integrator. This will open a terminal and start the ESB profile.



Open the ESB profile Management Console using https://localhost:9443/carbon, and log in using admin as the username and the password.
Download the SampleServicesCompositeApplication_1.0.0.car file from GitHub.
This file is a Carbon Application Archive (CAR file) containing the integration artifacts you will use in this tutorial, including:
An API resource, which acts as the endpoint that accepts incoming requests from a client, routes them to the back-end service for processing, receives a response from the service, and sends the response back to the client.
The back-end service that processes the appointment requests.
A Switch Mediator to route messages based on the message content to the relevant HTTP Endpoint defined in the ESB.
A Store Mediator to enqueues messages passing through its mediation sequence in a given message store.

Deploy the SampleServicesCompositeApplication_1.0.0.car file as follows:
Note:  If you already deployed the SampleServicesCompositeApplication_1.0.0.car file when following the first part of the QSG, make sure to go to  Manage > Carbon Applications > List and delete it before proceeding to the below steps.
On the Main tab of the management console, go to Manage > Carbon Applications and click Add. 
Click Choose File, select the SampleServicesCompositeApplication_1.0.0.car file that you downloaded, and click Upload. 
Refresh the page to see the carbon application you just added in the Carbon Applications List screen.
 WSO2 Enterprise Integrator 6.4.0 > Quick Start Guide > carbon-app.png
Sending requests to WSO2 EI
Create a JSON file named request.json with the following payload to specify the details the back-end service needs to make the appointment: patient information, doctor name, hospital name, and appointment date. 
{
"name": "John Doe",
"dob": "1940-03-19",
"ssn": "234-23-525",
"address": "California",
"phone": "8770586755",
"email": "johndoe@gmail.com",
"doctor": "thomas collins",
"hospital": "grand oak community hospital",
"cardNo": "7844481124110331"
}

Open a command line terminal and execute the following command from the location where request.jsonfile you created is saved:
curl -v -X POST --data @request.json http://localhost:8280/healthcare/categories/surgery/reserve --header "Content-Type:application/json"

This command sends the JSON payload you created in the previous step to the API resource http://localhost:8280/healthcare/categories/surgery/reserve, which was included in the CAR file you uploaded. The API resource contains the logic for routing appointment requests to the back-end service you deployed in the microservices directory. 
You will see the response as follows:
{"message":"Payment request successfully submitted. Payment confirmation will be sent via email."}

Check the terminal window and you see that the response from SettlePaymentEP is logged as follows:
[2018-06-07 11:46:48,936] [EI-Core]  INFO - LogMediator message = Routing to grand oak community hospital

[2018-06-07 11:46:48,949] [EI-Core]  INFO - TimeoutHandler This engine will expire all callbacks after GLOBAL_TIMEOUT: 120 seconds, irrespective of the timeout action, after the specified or optional timeout

[2018-06-07 11:46:52,003] [EI-Core]  INFO - LogMediator To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:995e37e3-8900-4da8-8eea-67da91de2c12, Direction: request, Envelope: <?xml version='1.0' encoding='utf-8'?><soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"><soapenv:Body><jsonObject><appointmentNo>1</appointmentNo><doctorName>thomas collins</doctorName><patient>John Doe</patient><actualFee>7000.0</actualFee><discount>20</discount><discounted>5600.0</discounted><paymentID>eb83d9cc-0230-4dc8-a613-33b419cebbd7</paymentID><status>Settled</status></jsonObject></soapenv:Body></soapenv:Envelope>

Congratulations, you have successfully sent a message to the WSO2 ESB profile!
For more information on the artifacts used in the section, you can try out the Storing and Forwarding Messages tutorial.



Defining a BPMN process

In this section, a simple BPMN process is used. It prints out a 'Hello World!' message when the process instance is initiated.
Download the HelloWorldServiceTask-1.0.0.jar file from GitHub. This includes the defined BPMN process.
Copy the JAR file to the <EI_HOME>/lib directory.
Start the BPS profile:
The WSO2 EI product has many profiles. long-running, stateful business processes are run using the Business Process profile. For more information, see the WSO2 EI Overview.



Open a terminal and execute the following command:
wso2ei-6.4.0-business-process


Go to Start Menu -> Programs -> WSO2 -> Enterprise Integrator 6.4.0 Business Process. This will open a terminal and start the Business Process profile.



In a new browser window or tab, open the EI-Business Process Management Console: https://localhost:9445/carbon/
Log in to the EI-Business Process Management Console using  admin  for both the username and password.
Download the HelloWorld.bar file from GitHub.
In the Management Console, navigate to the main tab, click BPMN, and upload the HelloWorld.bar file. 
Refresh the page to see the file you just added
Log into the BPMN-explorer at https://localhost:9445/bpmn-explorer using admin for both the username and password.
Go to the PROCESS tab and click Start to start the Hello World Process.
 WSO2 Enterprise Integrator 6.4.0 > Quick Start Guide > BPS_BPMNExplorer.jpg
In the terminal, the "Hello World ...!!!" string is printed out. 
You have successfully defined a BPMN process and initiated it!
For more information on the artifacts used in this section, try out the Creating a BPMN Process tutorial.
WSO2 EI offers much more! Check out the Additional section and explore the product.


Additional
In comparison with the conventional ESB profile, the start-up time of the Micro Integrator profile is considerably less, which makes it container-friendly, and thereby, is ideal to use it for microservices in a container-based environment. For more information, try out the  Sending a Simple Message to a Service Using the Micro Integratortutorial.
Work with WSO2 EI connectors.
For all the connectors supported by WSO2 EI, see WSO2 ESB Connectors.
Use the Gmail connector in WSO2 EI by trying out the Using the Gmail Connector tutorial.
All the CAR files that were used in this tutorial was developed using EI Tooling. You can try it out by following the tutorials listed under Integration Tutorials.
Use the EI Analytics to analyze the mediation statistics. For more information, try out the tutorial on Using the Analytics Dashboard.
