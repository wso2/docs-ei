# Configure with HornetQ

This section describes how to configure WSO2 Enterprise Integrator's
[JMS transport](https://docs.wso2.com/display/ESB500/JMS+Transport) with
HornetQ, which is an open source project to build a multi-protocol,
asynchronous messaging system.

When configuring WSO2 EI's [JMS
transport](https://docs.wso2.com/display/ESB500/JMS+Transport) with
HornetQ, you can either configure with a standalone HornetQ server or
with HornetQ embedded in a JBoss Enterprise Application Platform (JBoss
EAP) server.

Go to the required tab for step by step instructions based on how you
need to configure the WSO2 EI's [JMS
transport](https://docs.wso2.com/display/ESB500/JMS+Transport) with
HornetQ.

!!! note

From the below configurations, do the ones in the **axis2.xml** file
based on the profile you use as follows:

-   To enable the JMS transport in the Integration profile, edit the
    `          <EI_HOME>/conf/axis2/axis2.xml         ` file.
-   To enable the JMS transport in other profiles, edit the
    `          <EI_HOME>/wso2/<PROFILE_HOME>/conf/axis2/axis2.xml         `
    file. `          <PROFILE_HOME>         ` refers to the main
    directory of the profile inside the WSO2 EI distribution. For
    example, to enable the JMS transport in the Business Process
    profile, edit the
    `          <EI_HOME>/wso2/business-process/conf/axis2/axis2.xml         `
    file


-   [**Configure with a standalone HornetQ
    server**](#0cd76aaee4504285a5890d0c2a914656)
-   [**Configure with HornetQ embedded in a JBoss EAP
    server**](#188ba5643f3644bfa2597d6815a40562)

Follow the instructions below to configure WSO2 EI JMS transport with
a standalone HornetQ server.

1.  Download HornetQ from the [HornetQ
    Downloads](http://hornetq.jboss.org/downloads.html) site.  
2.  Create a sample queue by editing the
    `              <HORNET_HOME>/config/stand-alone/non-clustered/hornetq-jms.xml             `
    file as follows:

    ``` xml
        <queue name="wso2">
              <entry name="/queue/mySampleQueue"/>
        </queue>
    ```

3.  Add the following two connection entries to the same file. These
    entries are required to enable WSO2 EI to act as a JMS consumer.

    ``` xml
            <connection-factory name="QueueConnectionFactory">
                  <xa>false</xa>
                  <connectors>
                     <connector-ref connector-name="netty"/>
                  </connectors>
                  <entries>
                     <entry name="/QueueConnectionFactory"/>
                  </entries>
            </connection-factory>
             
             
            <connection-factory name="TopicConnectionFactory">
                  <xa>false</xa>
                  <connectors>
                     <connector-ref connector-name="netty"/>
                  </connectors>
                  <entries>
                     <entry name="/TopicConnectionFactory"/>
                  </entries>
            </connection-factory>
    ```

4.  If you have not already done so, download WSO2 EI and install as
    described in the [Installation
    Guide.](https://docs.wso2.com/display/EI650/Installation+Guide)
5.  Download the
    `                             hornet-all-new.jar                           `
    file and copy it into the
    `              <EI_HOME>/lib/             ` directory. .

        !!! info
    
        Note
    
        If you are packing the JARs yourself, make sure you remove the
        javax.jms package from the assembled JAR to avoid the carbon runtime
        from picking this implementation of JMS over the bundled-in
        distribution.
    

6.  Uncomment the following line in the
    `              <EI_HOME>/conf/axis2/axis2.xml             ` file to
    enable the JMS transport sender on axis2core.

    ``` xml
        <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender"/>
    ```

7.  Enable the JMS listener with the HornetQ configuration parameters in
    the `              <EI_HOME>/conf/axis2/axis2.xml             ` file
    by un-commenting the following lines of code.

    ``` xml
            <transportReceiver name="jms"
             class="org.apache.axis2.transport.jms.JMSListener">
             <parameter name="myTopicConnectionFactory" locked="false">
              <parameter name="java.naming.factory.initial"locked="false">org.jnp.interfaces.NamingContextFactory</parameter>
              <parameter name="java.naming.factory.url.pkgs"locked="false">org.jboss.naming:org.jnp.interfaces</parameter>
              <parameter name="java.naming.provider.url" locked="false">jnp://localhost:1099</parameter>
              <parameter name="transport.jms.ConnectionFactoryJNDIName"
               locked="false">TopicConnectionFactory</parameter>
              <parameter name="transport.jms.ConnectionFactoryType"
               locked="false">topic</parameter>
             </parameter>
             <parameter name="myQueueConnectionFactory" locked="false">
              <parameter name="java.naming.factory.initial"locked="false">org.jnp.interfaces.NamingContextFactory</parameter>
              <parameter name="java.naming.factory.url.pkgs"locked="false">org.jboss.naming:org.jnp.interfaces</parameter>
              <parameter name="java.naming.provider.url" locked="false">jnp://localhost:1099</parameter>
              <parameter name="transport.jms.ConnectionFactoryJNDIName"
               locked="false">QueueConnectionFactory</parameter>
              <parameter name="transport.jms.ConnectionFactoryType"
               locked="false">queue</parameter>
             </parameter>
             <parameter name="default" locked="false">
              <parameter name="java.naming.factory.initial"locked="false">org.jnp.interfaces.NamingContextFactory</parameter>
              <parameter name="java.naming.factory.url.pkgs"locked="false">org.jboss.naming:org.jnp.interfaces</parameter>
              <parameter name="java.naming.provider.url" locked="false">jnp://localhost:1099</parameter>
              <parameter name="transport.jms.ConnectionFactoryJNDIName"
               locked="false">QueueConnectionFactory</parameter>
              <parameter name="transport.jms.ConnectionFactoryType"
               locked="false">queue</parameter>
             </parameter>
             
            </transportReceiver>
    ```

8.  Start HornetQ with the following command.
    -   On Windows:
        `               <HORNETQ_HOME>\bin\run.bat --run              `

    <!-- -->

    -   On Linux/Solaris:
        `                sh <HORNETQ_HOME>/bin/run.sh               `

Now you have configured WSO2 EI's JMS transport
with a standalone HornetQ server. The next section describes how you can
test the configuration.

### Testing the configuration

To test the configuration, we create a proxy service named
`            JMSPublisher           ` to publish messages from WSO2 EI
to HornetQ sample queue, and create the
`            JMSListener           ` queue to read messages from the
HornetQ sample queue.

![](attachments/119130351/119130355.png)

1.  Start the WSO2 EI management console. See [Running the
    Product](https://docs.wso2.com/display/EI650/Running+the+Product)
    for more information.
2.  Create the `              JMSPublisher             ` proxy service
    with the following configuration:

    ``` xml
             <proxy xmlns="http://ws.apache.org/ns/synapse"
                   name="JMSPublisher"
                   transports="https,http"
                   statistics="enable"
                   trace="enable"
                   startOnLoad="true">
               <target>
                  <inSequence>
                     <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
                     <property name="Accept-Encoding" scope="transport" action="remove"/>
                     <property name="Content-Length" scope="transport" action="remove"/>
                     <property name="Content-Type" scope="transport" action="remove"/>
                     <property name="User-Agent" scope="transport" action="remove"/>
                     <property name="OUT_ONLY" value="true"/>
                     <log level="full"/>
                     <send>
                        <endpoint>
                           <address uri="jms:/queue/mySampleQueue?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.jnp.interfaces.NamingContextFactory&amp;java.naming.provider.url=jnp://localhost:1099&amp;transport.jms.DestinationType=queue"/>
                        </endpoint>
                     </send>
                  </inSequence>
               </target>
               <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
               <description>HornetQ-WSO2 ESB sample</description>
            </proxy>
    ```

        !!! info
    
        Note
    
        -   The proxy service is created for the
            `               SimpleStockQuoteService              ` service
            shipped with WSO2 EI samples in this example so that the
            configuration can be tested by sending a request to call one of
            the operations of the service.
        -   The `               OUT_ONLY              ` parameter is set to
            `               true              ` since this proxy service is
            created only for the purpose of publishing the messages of WSO2
            EI to the `               mySampleQueue              ` queue
            specified in the address URI.
        -   You may have to change the host name, port etc. of the JMS
            string based on your environment
    

3.  Create the `              JMSListener             ` proxy service
    with the following configuration:

    ``` xml
        <proxy xmlns="http://ws.apache.org/ns/synapse"
               name="JMSListener"
               transports="jms"
               statistics="disable"
               trace="disable"
               startOnLoad="true">
           <target>
              <inSequence>
                 <log level="custom">
                    <property name="JMS LISTENER PROXY" value="LOCATED"/>
                 </log>
                 <log level="full"/>
                 <drop/>
              </inSequence>
           </target>
           <parameter name="transport.jms.ContentType">
              <rules>
                 <jmsProperty>contentType</jmsProperty>
                 <default>application/xml</default>
              </rules>
           </parameter>
           <parameter name="transport.jms.Destination">queue/mySampleQueue</parameter>
           <description/>
        </proxy>
    ```

4.  Use a client application of your choice to send a request to the
    endpoint of the `              JMSPublisher             ` proxy
    service. In this example, the following request is sent using
    SOAPUI, calling the `              placeOrder             `
    operation of the
    `              SimpleStockQuoteService             ` for which the
    `              JMSPublisher             ` proxy service is created.

    ``` xml
            <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
               <soapenv:Header/>
               <soapenv:Body>
                  <ser:placeOrder>
                     <!--Optional:-->
                     <ser:order>
                        <!--Optional:-->
                        <xsd:price>20</xsd:price>
                        <!--Optional:-->
                        <xsd:quantity>20</xsd:quantity>
                        <!--Optional:-->
                        <xsd:symbol>IBM</xsd:symbol>
                     </ser:order>
                  </ser:placeOrder>
               </soapenv:Body>
            </soapenv:Envelope>
    ```

5.  Check the log on your WSO2 EI terminal.  You will see the following
    log, which indicates that the request published in the queue is
    picked by the `              JMSListener             ` proxy.

    ``` xml
            [<TIME Stamp>]  INFO - LogMediator JMS LISTENER PROXY = LOCATED
        
            [<TIME Stamp>]  INFO - LogMediator To: , WSAction: "urn:placeOrder", SOAPAction: "urn:placeOrder", MessageID: ID:be08707e-033d-11e4-8307-25263fbb9173, Direction: request, Envelope: <?xml version="1.0" encoding="utf-8"?><soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"><soapenv:Body><soapenv:Envelope xmlns:xsd="http://services.samples/xsd" xmlns:ser="http://services.samples"><soapenv:Body>
        
                  <ser:placeOrder>
                     <!--Optional:-->
                     <ser:order>
                        <!--Optional:-->
                        <xsd:price>20</xsd:price>
                        <!--Optional:-->
                        <xsd:quantity>20</xsd:quantity>
                        <!--Optional:-->
                        <xsd:symbol>IBM</xsd:symbol>
                     </ser:order>
                  </ser:placeOrder>
               </soapenv:Body></soapenv:Envelope></soapenv:Body></soapenv:Envelope>
    ```

Follow the instructions below to set up and configure WSO2 EI with
HornetQ embedded in a JBoss EAP server.

### Setting up JBoss EAP

This section describes the steps to install JBoss EAP server and create
a message queue within the server.

1.  Download JBoss EAP Server 7.0.0 from [JBoss EAP
    Downloads](http://developers.redhat.com/products/eap/download/) and
    run the JBoss EAP installer as described
    [here](https://access.redhat.com/documentation/en/red-hat-jboss-enterprise-application-platform/version-7.0/installation-guide/#running_the_jboss_eap_installer)
    .
2.  Execute one of the following commands in command prompt to create a
    new application user.  
    -   On Windows:
        `               <EAP_HOME>\bin\add-user.bat -a -u 'SampleUser' -p 'SamplePwd1!' -g 'guest'              `
    -   On Linux/Mac:
        `               <EAP_HOME>/bin/add-user.sh -a -u 'SampleUser' -p 'SamplePwd1!' -g 'guest'              `
3.  Create a sample queue by editing the
    `              <EAP_HOME>/standalone/configuration/standalone-full.xml             `
    file. Add the following content within the \<hornetq-server\>
    element:

    ``` xml
            <jms-destinations>
                  <jms-queue name="sampleQueue">
                      <entry name="queue/test"/>
                      <entry name="java:jboss/exported/jms/queue/test"/>
                  </jms-queue>
            </jms-destinations>
    ```

4.  Start the JBoss EAP server by executing one of the following
    commands in command prompt:
    -   `                On Windows: <EAP_HOME>\bin\standalone.bat -c standalone-full.xml                               `

    -   `                On Linux/Mac: <EAP_HOME>/bin/standalone.sh -c standalone-full.xml               `

5.  Acess the management console of the JBoss EAP server using the
    following URL:  
    `                           http://127.0.0.1:9990                         `
6.  Log in to the Management Console using **admin** as both the
    username and password. In the Profile menu, click Messaging -\>
    Destinations and you will be able to see the queue you added in Step
    4 in the **Queues/Topics** section.  
    ![](attachments/119130351/119130354.png){width="638" height="459"}

Now you have configured the JBoss EAP Server. The next section describes
how to configure WSO2 EI to listen and fetch messages from the queue
that you created above.

### Configuring WSO2 EI

1.  If you have not already done so, download and [install WSO2
    EI](https://docs.wso2.com/display/EI650/Installation+Guide) .
2.  Enable the JMS listener with the JBoss EAP configuration parameters
    in the `              <EI_HOME>/conf/axis2/axis2.xml             `
    file by adding the following lines of code.

    ``` xml
            <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
                <parameter name="QueueConnectionFactory" locked="false">
                        <parameter name="java.naming.factory.initial" locked="false">org.jboss.naming.remote.client.InitialContextFactory</parameter>
                         <parameter name="java.naming.provider.url" locked="false">remote://localhost:4447</parameter>
                        <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">jms/RemoteConnectionFactory</parameter>
                        <parameter name="transport.jms.UserName" locked="false">SampleUser</parameter>
                        <parameter name="transport.jms.Password" locked="false">SamplePwd1!</parameter>
                        <parameter name="java.naming.security.principal" locked="false">SampleUser</parameter>
                        <parameter name="java.naming.security.credentials" locked="false">SamplePwd1!</parameter>
                        <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                </parameter>
            </transportReceiver>
    ```

        !!! info
    
        The username and password created for the guest user in the above
        section are used in the configuration.
    

3.  Copy the `              jboss-client.jar             ` file from the
    `              <EAP_HOME>/bin/client             ` directory to the
    `              <EI_HOME>/lib             ` directory.

        !!! note
    
        Note
    
        After copying the `              jboss-client.jar             ` file
        from the `              <EAP_HOME>/bin/client             `
        directory to the `              <EI_HOME>/lib             `
        directory, be sure to remove the
        `              javax.jms             ` package from the
        `              jboss-client.jar             ` file.  
        ![](attachments/119130351/119130352.png){width="627" height="349"}
    

4.  Enable the JMS sender by uncommenting the following line of code
    in the `              <EI_HOME>/conf/axis2/axis2.xml             `
    file.

    ``` xml
        <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender"/> 
    ```

Now you have configured the WSO2 EI's JMS transport with HornetQ
embedded in a JBoss EAP server. The next section describes how you can
test the configuration.

### Testing the configuration

To test the configuration we create a proxy service named
`            JMSPublisher           ` to publish messages from WSO2 EI
to the HornetQ sample queue, and create the
`            JMSListener           ` queue to read messages from the
HornetQ sample queue.

![](attachments/119130351/119130355.png)

1.  Start WSO2 EI management console. See [Running the
    Product](https://docs.wso2.com/display/EI650/Running+the+Product)
    for more information.
2.  Create the `              JMSPublisher             ` proxy service
    with the following configuration:

    ``` xml
            <proxy xmlns="http://ws.apache.org/ns/synapse"
                   name="JMSPublisher"
                   transports="https,http"
                   statistics="enable"
                   trace="enable"
                   startOnLoad="true">
               <target>
                  <inSequence>
                     <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
                     <property name="Accept-Encoding" scope="transport" action="remove"/>
                     <property name="Content-Length" scope="transport" action="remove"/>
                     <property name="Content-Type" scope="transport" action="remove"/>
                     <property name="User-Agent" scope="transport" action="remove"/>
                     <property name="OUT_ONLY" value="true"/>
                     <log level="full"/>
                     <send>
                        <endpoint>
                           <address uri="jms:/jms/queue/test?transport.jms.ConnectionFactoryJNDIName=jms/RemoteConnectionFactory&amp;java.naming.factory.initial=org.jboss.naming.remote.client.InitialContextFactory&amp;java.naming.provider.url=remote://localhost:4447&amp;transport.jms.DestinationType=queue&amp;transport.jms.UserName=SampleUser&amp;transport.jms.Password=SamplePwd1!&amp;java.naming.security.principal=SampleUser&amp;java.naming.security.credentials=SamplePwd1!"/>            </endpoint>
            </send>
        
                  </inSequence>
               </target>
               <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
               <description>HornetQ-WSO2 ESB sample</description>
            </proxy>
    ```

        !!! info
    
        Note
    
        -   The proxy service is created for the
            `               SimpleStockQuoteService              ` service
            shipped with WSO2 EI samples in this example so that the
            configuration can be tested by sending a request to call one of
            the operations of the service.
        -   The `               OUT_ONLY              ` parameter is set to
            `               true              ` since this proxy service is
            created only for the purpose of publishing the messages of WSO2
            EI to the `               mySampleQueue              ` queue
            specified in the address URI.
        -   You may have to change the host name, port etc. of the JMS
            string based on your environment
    

3.  Create the `              JMSListener             ` proxy service
    with the following configuration:

    ``` xml
        <proxy xmlns="http://ws.apache.org/ns/synapse"
               name="JMSListener"
               transports="jms"
               statistics="disable"
               trace="disable"
               startOnLoad="true">
           <target>
              <inSequence>
                 <log level="custom">
                    <property name="JMS LISTENER PROXY" value="LOCATED"/>
                 </log>
                 <log level="full"/>
                 <drop/>
              </inSequence>
           </target>
           <parameter name="transport.jms.ContentType">
              <rules>
                 <jmsProperty>contentType</jmsProperty>
                 <default>text/plain</default>
              </rules>
           </parameter>
        <parameter name="transport.jms.ConnectionFactory">QueueConnectionFactory</parameter>
           <parameter name="transport.jms.Destination">jms/queue/test</parameter>
        <description/>
    
        </proxy>
    ```

4.  Use a client application of your choice to send a request to the
    endpoint of the `              JMSPublisher             ` proxy
    service. In this example, the following request is sent using
    SOAPUI, calling the `              placeOrder             `
    operation of the
    `              SimpleStockQuoteService             ` for which the
    `              JMSPublisher             ` proxy service is created.

    ``` xml
            <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
               <soapenv:Header/>
               <soapenv:Body>
                  <ser:placeOrder>
                     <!--Optional:-->
                     <ser:order>
                        <!--Optional:-->
                        <xsd:price>20</xsd:price>
                        <!--Optional:-->
                        <xsd:quantity>20</xsd:quantity>
                        <!--Optional:-->
                        <xsd:symbol>IBM</xsd:symbol>
                     </ser:order>
                  </ser:placeOrder>
               </soapenv:Body>
            </soapenv:Envelope>
    ```

5.  Check the log on your WSO2 EI terminal.  You will see the following
    log, which indicates that the request published in the queue is
    picked by the `              JMSListener             ` proxy.

    ``` xml
            [<TIME Stamp>]  INFO - LogMediator JMS LISTENER PROXY = LOCATED
        
            [<TIME Stamp>]  INFO - LogMediator To: , WSAction: "urn:placeOrder", SOAPAction: "urn:placeOrder", MessageID: ID:be08707e-033d-11e4-8307-25263fbb9173, Direction: request, Envelope: <?xml version="1.0" encoding="utf-8"?><soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"><soapenv:Body><soapenv:Envelope xmlns:xsd="http://services.samples/xsd" xmlns:ser="http://services.samples"><soapenv:Body>
        
                  <ser:placeOrder>
                     <!--Optional:-->
                     <ser:order>
                        <!--Optional:-->
                        <xsd:price>20</xsd:price>
                        <!--Optional:-->
                        <xsd:quantity>20</xsd:quantity>
                        <!--Optional:-->
                        <xsd:symbol>IBM</xsd:symbol>
                     </ser:order>
                  </ser:placeOrder>
               </soapenv:Body></soapenv:Envelope></soapenv:Body></soapenv:Envelope>
    ```
