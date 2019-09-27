# Connection Throttling

With the default HTTP transport (PassThrough transport) you can enable connection throttling to restrict the number of simultaneous open connections. 

To enable connection throttling, update the following property in the deployment.toml file:

```toml
[[transport.http]]
max_open_connections = 2
```

This will restrict simultaneous open incoming connections to 2. To disable throttling, delete the `         max_open_connections        `
setting or set it to -1.

!!! Info
    Connection throttling is never exact. For example, setting this property to 2 will result in roughly two simultaneous open connections at any given time.
