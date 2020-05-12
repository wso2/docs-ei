---
title: XML Only SOAP Web Service
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example illustrates how to expose a SOAP Web service using a custom WSDL. Here we have used a Registry project to save all the custom WSDL files and XSD files which are needed to expose the back-end services. This example is based on the use case of patient admission into a hospital.

### Assumptions ###

This document assumes that you are familiar with WSO2 EI and the 
[Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/overview/quick-start-guide/). To 
increase your familiarity with Integration Studio, consider completing one or more 
[WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case

This example demonstrates service orchestration(SOAP operations) and content-based(payload) routing within the context of a simple use case: to facilitate patient pre-admission into a hospital. Here, the hospital has exposed a SOAP web service called AdmissionService which could be used for following use cases.

1. For new patients, create a new patient record(EHR) using EHR SOAP web service and create an episode on an EHR to initiate a patient's admission into the hospital. 
2. For an existing patient, get the patient id and locate to existing EHR and create a new episode.

Finally, AdmissionService will respond to the Episode and Billing details as the admitSubjectResponse.
![AdmitPatientService](../../../assets/img/migration-examples/mainSequence.png?raw=true "AdmitPatientService")

### Set Up and Run the Example with Integration Studio

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).
2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.
3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.
4. Browse and select the file path to the downloaded sample of this Github project 
(`integration-studio-examples/migration/mule/xml-only-soap-webservice`) and click **finish**.
5. Run the sample by right click on the **XMLonlySOAPserviceCompositeApplication** under the main 
**xml-only-soap-webservice** project and selecting **Export Project Artifacts and Run**.
6. Open HTTP Client in Integration Studio. Follow [HTTP Client Guidelines](../../../docs/common/adding-http-client-to-integration-studio.md)
to open HTTP Client if the window is not visible in the interface.
7. Make a POST request to `http://localhost:8290/services/xml-only-soap-webservice`.
8. Add following `SOAPAction` and `Content-Type` headers.
    - SOAPAction: http://wso2.org/hospital-admission-service/admitSubject
    - Content-Type: text/xml
9. Add the following input payload.
    ```xml
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns="http://wso2.org/hospital-admission-service" xmlns:ns1="http://wso2.org/hospital-admission-service/modal">
       <soapenv:Header/>
       <soapenv:Body>
          <ns:admitSubject>
             <ns1:Referer>
                <clientId>clientid</clientId>
             </ns1:Referer>
             <ns1:Referral>
                <procedure>
                   <code>3333</code>
                   <admission>Elective</admission>
                   <department>New</department>
                </procedure>
             </ns1:Referral>
             <ns1:Subject>
                <nationalId>3123123</nationalId>
                <firstName>Dave</firstName>
                <lastName>Duck</lastName>
                <address1>address</address1>
                <nationality>nationality</nationality>
                <gender>Male</gender>
                <dateOfBirth>1990-12-04</dateOfBirth>
             </ns1:Subject>
          </ns:admitSubject>
       </soapenv:Body>
    </soapenv:Envelope>
    ```
10. Following response can be observed in the HTTP Response pane.
    ```xml
    <?xml version='1.0' encoding='UTF-8'?>
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns0="http://wso2.org/hospital-admission-service" xmlns:ns1="http://wso2.org/hospital-admission-service/modal">
        <soapenv:Body>
            <ns0:admitSubjectResponse>
                <ns1:Episode>
                    <episodeId xmlns="http://ws.apache.org/ns/synapse">4/29/20 4:08 PM</episodeId>
                    <ns1:PatientId>4/29/20 4:08 PM</ns1:PatientId>
                    <admission xmlns="http://ws.apache.org/ns/synapse">Elective</admission>
                    <startDate xmlns="http://ws.apache.org/ns/synapse">4/29/20 4:08 PM</startDate>
                    <endDate xmlns="http://ws.apache.org/ns/synapse">4/29/20 4:08 PM</endDate>
                    <care xmlns="http://ws.apache.org/ns/synapse">Duck</care>
                </ns1:Episode>
                <ns1:Bill>
                    <costPerNight xmlns="http://ws.apache.org/ns/synapse">100</costPerNight>
                    <initialStateEstimate xmlns="http://ws.apache.org/ns/synapse">5</initialStateEstimate>
                    <runningTotal xmlns="http://ws.apache.org/ns/synapse">500</runningTotal>
                    <status xmlns="http://ws.apache.org/ns/synapse">ADMITTED</status>
                </ns1:Bill>
            </ns0:admitSubjectResponse>
        </soapenv:Body>
    </soapenv:Envelope>
    ```

### Set Up and Run the Example with SoapUI
1. Follow the steps 1 to 5 as in the **Set Up and Run the Example with Integration Studio** section.
2. Create a new SOAP project in SoapUI interface.
3. Add the initial WSDL path as `http://localhost:8290/services/xml-only-soap-webservice?wsdl` and create the project.
4. Expand the folders to reveal Request 1 in created SOAP project. Double-click Request 1 to open the request-response window.
5. Click the submit request icon (green "play" button at upper left) to submit the request to WSO2 EI server.
6. SoapUI displays the response from the WSO2 EI server in the response pane.

### Go Further

* Learn more about [Publishing a Custom WSDL](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/examples/proxy_service_examples/publishing-a-custom-wsdl/).
* Read more on [WSO2 Proxy Services](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/examples/proxy_service_examples/Introduction-to-Proxy-Services/)
