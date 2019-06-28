# PassThrough Transport

**PassThrough Transport** is a non-blocking HTTP transport
implementation based on HTTP Core NIO, and is the default HTTP transport
shipped with the ESB Profile of WSO2 Enterprise Integrator (WSO2 EI).
Although the PassThrough Transport is somewhat similar to the NHTTP
transport, it overcomes all the limitations of the NHTTP transport and
provides a significant performance gain. The PassThrough Transport also
has a simpler and cleaner model for forwarding messages back and forth.

`         org.apache.synapse.transport.passthru.PassThroughHttpSSLListener        `
is the listener class of the PassThrough Transport, and it receives
HTTPS inbound requests.

`         org.apache.synapse.transport.passthru.PassThroughHttpSSLSender        `
is the sender class of the PassThrough Transport, and it sends out HTTPS
outbound requests. Both the listener and sender of the PassThrough
Transport can [verify certificate
revocation](#PassThroughTransport-revocation) .

-   [Verifying certificate
    revocation](#PassThroughTransport-Verifyingcertificaterevocationrevocation)
-   [Connection throttling](#PassThroughTransport-Connectionthrottling)
-   [Configuring Listeners](#PassThroughTransport-ConfiguringListeners)

### Verifying certificate revocation

`         org.apache.synapse.transport.passthru.PassThroughHttpSSLListener        `
as well as
`         org.apache.synapse.transport.passthru.PassThroughHttpSSLSender        `
can verify with the certificate authority whether a certificate is still
trusted before it completes a SSL connection. If the certificate
authority has revoked the certificate, a connection will not be
completed. To enable this feature, you need to add the
`         CertificateRevocationVerifier        ` parameter to the
receiver or sender in the
`         <EI_HOME>/conf/axis2/axis2.xml        ` file.

Setting the `         CertificateRevocationVerifier        `
parameter at the transport listener allows you to verify client
certificates when a client tries to make an HTTPS connection with the
ESB Profile of WSO2 EI. Following is a sample transport listener
configuration that you can add in the `         axis2.xml        ` file
to enable certificate revocation verification:

``` java
    <transportReceiver name="https" class="org.apache.synapse.transport.passthru.PassThroughHttpSSLListener">
    ...
       <parameter name="CertificateRevocationVerifier" enable="true">
                    <CacheSize>1024</CacheSize>
                    <CacheDelay>1000</CacheDelay>
       </parameter>
    </transportReceiver>
```

Setting the `         CertificateRevocationVerifier        `
parameter at the transport sender allows you to verify server
certificates when the ESB Profile of WSO2 EI tries to make an HTTPS
connection with a backend server. Following is a sample transport sender
configuration that you can add in the `         axis2.xml        ` file
to enable certificate revocation verification:

``` java
    <transportSender name="https" class="org.apache.synapse.transport.passthru.PassThroughHttpSSLSender">
    ...
       <parameter name="CertificateRevocationVerifier" enable="true">
                    <CacheSize>1024</CacheSize>
                    <CacheDelay>1000</CacheDelay>
       </parameter>
    </transportSender>
```

When the `         CertificateRevocationVerifier        ` parameter is
set to `         true        ` , the ESB Profile of WSO2 EI attempts to
use the Online Certificate Status Protocol (OCSP) to verify with the
certificate authority at the handshake phase of the SSL protocol. If the
OCSP is not supported by the certificate authority, the ESB Profile of
WSO2 EI uses Certified Revocation Lists (CRL) instead. The verification
process checks all the certificates in a certificate chain.

The response from the certificate authority includes the verification
and the duration for which the verification is valid. To prevent any
performance overhead of continuous HTTP calls, this verification
response is cached for the duration specified by the certificate
authority, so that subsequent verification calls are not required until
the response has expired. There are two Least Recently Used (LRU)
in-memory caches for OCSP and CRL, which are automatically managed by a
dedicated CacheManager thread for each cache. These CacheManagers update
expired cache entries and maintain the LRU cache replacement policy.

### Connection throttling

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

!!! info

Connection throttling is never exact. For example, setting this property
to 2 will result in roughly two simultaneous open connections at any
given time.


### Configuring Listeners

The PassThrough transport has 4 HTTP/HTTPS listneres by default. It
includes 2 `         PassThroughHttpListener        ` threads and 2
`         PassThroughHttpSSLListener        ` threads.

You can configure the number of listeners in the
`         <EI_HOME>/conf/passthru-http.properties        ` file using
the `         io_threads_per_reactor        ` property. You are able to
define any number of listeners as there is no maximum limit defined in
the code level.

!!! note

The number of listener threads is double the value of the
`         io_threads_per_reactor        ` property because the same
number of `         PassThroughHttpListener        ` and
`         PassThroughHttpSSLListener        ` threads are created.

For example, if you defined the value for the
`         io_threads_per_reactor        ` property as 5, you have 5
`         PassThroughHttpListener        ` threads and 5
`         PassThroughHttpSSLListener        ` threads. Therefore, the
total number of listeners are 10.

