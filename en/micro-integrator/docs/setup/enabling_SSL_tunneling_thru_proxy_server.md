# Enabling SSL Tunneling through a Proxy Server

If your proxy service connects to a back-end server through a proxy
server, you can enable secure socket layer (SSL) tunneling through the
proxy server to prevent any intermediate proxy services from interfering
with the communication. SSL tunneling is available when your proxy
service uses either the [HTTP PassThrough
transport](https://docs.wso2.com/display/EI650/PassThrough+Transport) or
the [HTTP-NIO
transport](https://docs.wso2.com/display/EI650/HTTP-NIO+Transport).

The following section walks you through the steps to enable SSL
tunneling through a proxy server. Here we will use
[Squid](http://www.squid-cache.org/) as the caching and forwarding HTTP
web proxy.

## Setting up Squid

Follow the steps below to set up Squid:

1.  Install Squid as described [here](http://wiki.squid-cache.org/SquidFaq/InstallingSquid) .
2.  Add the following lines in the `<SQUID_HOME>/etc/squid3/squid.conf` file:

    ``` java
        acl SSL_ports port 443 8443 8448 8248 8280
        acl Safe_ports port 80 # http
        acl Safe_ports port 21 # ftp
        acl Safe_ports port 443 # https
        acl Safe_ports port 70 # gopher
        acl Safe_ports port 210 # wais
        acl Safe_ports port 1025–65535 # unregistered ports
        acl Safe_ports port 280 # http-mgmt
        acl Safe_ports port 488 # gss-http
        acl Safe_ports port 591 # filemaker
        acl Safe_ports port 777 # multiling http
        acl CONNECT method CONNECT
    
        auth_param basic program /usr/lib/squid3/basic_ncsa_auth /etc/squid3/basic_pw
        auth_param basic children 5
        auth_param basic realm Squid proxy-caching web server
        auth_param basic credentialsttl 2 hours
        auth_param basic casesensitive off
    
        acl ncsa_users proxy_auth REQUIRED
        http_access allow ncsa_users
    
        http_port 3128
    ```

## Configuring SSL tunneling

To configure SSL tunneling through the proxy server, open the esb.toml file and update the following parameters:

```java
[config_heading]
http.proxyHost=false                           
hostName=
portNumber=               
HostnameVerifier=AllowAll

```

For example, if the host name and port number of proxy server is
`         localhost:8080        ` , your
`         transportSender        ` configurations for
`         PassThroughHttPSender        ` and
`         PassThroughHttpSSLSender        ` would be as follows:

  **PassThroughHTTPSender**

   ``` xml
    <transportSender name="http" class="org.apache.synapse.transport.passthru.PassThroughHttpSender">
            <parameter name="non-blocking" locked="false">true</parameter>
            <parameter name="http.proxyHost" locked="false">localhost</parameter>
            <parameter name="http.proxyPort" locked="false">8080</parameter>
    </transportSender>
   ```

  **PassThroughHttpSSLSender**

   ```xml
    <transportSender name="https" class="org.apache.synapse.transport.passthru.PassThroughHttpSSLSender">
            <parameter name="non-blocking" locked="false">true</parameter>
            <parameter name="keystore" locked="false">
                <KeyStore>
                    <Location>repository/resources/security/wso2carbon.jks</Location>
                    <Type>JKS</Type>
                    <Password>wso2carbon</Password>
                    <KeyPassword>wso2carbon</KeyPassword>
                </KeyStore>
            </parameter>
            <parameter name="truststore" locked="false">
                <TrustStore>
                    <Location>repository/resources/security/client-truststore.jks</Location>
                    <Type>JKS</Type>
                    <Password>wso2carbon</Password>
                </TrustStore>
            </parameter>
            <parameter name="http.proxyHost" locked="false">localhost</parameter>
            <parameter name="http.proxyPort" locked="false">8080</parameter>
            <parameter name="HostnameVerifier">AllowAll</parameter>
    </transportSender>
   ```