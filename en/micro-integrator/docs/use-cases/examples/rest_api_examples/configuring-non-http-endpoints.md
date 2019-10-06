# Configuring non-HTTP endpoints
## Example use case

## Synapse configuration

When using a non-HTTP endpoint, such as a JMS endpoint, in the API definition, you must remove the REST_URL_POSTFIX property to avoid any characters specified after the context (such as a trailing slash) in the request from being appended to the JMS endpoint. 

Notice that we have specified the REST_URL_POSTFIX property with the value set to "remove". When invoking this API, even if the request contains a trailing slash after the context (e.g., `POST http://127.0.0.1:8287/orderdelayAPI/` instead of `POST  http://127.0.0.1:8287/orderdelayAPI`, the endpoint will be called correctly.

```xml
<api xmlns="http://ws.apache.org/ns/synapse" name="EventDelayOrderAPI" context="/orderdelayAPI"> 
        <resource methods="POST" url-mapping="/"> 
           <inSequence> 
              <property name="REST_URL_POSTFIX" action="remove" scope="axis2"></property> 
              <send> 
                 <endpoint> 
                    <address uri=
    "jms:/DelayOrderTopic?transport.jms.ConnectionFactoryJNDIName=TopicConnectionFactory&
    java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&
    java.naming.provider.url=tcp://localhost:61616&transport.jms.DestinationType=topic">
                  </address> 
                 </endpoint> 
              </send> 
           </inSequence> 
        </resource> 
</api>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. [Create the rest api](../../../../develop/creating-artifacts/creating-an-api) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Set up the back-end service:

........

Invoke the sample Api.