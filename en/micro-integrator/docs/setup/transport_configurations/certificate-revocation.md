# Verifying certificate revocation

The default HTTPS transport listener (Secured Passthrough) and transport sender, as well as the HTTP NIO transport listener and sender can verify with the certificate authority whether a certificate is still trusted before it completes an SSL connection. If the certificate authority has revoked the certificate, a connection will not be completed. 

When this feature is enabled, the transport listener verifies client
certificates when a client tries to make an HTTPS connection with the
Micro Integrator. The transport sender verifies server
certificates when the Micro Integrator tries to make an HTTPS
connection with a backend server. 

When this feature is enabled, the Micro Integrator attempts to
use the Online Certificate Status Protocol (OCSP) to verify with the
certificate authority at the handshake phase of the SSL protocol. If the
OCSP is not supported by the certificate authority, the Micro Integrator uses Certified Revocation Lists (CRL) instead. The verification
process checks all the certificates in a certificate chain.

## Enabling Certificate Revocation (Passthrough Transport)

To enable this feature for the HTTP passthrough, add the following parameters for the HTTP transport receiver and sender in the ei.toml file:

```toml tab='Passthrough Listener'
[[transport.http]]
listener.CertificateRevocationVerifier=""
listener.CacheSize=1024
listenerCacheDelay=1000

``` 

```toml tab='Passthrough Sender'
[[transport.http]]
sender.CertificateRevocationVerifier=""
sender.CacheSize=1024
sender.CacheDelay=1000

``` 

## Enabling Certificate Revocation (HTTPS-NIO Transport)

To enable this feature for the HTTPS NIO transport, add the following parameters for the transport receiver and sender in the ei.toml file:

```toml tab='Passthrough Listener'
[[[custom_transport.listener]]]
class="org.apache.synapse.transport.nhttp.HttpCoreNIOSSLListener"
listener.CertificateRevocationVerifier=""
``` 

```toml tab='Passthrough Sender'
[[[custom_transport.sender]]]
sender.CertificateRevocationVerifier="org.apache.synapse.transport.nhttp.HttpCoreNIOSSLSender"
sender.CertificateRevocationVerifier=""
```