# Migrating from Mulesoft

Mulesoft provides an integration platform to connect SaaS and enterprise applications on the cloud or on-premise. If you wish to migrate your solutions from Mulesoft to WSO2 EI, you can easily do this by implementing the same use cases in WSO2 Integration Studio.

To help with this migration, see the following set of examples that are similar to [Mulesoft Anypoint Examples](https://github.com/mulesoft/anypoint-examples). You will find that these examples can be easily replicated in WSO2 Integration Studio. 

!!! Note 
    These examples are implemented with the following identical parameters.
    * Same payloads for input and output.
    * Same configuration values and variable names.
    * Similar Synapse configuration.

<table>
    <tr>
        <th>Example</th>
        <th>Description</th>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/addition-using-javascript-transformer">Addition Using JavaScript Transformer</td>
        <td>This application demonstrates how to parse an incoming JSON message and process it using the <a href="https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/script-Mediator/">Script mediator</a> with JavaScript code. This example was designed to demonstrate the ability of WSO2 Integration applications to interact with JSON payload.</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/authenticating-salesforce-using-oauth2">Authenticating Salesforce Using OAuth2</td>
        <td>This example shows you how to utilize the <a href="https://store.wso2.com/store/assets/esbconnector/details/43e44763-0d73-4ab3-8ae9-d6f73532d164">Salesforce REST Connector</a> to connect to Salesforce using OAuth as the security protocol.</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/content-based-routing">Content-based Routing</td>
        <td>Learn the basics of using Integration Studio to route messages in a flow by using a <a href="https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/switch-Mediator/">Switch mediator</a>.</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/data-handling-basics">Data-handling Basics</td>
        <td>Introduces basic data handling using the <a href="https://store.wso2.com/store/assets/esbconnector/details/5d6de1a4-1fa7-434e-863f-95c8533d3df2">File connector</a>.</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/extracting-data-from-LDAP-directory">Extracting Data from an LDAP Directory</td>
        <td>This example illustrates how to connect to an LDAP directory using WSO2 Micro Integrator and retrieve a list of users which is stored in that directory. We are using the <a href="https://store.wso2.com/store/assets/esbconnector/details/4ecf8dde-60f3-4e91-ba22-5f49a4e302f4">LDAP connector</a> to achieve this.</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/filtering-a-message">Filtering a Message</td>
        <td>This example shows how to use validation components within Anypoint Studio to validate an incoming message.</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/hello-world">Hello World</td>
        <td>This application demonstrates a simple HTTP request-response activity. WSO2 EI responds to end user calls submitted via a Web browser with a message that reads, "Hello World".</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/http-request-response-with-logger">HTTP Request-Response with Logger</td>
        <td>This example application demonstrat how to use WSO2 EI to build a simple HTTP request-response application. This example was designed to demonstrate the ability of a WSO2 Integration applications to interact with HTTP request and read parameters from the URL. </td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/implementing-a-choice-exception-strategy">Implementing an Exception Strategy</td>
        <td>This example illustrates the concept of error handling in WSO2 Enterprise Integrator. This particular example deals with exception strategy.</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/oauth2-authorization-code-using-the-http-connector">OAuth2 Authorization Code Using the HTTP Connector</td>
        <td>Often you are faced with a requirement to handle authorization to the third party service. This example application illustrates how to execute it using WSO2 EI.</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/proxying-a-rest-api">Proxying a REST API</td>
        <td>This example shows how to proxy your API. Applications send service requests to your proxy, which in turn calls the real API.</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/proxying-a-soap-api">Proxying a SOAP API</td>
        <td>This example shows how to proxy your API. Applications send service requests to your proxy, which in turn calls the real API.</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/querying-a-mysql-database">Querying a MySQL Database</td>
        <td>This example illustrates how to use the database connector to connect to a MySQL database.</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/sending-json-data-to-a-jms-queue">Sending JSON Data to a JMS Queue</td>
        <td>WSO2 offers components that are easy to use for connecting to JMS queues and topics. This example shows how to use Apache ActiveMQ, a leading open-source JMS implementation from Apache that supports the JMS 1.1 specification.</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/unittest-short-tutorial">Unit Testing</td>
        <td>This tutorial demonstrates the process of creating unit tests to validate the behaviour of the integration solutions developed via the Integration Studio.</td>
    </tr>
    <tr>
        <td><a href="https://github.com/wso2/integration-studio-examples/tree/master/migration/mule/upload-to-ftp-after-converting-json-to-xml">Converting JSON to XML and Uploading to FTP</td>
        <td>This example application illustrates the concept of data-mapping to convert JSON data to XML. It also shows you how to configure and use the <a href="https://store.wso2.com/store/assets/esbconnector/details/5d6de1a4-1fa7-434e-863f-95c8533d3df2">File connector</a> to upload a file to a FTP server.</td>
    </tr>
</table>
