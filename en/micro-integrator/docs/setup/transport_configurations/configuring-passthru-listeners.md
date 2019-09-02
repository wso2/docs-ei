# Configuring Listeners

The PassThrough transport has 4 HTTP/HTTPS listneres by default. It
includes 2 `         PassThroughHttpListener        ` threads and 2
`         PassThroughHttpSSLListener        ` threads.

You can configure the number of listeners in the
`         <EI_HOME>/conf/passthru-http.properties        ` file using
the `         io_threads_per_reactor        ` property. You are able to
define any number of listeners as there is no maximum limit defined in
the code level.

!!! Note
    The number of listener threads is double the value of the `         io_threads_per_reactor        ` property because the same number of `         PassThroughHttpListener        ` and `         PassThroughHttpSSLListener        ` threads are created.

For example, if you defined the value for the
`         io_threads_per_reactor        ` property as 5, you have 5
`         PassThroughHttpListener        ` threads and 5
`         PassThroughHttpSSLListener        ` threads. Therefore, the
total number of listeners are 10.
