---
title: Sending a CSV file through email using SMTP
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---


This example shows you how to use the MailTo Transport with SMTP to facilitate information transfer through email. It also illustrates how you can use the File connector to input a CSV file and then transform it using Data Mapper mediator.

### Assumptions

This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/), WSO2 EI’s graphical developer tool. To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case

In this example, a CSV file containing sample sales data which is stored in the local directory is converted to the collection of Maps using the Data Mapper mediator and is sent to an email address using the MailTo transport. The Data Mapper mediator computes the total price for each order by multiplying the unit price with the number of units.

<p align="center">
  <img width="50%" src="../../../assets/img/migration-examples/sending-a-csv-through-email-using-smtp-use-case.png">
</p>

### Set Up and Run the Example

Follow the steps in this procedure to create and run this example in your own instance of Integration Studio. You can create template applications straight out of the box in Integration Studio and tweak the configurations of the use case-based templates to create your own customized applications in WSO2 Integrator.

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

4. Browse and select the file path to the downloaded sample of this github project ("sending-a-csv-file-through-email-using-smtp" folder of the downloaded github repository).

5. Copy the **input.csv** under **sending-a-csv-file-through-email-using-smtp/SendingACsvThroughEmailUsingSmtp/src/test/resources** in to any location in your file system.

6. Open the **FileReadingSequence.xml** under **sending-a-csv-file-through-email-using-smtp/SendingACsvThroughEmailUsingSmtp/src/main/synapse-config/sequences** directory. 

7. Double click on the **File mediator** to access its property view. 

8. In the property view under **Connector Parameters** double click on **Call Template Parameter - source**.

9. In the popup window, replace the value of  **Parameter Value** text box with the path of the **input.csv** that you copied in step 5, and click Finish.

10. In the design view of the  **FileReadingSequence.xml**, double click on the **Address Endpoint** under **Send Mediator** to access its properties view.

11. In the **URI** text box, replace the email address after **mailto:** with the email address that you need to receive emails.<br>
    <img width="70%" src="../../../assets/img/migration-examples/sending-a-csv-through-email-using-smtp.png">

12. Enable and configure **MailTo Transport** in Micro Integrator as explained [here](https://ei.docs.wso2.com/en/latest/micro-integrator/setup/transport_configurations/configuring-transports/#configuring-the-mailto-transport).

13. Run the sample by right click on the **SendingACsvThroughEmailUsingSmtpCompositeApplication** under the main project and selecting **Export Project Artifacts and Run**.

14. Login to the reciever email you specify to verify if the sales data was received via email. You should get an email that has the following content:

        [
		  {
		    "name": "T-shirt",
		    "orderId": "1",
		    "pricePerUnit": "10",
		    "units": "2",
		    "totalPrice": 20
		  },
		  {
		    "name": "Jacket",
		    "orderId": "2",
		    "pricePerUnit": "5",
		    "units": "4.15",
		    "totalPrice": 20.75
		  }
		]

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/sending-a-csv-through-email-using-smtp.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Read about MailTo Transport [here](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/transport-parameters/mailto-transport-parameters/)
* Read about File Connector [here](https://docs.wso2.com/display/ESBCONNECTORS/File+Connector)
* Read about Data Mapper mediator [here](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/data-Mapper-Mediator/)
