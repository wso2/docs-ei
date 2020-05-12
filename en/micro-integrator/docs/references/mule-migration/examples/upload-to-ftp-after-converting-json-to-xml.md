---
title: Upload to FTP after converting JSON to XML
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example application illustrates the concept of data-mapping to convert JSON data to XML. It also shows you how to 
configure and use the [File connector](https://store.wso2.com/store/assets/esbconnector/details/5d6de1a4-1fa7-434e-863f-95c8533d3df2) to upload a file to a FTP server.

### Assumptions ###

This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/), WSO2 EI’s graphical developer tool. To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case
In this example JSON data is sent to the application and then converted to the XML format using the messageType property. Then the message payload is uploaded to the FTP folder.

<img width="60%" src="../../../assets/img/migration-examples/upload-to-ftp-after-converting-json-to-xml-use-case.png">

### Set Up and Run the Example

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

4. Browse and select the file path to the downloaded sample of this Github project (``integration-studio-examples/migration/mule/upload-to-ftp-after-converting-json-to-xml``) and click **Finish**.

5. Lets add the file connector into the workspace. Right click on the **UploadToFtpAfterConvertingJsonToXml** and select **Add or Remove Connector**. Keep the **Add connector** option selected and click **Next>**. Search for 'file' using the search bar and click the download button located at the bottom right corner of the file connector. Click **Finish**.

6. Open the **UploadToFtpAfterConvertingJsonToXml.xml** under **upload-to-ftp-after-converting-json-to-xml/UploadToFtpAfterConvertingJsonToXml/src/main/synapse-config/api/** directory. Configure destination property accordingly.<br>
    <img width="60%" src="../../../assets/img/migration-examples/upload-to-ftp-after-converting-json-to-xml.png">

7. Run the sample by right click on the **UploadToFtpAfterConvertingJsonToXmlCompositeApplication** under the main **upload-to-ftp-after-converting-json-to-xml** project and selecting **Export Project Artifacts and Run**.

8. Open HTTP Client in Integration Studio. Follow [HTTP Client Guidelines](../../../docs/common/adding-http-client-to-integration-studio.md) to open HTTP Client if the window is not visible in the interface.

9. Make a POST request to *http://localhost:8290/upload* with following JSON message body, and setting the `Content-Type` header to `application/json`:
    ```json
    {
        "employees": {
            "employee": [
                {
                    "name": "John",
                    "lastName": "Doe",
                    "addresses": {
                        "address": [
                            {
                                "street": "123 Main Street",
                                "zipCode": "111"
                            },
                            {
                                "street": "987 Cypress Avenue",
                                "zipCode": "222"
                            }
                        ]
                    }
                },
                {
                    "name": "Jane",
                    "lastName": "Doe",
                    "addresses": {
                        "address": [
                            {
                                "street": "345 Main Street",
                                "zipCode": "111"
                            },
                            {
                                "street": "654 Sunset Boulevard",
                                "zipCode": "333"
                            }
                        ]
                }
            ]
        }
    }
    ```

10. Verify that the file `miExample.xml` was uploaded to the `upload` folder on your FTP server.

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/upload-to-ftp-after-converting-json-to-xml.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Learn more about [file connector](https://docs.wso2.com/display/ESBCONNECTORS/Working+with+the+File+Connector#WorkingwiththeFileConnector-append).
* Read more on [WSO2 connectors](https://docs.wso2.com/display/ESBCONNECTORS/WSO2+ESB+Connectors+Documentation)
