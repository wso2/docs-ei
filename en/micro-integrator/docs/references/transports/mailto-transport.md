# MailTo Transport

When you use the ESB profile of WSO2 Enterprise Integrator (WSO2 EI) to
mediate messages, the mediation sequence can be configured to send
emails (over SMTP) or receive emails (Over POP3 or IMAP) by using the
MailTo transport protocol. The MailTo transport sender and receiver
should be enabled in the `         axis2.xml        ` file (stored in
the `         <EI_HOME>/conf/axis2/        ` directory) by uncommenting
the sender/receiver configurations. In addition to enabling the
transport, you can add more properties to this configuration to control
the behavior of the transport.

!!! Info
    This transport implementation is available as a module of the WS-Commons Transports project. The JAR consisting of the MailTo transport implementation is named `         axis2-transport-mail.jar        ` and the following sender and receiver classes should be included in the WSO2 EI configuration to enable the MailTo transport:

-   `          org.apache.axis2.transport.mail.MailTransportSender         `
-   `          org.apache.axis2.transport.mail.MailTransportListener         `