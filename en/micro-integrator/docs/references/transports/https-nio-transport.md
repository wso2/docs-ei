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

##### Transport Parameters (Common to both receiver and the sender):

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Requried</p></th>
<th><p>Possible Values</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>keystore</p></td>
<td><p>The default keystore to be used by the receiver or the sender should be specified here along with its related parameters as an XML fragment. The path to the keystore file, its type and the passwords to access the keystore should be stated in the XML. The keystore would be used by the transport to initialize a set of key managers.</p></td>
<td><p>Yes</p></td>
<td><p>&lt;parameter name="keystore"&gt;<br />
&lt;KeyStore&gt;<br />
&lt;Location&gt;lib/identity.jks&lt;/Location&gt;<br />
&lt;Type&gt;JKS&lt;/Type&gt;<br />
&lt;Password&gt;password&lt;/Password&gt;<br />
&lt;KeyPassword&gt;password&lt;/KeyPassword&gt;<br />
&lt;/KeyStore&gt;<br />
&lt;/parameter&gt;</p></td>
</tr>
<tr class="even">
<td><p>truststore</p></td>
<td><p>The default trust store to be used by the receiver or the sender should be specified here along with its related parameters as an XML fragment. The location of the trust store file, its type and the password should be stated in the XML body. The truststore is used by the transport to initialize a set of trust managers.</p></td>
<td><p>Yes</p></td>
<td><p>&lt;parameter name="truststore"&gt;<br />
&lt;TrustStore&gt;<br />
&lt;Location&gt;lib/identity.jks&lt;/Location&gt;<br />
&lt;Type&gt;JKS&lt;/Type&gt;<br />
&lt;Password&gt;password&lt;/Password&gt;<br />
&lt;/TrustStore&gt;<br />
&lt;/parameter&gt;</p></td>
</tr>
<tr class="odd">
<td><p>customSSLProfiles</p></td>
<td><p>The HTTPS NIO transport supports the concept of custom SSL profiles. An SSL profile is a user defined keystore-truststore pair. Such an SSL profile can be associated with one or more target servers. For example, when the HTTPS sender connects to a target server, it will use the SSL profile associated with the target server. If no custom SSL profiles are configured for the target server, the default keystore-truststore pair will be used. Using this feature the NIO HTTPS sender can connect to different target servers using different certificates and identities. The given example only contains a single SSL profile, but there can be as many profiles as required. Each profile must be associated with at least one target server. If a profile should be associated with multiple target servers, the server list should be specified as a comma-separated<br />
list. A target server is identified by a host-port pair.</p></td>
<td><p>No</p></td>
<td><p>&lt;parameter name="customSSLProfiles&gt;<br />
&lt;profile&gt;<br />
&lt;servers&gt; <a href="http://www.test.org:80">www.test.org:80</a> ,<br />
<a href="http://www.test2.com:9763">www.test2.com:9763</a> &lt;/servers&gt;<br />
&lt;KeyStore&gt;<br />
&lt;Location&gt;/path/to/identity/store<br />
&lt;/Location&gt;<br />
&lt;Type&gt;JKS&lt;/Type&gt;<br />
&lt;Password&gt;password&lt;/Password&gt;<br />
&lt;KeyPassword&gt;password<br />
&lt;/KeyPassword&gt;<br />
&lt;/KeyStore&gt;<br />
&lt;TrustStore&gt;<br />
&lt;Location&gt;path/to/trust/store<br />
&lt;/Location&gt;<br />
&lt;Type&gt;JKS&lt;/Type&gt;<br />
&lt;Password&gt;password&lt;/Password&gt;<br />
&lt;/TrustStore&gt;<br />
&lt;/profile&gt;<br />
&lt;/parameter&gt;</p></td>
</tr>
</tbody>
</table>

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
