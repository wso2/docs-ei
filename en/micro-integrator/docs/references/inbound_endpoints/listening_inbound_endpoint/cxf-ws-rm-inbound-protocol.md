# CXF WS-RM Inbound Protocol

WS­ReliableMessaging allows SOAP messages to be reliably delivered
between distributed applications, regardless of software or hardware
failures. The CXF WS­-RM inbound endpoint allows a client (RM Source) to
communicate with the ESB profile of WSO2 EI (RM Destination) with a
guarantee that a message sent will be delivered.

!!! info

Note

To configure the CXF WS-RM Inbound endpoint, you need to install the
**CXF WS Reliable Messaging** feature. For information on how to install
this feature, see [Installing
Features](https://docs.wso2.com/display/EI500/Installing+Features) .
After you install this feature, you can configure the CXF WS-RM inbound
endpoint as a custom inbound endpoint.


Following is a sample sequence that sends messages that are from a RM
source to a non RM backend:

``` xml
    <sequence xmlns="http://ws.apache.org/ns/synapse" name="RMIn" onError="fault">
       <in>
          <property name="PRESERVE_WS_ADDRESSING" value="true"/>
          <header xmlns:wsrm="http://schemas.xmlsoap.org/ws/2005/02/rm" name="wsrm:Sequence" action="remove"/>
          <header xmlns:wsa="http://www.w3.org/2005/08/addressing" name="wsa:To" action="remove"/>
          <header xmlns:wsa="http://www.w3.org/2005/08/addressing" name="wsa:FaultTo" action="remove"/>
          <send>
             <endpoint>
                <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
             </endpoint>
          </send>
       </in>
       <out>
          <log level="custom">
             <property name="RM OutSequence" value="Sending Response"/>
          </log>
          <send/>
       </out>
    </sequence>
```

Since the WS-ReliableMessaging protocol uses specific SOAP headers,
those SOAP headers should be removed from the sequence since in most
cases the backend service would fail to understand them.

The CXF bus should be configured as required in order to control the
manner in which WS-RM communication takes place. This can be done
through a CXF spring configuration. Create a directory named
`         cxf        ` within `         <EI_HOME>/conf        ` and save
the CXF spring configuration file in the `         <EI_HOME>        `
`         /conf/        ` `         cxf        ` directory .

``` xml
    <beans
        xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:wsa="http://cxf.apache.org/ws/addressing"
        xmlns:http="http://cxf.apache.org/transports/http/configuration"
        xmlns:wsrm-policy="http://schemas.xmlsoap.org/ws/2005/02/rm/policy"
        xmlns:wsrm-mgr="http://cxf.apache.org/ws/rm/manager" 
        xsi:schemaLocation="http://cxf.apache.org/core 
        http://cxf.apache.org/schemas/core.xsd        
        http://cxf.apache.org/transports/http/configuration 
        http://cxf.apache.org/schemas/configuration/http-conf.xsd        
        http://schemas.xmlsoap.org/ws/2005/02/rm/policy 
        http://schemas.xmlsoap.org/ws/2005/02/rm/wsrm-policy.xsd        
        http://cxf.apache.org/ws/rm/manager 
        http://cxf.apache.org/schemas/configuration/wsrm-manager.xsd        
        http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans.xsd"
        xmlns:cxf="http://cxf.apache.org/core">
        <cxf:bus>
            <cxf:features>
                <wsa:addressing/>
                <wsrm-mgr:reliableMessaging>
                    <wsrm-policy:RMAssertion>
                        <wsrm-policy:BaseRetransmissionInterval Milliseconds="4000"/>
                        <wsrm-policy:AcknowledgementInterval Milliseconds="2000"/>
                    </wsrm-policy:RMAssertion>
                    <wsrm-mgr:destinationPolicy>
                        <wsrm-mgr:acksPolicy intraMessageThreshold="0"/>
                    </wsrm-mgr:destinationPolicy>
                </wsrm-mgr:reliableMessaging>
            </cxf:features>
        </cxf:bus>
    </beans>
```

If you need to secure the endpoint, you should change the CXF spring
configuration as follows:

``` xml
    <beans
        xmlns="http://www.springframework.org/schema/beans"
        xmlns:sec="http://cxf.apache.org/configuration/security"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:cxf="http://cxf.apache.org/core"
        xmlns:wsa="http://cxf.apache.org/ws/addressing"
        xmlns:httpj="http://cxf.apache.org/transports/http-jetty/configuration"
        xmlns:http="http://cxf.apache.org/transports/http/configuration"
        xmlns:wsrm-policy="http://schemas.xmlsoap.org/ws/2005/02/rm/policy"
        xmlns:wsrm-mgr="http://cxf.apache.org/ws/rm/manager" 
        xsi:schemaLocation="http://cxf.apache.org/core http://cxf.apache.org/schemas/core.xsd        
        http://cxf.apache.org/transports/http/configuration 
        http://cxf.apache.org/schemas/configuration/http-conf.xsd        
        http://schemas.xmlsoap.org/ws/2005/02/rm/policy 
        http://schemas.xmlsoap.org/ws/2005/02/rm/wsrm-policy.xsd 
        http://cxf.apache.org/configuration/security 
        http://cxf.apache.org/schemas/configuration/security.xsd        
        http://cxf.apache.org/ws/rm/manager http://cxf.apache.org/schemas/configuration/wsrm-manager.xsd        
        http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans.xsd 
        http://cxf.apache.org/transports/http-jetty/configuration             
        http://cxf.apache.org/schemas/configuration/http-jetty.xsd">
        <cxf:bus>
            <cxf:features>
                <wsa:addressing/>
                <wsrm-mgr:reliableMessaging>
                    <wsrm-policy:RMAssertion>
                        <wsrm-policy:BaseRetransmissionInterval Milliseconds="4000"/>
                        <wsrm-policy:AcknowledgementInterval Milliseconds="2000"/>
                    </wsrm-policy:RMAssertion>
                    <wsrm-mgr:destinationPolicy>
                        <wsrm-mgr:acksPolicy intraMessageThreshold="0"/>
                    </wsrm-mgr:destinationPolicy>
                </wsrm-mgr:reliableMessaging>
            </cxf:features>
        </cxf:bus>
        <http:destination name="{http://apache.org/hello_world_soap_http}GreeterPort.http-destination"></http:destination>
        <httpj:engine-factory>
            <httpj:engine port="8081">
                <httpj:tlsServerParameters>
                    <sec:keyManagers keyPassword="wso2carbon">
                        <sec:keyStore file="/path/to/file/wso2carbon.jks" password="wso2carbon" type="JKS"/>
                    </sec:keyManagers>
                    <sec:trustManagers>
                        <sec:keyStore file="/path/to/file/client-truststore.jks" password="wso2carbon" type="JKS"/>
                    </sec:trustManagers>
                    <sec:cipherSuitesFilter>
                        <!-- these filters ensure that a ciphersuite with
                        export-suitable or null encryption is used,
                        but exclude anonymous Diffie-Hellman key change as
                        this is vulnerable to man-in-the-middle attacks -->
                        <sec:include>.*_EXPORT_.*</sec:include>
                        <sec:include>.*_EXPORT1024_.*</sec:include>
                        <sec:include>.*_WITH_DES_.*</sec:include>
                        <sec:include>.*_WITH_AES_.*</sec:include>
                        <sec:include>.*_WITH_NULL_.*</sec:include>
                        <sec:exclude>.*_DH_anon_.*</sec:exclude>
                    </sec:cipherSuitesFilter>
                    <sec:clientAuthentication want="true" required="true"/>
                </httpj:tlsServerParameters>
            </httpj:engine>
        </httpj:engine-factory>
    </beans>
```

Sample CXF WS-RM Inbound

``` xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                     name="RM_INBOUND"
                     sequence="RMIn"
                     onError="fault"
                     class="org.wso2.carbon.inbound.endpoint.ext.wsrm.InboundRMHttpListener"
                     suspend="false">
       <parameters>
          <parameter name="inbound.cxf.rm.port">20940</parameter>
          <parameter name="inbound.cxf.rm.config-file">conf/cxf/server.xml</parameter>
          <parameter name="coordination">true</parameter>
          <parameter name="inbound.cxf.rm.host">127.0.0.1</parameter>
          <parameter name="inbound.behavior">listening</parameter>
          <parameter name="sequential">true</parameter>
       </parameters>
    </inboundEndpoint>
```

  

The CXF WS-RM Inbound endpoint can be configured by specifying the
following parameters:

-   `           Sequence          ` - The sequence that the message will
    be injected to.

-   `           Error sequence          ` - The sequence to be called if
    a fault occurs.

-   `          Suspend         ` - If the inbound listener should pause
    when accepting incoming requests, set this to
    `          true         ` . If the inbound listener should not pause
    when accepting incoming requests, set this to
    `          false         ` .
-   `           inbound.cxf.rm.host          ` - The host name.

-   `           inbound.cxf.rm.port          ` - The port to listen to.

        !!! info
    
        Note
    
        When configuring a SSL enabled cxf\_ws\_rm inbound endpoint, the
        `           inbound.cxf.rm.port          ` parameter should be set
        to the same value as the engine port number specified in the CXF
        spring configuration file saved in the
        `           <EI_HOME>/conf/cxf          ` directory.
    
        For example, in the above CXF spring configuration, the engine port
        is 8081. This same port number should be specified as the
        `           inbound.cxf.rm.port          ` in the cxf\_ws\_rm
        inbound endpoint configuration.
    

-   `           inbound.cxf.rm.config-file          ` - The path to the
    CXF Spring configuration file.

-   `           enableSSL          ` - Set to
    `           true          ` if SSL is enabled in the CXF Spring
    configuration file.
