# Introduction to Tasks with a Simple 
## Example use case

This sample introduces the concept of tasks and demonstrates how a simple trigger works. Here the `MessageInjector` class is used, which injects a specified message to the Micro Integrator environment. You can write your own task class implementing the `org.apache.synapse.startup.Task` interface and implement the `execute` method to run the task.

If the task should send the message directly to the endpoint through the main sequence, the endpoint address should be specified. For example, if the address of the endpoint is `http://localhost:9000/services/SimpleStockQuoteService`, the Synapse configuration of the scheduled task will be as follows:

## Synapse configurations

Following are the integration artifacts that we can used to implement this scenario. See the instructions on how to [build and run](#build-and-run) this example.

```xml tab='Scheduled Task'
<?xml version="1.0" encoding="UTF-8"?>
<task class="org.apache.synapse.startup.tasks.MessageInjector" group="synapse.simple.quartz" name="CheckPrice" xmlns="http://ws.apache.org/ns/synapse">
    <trigger interval="5"/>
    <property name="soapAction" value="urn:getQuote" xmlns:task="http://www.wso2.org/products/wso2commons/tasks"/>
    <property name="to" value="http://localhost:9000/services/SimpleStockQuoteService" xmlns:task="http://www.wso2.org/products/wso2commons/tasks"/>
    <property name="message" xmlns:task="http://www.wso2.org/products/wso2commons/tasks">
        <m0:getQuote xmlns:m0="http://services.samples">
            <m0:request>
                <m0:symbol>IBM</m0:symbol>
            </m0:request>
        </m0:getQuote>
    </property>
</task>
```

```xml tab='Main Sequence'
<?xml version="1.0" encoding="UTF-8"?>
<sequence name="main" xmlns="http://ws.apache.org/ns/synapse">
    <in>
            <send/>
        </in>
        <out>
            <log level="custom">
                <property name="Stock_Quote_on" expression="//ns:return/ax21:lastTradeTimestamp/child::text()"
                          xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples"/>
                <property name="For_the_organization" expression="//ns:return/ax21:name/child::text()"
                          xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples"/>
                <property name="Last_Value" expression="//ns:return/ax21:last/child::text()"
                          xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples"/>
            </log>
            <drop/>
        </out>
</sequence>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. Create the [main sequence](../../../../develop/creating-artifacts/creating-reusable-sequences) and a [scheduled task](../../../../develop/creating-an-inbound-endpoint) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Setup a backend server.

When the Micro Integrator is invoked, you will see that the Axis2 server generates a quote every 5 seconds and that the Micro Integrator receives the stock quote response. This is because the injected message is sent to the sample Axis2 server, which sends back a response to the Micro Integrator. 
