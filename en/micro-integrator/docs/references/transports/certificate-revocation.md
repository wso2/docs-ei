# Verifying certificate revocation

`org.apache.synapse.transport.passthru.PassThroughHttpSSLListener`
as well as
` org.apache.synapse.transport.passthru.PassThroughHttpSSLSender        `
can verify with the certificate authority whether a certificate is still
trusted before it completes an SSL connection. If the certificate
authority has revoked the certificate, a connection will not be
completed. 

To enable this feature, you need to add the
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
certificates when the Micro Integrator tries to make an HTTPS
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
set to `         true        ` , the Micro Integrator attempts to
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

## HTTPS-NIO

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
