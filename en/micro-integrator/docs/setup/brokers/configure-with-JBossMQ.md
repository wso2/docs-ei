# Connecting to JBossMQ

This section describes how to configure WSO2 Micro Integrator to connect with [JBossMQ](https://community.jboss.org/wiki/JBossMQ). The default JMS provider in JBoss Application Server 4.2. (JBossMQ was replaced by [JBoss Messaging](http://www.jboss.org/jbossmessaging) in JBoss Application Server 5.0.).

To configure the JMS transport with JBossMQ:

1.  Copy the following client libraries to the `MI_HOME/lib` directory.  
    -   `JBOSS_HOME/lib/jboss­system.jar`
    -   `JBOSS_HOME/client/jbossall­client.jar`

2.  If you want the Micro Integrator to receive messages from an JBossMQ instance, or to send messages to an ActiveMQ instance, you need to update the deployment.toml file with the relevant connection parameters.

    - Add the following configurations to enable the JMS listener with ActiveMQ connection parameters.
        ```toml
        [[transport.jms.listener]]
        name = "myQueueListener"
        parameter.initial_naming_factory = "org.jnp.interfaces.NamingContextFactory"
        parameter.broker_name = "jboss" 
        parameter.provider_url = "jnp://localhost:1099"
        parameter.connection_factory_name = "QueueConnectionFactory"
        parameter.connection_factory_type = "queue/susaQueue"
        ```
        <parameter name="java.naming.factory.url.pkgs" locked="false">org.jnp.interfaces:org.jboss.naming</parameter>

    - Add the following configurations to enable the JMS sender with ActiveMQ connection parameters.
        ```toml
        [[transport.jms.sender]]
        name = "myQueueSender"
        parameter.initial_naming_factory = "org.jnp.interfaces.NamingContextFactory"
        parameter.broker_name = "jboss" 
        parameter.provider_url = "jnp://localhost:1099"
        parameter.connection_factory_name = "QueueConnectionFactory"
        parameter.connection_factory_type = "queue/susaQueue"
        ```

4.  Start WSO2 Micro Integrator and ensure that the logs print messages indicating that the JMS listener and sender are started, and that the JMS transport is initialized.
