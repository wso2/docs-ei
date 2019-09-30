# Configuring non-HTTP endpoints

When using a non-HTTP endpoint, such as a JMS endpoint, in the API definition, you must remove the REST\_URL\_POSTFIX property to avoid any characters specified after the context (such as a trailing slash) in the request from being appended to the JMS endpoint. For example:

```
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

Notice that we have specified the REST\_URL\_POSTFIX property with the value set to "remove". When invoking this API, even if the request contains a trailing slash after the context (e.g., `POST http://127.0.0.1:8287/orderdelayAPI/` instead of `POST  http://127.0.0.1:8287/orderdelayAPI`, the endpoint will be called correctly.