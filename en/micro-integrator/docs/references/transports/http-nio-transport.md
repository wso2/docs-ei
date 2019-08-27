# HTTP-NIO Transport

**HTTP-NIO transport** is a module of the Apache Synapse project. The
two classes that implement the receiver and sender APIs are
`         org.apache.synapse.transport.nhttp.HttpCoreNIOListener        `
and
`         org.apache.synapse.transport.nhttp.HttpCoreNIOSender        `
respectively. These classes are available in the JAR file named
`         synapse-nhttp-transport.jar        ` . The transport
implementation is based on Apache HTTP Core - NIO and uses a
configurable pool of non-blocking worker threads to grab incoming HTTP
messages off the wire.


!!! Info
	In transport parameter tables, literals displayed in italic mode under the "Possible Values" column should be considered as fixed literal constant values. Those values can be directly put in transport configurations.

## Example: Connection throttling

With the HTTP PassThrough and HTTP NIO transports, you can enable
connection throttling to restrict the number of simultaneous open
connections. To enable connection throttling, edit the
`         <EI_HOME>/conf/nhttp.properties        ` (for the HTTP NIO
transport) or `         <EI_HOME>/conf/passthru-http.properties        `
(for the PassThrough transport) and add the following line:

`         max_open_connections = 2        `

This will restrict simultaneous open incoming connections to 2. To
disable throttling, delete the `         max_open_connections        `
setting or set it to -1.

!!! info

Connection throttling is never exact. For example, setting this property
to 2 will result in roughly two simultaneous open connections at any
given time.

