---
title: Querying a DB and attaching results to an Email
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example application shows you how to query a MySQL database, aggregate the query results, transform the result into CSV format and send it as an attachment via email.


### Assumptions

This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/), WSO2 EI’s graphical developer tool. To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).


### Example use case
The XML data containing employee names is sent to the application using the HTTP POST method. The ForEach mediator then queries the MySQL DB individually for employee details. The result of each query is aggregated into a List. This List is then transformed to a CSV format and attached as a CSV file to an email which is sent using SMTP.

<p align="center">
  <img width="80%" src="../../../assets/img/migration-examples/querying-a-db-and-attaching-results-to-an-email-use-case.png">
</p>

### Set up and run the example

1. Install the MySQL server.

2. Download the JDBC driver for MySQL from [here](https://dev.mysql.com/downloads/connector/j/) and copy it to your <MI_HOME>/lib directory.

3. Start the MySQL server on your machine and create a connection by navigating to your mysql home directory and using the following command:
                  
        mysql  -u root -p

4. Now, run **db_script.sql** which is placed under src/test/resources to create a DB schema *company* and table *employees*

5. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

6. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

7. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

8. Browse and select the file path to the downloaded sample of this github project ("querying-a-db-and-attaching-results-to-an-email" folder of the downloaded github repository).<br>
    <img width="100%" src="../../../assets/img/migration-examples/querying-a-db-and-attaching-results-to-an-email.png">

9. Double-click the **DB Lookup** mediator and change the configuration according to your database configs.

10. Enable and configure **MailTo Transport** in Micro Integrator as explained [here](https://ei.docs.wso2.com/en/latest/micro-integrator/setup/transport_configurations/configuring-transports/#configuring-the-mailto-transport).

11. Double-click the **Address EndPoint** inside the **Clone Mediator** and update the email address after **mailto** with the email address, that you need to receive the email.

13. Run the sample by right click on the **QueryinngaDbAndAttachingResultsToAnEmailCompositeApplication** under the main project and selecting **Export Project Artifacts and Run**.

14. Make a POST request to localhost:8290/employees using **HTTP Client** in Integration Studio with the following xml code as the body:

        <root>
         <employees>
          <employee>Chava Puckett</employee>
          <employee>Quentin Puckett</employee>
          <employee>Mona Sosa</employee>
         </employees>
        </root>

15. Verify that you recieved an email with the attachment which is basically a csv file of the queried employee records.

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/querying-a-db-and-attaching-results-to-an-email.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go further
* Read about MailTo Transport [here](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/transport-parameters/mailto-transport-parameters/)
* Read more about DBLookup Mediator [here](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/dBLookup-Mediator/)
