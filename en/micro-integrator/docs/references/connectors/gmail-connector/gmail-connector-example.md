# Gmail Connector Example

The Gmail connector allows you to access the Gmail REST API through WSO2 ESB. Gmail is a free, Web-based e-mail service provided by Google. It allows you to send, read, and delete emails through the Gmail REST API. Furthermore, it provides the ability to read, trash, untrash, and delete threads, create, update and detele drafts, get the Gmail profile, and access the mailbox history as well, while handling OAuth 2.0 authentication. 

## What you'll build

<img src="/assets/img/connectors/GmailConnector.png" title="Using Gmail Connector" width="800" alt="Using Gmail Connector"/>

This example demonstrates a scenario where a customer feedback Gmail account of a company can be easily managed using the WSO2 Gmail Connector. This application contains a service that can be invoked through an HTTP GET request. Once the service is invoked, it returns the contents of unread emails in the Inbox under the label of Customers, while sending an automated response to the customer, thanking them for their feedback. The number of emails that can be handled in a single invocation is specified in the application.

## Configure the connector in WSO2 Integration Studio
1. Follow these steps to set up the ESB Solution Project and the Connector Exporter Project. 
{!references/connectors/importing-connector-to-integration-studio.md!}

2. Right click on the created ESB Solution Project and select, -> **New** -> **Rest API** to create the REST API. 
    <img src="/assets/img/connectors/adding-an-api.png" title="Adding a Rest API" width="800" alt="Adding a Rest API"/>

3. Follow these steps to configure the Gmail API and obtain the Client Id, Client Secret, Access Token and Refresh Token. 
{!references/connectors/gmail-connector/configuring-gmail-api.md!}

4. Provide the API name as **SendMails**. You can go to the source view of the xml configuration file of the API and copy the following configuration. 
```
<?xml version="1.0" encoding="UTF-8"?>
<api context="/sendmails" name="SendMails" xmlns="http://ws.apache.org/ns/synapse">
    <resource methods="GET">
        <inSequence>
            <gmail.init>
                <userId></userId>
                <accessToken></accessToken>
                <apiUrl>https://www.googleapis.com/gmail</apiUrl>
                <clientId></clientId>
                <clientSecret></clientSecret>
                <refreshToken></refreshToken>
            </gmail.init>
            <gmail.listAllMails>
                <includeSpamTrash>false</includeSpamTrash>
                <maxResults>20</maxResults>
                <q>is:unread label:customers</q>
            </gmail.listAllMails>
            <iterate expression="json-eval($.messages)">
                <target>
                    <sequence>
                        <sequence key="reply"/>
                    </sequence>
                </target>
            </iterate>
            <respond/>
        </inSequence>
        <outSequence/>
        <faultSequence/>
    </resource>
</api>
```

5. Right click on the created ESB Solution Project and select, -> **New** -> **Sequence** to create the defined sequence called **reply**. 

6. Provide the Sequence name as **reply**. You can go to the source view of the xml configuration file of the API and copy the following configuration. 
```
<?xml version="1.0" encoding="UTF-8"?>
<sequence name="reply" trace="disable" xmlns="http://ws.apache.org/ns/synapse">
    <property expression="json-eval($.id)" name="msgId" scope="default" type="STRING"/>
    <gmail.getAccessTokenFromRefreshToken>
        <clientId></clientId>
        <clientSecret></clientSecret>
        <refreshToken></refreshToken>
    </gmail.getAccessTokenFromRefreshToken>
    <gmail.readMail>
        <id>{$ctx:msgId}</id>
    </gmail.readMail>
    <property expression="json-eval($.payload.headers[6].value)" name="response" scope="default" type="STRING"/>
    <log level="custom">
        <property expression="get-property('response')" name="response1"/>
    </log>
    <gmail.getAccessTokenFromRefreshToken>
        <clientId></clientId>
        <clientSecret></clientSecret>
        <refreshToken></refreshToken>
    </gmail.getAccessTokenFromRefreshToken>
    <gmail.sendMail>
        <to>{$ctx:response}</to>
        <subject>Best of Europe - 6 Countries in 9 Days</subject>
        <from>isurumuy@gmail.com</from>
        <messageBody>Thank you for your valuable feedback.</messageBody>
    </gmail.sendMail>
</sequence>
```
7. In the Rest API and in the Sequence, provide your obtained **Client ID**, **Client Secret**, **Access Token** and **Refresh Token** accordingly. The **userID** should be your gmail address. 

8. Follow these steps to export the artifacts. 
{!references/connectors/exporting-artifacts.md !}

## Deployment
Follow these steps to deploy the exported CApp in the Enterprise Integrator Runtime. 
{!references/connectors/deploy-capp.md!}

## Testing
Invoke the API as shown below using the curl command. Curl Application can be downloaded from [here](https://curl.haxx.se/download.html).

```
  curl -H "Content-Type: application/json" --request GET http://localhost:8290/sendmails
```

The senders should receive an email with a subject of "Best of Europe — 6 Countries in 9 Days", and a body of "Thank you for your valuable feedback."

