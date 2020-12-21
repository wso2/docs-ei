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
    command (on MacOS) starts the server with the default port incremented by 3: `./micro-integrator.sh -DportOffset=3`
-   Open the `deployment.toml` file (stored in the `<MI_HOME>/conf` directory) and add the port offset as follows:

    ```toml
    [server]
    offset = 3
    ```

When you set the server-level port offset as shown above, all the ports used by the server will change automatically.

## Default Ports of WSO2 Micro Integrator

This page describes the default ports that are used for WSO2 Micro Integrator when the port offset is 0.

### Passthrough transport ports

Listed below are the default ports that are used in WSO2 Micro Integrator when the port offset is 10.

- 8290: HTTP Passthrough transport
- 8253: HTTPS Passthrough transport

### Management API port

The [Management API](../../administer-and-observe/working-with-management-api) of WSO2 Micro Integrator is an internal REST API, which is used to obtain administrative information of the server instance. The default port of the Management API used in WSO2 Micro Integrator when the port offset is 10:

-   9164: HTTPS Internal API port

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
The JMX ports (RMIRegistryPort and the RMIServerPort) can be configured in the `deployment.toml ` file (stored in the `<MI_HOME>/conf` directory) as shown below. 

```toml
[monitoring.jmx]
rmi_hostname = localhost
rmi_registry_port = 9999
rmi_server_port = 11111
```

-   9999: RMIRegistry port. Used to monitor Carbon remotely.
-   11111: RMIServer port. Used along with the RMIRegistry port when the server is monitored from a JMX client that is behind a firewall.

## Analytics ports

By default, EI Analytics is **internally** configured with a port offset of 1. Listed below are the ports that are effective in EI Analytics by default (due to the internal port offset of 1).

<table>
	<tr>
		<th>
			Default Port
		</th>
		<th>
			Description
		</th>
	</tr>
	<tr>
    	<td>
    		<code>9645</code>
    	</td>
    	<td>
    		The port of the Analytics Portal.
    	</td>
    </tr>
	<tr>
		<td>
			<code>9091</code>
		</td>
		<td>
			The HTTP port of the Management API of WSO2 Stream Processor.</br></br>
			<b>Configuring the default HTTP port</b></br>
			If required, you can manually change the HTTP port in the <code>deployment.yaml</code> file (stored in <code>EI_ANALYTICS_HOME/conf/server</code> folder) as shown below.
			```yaml
			wso2.transport.http:
              listenerConfigurations:
                -
                  id: "default"
                  host: "0.0.0.0"
                  port: http_port
			```
			<b>Note</b>: With the default internal port offset, the effective port will be <code>http_port + 1</code>.
		</td>
	</tr>
	<tr>
		<td>
			<code>9444</code>
		</td>
		<td>
			The HTTPS port of the Management API of WSO2 Stream Processor.</br></br>
			<b>Configuring the default HTTPS port</b></br>
			If required, you can manually change the HTTPS port in the <code>deployment.yaml</code> file (stored in <code>EI_ANALYTICS_HOME/conf/server</code> folder) as shown below.
			```yaml
			wso2.transport.http:            
              listenerConfigurations:
                -
                  id: "msf4j-https"
                  host: "0.0.0.0"
                  port: https_port
                  scheme: https
			```
			<b>Note</b>: With the default internal port offset, the effective port will be <code>https_port + 1</code>.
		</td>
	</tr>    
	<tr>
    	<td>
    		<code>7712</code>
    	</td>
    	<td>
    		Thrift SSL port for secure transport, where the client is authenticated to WSO2 Stream Processor.
    	</td>
    </tr>
	<tr>
    	<td>
    		<code>7612</code>
    	</td>
    	<td>
    		Thrift TCP port to receive events from clients to WSO2 Stream Processor.
    	</td>
    </tr>
</table>

## Random ports

Certain ports are randomly opened during server startup. This is due to
specific properties and configurations that become effective when the
product is started. Note that the IDs of these random ports will change
every time the server is started.

-   A random TCP port will open during server startup because of the
    `-Dcom.sun.management.jmxremote` property set in
    the server startup script. This property is used for the
    JMX monitoring facility in JVM.
-   A random UDP port is opened at server startup due to the log4j
    appender (`SyslogAppender`), which is configured in the `<MI_HOME>/conf/log4j2.properties` file.