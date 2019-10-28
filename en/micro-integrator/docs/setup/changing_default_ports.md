# Changing the Default Ports

When you run multiple WSO2 products, multiple instances of the same
product, or multiple WSO2 product clusters on the same server or virtual
machines (VMs), you must change their default ports with an offset value
to avoid port conflicts. Port offset defines the number by which all
ports defined in the runtime such as the HTTP/S ports will be changed.
For example, if the default HTTP port is 9763 and the port offset is 1,
the effective HTTP port will change to 9764. For each additional WSO2
product instance, you set the port offset to a unique value.

## Setting a port offset
The default port offset value is 10. There are two ways to set an offset
to a port:

-   Pass the port offset to the server during startup. The following
    command starts the server with the default port incremented by 3
    `          :./micro-integrator.sh -DportOffset=3         `
-   Set the Ports section the deployment.toml file as follows:

    ```toml
    [server]
    offset = 3
    ```

When you set the server-level port offset as shown above, all the ports used by the server will change automatically.

## Default Ports of WSO2 Micro Integrator

This page describes the default ports that are used for each WSO2
product when the port offset is 0.

### Passthrough transport ports

Listed below are the default ports that are used in WSO2 Micro Integrator when the port offset is 10.

- 8290: HTTP Passthrough transport
- 8253: HTTPS Passthrough transport

### Management API port

The Management API of WSO2 Micro Integrator is an internal REST API, which is used to obtain administrative information of the server instance. 
See [Management API](../../administer-and-observe/working-with-management-api)

The default port of the Management API used in WSO2 Micro Integrator when the port offset is 10:

9164 - HTTPS Internal API port

The Management API port can be configured in the `deployment.toml ` file (stored in the
`         <MI_HOME>/conf        ` directory) as shown below. 

```toml
[mediation]
internal_http_api_enable = true 
internal_https_api_port = 9154 
```

### JMX monitoring ports

WSO2 Micro Integrator uses TCP ports to monitor a running server instance
using a JMX client such as JConsole. 
The JMX ports (RMIRegistryPort and the RMIServerPort) can be
configured in the `deployment.toml ` file (stored in the
`         <MI_HOME>/conf        ` directory) as shown
below. 

```toml
[monitoring.jmx]
rmi_hostname = localhost
rmi_registry_port = 9999
rmi_server_port = 11111
```

-   9999: RMIRegistry port. Used to monitor Carbon remotely
-   11111: RMIServer port. Used along with the RMIRegistry port when the server is monitored from a JMX client that is behind a firewall.

### Random ports

Certain ports are randomly opened during server startup. This is due to
specific properties and configurations that become effective when the
product is started. Note that the IDs of these random ports will change
every time the server is started.

-   A random TCP port will open at server startup because of the
    `          -Dcom.sun.management.jmxremote         ` property set in
    the server startup script. This property is used for the
    JMX monitoring facility in JVM.
-   A random UDP port is opened at server startup due to the log4j
    appender ( `          SyslogAppender         ` ), which is
    configured in the
    `MI_HOMEy/conf/log4j2.properties         `
    file.


## Default ports of other WSO2 products

See the relevant links to view the ports used by all other WSO2 products:

- Ports of [WSO2 API Manager](https://docs.wso2.com/display/AM260/Default+Ports+of+WSO2+API-M+Analytics)
- Ports of [WSO2 Identity Server](https://docs.wso2.com/display/IS580/Default+Ports+of+WSO2+Products)