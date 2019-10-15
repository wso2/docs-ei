# Receiving Notifications from Data Services

Eventing support is provided by the WS-Eventing Web services standard.
When a data service request or response triggers an event, the
subscribers listening to those events receive notifications. The
criteria for triggering an event as well as the destination to which the
event notifications should be sent are defined per data service query.
 When a certain event-trigger is activated, emails will be sent to all
the respective subscribers. You can set an event trigger as an input or
an output event trigger in a data service query. Let's look at the usage
of the two approaches.

**Input event trigger**

When an input event trigger is applied to a query, the event trigger is
evaluated when the parameters are received by the query. An
event-trigger executes an XPath expression against an XML element. This
XML element is built from the input parameters that are represented by
the element of the parameters wrapped by an element with the name of the
query.

For example, take the following query:

```xml
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

```xml
<incrementEmployeeSalaryQuery>
   <increment>value1</increment>
   <employeeNumber>value2</employeeNumber>
</incrementEmployeeSalaryQuery>
```

**Output event trigger**

In this case, the event trigger is evaluated when a specific query is
returning its result. There isn't a specific creation of an XML element
that is used with the XPath expression like in the input event trigger.
But the full result XML is used to evaluate it. The result will be
namespace qualified. Therefore, you must write the XPath expressions
accordingly.

## Prerequisites

Let's create a MySQL database with the required data.

1.  Install the MySQL server.   
2.  Create the following database:  Company
    
    ```bash
    CREATE DATABASE Company;
    ```
        
3.  Create the ACCOUNT table in the Company database:
        
    ```bash
    USE Company;

    CREATE TABLE ACCOUNT(AccountID int NOT NULL,Branch varchar(255) NOT NULL, AccountNumber varchar(255),AccountType ENUM('CURRENT', 'SAVINGS') NOT NULL,Balance FLOAT,ModifiedDate DATE,PRIMARY KEY (AccountID));
    ```
4.  Enter the following data into the ACCOUNT table:
            
    ```bash
    INSERT INTO ACCOUNT VALUES (1,"AOB","A00012","CURRENT",231221,'2014-12-02');
    ```

## Synapse configuration

```xml
<data name="receiving_notifications" transports="http https local">
   <config enableOData="false" id="Datasource">
      <property name="driverClassName">com.mysql.jdbc.Driver</property>
      <property name="url">jdbc:mysql://localhost:3306/Company</property>
   </config>
   <query id="UpdateAccBalance" input-event-trigger="account_balance_low_trigger" useConfig="Datasource">
      <sql>UPDATE ACCOUNT SET Balance=:Balance WHERE AccountID=:AccountID</sql>
      <param name="Balance" sqlType="STRING"/>
      <param name="AccountID" sqlType="STRING"/>
   </query>
   <event-trigger id="account_balance_low_trigger">
      <expression>/UpdateAccBalance/Balance&lt;200</expression>
      <target-topic>product_stock_low_topic</target-topic>
      <subscriptions>
         <subscription>mailto:name@email.com</subscription>
      </subscriptions>
   </event-trigger>
   <operation name="UpdateAccBalanceOp">
      <call-query href="UpdateAccBalance">
         <with-param name="Balance" query-param="Balance"/>
         <with-param name="AccountID" query-param="AccountID"/>
      </call-query>
   </operation>
</data>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio). The path to this folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.
2.  Download the JDBC driver for MySQL from [here](http://dev.mysql.com/downloads/connector/j/) and copy it to
    your `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/lib/` directory.

    !!! Note
        If the driver class does not exist in the relevant folders when you create the datasource, you will get an exception, such as `Cannot load JDBC driver class com.mysql.jdbc.Driver`. 
        
3. Open the `deployment.toml` file from the `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/lib/` directory and configuring the MailTo sender with your email credentials.

    ```toml
    [[transport.mail.sender]]
    name = "mailto"
    parameter.hostname = "smtp.gmail.com"
    parameter.port = "587"
    parameter.enable_tls = true
    parameter.auth = true
    parameter.username = "demo_user"
    parameter.password = "mailpassword"
    parameter.from = "demo_user@wso2.com"
    ```
    
4. [Create a Data Service project](../../../../develop/creating-projects/#data-services-project)
5. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.
6. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

You can try the data service you created by using the TryIt tool that is
in your product by default.

1.  Execute **UpdateAccBalance** operation.

    !!! Info
    
        To trigger the notification, you need to have a balance that is less
        than 200.
    
    ```xml
    <body>
       <p:UpdateAccBalanceOp xmlns:p="http://ws.wso2.org/dataservice">
          <!--Exactly 1 occurrence-->
          <xs:Balance xmlns:xs="http://ws.wso2.org/dataservice">189</xs:Balance>
          <!--Exactly 1 occurrence-->
          <xs:AccountID xmlns:xs="http://ws.wso2.org/dataservice">1</xs:AccountID>
       </p:UpdateAccBalanceOp>
    </body>
    ```

2.  Since the balance is less than 200, you receive an email **.**  
    In the event notification messages, additional information about the
    event is added to the SOAP Envelope/Body element. The following code
    demonstrates this.

    ```xml
    <data-services-event>
        <service-name>$SERVICE_NAME</service-name>
        <query-id>$QUERY_ID</query-id>
        <time>$TIME</time>
        <content>
          $CONTENT
        </content>
    </data-services-event>
    ```

    -   `$SERVICE_NAME` : Name of the service from which the event originated
    -   `$QUERY_ID` : The id of the query that triggered the event
    -   `$TIME` : The date/time when the event occurred
    -   `$CONTENT` : The XML element generated in
        case of an input event trigger. It is used when executing the
        XPath expression. It contains the input parameters wrapped with
        the query id value. In the case of an output event trigger, it
        contains the full result XML
