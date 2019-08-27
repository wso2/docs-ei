# HTTPS-NIO Transport

HTTPS-NIO transport is also a module that comes from the Apache Synapse
code base. The receiver class is named as follows:

`         org.apache.synapse.transport.nhttp.HttpCoreNIOSSLListener        `

The sender class is named as follows:

`         org.apache.synapse.transport.nhttp.HttpCoreNIOSSLSender        `

These classes are available in the
`         synapse-nhttp-transport.jar        ` bundle. These classes
simply extend the [HTTP-NIO](_HTTP-NIO_Transport_) implementation by
adding SSL support to it. Therefore, they support all the configuration
parameters supported by the HTTP-NIO receiver and sender plus the
parameters in the following table. The sender can also [verify
certification revocation](#HTTPS-NIOTransport-revocation) . For general
information on transports, see [Working with
Transports](https://docs.wso2.com/display/EI650/Carrying+Messages) .

### Verifying certificate revocation

The HTTPS-NIO transport sender can verify with the certificate authority
whether the certificate is still trusted before it completes the SSL
connection. If the certificate authority has revoked the certificate,
the connection will not be completed. To enable this feature, add the
`         CertificateRevocationVerifier        ` parameter to the sender
as follows:

``` java
    <transportSender name="https" class="org.apache.synapse.transport.nhttp.HttpCoreNIOSSLSender">
    ...
        <parameter name="CertificateRevocationVerifier">true</parameter>
    </transportSender>
```

When this parameter is set to `         true        ` , Synapse attempts
to use Online Certificate Status Protocol (OCSP) to perform the
verification with the certificate authority at the handshake phase of
SSL protocol. If OCSP is not supported by the certificate authority,
Synapse uses Certified Revocation Lists (CRL) instead. The verification
process checks all the certificates in the certificate chain.

The response from the certificate authority includes the verification
and the duration for which the verification is valid. To prevent the
performance overhead of continuous HTTP calls, this verification
response is cached for the duration specified by the certificate
authority so that subsequent verification calls are not needed until the
response has expired. There are two Least Recently Used (LRU) in-memory
caches for OCSP and CRL, which are automatically managed by a dedicated
CacheManager thread for each cache. These CacheManagers update expired
cache entries and maintain the LRU cache replacement policy.
