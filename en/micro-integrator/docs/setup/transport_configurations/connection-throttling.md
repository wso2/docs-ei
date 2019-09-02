# Connection throttling

With the PassThrough transport and HTTP NIO transport, you can enable
connection throttling to restrict the number of simultaneous open
connections. To enable connection throttling, edit the
`         <EI_HOME>/conf/nhttp.properties        ` (for the HTTP NIO
transport) or `         <EI_HOME>/conf/passthru-http.properties        `
(for the PassThrough transport), and add the following line:

`         max_open_connections = 2        `

This will restrict simultaneous open incoming connections to 2. To
disable throttling, delete the `         max_open_connections        `
setting or set it to -1.

!!! Info
    Connection throttling is never exact. For example, setting this property to 2 will result in roughly two simultaneous open connections at any given time.
