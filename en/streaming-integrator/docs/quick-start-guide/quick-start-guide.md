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

