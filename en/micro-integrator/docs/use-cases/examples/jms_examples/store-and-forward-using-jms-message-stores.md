# Store and Forward Using JMS Message Stores

A message store is a storage in the ESB Profile of WSO2 Enterprise
Integrator (WSO2 EI) for messages. The ESB profile of WSO2 EI ships with
the following message store implementations: [In Memory Message
Store](https://docs.wso2.com/display/EI611/In+Memory+Message+Store) ,
[JMS Message
Store](https://docs.wso2.com/display/EI611/JMS+Message+Store) ,
[RabbitMQ Message
Store](https://docs.wso2.com/display/EI611/RabbitMQ+Message+Store) ,
[JDBC Message
Store](https://docs.wso2.com/display/EI611/JDBC+Message+Store) . You
also have the option to create a [Custom Message
Store](https://docs.wso2.com/display/EI650/Custom+Message+Store) with
your own message store implementation.

For more information, see [Message
Stores](https://docs.wso2.com/display/EI650/Message+Stores) . Next,
let's take a look at a few business use case scenarios of JMS message
stores.

-   [Use case scenario
    1](#StoreandForwardUsingJMSMessageStores-Usecasescenario1sample1)
    -   [Configuring the message
        store](#StoreandForwardUsingJMSMessageStores-Configuringthemessagestore)
    -   [Configuring the proxy
        service](#StoreandForwardUsingJMSMessageStores-Configuringtheproxyservice)
    -   [Configuring the message
        processor](#StoreandForwardUsingJMSMessageStores-Configuringthemessageprocessor)
    -   [Testing the use
        case](#StoreandForwardUsingJMSMessageStores-Testingtheusecase)
-   [Use case scenario
    2](#StoreandForwardUsingJMSMessageStores-Usecasescenario2)
    -   [Configure the
        broker](#StoreandForwardUsingJMSMessageStores-Configurethebroker)
    -   [Configuring the back-end
        service](#StoreandForwardUsingJMSMessageStores-Configuringtheback-endservice)
    -   [Configuring the ESB Profile of WSO2
        EI](#StoreandForwardUsingJMSMessageStores-ConfiguringtheESBProfileofWSO2EI)
    -   [Executing the
        sample](#StoreandForwardUsingJMSMessageStores-Executingthesample)

### Use case scenario 1

In this sample:

-   The client sends requests to a proxy service.
-   The proxy service stores the messages in a JMS message store.  
-   The back-end service is invoked by a message forwarding processor,
    which picks stored messages from the JMS message store.

![](attachments/119130313/119130315.png){width="560"}  

Let's proceed to configure the sample scenario.

#### Configuring the message store

Create a JMS message store for the broker. Given below are the sample
configurations for the WSO2 Message Broker profile and ActiveMQ broker.

-   [**ActiveMQ broker**](#b7c35d5026ea4a20b5e71147af2d8f6d)
-   [**WSO2 MB profile**](#61ec45064d80434fa26eec0dd7845da4)

Set the value of the \<
`             java.naming.provider.url>            ` property to point
to the provider URL.

``` java
    <messageStore xmlns="http://ws.apache.org/ns/synapse"
                 class="org.apache.synapse.message.store.impl.jms.JmsStore"
                 name="JMSMS">
      <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
      <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>
    </messageStore>
```

S et the value of the the \<
`             java.naming.provider.url>            ` property to point
to the `             jndi.properties            ` file. In this case, \<
`             store.jms.destination>            ` is a mandatory
parameter. I f you are using the Message Broker Profile of WSO2 EI, y ou
need to create a q ueue named 'JMSMS' using the Message Broker Profile
(i.e., the value you specify for the
`             store.jms.destination            ` parameter ). For
instructions on creating the queue, see [Managing
Queues](https://docs.wso2.com/display/EI610/Managing+Queues#ManagingQueues-CreatingnewqueuesAdd)
.

``` java
    <messageStore name="JMSMS" class="org.apache.synapse.message.store.impl.jms.JmsStore" xmlns="http://ws.apache.org/ns/synapse">   
      <parameter name="java.naming.factory.initial">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>   
      <parameter name="java.naming.provider.url">conf/jndi.properties</parameter>
      <parameter name="store.jms.destination">JMSMS</parameter>
    </messageStore>
```

#### Configuring the proxy service

1.  Define an endpoint which is used to send the message to the back-end
    service.

    ``` html/xml
            <endpoint name="SimpleStockQuoteService">
              <address uri="http://127.0.0.1:9000/services/SimpleStockQuoteService"/>
            </endpoint>
    ```

2.  Create a proxy service which stores messages to the created Message
    Store.

    ``` html/xml
            <proxy name="Proxy1" transports="https http" startOnLoad="true" trace="disable">   
              <target>
                 <inSequence>
                    <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
                    <property name="OUT_ONLY" value="true"/>
                    <log level="full"/>
                    <store messageStore="JMSMS"/>
                 </inSequence>
              </target>
            </proxy>
    ```

    Note that y ou can use the FORCE\_SC\_ACCEPTED property in the
    message flow to send an Http 202 status to the client after the ESB
    accepts a message. If this property is not specified, the client
    that sends the request to the proxy service will timeout since it is
    not getting any response back from the proxy.

#### Configuring the message processor

Create a message forwarding processor using the below configuration.
Message forwarding processor consumes the messages stored in the message
store.

``` html/xml
    <messageProcessor class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor" name="Processor1" targetEndpoint="SimpleStockQuoteService" messageStore="JMSMS">
       <parameter name="max.delivery.attempts">4</parameter>
       <parameter name="interval">4000</parameter>
       <parameter name="is.active">true</parameter>
    </messageProcessor>
```

#### Testing the use case

!!! tip

**Before you begin:**

-   **Configure the ESB profile:** Enable the JMS transport (for your
    JMS broker) in the ESB profile. See [Configure with the Message
    Broker Profile](_Configure_with_the_Broker_Profile_) or [Configure
    with ActiveMQ](_Configure_with_ActiveMQ_) for instructions if you
    are using the MB profile or ActiveMQ.
-   **Set up the back-end service** : Deploy the SimpleStockQuoteService
    service in the axis2 server that is shipped with WSO2 EI, and then
    start the axis2 server. See the instructions in [Setting Up the ESB
    Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples)
    . The back-end service is now started!


To invoke the proxy service, we use the sample axis2 client shipped with
WSO2 EI. Navigate to `          <EI_HOME>/samples/axis2client         `
directory, and execute the following command to invoke the proxy
service.

``` java
    ant stockquote -Daddurl=http://localhost:8280/services/Proxy1 -Dmode=placeorder
```

Note a message similar to the following example printed in the Axis2
Server console.  

``` java
    SimpleStockQuoteService :: Accepted order for : 7482 stocks of IBM at $ 169.27205579038733
```

### Use case scenario 2

In the sample, when the message forwarding processor receives a response
from the back-end service, it forwards it to a **replySequence** to
process the response message.

![](attachments/119130313/119130314.png){width="580"}  

Let's proceed to configuring this sample scenario.

#### Configure the broker 

1\. See [Configure with the Message
Broker Profile](_Configure_with_the_Broker_Profile_) or [Configure with
ActiveMQ](_Configure_with_ActiveMQ_) for details on setting up a broker
server for this sample. Note that you only need to set up the broker
server itself and do **not** need to configure the listeners and senders
in the configuration (i.e., just perform steps 1 through 4 in the
Message Broker instructions or 1 through 3 in the ActiveMQ
instructions).

2\. Create a JMS message store for the broker set up in step 1. To create
proxy services, sequences, endpoints, message stores and processors, you
can either use the management console or copy the XML configuration to
the source view. You can find the source view under menu **Manage \>
Service Bus \> Source View** in the left navigation pane of the
Integration Profile management console. Alternatively, you can add an
XML file to
`          <EI_HOME>/repository/deployment/server/synapse-configs/default/message-stores         `
.  

For example,

-   If you use ActiveMQ, the sample message store configuration is as
    follows:

        !!! note
    
        Set the value of the the \<
        `            java.naming.provider.url>           ` property to point
        to the provider URL.
    

    ``` html/xml
        <messageStore xmlns="http://ws.apache.org/ns/synapse"
                     class="org.apache.synapse.message.store.impl.jms.JmsStore"
                     name="JMSMS">
          <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
          <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>
        </messageStore>
    ```

<!-- -->

-   If you use WSO2 Message Broker as the broker server, the sample
    message store configuration is as follows:

        !!! note
    
        Set the value of the the \<
        `            java.naming.provider.url>           ` property to point
        to the `            jndi.properties           ` file. In this
        case, \< `            store.jms.destination>           ` is a
        mandatory parameter.
    

    ``` html/xml
        <messageStore name="JMSMS" class="org.apache.synapse.message.store.impl.jms.JmsStore" xmlns="http://ws.apache.org/ns/synapse">   
          <parameter name="java.naming.factory.initial">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>   
          <parameter name="java.naming.provider.url">conf/jndi.properties</parameter>
          <parameter name="store.jms.destination">JMSMS</parameter>
        </messageStore>
    ```

#### Configuring the back-end service

1\. Set up the prerequisites given in the **Prerequisites** section in
[Setting Up the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples)
. Then, deploy the **SimpleStockQuoteService** client by navigating to
`          <         `
`          EI_HOME>/samples/axis2Server/src/SimpleStockQuoteService         `
, and run the **ant** command in the command prompt or shell script.
This will build the sample and deploy the service for you.  

2\. WSO2 EI comes with a default Axis2 server, which you can use as the
back-end service for this sample. To start the Axis2 server, navigate to
`          <EI_HOME>/samples/axis2server         ` and run
`          axis2Server.sh         ` (on Linux) or
`          axis2Server.bat         ` (on Windows).

3\. Point your browser to
<http://localhost:9000/services/SimpleStockQuoteService?wsdl> and verify
that the service is running.

You now have a JMS message store configured. Next, configure the
Integration Profile of WSO2 EI for the specific message broker you
use.  

#### Configuring the ESB Profile of WSO2 EI

1\. Define an endpoint, which is used to send the message to the back-end
service.

``` html/xml
    <endpoint name="SimpleStockQuoteService">
       <address uri="http://127.0.0.1:9000/services/SimpleStockQuoteService"/>
    </endpoint>
```

2\. Create a proxy service which stores messages to the created Message
Store.  

``` html/xml
    <proxy name="Proxy2" transports="https,http"
           statistics="disable" trace="disable" startOnLoad="true">
       <target>
          <inSequence>
             <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2" />
             <log level="full" />
             <store messageStore="JMSMS" />
          </inSequence>
       </target>
    </proxy>
```

3\. Create a sequence to  handle  the response received from the back-end
service.

``` html/xml
    <sequence name="replySequence">
      <log level="full">
         <property name="REPLY" value="MESSAGE" />
      </log>
      <drop/>
    </sequence>
```

4\. Create a message forwarding processor using the below configuration.
Message forwarding processor consumes the messages stored in the message
store. Compared to the message processor in [sample
1](#StoreandForwardUsingJMSMessageStores-sample1) , this has an
additional para meter **message.processor.reply.sequence** to point to a
sequence to handle the response message.  

``` html/xml
    <messageProcessor name="Processor2" class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor" targetEndpoint="SimpleStockQuoteService" messageStore="JMSMS" xmlns="http://ws.apache.org/ns/synapse">
       <parameter name="interval">1000</parameter>
       <parameter name="client.retry.interval">1000</parameter>
       <parameter name="max.delivery.attempts">4</parameter>
       <parameter name="message.processor.reply.sequence">replySequence</parameter>
       <parameter name="is.active">true</parameter>
       <parameter name="max.delivery.drop">Disabled</parameter>
       <parameter name="member.count">1</parameter>
    </messageProcessor>
```

Once the back-end service and the Integration Profile of WSO2 EI are
configured, proceed to invoking the sample as follows.  

#### Executing the sample

1\. To invoke the proxy service, we use the sample axis2 client shipped
with WSO2 EI. Navigate to
`          <EI_HOME>/samples/axis2client/         ` directory, and
execute the following command to invoke the proxy service.  

``` java
    ant stockquote -Daddurl=http://localhost:8280/services/Proxy2 
```

2\. Note the service being invoked and then the response being logged
from the replySequence . For example,  

``` java
    INFO - LogMediator To: /services/InOutProxy, WSAction: urn:getSimpleQuote, SOAPAction: urn:getSimpleQuote,
     MessageID: urn:uuid:dec12d9c-5289-476c-9d9a-b7bb7ebc7be4, Direction: request, REPLY = MESSAGE,
     Envelope:
     <?xml version='1.0' encoding='utf-8'?>
     <soapenv:envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
     <soapenv:body><ns:getsimplequoteresponse xmlns:ns="http://services.samples">
     <ns:return xmlns:ax21="http://services.samples/xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ax21:GetQuoteResponse">
     <ax21:change>-2.3141856129298564</ax21:change><ax21:earnings>12.877155014054368</ax21:earnings>
     <ax21:high>172.73334579339183</ax21:high><ax21:last>165.31090559096748</ax21:last>
     <ax21:lasttradetimestamp>Thu Dec 29 07:48:42 IST 2011</ax21:lasttradetimestamp>
     <ax21:low>-164.80767926468306</ax21:low><ax21:marketcap>9451314.231029626</ax21:marketcap>
     <ax21:name>IBM Company</ax21:name><ax21:open>-161.41234152690964</ax21:open>
     
    <ax21:peratio>25.74977555860659</ax21:peratio><ax21:percentagechange>-1.2214036358135663</ax21:percentagechange>
    
    <ax21:prevclose>189.46935681818218</ax21:prevclose><ax21:symbol>IBM</ax21:symbol><ax21:volume>8611</ax21:volume>
     </ns:return></ns:getsimplequoteresponse></soapenv:body></soapenv:envelope>
```
