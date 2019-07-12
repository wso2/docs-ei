# JMX Monitoring

Java Management Extensions (JMX) is a technology that lets you implement
management interfaces for Java applications. **JConsole** is a
JMX-compliant monitoring tool, which comes with the Java Development Kit
(JDK) 1.5 or later versions. Therefore, when you use a WSO2 product, JMX
is enabled by default, which allows you to monitor the product using
JConsole.

Java Management Extensions (JMX) is a technology that lets you implement
management interfaces for Java applications. A management interface, as
defined by JMX, is composed of named objects called MBeans (Management
Beans). MBeans are registered with a name (an ObjectName) in an
MBeanServer. To manage a resource or many resources in your application,
you can write an MBean defining its management interface and register
that MBean in your MBeanServer. The content of the MBeanServer can then
be exposed through various protocols, implemented by protocol
connectors, or protocol adaptors.

### Configuring JMX in a WSO2 product

JMX is enabled in WSO2 products by default, which ensures that the JMX
server starts automatically when you start a particular product.
 Additionally, you can enable JMX separately for the various datasources
that are used by the product. Once JMX is enabled, you can log in to the
JConsole tool and monitor your product as explained in the [next
section](#JMX-BasedMonitoring-MonitoringaWSO2productwithJConsole) .

#### Configuring JMX ports for the server

The default JMX ports (RMIRegistryPort and the RMIServerPort) are
configured in the `         carbon.xml        ` file (stored in the
`         <PRODUCT_HOME>/repository/conf        ` directory) as shown
below. If required, you can update these default values.

``` java
    <JMX>
         <!--The port RMI registry is exposed-->
         <RMIRegistryPort>9999</RMIRegistryPort>
         <!--The port RMI server should be exposed-->
         <RMIServerPort>11111</RMIServerPort>
    </JMX> 
```

#### Disabling JMX for the server

The JMX configuration is available in the jmx `         .xml        `
file (stored in the
`         <PRODUCT_HOME>/repository/conf/etc        ` directory) as
shown below. You can disable the JMX server for your product by setting
the `         <StartRMIServer>        ` property to
`         false        ` . Note that this configuration refers to the
[JMX ports configured in the `          carbon.         ` xml
file](#JMX-BasedMonitoring-ConfiguringJMXportsfortheserver) .  

``` java
    <JMX xmlns="http://wso2.org/projects/carbon/jmx.xml">
        <StartRMIServer>true</StartRMIServer>
        <!-- HostName, or Network interface to which this RMI server should be bound -->
        <HostName>localhost</HostName>
        <!--  ${Ports.JMX.RMIRegistryPort} is defined in the Ports section of the carbon.xml-->
        <RMIRegistryPort>${Ports.JMX.RMIRegistryPort}</RMIRegistryPort>
        <!--  ${Ports.JMX.RMIRegistryPort} is defined in the Ports section of the carbon.xml-->
        <RMIServerPort>${Ports.JMX.RMIServerPort}</RMIServerPort>
    </JMX>
```

#### Enabling JMX for a datasource

You can enable JMX for a datasource by adding the
`         <jmxEnabled>true</jmxEnabled>        ` element to the
datasource configuration file. For example, to enable JMX for the
default Carbon datasource in your product, add the following property to
the `         master-datasources        ` `         .        ` xml file
(stored in the
`         <PRODUCT_HOME>/repository/conf/datasources        `
directory).

``` java
    <datasource>
                <name>WSO2_CARBON_DB</name>
                <description>The datasource used for registry and user manager</description>
                <jndiConfig>
                    <name>jdbc/WSO2CarbonDB</name>
                </jndiConfig>
                <definition type="RDBMS">
                    <configuration>
                        <url>jdbc:h2:./repository/database/WSO2CARBON_DB;DB_CLOSE_ON_EXIT=FALSE;LOCK_TIMEOUT=60000</url>
                        <username>wso2carbon</username>
                        <password>wso2carbon</password>
                        <driverClassName>org.h2.Driver</driverClassName>
                        <maxActive>50</maxActive>
                        <maxWait>60000</maxWait>
                        <testOnBorrow>true</testOnBorrow>
                        <validationQuery>SELECT 1</validationQuery>
                        <validationInterval>30000</validationInterval>
                        <defaultAutoCommit>false</defaultAutoCommit>
                        <jmxEnabled>true</jmxEnabled>
                    </configuration>
                </definition>
            </datasource>
```

### Monitoring a WSO2 product with JConsole

Jconsole is a JMX-compliant monitoring tool, which comes with the Java
Development Kit (JDK) 1.5 and newer versions. You can find this tool
inside your `         <JDK_HOME>/bin        ` directory. See the
instructions on [Installing the JDK](index) for more information.

#### Starting the WSO2 product with JMX

First, start the WSO2 product:

1.  Open a command prompt and navigate to the
    `          <PRODUCT_HOME>/bin         ` directory.
2.  Execute the product startup script (
    `           wso2server.sh          ` for Linux and
    `           wso2server.bat          ` for Windows) to start the
    server.  

        !!! info
    
        If [JMX is enabled](_JMX-Based_Monitoring_) , the **JMX server URL**
        will be published on the console when the server starts as shown
        below.
    
    ``` java
            INFO {org.wso2.carbon.core.init.CarbonServerManager} -  JMX Service URL  : service:jmx:rmi://<your-ip>:11111/jndi/rmi://<your-ip>:9999/jmxrmi
    ```
        

Once the product server is started, you can start the JC
`         onsole        ` tool as follows:

1.  Open a command prompt and navigate to the
    `          <JDK_HOME>/bin         ` directory.
2.  Execute the j `          console         ` command to open the
    log-in screen of the **Java Monitoring & Management Console** as
    shown below.  
    ![](attachments/53125400/57746949.png){width="400"}
3.  Enter the connection details in the above screen as follows:
    1.  Enter the **JMX server URL** in the **Remote Process** field.
        This URL is published on the command prompt when you start the
        WSO2 server as explained
        [above](#JMX-BasedMonitoring-start_jconsole) .

                !!! info
        
                Tip
        
                If you are connecting with a remote IP address instead of
                localhost, you need to bind the JMX service to the externally
                accessible IP address by adding the following system property to
                the product startup script stored in the
                `             <PRODUCT_HOME>/bin            ` directory (
                `             wso2server.sh            ` for Linux and
                `             wso2server.bat            ` for Windows). For more
                information, read [Troubleshooting Connection Problems in
                JConsole](https://blogs.oracle.com/jmxetc/entry/troubleshooting_connection_problems_in_jconsole)
                .
        
        ``` java
                    -Djava.rmi.server.hostname=<IP_ADDRESS_WHICH_YOU_USE_TO_CONNECT_TO_SERVER>
        ```
            
                    Be sure to restart the server after adding the above property.
            

    2.  Enter values for the **Username** and **Password** fields to log
        in. If you are logging in as the administrator, you can use the
        same administrator account that is used to log in to the
        product's management console: admin/admin.

                !!! info
        
                Make sure that the user ID you are using for JMX monitoring is
                assigned a role that has the **Server Admin** permission. See
                [Configuring
                Roles](https://docs.wso2.com/display/ADMIN44x/Configuring+Roles)
                for further information about configuring roles assigned to
                users. Any user assigned to the **admin** role can log in
                to JMX.
        

4.  Click **Connect** to open the **Java Monitoring & Management
    Console** . The following tabs will be available:  

    -   [**Overview**](#9afcf1ee058b47f1b9aad001de3c9e3c)
    -   [**Memory**](#babfbfb43d7f4c6cb9aeb66a204ab4ba)
    -   [**Threads**](#4513d7c9cae94db5a55fc5ddb928d4e5)
    -   [**Classes**](#bf8eed7f5ec64f3dab024a7c4dc450c6)
    -   [**VM**](#5390757d2f1f4742820046d0634f84a9)
    -   [**MBeans**](#7ed52baab28a416aa9795d9c2929f8a7)

    See the Oracle documentation on [using
    JConsole](http://docs.oracle.com/javase/7/docs/technotes/guides/management/jconsole.html)
    for more information on these tabs.

    ![](attachments/53125400/57747081.png){width="600"}

    See the Oracle documentation on [using
    JConsole](http://docs.oracle.com/javase/7/docs/technotes/guides/management/jconsole.html)
    for more information on these tabs.

    ![](attachments/53125400/57747082.png){width="600"}

    See the Oracle documentation on [using
    JConsole](http://docs.oracle.com/javase/7/docs/technotes/guides/management/jconsole.html)
    for more information on these tabs.

    ![](attachments/53125400/57747083.png){width="600"}

    See the Oracle documentation on [using
    JConsole](http://docs.oracle.com/javase/7/docs/technotes/guides/management/jconsole.html)
    for more information on these tabs.

    ![](attachments/53125400/57747084.png){width="600"}

    See the Oracle documentation on [using
    JConsole](http://docs.oracle.com/javase/7/docs/technotes/guides/management/jconsole.html)
    for more information on these tabs.

    ![](attachments/53125400/57747085.png){width="600"}

    See the Oracle documentation on [using
    JConsole](http://docs.oracle.com/javase/7/docs/technotes/guides/management/jconsole.html)
    for more information on these tabs.

    ![](attachments/53125400/57747086.png){width="600"}

#### Using the ServerAdmin MBean

When you go to the **MBeans** tab in the JConsole, the **ServerAdmin**
MBean will be listed under the "org.wso2.carbon" domain as shown
below.  
![](attachments/53125400/57746985.png){width="600"}

The **ServerAdmin** MBean is used for administering the product server
instance. There are several server attributes such as "ServerStatus",
"ServerData" and "ServerVersion". The "ServerStatus" attribute can take
any of the following values:

-   RUNNING
-   SHUTTING\_DOWN
-   RESTARTING
-   IN\_MAINTENANCE

![](attachments/53125400/57746978.png){width="650"}

The **ServerAdmin** MBean has the following operations:

| Operation              | Description                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------|
| **shutdown**           | Forcefully shut down the server.                                                                            |
| **restart**            | Forcefully restart the server.                                                                              |
| **restartGracefully**  | Wait till all current requests are served and then restart.                                                 |
| **shutdownGracefully** | Wait till all current requests are served and then shutdown.                                                |
| **startMaintenance**   | Switch the server to maintenance mode. No new requests will be accepted while the server is in maintenance. |
| **endMaintenance**     | Switch the server to normal mode if it was switched to maintenance mode earlier.                            |

  
![](attachments/53125400/57746982.png){width="650"}

#### Using the ServiceAdmin MBean

This MBean is used for administering services deployed in your product.
Its attributes are as follows:

| Attribute                    | Description                                                          |
|------------------------------|----------------------------------------------------------------------|
| **NumberOfActiveServices**   | The number of services which can currently serve requests.           |
| **NumberOfInactiveServices** | The number of services which have been disabled by an administrator. |
| **NumberOfFaultyServices**   | The number of services which are faulty.                             |

![](attachments/45968791/57747545.png){width="600"}

The operations available in the ServiceAdmin MBean:

| Operation                                          | Description                                                                                      |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------|
| **startService** ( [p1:string](http://p1string/) ) | The p1 parameter is the service name. You can activate a service using this operation.           |
| **stopService** ( [p1:string](http://p1string/) )  | The p1 parameter is the service name. You can deactivate/disable a service using this operation. |

![](attachments/45968791/57747543.png){width="600"}

#### Using the StatisticsAdmin MBean

This MBean is used for monitoring system and server statistics. Its
attributes are as follows:

| Attributes                | Description                                                                                                                                      |
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| **AvgSystemResponseTime** | The average response time for all the services deployed in the system. The beginning of the measurement is the time at which the server started. |
| **MaxSystemResponseTime** | The maximum response time for all the services deployed in the system. The beginning of the measurement is the time at which the server started. |
| **MinSystemResponseTime** | The minimum time for all the services deployed in the system. The beginning of the measurement is the time at which the server started.          |
| **SystemFaultCount**      | The total number of faults that occurred in the system since the server was started.                                                             |
| **SystemRequestCount**    | The total number of requests that has been served by the system since the server was started.                                                    |
| **SystemResponseCount**   | The total number of response that has been sent by the system since the server was started.                                                      |

![](attachments/45968791/57747542.png){width="600"}

Operations available in the **Statistics** MBean:

| Operation                                                                                         | Description                                                                                                                                                                                                         |
|---------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **getServiceRequestCount** ( [p1:string](http://p1string/) )                                      | The p1 parameter is the service name. You can get the total number of requests received by this service since the time it was deployed, using this operation.                                                       |
| **getServiceResponseCount** ( [p1:string](http://p1string/) )                                     | The p1 parameter is the service name. You can get the total number of responses sent by this service since the time it was deployed, using this operation.                                                          |
| **getServiceFaultCount** ( [p1:string](http://p1string/) )                                        | The p1 parameter is the service name. You can get the total number of fault responses sent by this service since the time it was deployed, using this operation.                                                    |
| **getMaxServiceResponseTime** ( [p1:string](http://p1string/) )                                   | The p1 parameter is the service name. You can get the maximum response time of this service since deployment.                                                                                                       |
| **getMinServiceResponseTime** ( [p1:string](http://p1string/) )                                   | The p1 parameter is the service name. You can get the minimum response time of this service since deployment.                                                                                                       |
| **getAvgServiceResponseTime** ( [p1:string](http://p1string/) )                                   | The p1 parameter is the service name. You can get the average response time of this service since deployment.                                                                                                       |
| **getOperationRequestCount** ( [p1:string](http://p1string/) , [p2:string](http://p2string/) )    | The p1 parameter is the service name. The p2 parameter is the operation name. You can get the total number of requests received by this operation since the time its service was deployed, using this operation.    |
| **getOperationResponseCount** ( [p1:string](http://p1string/) , [p2:string](http://p2string/) )   | The p1 parameter is the service name. The p2 parameter is the operation name. You can get the total number of responses sent by this operation since the time its service was deployed, using this operation.       |
| **getOperationFaultCount** ( [p1:string](http://p1string/) , [p2:string](http://p2string/) )      | The p1 parameter is the service name. The p2 parameter is the operation name. You can get the total number of fault responses sent by this operation since the time its service was deployed, using this operation. |
| **getMaxOperationResponseTime** ( [p1:string](http://p1string/) , [p2:string](http://p2string/) ) | The p1 parameter is the service name. The p2 parameter is the operation name. You can get the maximum response time of this operation since deployment.                                                             |
| **getMinOperationResponseTime** ( [p1:string](http://p1string/) , [p2:string](http://p2string/) ) | The p1 parameter is the service name. The p2 parameter is the operation name. You can get the minimum response time of this operation since deployment.                                                             |
| **getAvgOperationResponseTime** ( [p1:string](http://p1string/) , [p2:string](http://p2string/) ) | The p1 parameter is the service name. The p2 parameter is the operation name. You can get the average response time of this operation since deployment.                                                             |

![](attachments/45968791/57747540.png){width="600"}

#### Using the DataSource MBean

If you have [JMX enabled for a datasource connected to the
product](#JMX-BasedMonitoring-EnablingJMXforadatasource) , you can
monitor the performance of the datasource using this MBean. The
**DataSource** MBean will be listed as shown below.  
![](attachments/53125400/57747100.png){width="600"}

**Example:** If you have JMX enabled for the default Carbon datasource
in the `         master-datasources.xml.        ` file, the [JDBC
connection pool
parameters](http://tomcat.apache.org/tomcat-7.0-doc/jdbc-pool.html) that
are configured for the Carbon datasource will be listed as attributes as
shown below. See the [performance tuning
guide](https://docs.wso2.com/display/ADMIN44x/Performance+Tuning) for
instructions on how these parameters are configured for a datasource.  
![](attachments/53125400/57747097.png){width="650"}

#### Using product-specific MBeans

The WSO2 product that you are using may have product-specific MBeans
enabled for monitoring and managing specific functions. See the
documentation for your product for detailed instructions on such
product-specific MBeans.

### Monitoring a WSO2 product with Jolokia

[Jolokia](https://jolokia.org) is a JMX-HTTP bridge, which is an
alternative to JSR-160 connectors. It is an agent-based approach that
supports many platforms. In addition to basic JMX operations, it
enhances JMX monitoring with unique features like bulk requests and
fine-grained security policies.

Follow the steps below to use Jolokia to monitor a WSO2 product.

1.  Download [Jolokia OSGi Agent](https://jolokia.org/download.html) .
    (These instructions are tested with the Jolokia OSGI Agent version
    1.3.6 by downloading the `          jolokia-osgi-1.3.6.jar         `
    file.)
2.  Add it to the
    `           <PRODUCT-HOME>/repository/components/dropins/          `
    directory.

        !!! tip
    
        In WSO2 EI, add it to the `           <EI-HOME>          `
        `           /dropins/          ` directory.
    

3.  Start the WSO2 product server.

Once the server starts, you can read MBeans using Jolokia APIs.
Following are a few examples.

-   List all available MBeans: <http://localhost:9763/jolokia/list>
    (Change the appropriate hostname and port accordingly.)
-   WSO2 ESB MBean:
    <http://localhost:9763/jolokia/read/org.apache.synapse:Name=https-sender,Type=PassThroughConnections/ActiveConnections>
-   Reading Heap Memory:
    <http://localhost:9763/jolokia/read/java.lang:type=Memory/HeapMemoryUsage>

For more information on the JMX MBeans that are available in WSO2
products, see [Monitoring a WSO2 product with
JConsole](#JMX-BasedMonitoring-mbeans) .


## MBeans for WSO2 EI

When JMX is enabled, WSO2 EI exposes a number of management resources as
JMX Management Beans (MBeans) that can be used for managing and
monitoring the running server.  When you start JConsole, you can monitor
these MBeans from the **MBeans** tab. While some of these MBeans (
**ServerAdmin** and **DataSource** ) are common to all WSO2 products,
some MBeans are specific to WSO2 EI.

!!! tip

The common MBeans are explained in detail in the [WSO2 Administration
Guide](https://docs.wso2.com/display/ADMIN44x/JMX-Based+Monitoring) .
Listed below are the MBeans that are specific to WSO2 EI.

For details on using JMX based mediation flow statistics in your EI
server and detailed explanation of the relevant MBeans, see [Monitoring
JMX Based
Statistics](https://docs.wso2.com/display/EI650/Monitoring+JMX+Based+Statistics)
.


![](attachments/119130236/119130239.png){height="400"}

This section summarizes the attributes and operations available for the
following WSO2 EI specific MBeans:

-   [Connection MBeans](#JMXMonitoring-ConnectionMBeans)
-   [Latency MBeans](#JMXMonitoring-LatencyMBeans)
-   [Threading MBeans](#JMXMonitoring-ThreadingMBeans)
-   [Transport MBeans](#JMXMonitoring-TransportMBeans)
-   [Broker-specific MBeans](#JMXMonitoring-Broker-specificMBeans)

### Connection MBeans

These MBeans provide connection statistics for the HTTP and HTTPS
transports.

You can view the following Connection MBeans:

-   `          org.apache.synapse/PassThroughConnections/http-listener         `
-   `          org.apache.synapse/PassThroughConnections/http-sender         `
-   `          org.apache.synapse/PassThroughConnections/https-listener         `
-   `          org.apache.synapse/PassThroughConnections/https-sender         `

**Attributes**

| Attribute Name                                       | Description                                                        |
|------------------------------------------------------|--------------------------------------------------------------------|
| `             ActiveConnections            `         | Number of currently active connections.                            |
| `             ActiveConnectionsPerHosts            ` | A map of the number of connections against hosts.                  |
| `             LastXxxConnections            `        | The number of connections created during last Xxx time period.     |
| `             RequestSizesMap            `           | A map of the number of requests against their sizes.               |
| `             ResponseSizesMap            `          | A map of the number of responses against their sizes.              |
| `             LastResetTime            `             | The time when the connection-statistic recordings were last reset. |

**Operations**

| Operation Name                     | Description                                                 |
|------------------------------------|-------------------------------------------------------------|
| `             reset()            ` | Clear recorded connection statistics and restart recording. |

### Latency MBeans

This view provides statistics of the latencies from all backend services
connected through the HTTP  and HTTPS transports. These statistics are
provided as an aggregate value.

You can view the following Latency MBeans:

-   `          org.apache.synapse/PassthroughLatencyView/nio-http-http         `
-   `          org.apache.synapse/PassthroughLatencyView/nio-https-https         `

**Attributes**

| Attribute Name                               | Description                                                                                                              |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| `             AllTimeAvgLatency            ` | Average latency since the latency recording was last reset.                                                              |
| `             LastxxxAvgLatency            ` | Average latency for last xxx time period. For example, LastHourAvgLatency returns the average latency for the last hour. |
| `             LastResetTime            `     | The time when the latency-statistic recordings were last reset.                                                          |

**Operations**

| Operation Name                     | Description                                              |
|------------------------------------|----------------------------------------------------------|
| `             reset()            ` | Clear recorded latency statistics and restart recording. |

### Threading MBeans

These MBeans are only available in the NHTTP transport and not in the
default Pass-Through transport.

You can view the following Threading MBeans:

-   `          org.apache.synapse/Threading/HttpClientWorker         `
-   `          org.apache.synapse/Threading/HttpServerWorker         `

**Attributes**

| Attribute Name                                            | Description                                                         |
|-----------------------------------------------------------|---------------------------------------------------------------------|
| `             TotalWorkerCount            `               | Total worker threads related to this server/client.                 |
| `             AvgUnblockedWorkerPercentage            `   | Time-averaged unblocked worker thread percentage.                   |
| `             AvgBlockedWorkerPercentage            `     | Time-averaged blocked worker thread percentage.                     |
| `             LastXxxBlockedWorkerPercentage            ` | Blocked worker thread percentage averaged for last Xxx time period. |
| `             DeadLockedWorkers            `              | Number of deadlocked worker threads since last statistics reset.    |
| `             LastResetTime            `                  | The time the thread statistic recordings were last reset.           |

**Operations**

| Operation Name                     | Description                                            |
|------------------------------------|--------------------------------------------------------|
| `             reset()            ` | Clear recorded thread statistic and restart recording. |

### Transport MBeans

For each transport listener and sender enabled in WSO2 EI, there will be
an MBean under the `         org.apache.axis2/Transport        ` domain.
For example, when the JMS transport is enabled, the following MBean will
be exposed:

-   `          org.apache.axis2/Transport/jms-sender-                     n                   `

You can also view the following Transport MBeans:

-   `          org.apache.synapse/Transport/passthru-http-receiver         `
-   `          org.apache.synapse/Transport/passthru-http-sender         `
-   `          org.apache.synapse/Transport/passthru-https-receiver         `
-   `          org.apache.synapse/Transport/passthru-https-sender         `

**Attributes**

| Attribute Name                               | Description                                                                                                                    |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------|
| `             ActiveThreadCount            ` | Threads active in this transport listener/sender.                                                                              |
| `             AvgSizeReceived            `   | The average size of received messages.                                                                                         |
| `             AvgSizeSent            `       | The average size of sent messages.                                                                                             |
| `             BytesReceived            `     | The number of bytes received through this transport.                                                                           |
| `             BytesSent            `         | The number of bytes sent through this transport.                                                                               |
| `             FaultsReceiving            `   | The number of faults encountered while receiving.                                                                              |
| `             FaultsSending            `     | The number of faults encountered while sending.                                                                                |
| `             LastResetTime            `     | The time when the last transport listener/sender statistic recording was reset.                                                |
| `             MaxSizeReceived            `   | Maximum message size of received messages.                                                                                     |
| `             MaxSizeSent            `       | Maximum message size of sent messages.                                                                                         |
| `             MetricsWindow            `     | Time difference between current time and last reset time in milliseconds.                                                      |
| `             MinSizeReceived            `   | Minimum message size of received messages.                                                                                     |
| `             MinSizeSent            `       | Minimum message size of sent messages.                                                                                         |
| `             MessagesReceived            `  | The total number of messages received through this transport.                                                                  |
| `             MessagesSent            `      | The total number of messages sent through this transport.                                                                      |
| `             QueueSize            `         | The number of messages currently queued. Messages get queued if all the worker threads in this transport thread pool are busy. |
| `             ResponseCodeTable            ` | The number of messages sent against their response codes.                                                                      |
| `             TimeoutsReceiving            ` | Message receiving timeout.                                                                                                     |
| `             TimeoutsSending            `   | Message sending timeout.                                                                                                       |

**Operations**

| Operation Name                                                   | Description                                                                                                                                        |
|------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| `             start()            `                               | Start this transport listener/sender.                                                                                                              |
| `             stop()            `                                | Stop this transport listener/sender.                                                                                                               |
| `             resume()            `                              | Resume this transport listener/sender which is currently paused.                                                                                   |
| `             resetStatistics()            `                     | Clear recorded transport listener/sender statistics and restart recording.                                                                         |
| `             pause()            `                               | Pause this transport listener/sender which has been started.                                                                                       |
| `             maintenenceShutdown(long gracePeriod)            ` | Stop processing new messages, and wait the specified maximum time for in-flight requests to complete before a controlled shutdown for maintenence. |

## Disabling the JMX thread view

Dumping JMX threads is an expensive operation that affects the CPU
consumption when many threads are being processed at the same time.

WSO2 EI has enabled thread dumping by default. Therefore, to avoid
dumping all the threads
[here](https://github.com/wso2/wso2-synapse/blob/master/modules/commons/src/main/java/org/apache/synapse/commons/jmx/ThreadingView.java#L268)
, you can configure the property given below.

!!! info

It is recommended not to dump the thread especially when you have
enabled WSO2 EI analytics in a production environment.


1.  Open the `          <EI_HOME>/         `
    `          synapse.properties         ` file.
2.  Add the following property to the file and save the file.

    ``` java
        synapse.jmx.thread.view.enabled=false
    ```
