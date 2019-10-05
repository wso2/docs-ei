# Using the JMS Inbound Protocol with WSO2 MB

This section describes how to configure the JMS inbound protocol with
the Broker profile of WSO2 Enterprise Integrator (WSO2 EI).

1.  If you have not already done so, see [Installing the
    Product](https://docs.wso2.com/display/EI650/Installing+the+Product)
    for details on installing WSO2 EI.
2.  Open a command line terminal and start the broker profile that is
    shipped with your WSO2 EI distribution by running one of the
    following startup scripts from the
    `          <EI_HOME>/wso2/broker/bin         ` directory:
    -   On Linux/Mac OS:  sh wso2server.sh
    -   On Windows:  wso2server.bat --run

3.  Configure the JMS inbound listener. Following is a sample JMS
    inbound listener configuration:

    ``` 
        <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                         name="jms_inbound"
                         sequence="request"
                         onError="fault"
                         protocol="jms"
                         suspend="false">
           <parameters>
              <parameter name="interval">1000</parameter>
              <parameter name="sequential">true</parameter>
              <parameter name="coordination">true</parameter>
              <parameter name="java.naming.factory.initial">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
              <parameter name="java.naming.provider.url">conf/jndi.properties</parameter>
              <parameter name="transport.jms.ConnectionFactoryJNDIName">QueueConnectionFactory</parameter>
              <parameter name="transport.jms.ConnectionFactoryType">queue</parameter>
              <parameter name="transport.jms.Destination">JMSMS</parameter>
              <parameter name="transport.jms.SessionTransacted">false</parameter>
              <parameter name="transport.jms.SessionAcknowledgement">AUTO_ACKNOWLEDGE</parameter>
              <parameter name="transport.jms.CacheLevel">1</parameter>
              <parameter name="transport.jms.SubscriptionDurable">false</parameter>
              <parameter name="transport.jms.SharedSubscription">false</parameter>
           </parameters>
        </inboundEndpoint>
    ```

    !!! Info
        For more information on the JMS configuration parameters used in the code segments above, see [JMS Connection Factory Parameters](https://docs.wso2.com/display/EI650/JMS+Transport#JMSTransport-ConnectionFactoryParams).

4.  Open `          <EI_HOME>/conf/jndi.properties         ` file and
    make a reference to the running EI-Broker runtime as specified
    below:  
    1.  Use **carbon** as the virtual host.
    2.  Define a queue named `            JMSMS           ` .
    3.  Comment out the topic, since it is not required in this
        scenario. However, in order to avoid getting the
        `             javax.naming.NameNotFoundException:TopicConnectionFactory            `
        exception during server startup, make a reference to the Broker
        profile from the
        `             TopicConnectionFactory            ` as well.

        For example:

        ``` java
                # register some connection factories
                # connectionfactory.[jndiname] = [ConnectionURL]
                connectionfactory.QueueConnectionFactory = amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'
                connectionfactory.TopicConnectionFactory = amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'
                # register some queues in JNDI using the form
                # queue.[jndiName] = [physicalName]
                queue.JMSMS=JMSMS
                queue.StockQuotesQueue = StockQuotesQueue
        ```

5.  Ensure that Broker profile is running, and then open a command
    prompt (or a shell in Linux) and go to the
    `          <EI_HOME>/bin         ` directory.
6.  Start the Integration runtime server by executing  
    `          sh integrator.sh -Dqpid.dest_syntax=BURL         ` (on
    Linux/OS X) **or**  
    `          integrator.bat -Dqpid.dest_syntax=BURL         ` (on
    Windows).

Now you have an instance of Broker profile and a WSO2 EI inbound
endpoint configured, up and running.
