# Content-Based Routing Example

You can look through and run this example application to learn the basics of using Integration Studio to route messages in a flow by using a *Switch* mediator.

<img src="../../../../assets/img/migration-examples/content-based-routing-use-case.png" title="Content-based Routing Use Case" width="600" alt="Content-based Routing Use Case"/>

This example application performs the following actions:

1. Listens for messages.
1. Passes messages to a *Property* mediator that sets the property `language` to the language that is passed in the message by the parameter `language`.
1. Uses a  *Switch* mediator to find out whether each message contains a `language` attribute. The presence and value of this attribute determine how the *Switch* mediator routes the message:

  - If the value is `French`, the router routes the message to a *PayloadFactory* component that is named *Reply in French*. This latter component returns the message `Bonjour!` to the requester.
  - If the value is `Spanish`, the router routes the message to a *PayloadFactory* component that is named *Reply in Spanish*. This latter component returns the message `Hola!` to the requester.
  - If the message contains no `language` attribute, the switch routes the message to the default path, which is a sequence that:

    1. Logs the message "No language specified. Using English as a default." to the console
    1. Sets the value of `language` to `English`.
    1. Returns the message `Hello!`.

This example demonstrates that, when you are planning to route messages in a flow by using a *Switch* mediator, there are four aspects to planning that you need to consider:

* The content that the *Switch* mediator evaluates to determine how it routes messages
* The number of routes
* The default routing option
* The processing that the flow performs for each routing option

### Assumptions

This document describes the details of the example within the context of WSO2 Integration Studio, WSO2 EI’s graphical 
developer tool. This document assumes that you are familiar with WSO2 EI and the 
[Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To 
increase your familiarity with Integration Studio, consider completing one or more 
[WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Set Up and Run the Example

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).
2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.
3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.
4. Browse and select the file path to the downloaded sample of this github project ("content-based-routing" folder of the downloaded github repository).
5. Open the **ContentBasedRoutingAPI.xml** under **content-based-routing/ContentBasedRouting/src/main/synapse-config/api/ContentBasedRoutingAPI.xml** directory. 
6. The **ContentBasedRoutingAPI.xml** is the graphical view of the content based routing sample.<br>
    <img src="../../../../assets/img/migration-examples/content-based-routing.png" title="Content-based Routing" width="800" alt="Content-based Routing"/>
7. Run the sample by right click on the **ContentBasedRoutingCompositeApplication** under the main **content-based-routing** project and selecting **Export Project Artifacts and Run**.

4. Open any web browser, paste the following URL in the address bar, and press Enter:

        http://localhost:8290/hello?language=Spanish

      **Result:** Your browser presents a message that reads "Hola!".
   Check the console log in Studio and look for a log message that is similar to this one:

         [2020-04-01 16:51:17,726]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: /hello?language=Spanish, MessageID: urn:uuid:b7ef3404-d097-49ac-845f-26c491fcd158, Direction: request, message = The reply Hola! means hello in Spanish

5. In your browser’s address bar, replace the current URL with this one, and then press Enter:

        http://localhost:8290/hello?language=French

      **Result:** Your browser presents a message that reads "Bonjour!". Check the console log in Studio again and look for a log message that is similar to this one:

           [2020-04-01 16:51:58,093]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: /hello?language=French, MessageID: urn:uuid:f7a36a9d-a0b7-4ef8-a9e9-c9f994667a03, Direction: request, message = The reply Bonjour! means hello in French

6. Try accessing `http://localhost:8290/hello` without specifying a language:

        http://localhost:8290/hello

    **Result:** Your browser presents a message that reads "Hello!".

Check the console log in Studio again and look for a log message that are similar to these:

```
    [2020-04-01 16:52:39,713]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: /hello, MessageID: urn:uuid:fb9ecdb2-21c6-4c4b-9aec-822853b3d5b2, Direction: request, message = No language specified. Using English as a default.
    [2020-04-01 16:52:39,714]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: /hello, MessageID: urn:uuid:fb9ecdb2-21c6-4c4b-9aec-822853b3d5b2, Direction: request, message = The reply Hello! means hello in English
```

### Download the project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../../assets/attach/connectors/FileConnector.zip">
    <img src="../../../../assets/img/connectors/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go further

* Learn more about configuring an [REST API](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/rest-api-properties/) in Studio.
* Read about the concept of [WSO2 Message Transforming Mediators](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/about-mediators/) in WSO2 Enterprise Integrator.