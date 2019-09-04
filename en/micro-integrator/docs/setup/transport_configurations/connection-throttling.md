# Connection throttling

With the default HTTP transport (PassThrough transport) and the HTTP NIO transport, you can enable connection throttling to restrict the number of simultaneous open connections. 

To enable connection throttling, update the following property in the ei.toml file:

```toml tab='HTTP Passthrough'
[[transport.http]]
max_open_connections = 2
```

```toml tab='HTTP-NIO'
[[[custom_transport.listener]]]
max_open_connections=2

[[[custom_transport.sender]]]
max_open_connections=2
```

This will restrict simultaneous open incoming connections to 2. To disable throttling, delete the `         max_open_connections        `
setting or set it to -1.

!!! Info
    Connection throttling is never exact. For example, setting this property to 2 will result in roughly two simultaneous open connections at any given time.
