# Using Scheduled Tasks

## Example 1

The sections below demonstrate an example of scheduling a task (using the default implementation) to inject an XML message and to print it in the logs of the server.

### Creating the Sequence

1.  In the **Project Explorer**, right click the **ScheduleDefaultTask** project, and click **New** → **Sequence**.  
    ![](attachments/119130430/119130439.png)
2.  Click **Create New Sequence** and click **Next**.
3.  Enter **InjectXMLSequence** as the sequence name and click **Finish**.  
    ![](attachments/119130430/119130438.png)  
4.  Drag and drop a **Log** mediator and a **Drop** mediator from the **Mediators** Palette.  
    ![](attachments/119130430/119130437.png) 
5.  Click the **Log** mediator and enter the following details in the **Properties** section.  
    -  **Log Category**: `INFO`
    -  **Log Level**: `CUSTOM`
    - Add a new property with the following details:
      
      <table>
         <tr>
            <th>Property</th>
            <th>Description</th>
         </tr>
         <tr>
            <td>Property Name</td>
            <td>City</td>
         </tr> 
         <tr>
            <td>Value Type</td>
            <td>EXPRESSION</td>
         </tr> 
         <tr>
            <td>Expression</td>
            <td>//city</td>
         </tr> 
      </table>
    
Shown below is the complete source configuration of the Sequence (i.e., the `InjectXMLSequence.xml` file).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sequence name="InjectXMLSequence" trace="disable" xmlns="http://ws.apache.org/ns/synapse">
   <log level="custom">
       <property expression="//city" name="City"/>
   </log>
   <drop/>
</sequence>
```
### Creating the Scheduled Task

Shown below is the complete source configuration of the Scheduled Task (i.e., the `InjectXMLTask.xml` file).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<task class="org.apache.synapse.startup.tasks.MessageInjector" group="synapse.simple.quartz" name="InjectXMLTask" xmlns="http://ws.apache.org/ns/synapse">
   <trigger interval="5"/>
   <property name="injectTo" value="sequence" xmlns:task="http://www.wso2.org/products/wso2commons/tasks"/>
   <property name="sequenceName" value="InjectXMLSequence" xmlns:task="http://www.wso2.org/products/wso2commons/tasks"/>
   <property name="message" xmlns:task="http://www.wso2.org/products/wso2commons/tasks">
       <request xmlns="">
           <location>
               <city>London</city>
               <country>UK</country>
           </location>
       </request>
   </property>
</task>
``` 
The following parameters values are used:

-   **Task Name:** `InjectXMLTask`
-   **Count:** `-1`
-   **Interval (in seconds):** 5
-   **injectTo:** `InjectXMLTask`
-   **sequenceName:** `InjectXMLSequence`

In the **Form View** of the `InjectXMLTask.xml` file, click **Task Implementation Properties**.  
    
![](attachments/119130430/119130433.png)

Select **XML** as the **Parameter Type** of the **message** parameter, and enter the following as the XML message in the **Value/Expression** field and click **OK**. 
```
<request xmlns="">
   <location>
       <city>London</city>
       <country>UK</country>
   </location>
</request>
 ``` 
![](attachments/119130430/119130451.png)

### Injecting messages to RESTful Endpoints

In order to use the Message Injector to inject a message to a RESTful endpint, we can specify the injector with the required payload and inject the message to sequence or proxy service as defined above. The sample below shows a RESTful message injection through a proxy service.

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

### Viewing the output

You view the XML message you injected (i.e., `<abc>This is a scheduled task of the default implementation.</abc>`) getting printed in the logs of the Micro Integrator every 5 seconds.

![](attachments/119130430/119130443.png)

## Introduction to Tasks with a Simple Trigger

This sample introduces the concept of tasks and demonstrates how a simple trigger works. Here the `MessageInjector` class is used, which injects a specified message to the Micro Integrator environment. You can write your own task class implementing the `org.apache.synapse.startup.Task` interface and implement the `execute` method to run the task.

If the task should send the message directly to the endpoint through the main sequence, the endpoint address should be specified. For example, if the address of the endpoint is `http://localhost:9000/services/SimpleStockQuoteService`, the Synapse configuration of the scheduled task will be as follows:


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

When invoked, you will see that the Axis2 server generates a quote every 5 seconds and that the Micro Integrator receives the stock quote response. This is because the injected message is sent to the sample Axis2 server, which sends back a response to the Micro Integrator. 
