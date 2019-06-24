# Advanced Callback Properties

The abstract EntitlementCallbackHandler class supports the following
properties for getting the XACML subject (user name), specifying the
action, and setting the service name. The various implementations of
this class (UTEntitlementCallbackHandler,
X509EntitlementCallbackHandler, etc.) can use some or all of these
properties. You implement these properties by adding [Property
mediators](_Property_Mediator_) before the Entitlement mediator in the
sequence.

The default UTEntitlementCallbackHandler looks for a property called
`         username        ` in the Axis2 message context, which it uses
as the XACML request `         subject-id        ` value. Likewise, the
other handlers look at various properties for values for the attributes
and construct the XACML request. The following attribute IDs are used by
the default handlers.

-   `                     urn:oasis:names:tc:xacml:1.0:subject:subject-id                   `
    of category
    `                     urn:oasis:names:tc:xacml:1.0:subject-category:access-subject                   `
-   `                     urn:oasis:names:tc:xacml:1.0:action:action-id                   `
    of category
    `                     urn:oasis:names:tc:xacml:3.0:attribute-category:action                   `
-   `                     urn:oasis:names:tc:xacml:1.0:resource:resource-id                   `
    of category
    `                     urn:oasis:names:tc:xacml:3.0:attribute-category:resource                   `
-   `          IssuerDN         ` of category
    `                     urn:oasis:names:tc:xacml:3.0:attribute-category:environment                   `
    (used only by X509 handler)
-   `          SignatureAlgorithm         ` of category
    `                     urn:oasis:names:tc:xacml:3.0:attribute-category:environment                   `
    (used only by X509 handler)

!!! info

In most scenarios, you do not need to configure any of these properties.


| Property name                 | Acceptable values | Scope | Description                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|-------------------------------|-------------------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| xacml\_subject\_identifier    | string            | axis2 | By default, the Entitlement mediator expects to find the XACML subject (user name) in a property called `              username             ` in the message's Axis2 context. If your authentication mechanism specifies the user name by adding a property of a different name, create a property called `              xacml_subject_identifier             ` and set it to the name of the property in the message context that contains the subject. |
| xacml\_action                 | string            | axis2 | If you are using REST and want to specify a different HTTP verb to use with the service, specify it with the xacml\_action property and specify the xacml\_use\_rest property to true.                                                                                                                                                                                                                                                                   |
| xacml\_use\_rest              | true/false        | axis2 | If you are using REST, and you want to override the HTTP verb to send with the request, you can set this property to true to set to true.                                                                                                                                                                                                                                                                                                                |
| xacml\_resource\_prefix       | string            | axis2 | If you want to change the service name, use this property to specify the new service name or the text you want to prepend to the service name.                                                                                                                                                                                                                                                                                                           |
| xacml\_resource\_prefix\_only | true/false        | axis2 | If set to true, the xacml\_resource\_prefix value is used as the whole service name. If set to false (default), the xacml\_resource\_prefix is prepended to the service name.                                                                                                                                                                                                                                                                            |
