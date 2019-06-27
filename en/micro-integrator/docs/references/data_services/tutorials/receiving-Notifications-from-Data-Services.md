# Receiving Notifications from Data Services

You can create an event trigger from a query as explained below.

-   [Introduction](#ReceivingNotificationsfromDataServices-Introduction)
    -   [Input event
        trigger](#ReceivingNotificationsfromDataServices-Inputeventtrigger)
    -   [Output event
        trigger](#ReceivingNotificationsfromDataServices-Outputeventtrigger)
-   [Get started!](#ReceivingNotificationsfromDataServices-Getstarted!)
    -   [Enabling notifications for a query in a data
        service](#ReceivingNotificationsfromDataServices-Enablingnotificationsforaqueryinadataservice)
    -   [Invoking the data
        service](#ReceivingNotificationsfromDataServices-Invokingthedataservice)

### Introduction

Eventing support is provided by the WS-Eventing Web services standard.
When a data service request or response triggers an event, the
subscribers listening to those events receive notifications. The
criteria for triggering an event as well as the destination to which the
event notifications should be sent are defined per data service query.
 When a certain event-trigger is activated, emails will be sent to all
the respective subscribers. You can set an event trigger as an input or
an output event trigger in a data service query. Let's look at the usage
of the two approaches.

#### Input event trigger

When an input event trigger is applied to a query, the event trigger is
evaluated when the parameters are received by the query. An
event-trigger executes an XPath expression against an XML element. This
XML element is built from the input parameters that are represented by
the element of the parameters wrapped by an element with the name of the
query.

For example, take the following query:

``` html/xml
    <query id="incrementEmployeeSalaryQuery" useConfig="default">
       <sql>update Employees set salary=salary+? where employeeNumber=?</sql>
       <param name="increment" paramType="SCALAR" sqlType="DOUBLE" type="IN" ordinal="1" />
       <param name="employeeNumber" paramType="SCALAR" sqlType="INTEGER" type="IN" ordinal="2" />
    </query>
```

Assuming that the values of `         increment        ` and
`         employeeNumber        ` are `         value1        ` and
`         value2        ` respectively, the XML element to be evaluated
with the XPath expression is as follows. Also, note that this XML
element doesn't have any namespaces associated with it.

``` html/xml
    <incrementEmployeeSalaryQuery>
       <increment>value1</increment>
       <employeeNumber>value2</employeeNumber>
    </incrementEmployeeSalaryQuery>
```

#### Output event trigger

In this case, the event trigger is evaluated when a specific query is
returning its result. There isn't a specific creation of an XML element
that is used with the XPath expression like in the input event trigger.
But the full result XML is used to evaluate it. The result will be
namespace qualified. Therefore, you must write the XPath expressions
accordingly.

------------------------------------------------------------------------

### Get started!

Follow the steps given below.

!!! tip

-   Download the product installer from
    [here](http://wso2.com/integration/) , and run the installer.  
    Let's call the installation location of your product the
    **\<EI\_HOME\>** directory.

    If you installed the product using the **installer** , this is
    located in a place specific to your OS as shown below:

    <table style="width:100%;">
    <colgroup>
    <col style="width: 9%" />
    <col style="width: 90%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>OS</th>
    <th>Home directory</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Mac OS</td>
    <td><code>                /Library/WSO2/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    <tr class="even">
    <td>Windows</td>
    <td><code>                C:\Program Files\WSO2\EnterpriseIntegrator\6.5.0\               </code></td>
    </tr>
    <tr class="odd">
    <td>Ubuntu</td>
    <td><code>                /usr/lib/wso2/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    <tr class="even">
    <td>CentOS</td>
    <td><code>                /usr/lib64/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    </tbody>
    </table>

-   Update email configurations: Open the
    **`            <EI_HOME>/conf/axis2/axis2_client.xml           `**
    file and add the following XML configuration:

    ``` java
        <transportSender name="mailto" class="org.apache.axis2.transport.mail.MailTransportSender">
           <parameter name="mail.smtp.host">smtp.gmail.com</parameter>
           <parameter name="mail.smtp.port">587</parameter>
           <parameter name="mail.smtp.starttls.enable">true</parameter>
           <parameter name="mail.smtp.auth">true</parameter>
           <parameter name="mail.smtp.user">{EMIAL_USERNAME}</parameter>
           <parameter name="mail.smtp.password">{EMAIL_PASSWORD}</parameter>
           <parameter name="mail.smtp.from">{EMAIL_ADDRESS}</parameter>
        </transportSender>
    ```
    
    -   Set up a datasource:
    
        1.  Install the MySQL server.
        2.  Download the JDBC driver for MySQL from
            [here](http://dev.mysql.com/downloads/connector/j/) and copy it
            to your `             <EI_HOME>/lib            ` directory.
    
        3.  Create the following database:  Company
    
        ``` java
                CREATE DATABASE Company;
        ```
        
            4.  Create the ACCOUNT table in the Company database:
        
        ``` java
                    USE Company;
            
                    CREATE TABLE ACCOUNT(AccountID int NOT NULL,Branch varchar(255) NOT NULL, AccountNumber varchar(255),AccountType ENUM('CURRENT', 'SAVINGS') NOT NULL,Balance FLOAT,ModifiedDate DATE,PRIMARY KEY (AccountID));
        ```
            
                5.  Enter the following data into the ACCOUNT table:
            
        ``` java
                    INSERT INTO ACCOUNT VALUES (1,"AOB","A00012","CURRENT",231221,'2014-12-02');
        ```
            

#### Enabling notifications for a query in a data service

Let's create a data service using the **Create Data Service** wizard:

1.  Start or restart the ESB profile of WSO2 Enterprise Integrator.

        !!! info
    
        You need to restart the ESB profile for the changes you made to the
        `           axis2_client.xml          ` file to reflect in the
        server.
    

    -   [**On MacOS/Linux/CentOS**](#12ff4f4baf1a42c493db98bfaddfb6a0)
    -   [**On Windows**](#1022f026d61542ba9099c35011bf09bb)

    Open a terminal and execute the following command:

    ``` java
        sudo wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

2.  Open the ESB profile's Management Console using
    `                       https://localhost:9443/carbon                     `
    , and log in using `           admin          ` as the username and
    the password.

3.  In the **Data Service** menu click **Create** .
4.  Enter a name for the data service and click **Next** .
5.  Connect to the **Company** database that you defined above.

    <table>
    <tbody>
    <tr class="odd">
    <td>Datasource ID</td>
    <td>Datasource</td>
    </tr>
    <tr class="even">
    <td>Datasource Type</td>
    <td>RDBMS</td>
    </tr>
    <tr class="odd">
    <td>Datasource Type (Default/External)</td>
    <td>Leave <strong>Default</strong> selected.</td>
    </tr>
    <tr class="even">
    <td>Database Engine</td>
    <td>MySQL</td>
    </tr>
    <tr class="odd">
    <td>Driver Class</td>
    <td><code>               com.mysql.jdbc.Driver              </code></td>
    </tr>
    <tr class="even">
    <td>URL</td>
    <td><code>                               jdbc:mysql://localhost:3306/Company                             </code></td>
    </tr>
    <tr class="odd">
    <td>User Name</td>
    <td>Enter your MySQL server's username. Example: root</td>
    </tr>
    <tr class="even">
    <td>Password</td>
    <td>Enter your MySQL server's password.<br />
    If you have not assigned a password, keep this field empty.</td>
    </tr>
    </tbody>
    </table>

6.  Click **Next** to go to the **Queries** screen.
7.  Click **Add New Query** to specify the query details:
    1.  Enter **UpdateAccBalance** as the query ID.

    2.  Enter the following SQL dialect:

        ``` java
                UPDATE ACCOUNT SET Balance=:Balance WHERE AccountID=:AccountID
        ```

    3.  Click **Generate Input Mapping** to automatically generate input
        mappings for the **AccountID** and **Balance** fields.  
        ![](attachments/119133285/119133287.png){width="900"
        height="363"}
    4.  At the bottom of the page you will find the **Events** section
        :  
        ![](attachments/119133285/119133289.png){width="883"
        height="123"}

    5.  Click **Manage Events** and add a new event as shown below:

        <table>
        <tbody>
        <tr class="odd">
        <td>Event Id</td>
        <td>Enter <code>                                   account_balance_low_trigger                                 </code> .<br />
        This is the ID used for identifying the event-trigger used in data services queries.</td>
        </tr>
        <tr class="even">
        <td>Xpath</td>
        <td><div class="content-wrapper">
        <p>Enter <strong><code>                    /UpdateAccBalance/Balance&lt;200                   </code></strong> .</p>
        <p>Represents an XPath expression that is run against the XML message presented. That is the request/response message. When this evaluation returns <code>                   true                  </code> , the event is triggered.</p>
        </div></td>
        </tr>
        <tr class="odd">
        <td>Target Topic</td>
        <td>Enter <strong><code>                  product_stock_low_topic                 </code></strong> . The topic to which the event notifications are published.</td>
        </tr>
        <tr class="even">
        <td>Event Sink URL</td>
        <td>Enter the email to which the event notifications need to be sent in the following format: <strong><code>                  mailto:&lt;YOUR_EMAIL&gt;                 </code></strong> A subscription can be any endpoint that is compliant with WS-Eventing. For example, you can use an SMTP transport to send a message to a mail inbox, where an email address is given as the subscription. Here, many subscriptions can be defined for the given topic.</td>
        </tr>
        </tbody>
        </table>

        Example:  
        ![](attachments/119133285/119133288.png)

    6.  Click **Save** and click **Main Configuration** to go back to
        the query page.

    7.  You can now add the event as an event trigger for the query as
        shown below.  
        Since the event sends notifications related to the data input,
        you need to add the event as an **Input Event Trigger** .  
        ![](attachments/119133285/119133286.png)
    8.  Save the **UpdateAccBalance** query.

8.  Create an operation for the **UpdateAccBalance** query as shown
    below.

    |                |                                                   |
    |----------------|---------------------------------------------------|
    | Operation Name | `               UpdateAccBalanceOp              ` |
    | Query ID       | `               UpdateBalance              `      |

    ![](attachments/119133285/119133291.png){width="600" height="348"}

9.  Save the operation.

10. Click **Finish** to navigate to the **Deployed Services** window
    from where you can manage data services.

------------------------------------------------------------------------

#### Invoking the data service

You can try the data service you created by using the TryIt tool that is
in your product by default.

1.  Go to the **Deployed Services** screen.
2.  Click **Try this Service** to open the data service from the TryIt
    tool.
3.  Click the **UpdateAccBalance** operation.
4.  Copy the following into the TryIt tool:

        !!! info
    
        To trigger the notification, you need to have a balance that is less
        than 200.
    

    ``` java
        <body>
           <p:UpdateAccBalanceOp xmlns:p="http://ws.wso2.org/dataservice">
              <!--Exactly 1 occurrence-->
              <xs:Balance xmlns:xs="http://ws.wso2.org/dataservice">189</xs:Balance>
              <!--Exactly 1 occurrence-->
              <xs:AccountID xmlns:xs="http://ws.wso2.org/dataservice">1</xs:AccountID>
           </p:UpdateAccBalanceOp>
        </body>
    ```

5.  Since the balance is less than 200, you receive an email **.**  
    In the event notification messages, additional information about the
    event is added to the SOAP Envelope/Body element. The following code
    demonstrates this.

    ``` html/xml
            <data-services-event>
                <service-name>$SERVICE_NAME</service-name>
                <query-id>$QUERY_ID</query-id>
                <time>$TIME</time>
                <content>
                  $CONTENT
                </content>
            </data-services-event>
    ```

    -   `            $SERVICE_NAME           ` : Name of the service
        from which the event originated
    -   `            $QUERY_ID           ` : The id of the query that
        triggered the event
    -   `            $TIME           ` : The date/time when the event
        occurred
    -   `            $CONTENT           ` : The XML element generated in
        case of an input event trigger. It is used when executing the
        XPath expression. It contains the input parameters wrapped with
        the query id value. In the case of an output event trigger, it
        contains the full result XML
