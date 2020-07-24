# Connector Usage Guidelines

This document provides a set of guidelines on how to use connectors throughout their lifecycle.

## Using connectors in your integration project

Connectors can be added and used as part of the integration logic of your integration solution. This helps you configure inbound and outbound connections to third-party applications or to systems that support popular B2B protocols.

### Importing connectors 

All the connectors are hosted at [WSO2 EI Connector Store](https://store.wso2.com/store/assets/esbconnector/list). You can download the connector from the store as a .zip file. 

<img src="../../../assets/img/connectors/connector-store.png" title="Connector store" width="700" alt="Connector store"/>

The source code for connectors can also be found in the specific [WSO2 extensions GitHub repository](https://github.com/wso2-extensions/).

However, the recommended approach to use connectors for integration logic development is through WSO2 Integration Studio. Developers can browse and import connectors to the workplace using the Integration Studio itself. As a result, there is no need to go and download the connector from the store separately or obtain it from the source code.

**To import a connector**:

1. Open [WSO2 Integration Studio](https://wso2.com/integration/integration-studio/).

2. [Create an Integration Project](../../develop/create-integration-project.md).

3. Right-click on the ESB Configs folder and select **New** -> **Add/Remove Connector**. Search for the connector and follow the steps in the wizard to import the connector.

    <img src="../../../assets/img/connectors/import-connector.png" title="Import a connector" width="700" alt="Import a connector"/>

### Providing values for operation parameters 

After importing the connector, you can drag and drop operations to the design palette and use them. When providing values for operation parameters, you can provide static values or dynamic values. Dynamic values can be provided in one of the following ways. 

* As an [XPATH expression](https://www.w3schools.com/xml/xpath_syntax.asp)
* As a [JSON expression](https://docs.oracle.com/cd/E60058_01/PDF/8.0.8.x/8.0.8.0.0/PMF_HTML/JsonPath_Expressions.htm) 
* As a property. 
    * Most of the time this will be a custom property you set earlier in the mediation flow. The property exists throughout the mediation flow. 
    * You can also provide properties of other scopes as well (i.e., a header value).


### Transform message as operation needs 

Some connectors use message content in the $body to execute the operation. In such situations you may need to transform the current message in the way the connector operation needs before using that with the connector operation. Following are some of the mediators you can use to transform the message. 

    * [PayloadFactory mediator](../mediators/payloadFactory-Mediator.md) - This replaces the current message with a message in the format we specify. We can use the information of the current message to construct this new message.

    * [Enrich mediator](../mediators/enrich-Mediator.md) - Enrich the current message modifying or adding new elements. This is also useful to save the current message as a property and to place a message in a property as the current message.

    * [Datamapper mediator](../mediators/data-Mapper-Mediator.md) - Transform JSON, XML, CSV messages between formats.

    * [Script mediator](../mediators/script-Mediator.md) - Use JavaScript, Groovy or Ruby scripting languages to transform message in a custom manner.

    * [Custom class mediator](../mediators/class-Mediator.md) - Use Java to transform message in a custom manner (use Axiom, Jackson, or Gson libraries).

    * Mediator Modules (new) - Import module and use operations to transform message (currently CSV related transformations only).

The above mediators are useful to transform the message anywhere in the mediation flow. Hence, the same mediators can be used to transform the result of a certain connector operation in the way the next connector operation needs. 

### Result of the operation invocation 

Unless specified otherwise, the result of the connector operation (response from the connector application) will be available in the message context after using the connector operation. You can do any further mediation with the result or send it back to the invoker using [Respond mediator](../mediators/respond-Mediator.md). 

### Export and run a project with connectors 

The commended way to run any integration logic with WSO2 EI 6.x series or EI 7.x series is using Carbon applications. CApp is the deployable artifact for WSO2 EI runtime. The recommendation is the same even when the integration logic is using a WSO2 EI connector. 

In order to include a connector into a CApp and export, a ConnectorExporter project needs to be created and the connector needs to be added to that. Then you can add the ConnectorExporter project to the exporting artifact list when exporting CApp. 

The exported CApp needs to be copied to the deployment folder of the EI server (<EI_HOME>/repository/deployment/server/carbonapps). The changes will get hot-deployed if the server is already running.
 
## Configuring connectors 

Configurations required to initialize the connectors must be provided in one of the following ways depending on the connector. 

For new connector versions

Creating a connection with configurations and associate with operations 

For new connector versions this is available with Integration Studio 7.1.0 onwards. When creating a connection you can provide configuration values and they will get saved as a local-entry internally. 
