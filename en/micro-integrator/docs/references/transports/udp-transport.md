# UDP Transport

The UDP transport allows the WSO2 Enterprise Integrator to handle
messages in user datagram protocol (UDP) format. The
`         axis2-transport-udp.jar        ` archive file contains the
following UDP transport implementation classes, which are part of the
Apache WS-Commons Transports project:

-   `          org.apache.axis2.transport.udp.UDPListener         `
-   `          org.apache.axis2.transport.udp.UDPSender         `

## Configuring the UDP transport

Update the following configurations in the ei.toml file:

```toml
[transport.udp]
listener.enable = false
sender.enable =false
```

If you want to use the sample Axis2 client to send UDP messages, add the
UDP transport sender configuration in the
`         <EI_HOME>/samples/axis2Client/client_repo/conf/axis2.xml        `
file as well. 
