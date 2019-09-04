# Configuring Listeners

The default HTTP transport (PassThrough transport) of WSO2 Micro Integrator has 4 HTTP/HTTPS listneres configured. This includes 2 `         PassThroughHttpListener        ` threads and 2 `         PassThroughHttpSSLListener        ` threads.

You can configure the number of listeners for the HTTP transport in the ei.toml file:

```toml
[[transport.http]]
io_thread_count=2
```
You are able to define any number of listeners (by updating the `io_thread_count` value) as there is no maximum limit defined in the code level.

!!! Note
    The number of listener threads is double the value of the `io_threads_per_reactor` property because the same number of `         PassThroughHttpListener        ` and `         PassThroughHttpSSLListener        ` threads are created. For example, if you defined the value for the `         io_threads_per_reactor        ` property as 5, you have 5 `         PassThroughHttpListener        ` threads and 5 `         PassThroughHttpSSLListener        ` threads. Therefore, the total number of listeners are 10.
