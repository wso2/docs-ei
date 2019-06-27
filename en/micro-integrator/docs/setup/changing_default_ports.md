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
The default port offset value is 0. There are two ways to set an offset
to a port:

-   Pass the port offset to the server during startup. The following
    command starts the server with the default port incremented by 3
    `          :./wso2server.sh -DportOffset=3         `
-   Set the Ports section of
    `          <PRODUCT_HOME>/repository/conf/carbon.xml         ` as
    follows: `          <Offset>3</Offset>         `

When you set the server-level port offset as shown above,
all the ports used by the server
will change automatically.

## Default Ports of WSO2 Micro Integrator

This page describes the default ports that are used for each WSO2
product when the port offset is 0.

### Servlet transport ports

Listed below are the default ports that are used in WSO2 Enterprise
Integrator (WSO2 EI) when the port offset is 0.

- 8290 - HTTP servlet transport
- 8253 - HTTPS servlet transport

### LDAP server ports

Provided by default in the WSO2 Carbon platform.

-   10389 - Used in WSO2 products that provide an embedded LDAP server

### KDC ports

-   8000 - Used to expose the Kerberos key distribution center server

### JMX monitoring ports

WSO2 Carbon platform uses TCP ports to monitor a running Carbon instance
using a JMX client such as JConsole. By default, JMX is enabled in all
products. You can disable it using
`         <PRODUCT_HOME>/repository/conf/etc/jmx.xml        ` file.

-   11111 - RMIRegistry port. Used to monitor Carbon remotely
-   9999 - RMIServer port. Used along with the RMIRegistry port when
    Carbon is monitored from a JMX client that is behind a firewall

### Clustering ports

To cluster any running Carbon instance, either one of the following
ports must be opened.

-   45564 - Opened if the membership scheme is multicast
-   4000 - Opened if the membership scheme is wka

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
    `          <PRODUCT_HOME>/repository/conf/log4j.properties         `
    file.


## Default ports of other WSO2 products

See the relevant links to view the ports used by all other WSO2 products:

- Ports of [WSO2 API Manager](https://docs.wso2.com/display/AM260/Default+Ports+of+WSO2+API-M+Analytics)
- Ports of [WSO2 Identity Server](https://docs.wso2.com/display/IS580/Default+Ports+of+WSO2+Products)