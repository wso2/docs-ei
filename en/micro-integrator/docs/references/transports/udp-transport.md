# UDP Transport

The UDP transport allows the WSO2 Enterprise Integrator to handle
messages in user datagram protocol (UDP) format. The
`         axis2-transport-udp.jar        ` archive file contains the
following UDP transport implementation classes, which are part of the
Apache WS-Commons Transports project:

-   `          org.apache.axis2.transport.udp.UDPListener         `
-   `          org.apache.axis2.transport.udp.UDPSender         `

To enable the UDP transport , open the
`         <EI_HOME>/conf/axis2/axis2.xml        ` file in a text editor
and add the following transport configurations:

``` java
    <transportReceiver name="udp" class="org.apache.axis2.transport.udp.UDPListener"/>
    <transportSender name="udp" class="org.apache.axis2.transport.udp.UDPSender"/>
```

If you want to use the sample Axis2 client to send UDP messages, add the
UDP transport sender configuration in the
`         <EI_HOME>/samples/axis2Client/client_repo/conf/axis2.xml        `
file as well. For an example of using the UDP transport, see [Sample
267: Switching from UDP to
HTTP/S](https://docs.wso2.com/pages/viewpage.action?pageId=119129623) .
