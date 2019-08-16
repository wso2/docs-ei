# Creating a Custom Inbound Endpoint

The ESB profile of WSO2 Enterprise Integrator (WSO2 EI) supports several
inbound endpoints, but there can be scenarios that require functionality
not provided by the existing inbound endpoints. For example, you might
need an inbound endpoint to connect to a certain back-end server or
vendor specific protocol.

To support such scenarios, you can write your own custom inbound
endpoint by extending the behavior for
[listening](_Listening_Inbound_Endpoints_) ,
[polling](_Polling_Inbound_Endpoints_) , and
[event-based](_Event-Based_Inbound_Endpoints_) inbound endpoints.

## Developing a custom Inbound Endpoint

- Custom listening inbound endpoint: You can download the maven artifact used in the sample custom listening inbound endpoint configuration above from [Custom listening inbound endpoint sample](https://github.com/wso2-docs/ESB/tree/master/ESB-Artifacts/inbound/custom_inbound_listening).

- Custom polling inbound endpoint: You can download the maven artifact used in the sample custom polling inbound endpoint configuration above from [Custom polling inbound endpoint sample](https://github.com/wso2-docs/ESB/tree/master/ESB-Artifacts/inbound/custom_inbound).

- Custom event-based inbound endpoint: You can download the maven artifact used in the sample custom event-based inbound endpoint configuration above from [Custom event-based inbound endpoint sample](https://github.com/wso2-docs/ESB/tree/master/ESB-Artifacts/inbound/custom_inbound_waiting).

## Deploy the custom Inbound Endpint

You need to copy the built jar file to the `         <EI_HOME>/lib        ` directory and restart the the ESB
profile to load the class.

  
