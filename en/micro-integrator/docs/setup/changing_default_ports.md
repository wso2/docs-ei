# Changing the Default Ports

When you run multiple WSO2 Micro Integrator instances or a cluster of instances on a single server or virtual machine (VM),
you must change the default ports to avoid port conflicts.

## Default ports

By default, the Micro Integrator is **internally** configured with a port offset of 10. Listed below are the ports that are effective in the Micro Integrator by default (due to the internal port offset of 10).

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
			<code>8290</code>
		</td>
		<td>
			HTTP Passthrough transport.
		</td>
	</tr>
	<tr>
		<td>
			<code>8253</code>
		</td>
		<td>
			HTTPS Passthrough transport
		</td>
	</tr>
	<tr>
		<td>
			<code>9164</code>
		</td>
		<td>
			The <a href="../../administer-and-observe/working-with-management-api">Management API</a> of WSO2 Micro Integrator is an internal REST API, which is used to obtain administrative information of the server instance.</br></br>
			<b>Configuring the Management API port</b></br>
			The Management API port can be configured in the <code>deployment.toml</code> file (stored in the <code>MI_HOME/conf</code> folder) as shown below.</br>
			<div>
				<code>[mediation]</code></br>
				<code>internal_http_api_enable = true </code></br>
				<code>internal_https_api_port = 9154 </code>
			</div>
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

## Changing default ports

There are two ways to manually offset the [default ports](#default-ports).

!!! Tip
	-	The internal offset of 10 is overriden by this manual offset. That is, if the manual offset is 3, the default ports will change as follows:
		- `8293` -> `8290`
		- `8253` -> `8250`
		- `9164` -> `9161`
	-	Note that if you manually set an offset of 10 using the following method, you will get the same [default ports](#default-ports).

-   Pass the port offset to the server during startup.

    ```bash tab='On MacOS/Linux/Centos'
    ./micro-integrator.sh -DportOffset=3
    ```

    ```bash tab='On Windows'
    micro-integrator.bat -DportOffset=3
    ```

-   Open the `deployment.toml` file (stored in the `MI_HOME/conf` folder) and add the port offset as follows:

    ```toml
    [server]
    offset = 3
    ```