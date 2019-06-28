# Working with Transactions

A **transaction** is a set of operations executed as a single unit. It
also can be defined as an agreement, which is carried out between
separate entities or objects. A transaction can be considered as
indivisible or atomic when it has the characteristic of either being
completed in its entirety or not at all. During the event of a failure
for a transaction update, atomic transaction type guarantees transaction
integrity such that any partial updates are rolled back automatically.

Transactions have many different forms, such as financial transactions,
database transactions etc.

From the ESB profile point of view, there are two types of transactions:

-   [Distributed
    transactions](#WorkingwithTransactions-Distributedtransactions)
-   [Java Message Service (JMS)
    transactions](#WorkingwithTransactions-JavaMessageService(JMS)transactionsJavaMessageService(JMS)Transactions)
    -   [JMS consumer
        transactions](#WorkingwithTransactions-JMSconsumertransactions)
        -   [JMS local
            transactions](#WorkingwithTransactions-JMSlocaltransactionsLocal)
            -   [Sample
                scenario](#WorkingwithTransactions-Samplescenario)
            -   [Prerequisites](#WorkingwithTransactions-Prerequisites)
            -   [Configuring the sample
                scenario](#WorkingwithTransactions-Configuringthesamplescenario)
            -   [Executing the sample
                scenario](#WorkingwithTransactions-ExecuteJMSClientExecutingthesamplescenario)
            -   [Testing the sample
                scenario](#WorkingwithTransactions-Testingthesamplescenario)
        -   [JMS distributed
            transactions](#WorkingwithTransactions-JMSdistributedtransactionsDistributed)
            -   [XA two-phase commit
                process](#WorkingwithTransactions-XAtwo-phasecommitprocess)
            -   [Sample
                Scenario](#WorkingwithTransactions-SampleScenario)
            -   [Prerequisites](#WorkingwithTransactions-Prerequisites.1)
            -   [Configuring the sample
                scenario](#WorkingwithTransactions-Configuringthesamplescenario.1)
    -   [JMS publisher
        transactions](#WorkingwithTransactions-JMSpublishertransactions)
        -   [Sample scenario](#WorkingwithTransactions-Samplescenario.1)
        -   [Prerequisites](#WorkingwithTransactions-Prerequisites.2)
        -   [Configuring the sample
            scenario](#WorkingwithTransactions-Configuringthesamplescenario.2)
        -   [Executing the sample
            scenario](#WorkingwithTransactions-Executingthesamplescenario)
        -   [Testing the sample
            scenario](#WorkingwithTransactions-Testingthesamplescenario.1)

### Distributed transactions

A **distributed transaction** is a transaction that updates data on two
or more networked computer systems, such as two databases or a database
and a message queue such as JMS. Implementing robust distributed
applications is difficult because these applications are subject to
multiple failures, including failure of the client, the server, and the
network connection between the client and server. For distributed
transactions, each computer has a local transaction manager. When a
transaction works at multiple computers, the transaction managers
interact with other transaction managers via either a superior or
subordinate relationship. These relationships are relevant only for a
particular transaction.

For an example that demonstrates how the [transaction
mediator](https://docs.wso2.com/display/EI650/Transaction+Mediator) can
be used to manage distributed transactions , see [Transaction Mediator
Example](https://docs.wso2.com/display/EI650/Transaction+Mediator+Example)
.

### Java Message Service (JMS) transactions

In addition to the [transaction
mediator](https://docs.wso2.com/display/EI650/Transaction+Mediator) ,
WSO2 Enterprise Integrator (WSO2 EI) also supports JMS transactions.

!!! note

Note

In WSO2 EI, JMS transactions only work with either the Callout mediator
or the Call mediator in blocking mode.


The [JMS transport](https://docs.wso2.com/display/EI650/JMS+Transport)
shipped with WSO2 EI supports both local and distributed JMS
transactions. You can use local transactions to group messages
received in a JMS queue. Local transactions are not supported for
messages sent to a JMS queue.

This section describes:

-   [JMS consumer
    transactions](#WorkingwithTransactions-JMSconsumertransactions)
-   [JMS publisher
    transactions](#WorkingwithTransactions-JMSpublishertransactions)

#### JMS consumer transactions

Following sections describe the JMS consumer transactions.

-   [JMS Local Transactions](#WorkingwithTransactions-Local)
-   [JMS Distributed Transactions](#WorkingwithTransactions-Distributed)

##### JMS local transactions

A **local transaction** represents a unit of work on a single connection
to a data source managed by a resource manager. In JMS, you can use the
JMS API to get a transacted session and to call methods for commit or
roll back for the relevant transaction objects. This is managed
internally by a resource manager. There is no external transaction
manager involved in the coordination of such transactions.

Let's explore a sample scenario that demonstrates how to handle a
transaction using JMS in a situation where the back-end service is
unreachable.

###### Sample scenario

A message is read from a JMS queue and is processed by a back-end
service. In the successful scenario, the transaction will be committed
and the request will be sent to the back end service. In the failure
scenario, while executing a sequence, a failure occurs and WSO2 EI
receives a fault. This cause the JMS transaction to roll back.

The sample scenario can be depicted as follows:

![](attachments/119131477/119131479.png){height="250"}

###### Prerequisites

-   Windows, Linux or Solaris operating systems with WSO2 EI
    installed. For instructions on downloading and installing WSO2 EI,
    see [Installation
    Guide](https://docs.wso2.com/display/EI650/Installation+Guide) .
-   WSO2 ESB's JMS transport configured with the Broker profile of WSO2
    EI. For instructions, see [Configure with the Broker
    Profile](https://docs.wso2.com/display/EI650/Configure+with+the+Broker+Profile)
    .

###### Configuring the sample scenario

1.  Configure the JMS local transaction by defining the following
    parameter in the
    `           <EI_HOME>/conf/axis2/axis2.xml          ` file. By
    default the session is not transacted. In order to make it
    transacted, set the parameter to **true** .

    ``` html/xml
        <parameter name="transport.jms.SessionTransacted">true</parameter>
    ```

    Once done, the JMS listener configuration for WSO2 MB in the
    axis2.xml file should be as follows:

    ``` html/xml
            <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
                   <parameter name="myTopicConnectionFactory" locked="false">
                        <parameter name="java.naming.factory.initial" locked="false">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                        <parameter name="java.naming.provider.url" locked="false">repository/conf/jndi.properties</parameter>
                        <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">TopicConnectionFactory</parameter>
                        <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
                        <parameter name="transport.jms.SessionTransacted">true</parameter>
                   </parameter>
             
                   <parameter name="myQueueConnectionFactory" locked="false">
                        <parameter name="java.naming.factory.initial" locked="false">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                        <parameter name="java.naming.provider.url" locked="false">repository/conf/jndi.properties</parameter>
                        <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                        <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                        <parameter name="transport.jms.SessionTransacted">true</parameter>    
                   </parameter>
             
                   <parameter name="default" locked="false">
                        <parameter name="java.naming.factory.initial" locked="false">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                        <parameter name="java.naming.provider.url" locked="false">repository/conf/jndi.properties</parameter>
                        <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                        <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                        <parameter name="transport.jms.SessionTransacted">true</parameter>
                    </parameter>
               </transportReceiver>
    ```

2.  Copy and paste the following configuration into the Synapse
    configuration in \<
    `           EI_HOME>/repository/deployment/server/synapse-configs/<node>/synapse.xml          `
    .

    ``` html/xml
            <definitions xmlns="http://ws.apache.org/ns/synapse">
               <proxy name="StockQuoteProxy" transports="jms" startOnLoad="true">
                  <target>
                     <inSequence>
                        <property name="OUT_ONLY" value="true"/>
                        <callout serviceURL="http://localhost:9000/services/SimpleStockQuoteService">
                           <source type="envelope"/>
                           <target key="placeOrder"/>
                        </callout>
                        <log level="custom">
                           <property name="Transaction Action" value="Committed"/>
                        </log>
                     </inSequence>
                     <faultSequence>
                        <property name="SET_ROLLBACK_ONLY" value="true" scope="axis2"/>
                        <log level="custom">
                           <property name="Transaction Action" value="Rollbacked"/>
                        </log>
                     </faultSequence>
                  </target>
                  <parameter name="transport.jms.ContentType">
                     <rules>
                        <jmsProperty>contentType</jmsProperty>
                        <default>application/xml</default>
                     </rules>
                  </parameter>
               </proxy>
               <sequence name="fault">
                  <log level="full">
                     <property name="MESSAGE" value="Executing default &#34;fault&#34; sequence"/>
                     <property name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
                     <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
                  </log>
                  <drop/>
               </sequence>
               <sequence name="main">
                  <log/>
                  <drop/>
               </sequence>
            </definitions>
    ```

    According to the above configuration, a message will be read from
    the JMS queue and will be sent to the
    `           SimpleStockQuoteService          ` running on the Axis2
    back-end server. If a failure occurs, the transaction will roll
    back.

    In the above configuration, the following property is set to
    **true** in the fault handler, in order to roll back the transaction
    when a failure occurs.

    ``` html/xml
            <property name="SET_ROLLBACK_ONLY" value="true" scope="axis2"/>
    ```

        !!! tip
    
        If you are using a JMS Inbound endpoint for the transaction, set the
        scope of the `           SET_ROLLBACK_ONLY          ` property to
        `           default          ` as follows:
    
            <property name="SET_ROLLBACK_ONLY" scope="default" type="STRING" value="true"/>
    

3.  Deploy the back-end service
    `          SimpleStockQuoteService         ` . For instructions on
    deploying sample back-end services, see [Deploying sample back-end
    services](https://docs.wso2.com/display/EI600/Setting+Up+the+Service+Bus+Samples#SettingUptheServiceBusSamples-Deployingsampleback-endservices)
    .
4.  Start the Axis2 server. For instructions on starting the Axis2
    server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI600/Setting+Up+the+Service+Bus+Samples#SettingUptheServiceBusSamples-StartingtheAxis2server)
    .

    You now have a running WSO2 EI instance, EI-Broker instance and a
    sample back-end service to simulate the sample scenario. Now let's
    execute the JMS client.

        !!! info
    
        Due to the asynchronous behavior of the [Send
        Mediator](https://docs.wso2.com/display/EI650/Send+Mediator) , you
        cannot you use it with a http/https endpoint, but you can use it in
        asynchronous use cases, for example with another JMS as endpoint.
    

###### **** Executing the sample scenario

** To execute the JMS client**

-   Run the following command from the
    `           <EI_HOME>/samples/axis2Client          ` directory.

        !!! warning
    
        Currently, it is a [known
        issue](https://github.com/wso2/product-ei/issues/766) that this
        client does not work with the Broker profile of WSO2 EI. This will
        be fixed in future releases.
    

    ``` java
        ant jmsclient -Djms_type=pox -Djms_dest=dynamicQueues/StockQuoteProxy -Djms_payload=MSFT
    ```

    This will trigger a sample message to the JMS Server.

###### Testing the sample scenario

You can test the sample scenario as follows.

**Successful scenario**

If the message mediates successfully, you will view the output on the
Axis2 server start-up console. Also, the ESB debug log will display an
INFO message indicating that the transaction is committed.

**Failure scenario**

Stop the sample Axis2 Server and [execute the JMS
client](#WorkingwithTransactions-ExecuteJMSClient) once again to
simulate the failure scenario. In this scenario, the ESB debug log will
display an INFO message indicating that the transaction is rolled back.

##### JMS distributed transactions

WSO2 ESB also supports distributed JMS transactions. You can use the JMS
transport with more than one distributed resource, for example, two
remote database servers. An external transaction manager coordinates the
transaction. Designing and using JMS distributed transactions is more
complex than using local JMS transactions.

The transaction manager is the primary component of the distributed
transaction infrastructure and distributed JMS transactions are managed
by the
[XAResource](http://docs.oracle.com/javaee/5/api/javax/transaction/xa/XAResource.html)
enabled transaction manager in the Java 2 Platform, Enterprise Edition
(J2EE) application server.

!!! info

You will need to check if your message broker supports [XA
transactions](index) prior to implementing distributed JMS transactions.


###### XA two-phase commit process

XA is a two-phase commit specification that is used in distributed
transaction processing. Let's look at a sample scenario for JMS
distributed transactions.

\[ [Distributed
transactions](#WorkingwithTransactions-Distributedtransactions) \] \[
[Java Message Service (JMS)
transactions](#WorkingwithTransactions-JavaMessageService(JMS)transactionsJavaMessageService(JMS)Transactions)
\] \[ [JMS consumer
transactions](#WorkingwithTransactions-JMSconsumertransactions) \] \[
[JMS publisher
transactions](#WorkingwithTransactions-JMSpublishertransactions) \]

###### Sample Scenario

ESB listens to the message queue and sends that message to multiple
queues. If something goes wrong in sending the message to one of those
queues, the original message should be rolled back to the listening
queue and none of the other queues should receive the message. Thus, the
entire transaction should be rolled back.

###### Prerequisites

-   Windows, Linux or Solaris operating systems with WSO2 EI
    installed. For instructions on downloading and installing WSO2 EI,
    see [Installation
    Guide](https://docs.wso2.com/display/EI650/Installation+Guide) .
-   WSO2 EI JMS transport configured with ActiveMQ. For instructions,
    see [Configure with
    ActiveMQ](https://docs.wso2.com/display/EI650/Configure+with+ActiveMQ)
    .

###### Configuring the sample scenario

1.  Create the `             JMSListenerProxy            ` proxy service
    in WSO2 EI with the following configuration:

    ``` xml
        <proxy xmlns="http://ws.apache.org/ns/synapse"
               name="JMSListenerProxy"
               transports="https http jms"
               startOnLoad="true">
           <description/>
           <target>
              <inSequence>
                 <property name="OUT_ONLY" value="true"/>
                 <log level="custom">
                    <property name="MESSAGE_ID_A" expression="get-property('MessageID')"/>
                 </log>
                 <log level="custom">
                    <property name="BEFORE" expression="$body"/>
                 </log>
                 <property name="MESSAGE_ID_B"
                           expression="get-property('MessageID')"
                           scope="operation"
                           type="STRING"/>
                 <property name="failureResultProperty"
                           scope="default"
                           description="FailureResultProperty">
                    <result xmlns="">failure</result>
                 </property>
                 <enrich>
                    <source clone="true" xpath="$ctx:failureResultProperty"/>
                    <target type="body"/>
                 </enrich>
                 <log level="custom">
                    <property name="AFTER" expression="$body"/>
                 </log>
                 <property name="BEFORE1" value="ABCD" scope="axis2" type="STRING"/>
                <callout serviceURL="jms:/ActiveMQPublisher1?transport.jms.ConnectionFactoryJNDIName=XAConnectionFactory&amp;java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;java.naming.provider.url=tcp://localhost:61616&amp;transport.jms.DestinationType=queue;transport.jms.TransactionCommand=begin">
                    <source type="envelope"/>
                    <target xmlns:s12="http://www.w3.org/2003/05/soap-envelope"
                            xmlns:s11="http://schemas.xmlsoap.org/soap/envelope/"
                            xpath="s11:Body/child::*[fn:position()=1] | s12:Body/child::*[fn:position()=1]"/>
                 </callout>
                 <callout serviceURL="jms:/ActiveMQPublisher2?transport.jms.ConnectionFactoryJNDIName=XAConnectionFactory&amp;java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;java.naming.provider.url=tcp://localhost:61616&amp;transport.jms.DestinationType=queue">
                    <source type="envelope"/>
                    <target xmlns:s12="http://www.w3.org/2003/05/soap-envelope"
                            xmlns:s11="http://schemas.xmlsoap.org/soap/envelope/"
                            xpath="s11:Body/child::*[fn:position()=1] | s12:Body/child::*[fn:position()=1]"/>
                 </callout>
                 <callout serviceURL="jms:/ActiveMQPublisher3?transport.jms.ConnectionFactoryJNDIName=XAConnectionFactory&amp;java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;java.naming.provider.url=tcp://localhost:61616&amp;transport.jms.DestinationType=queue;transport.jms.TransactionCommand=end">
                    <source type="envelope"/>
                    <target xmlns:s12="http://www.w3.org/2003/05/soap-envelope"
                            xmlns:s11="http://schemas.xmlsoap.org/soap/envelope/"
                            xpath="s11:Body/child::*[fn:position()=1] | s12:Body/child::*[fn:position()=1]"/>
                 </callout>
                 <drop/>
              </inSequence>
              <faultSequence>
                 <log level="custom">
                    <property name="Transaction Action" value="Rollbacked"/>
                 </log>
                 <callout serviceURL="jms:/ActiveMQPublisherFault?transport.jms.ConnectionFactoryJNDIName=XAConnectionFactory&amp;java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;java.naming.provider.url=tcp://localhost:61616&amp;transport.jms.DestinationType=queue;transport.jms.TransactionCommand=rollback">
                    <source type="envelope"/>
                    <target xmlns:s12="http://www.w3.org/2003/05/soap-envelope"
                            xmlns:s11="http://schemas.xmlsoap.org/soap/envelope/"
                            xpath="s11:Body/child::*[fn:position()=1] | s12:Body/child::*[fn:position()=1]"/>
                 </callout>
              </faultSequence>
           </target>
           <parameter name="transport.jms.ContentType">
              <rules>
                 <jmsProperty>contentType</jmsProperty>
                 <default>application/xml</default>
              </rules>
           </parameter>
           <parameter name="transport.jms.Destination">MyJMSQueue</parameter>
        </proxy>
    ```

    In the above configuration,  WSO2 EI listens to a JMS queue named
    `             MyJMSQueue            ` and consumes messages as well
    as sends messages to multiple JMS queues in a transactional manner.

2.  To place a message into `             MyJMSQueue            ` ,
    execute the following command from
    `             <EI_HOME>/samples/axis2Client            ` directory:

    ``` xml
            ant stockquote -Dmode=placeorder -Dtrpurl="jms:/MyJMSQueue?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&java.naming.provider.url=tcp://localhost:61616&transport.jms.ContentTypeProperty=Content-Type&transport.jms.DestinationType=queue"
    ```

    You can see how WSO2 EI consumes messages from the queue named
    `             MyJMSQueue            ` and sends the messages to
    multiple queues.

    To check the rollback functionality provide an unreachable hostname
    to any destination queue and save the configurations. You should be
    able to observe WSO2 EI fault sequence getting invoked and failed
    message delivered to the destination configured in the fault
    sequence.

#### JMS publisher transactions

When you do not enable publisher transactions, the message publishing
call to the Broker will not wait until the messages are persisted to the
database. As a result, a successful HTTP response will be returned back
to the caller even in a state where the database is disconnected. Hence,
the message might actually be lost and not persisted in the Broker.
Therefore, you can achieve guaranteed delivery by enabling publisher
transactions.

The below is a sample scenario that demonstrates how to handle a
publisher transaction using JMS.

###### Sample scenario

In this scenario, the client publishes JMS messages to the ESB profile
of WSO2 EI. Then, the ESB profile publishes those messages to the JMS
queue, which acts as the JMS endpoint. The sample scenario can be
depicted as follows.

![](attachments/119131477/119131480.png)

###### Prerequisites

-   Install WSO2 EI. For instructions , see [Installation
    Guide](https://docs.wso2.com/display/EI650/Installation+Guide) .
-   Configure the JMS transport with the Broker Profile of WSO2 EI. For
    instructions, see [Configure with the Broker
    Profile](https://docs.wso2.com/display/EI650/Configure+with+the+Broker+Profile)
    .

###### Configuring the sample scenario

1.  Configure the JMS sender for the Broker profile in the JMS Sender
    section of the
    `             <EI_HOME>/conf/axis2/             axis2_blocking_client.xml            `
    file as follows:

        !!! info
    
        By default, the session is not transacted. Set the value of the
        `             sessionTransacted            ` property to true, to
        make it transacted to publish transactions successfully.
    

    ``` html/xml
        <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender">
            <parameter name="commonJmsSenderConnectionFactory" locked="false">
                <parameter locked="false" name="java.naming.factory.initial">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                <parameter locked="false" name="java.naming.provider.url">conf/jndi.properties</parameter>
                <parameter locked="false" name="transport.jms.ConnectionFactoryJNDIName">QueueConnectionFactory</parameter>
                <parameter locked="false" name="transport.jms.ConnectionFactoryType">queue</parameter>
                <parameter locked="false" name="transport.jms.SessionTransacted">true</parameter>
            </parameter>
            <parameter name="commonTopicPublisherConnectionFactory" locked="false">
                <parameter locked="false" name="java.naming.factory.initial">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                <parameter locked="false" name="java.naming.provider.url">conf/jndi.properties</parameter>
                <parameter locked="false" name="transport.jms.ConnectionFactoryJNDIName">TopicConnectionFactory</parameter>
                <parameter locked="false" name="transport.jms.ConnectionFactoryType">topic</parameter>
                <parameter locked="false" name="transport.jms.SessionTransacted">true</parameter>
            </parameter>
        </transportSender>
    ```

2.  Create an XML file with the below Synapse configuration of a sample
    publisher Proxy Service, and place the file inside the
    `             <            `
    `             EI_HOME>/repository/deployment/server/synapse-configs/default/proxy-services/            `
    directory.

    ``` xml
            <proxy name="SampleProxy" transports="http" startOnLoad="true" xmlns="http://ws.apache.org/ns/synapse">
               <description/>
               <target>
                  <inSequence>
                     <property name="OUT_ONLY" value="true"/>
                     <property name="messageType" value="text/xml" scope="axis2" type="STRING"/>
                     <property name="routingKey" value="example.MyTopic" type="STRING"/>
                     <call blocking="true">
                        <endpoint>
                           <address uri="jms:/example.MyTopic?transport.jms.ConnectionFactory=commonTopicPublisherConnectionFactory"/>
                        </endpoint>
                     </call>
                     <payloadFactory media-type="xml">
                        <format>
                           <serviceResponse xmlns="">
                              <returnCode>200</returnCode>
                              <returnDesc>Successful</returnDesc>
                              <returnData>
                                 <data name="routingKey" type="xml">$1</data>
                              </returnData>
                           </serviceResponse>
                        </format>
                        <args>
                           <arg evaluator="xml" expression="$ctx:routingKey"/>
                        </args>
                     </payloadFactory>
                     <property name="HTTP_SC" value="200" scope="axis2" type="STRING"/>
                     <respond/>
                  </inSequence>
                  <outSequence/>
                  <faultSequence>
                     <property name="SET_ROLLBACK_ONLY" value="true" scope="axis2"/>
                     <payloadFactory media-type="xml">
                        <format>
                           <serviceResponse xmlns="">
                              <returnCode>500</returnCode>
                              <returnDesc>Failure</returnDesc>
                              <returnData>
                                 <data name="routingKey" type="xml">$1</data>
                              </returnData>
                           </serviceResponse>
                        </format>
                        <args>
                           <arg evaluator="xml" expression="$ctx:routingKey"/>
                        </args>
                     </payloadFactory>
                     <property name="HTTP_SC" value="500" scope="axis2" type="STRING"/>
                     <log level="full"/>
                     <respond/>
                  </faultSequence>
               </target>
            </proxy>
    ```

###### Executing the sample scenario

Use a JMS client such as [Apache JMeter](https://jmeter.apache.org/) to
execute this sample scenario.

###### Testing the sample scenario

When a message is successfully published, it returns an HTTP 200
response to the client (successful scenario). In a case where it fails
to publish a message, it executes the fault sequence returning an HTTP
500 response to the client (failure scenario) .
