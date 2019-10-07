# Injecting messages to RESTful Endpoints
In order to use the Message Injector to inject a message to a RESTful endpint, we can specify the injector with the required payload and inject the message to sequence or proxy service as defined above. The sample below shows a RESTful message injection through a proxy service.

## Synapse configurations

Following are the integration artifacts that we can used to implement this scenario. See the instructions on how to [build and run](#build-and-run) this example.

```java tab='Task Configuration'
<task class="org.apache.synapse.startup.tasks.MessageInjector" group="synapse.simple.quartz" name="SampleInjectToProxyTask" xmlns="http://ws.apache.org/ns/synapse">
    <trigger count="2" interval="5"/>
    <property name="injectTo" value="proxy" xmlns:task="http://www.wso2.org/products/wso2commons/tasks"/>
    <property name="message" xmlns:task="http://www.wso2.org/products/wso2commons/tasks">
        <request xmlns="">
            <location>
                <city>London</city>
                <country>UK</country>
            </location>
        </request>
    </property>
    <property name="proxyName" value="SampleProxy" xmlns:task="http://www.wso2.org/products/wso2commons/tasks"/>
</task>
```
        
``` java tab='Proxy Configuration'
<?xml version="1.0" encoding="UTF-8"?>
<proxy name="SampleProxy" startOnLoad="true" transports="http https" xmlns="http://ws.apache.org/ns/synapse">
    <target>
        <inSequence>
            <property expression="//request/location/city" name="uri.var.city" scope="default" type="STRING"/>
            <property expression="//request/location/country" name="uri.var.cc" scope="default" type="STRING"/>
            <log>
                <property expression="get-property('uri.var.city')" name="Which city?"/>
                <property expression="get-property('uri.var.cc')" name="Which country?"/>
            </log>
            <send>
                <endpoint name="EP">
                    <http method="get" uri-template="http://api.openweathermap.org/data/2.5/weather?q={uri.var.city},{uri.var.cc}&amp;APPID=ae2a70399cf2c35940a6538f38fee3d3"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence>
            <log level="full"/>
        </outSequence>
        <faultSequence/>
    </target>
</proxy>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. Create the [proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service) and a [scheduled task](../../../../develop/creating-artifacts/creating-scheduled-task) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Setup a backend server.

You view the XML message you injected (i.e., `<abc>This is a scheduled task of the default implementation.</abc>`) getting printed in the logs of the Micro Integrator every 5 seconds.

![](/assets/img/tutorials/scheduled-tasks/119130430/119130443.png)