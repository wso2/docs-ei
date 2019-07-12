# HTTPS Servlet Transport

Similar to the HTTP transport, the HTTPS transport consists of a
receiver implementation which comes from the Carbon core component and a
sender implementation which comes from the Apache Axis2 transport
module. In fact, this transport uses exactly the same transport sender
implementation as the HTTP transport. So the two classes that should be
specified in the configuration are
`         org.wso2.carbon.core.transports.http.HttpsTransportListener        `
and
`         org.apache.axis2.transport.http.CommonsHTTPTransportSender        `
for the receiver and sender in the specified order. The configuration
parameters associated with the receiver and the sender are the same as
in HTTP transport. This is also a blocking transport implementation.

However, when using the following class as the receiver implementation,
we need to specify the servlet HTTPS transport configuration in the
transport's XML file.

-   `          org.wso2.carbon.core.transports.http.HttpsTransportListener         `

The class that should be specified as the transport implementation is
`         org.wso2.carbon.server.transports.http.HttpsTransport        `
.

### Transport connector parameters

In addition to the configuration parameters supported by the [HTTP
servlet
transport](https://docs.wso2.com/display/ADMIN44x/HTTP+Servlet+Transport)
, HTTPS servlet transport supports the following configuration
parameters:

!!! tip

In transport parameter tables, literals displayed in italic mode under
the "Possible Values" column should be considered as fixed literal
constant values. Those values can be directly put in transport
configurations.


| Parameter Name | Description                                                        | Required | Possible Values                    | Default Value |
|----------------|--------------------------------------------------------------------|----------|------------------------------------|---------------|
| sslProtocol    | Transport level security protocol to be used.                      | No       | *TLS, SSL*                         | TLS           |
| keystore       | Path to thekeystorewhich should be used for encryption/decryption. | Yes      | A valid file path to akeystorefile |               |
| keypass        | Password to access the specifiedkeystore.                          | Yes      | A valid password                   |               |

Similar to the servlet HTTP transport, this transport is also based on
Apache Tomcat's connector implementation. Please refer Tomcat *connector
configuration reference* f or a complete list of supported parameters.

### Defining multiple tomcat connectors

You have the option of defining multiple tomcat connectors in the
`         catalina-server.xml        ` file. Note that when you define
multiple connectors, all the endpoints of the applications deployed in
your WSO2 server will still be exposed through all the connector ports.
However, you can configure your load balancer to ensure that only the
relevant applications are exposed through the required connector port.

Therefore, you can use multiple connectors to strictly separate the
applications deployed in your server as explained below.

1.  See the example given below where two connectors are defined in the
    `           catalina-server.xml          ` file.

    ``` java
        <!-- Connector using port 9763 -->
         <Connector protocol="org.apache.coyote.http11.Http11NioProtocol"
                           port="9763"
                           ......
                           ....../>
        <!-- Connector using port 9764 -->
         <Connector protocol="org.apache.coyote.http11.Http11NioProtocol"
                           port="9764"
                           ......
                           ....../>
    ```

2.  Configure your load balancer so that the relevant applications are
    exposed through the required connector port.
