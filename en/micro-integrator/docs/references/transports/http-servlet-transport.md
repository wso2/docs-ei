# HTTP Servlet Transport

The transport receiver implementation of the HTTP transport is available
in the Carbon core component. The transport sender implementation comes
from the Tomcat http connector. This transport is shipped with WSO2
Carbon and all WSO2 Carbon-based products, which use this transport as
the default transport, except WSO2 ESB and WSO2 Enterpise Integrator. By
default, we use non-blocking Tomcat Java connector,
`         org.apache.coyote.http11.Http11NioProtocol.        `

!!! Info
    -   This is a non-blocking HTTP transport implementation, which means
    that I/O the threads does not get blocked while received messages
    are processed.
    -   Although the `          axis2.xml         ` file contains
    configurations for HTTP/S transports by default, they are not used
    by WSO2 products. Instead, the products use the HTTP/S transport
    configurations in Tomcat-level; therefore, changing the HTTP/S
    configurations in the `          axis2.xml         ` file has
    no effect.

!!! Tip
    In the transport parameter tables, the literals displayed in italics under the "Possible Values" column should be considered as fixed literal constant values. Those values can be directly put into the transport configurations.

This servlet transport implementation can be further tuned up using the
following parameters.

This is only a subset of all the supported parameters. The servlet HTTP
transport uses the
[org.apache.catalina.connector.Connector](http://tomcat.apache.org/tomcat-7.0-doc/api/org/apache/catalina/connector/Connector.html)
implementation from Apache Tomcat. So the servlet HTTP transport
actually accepts any parameter accepted by the connector implementation.
For a complete list of supported parameters, see [Apache Tomcat's
connector configuration
reference](http://tomcat.apache.org/tomcat-7.0-doc/config/http.html) .

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
