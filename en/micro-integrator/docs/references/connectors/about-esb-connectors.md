# Working with Connectors

A connector is a collection of
[templates](https://docs.wso2.com/display/EI650/Working+with+Templates)
that define operations users can call from their configurations to
easily access specific logic for processing messages. Typically,
connectors are used to wrap the API of an external service such as
Twitter or Google Spreadsheet. Each connector provides operations that
perform different actions in that service. For example, the Twitter
connector has operations for creating a tweet, getting a user's
followers, and more.

WSO2 EI supports the complete list of connectors provided with WSO2 ESB.
These connectors allow your message flows to connect to and interact
with services such as Twitter and Salesforce. To download a required
connector, go to the [WSO2 Connector
Store](https://store.wso2.com/store) . To browse through the complete
list of connectors and for information on configuring operations for
each connector, see [WSO2 Connectors
Documentation](https://docs.wso2.com/display/ESBCONNECTORS/WSO2+ESB+Connectors+Documentation)
.

You can also [write your own
connector](https://docs.wso2.com/display/ESBCONNECTORS/Writing+a+Connector)
to provide access to other services. Before you use a connector or a
custom connector with WSO2 EI, you need to add and enable the connector
in your WSO2 EI instance.

The following topics provide more information on working with
connectors:

-   [Working with Connectors via
    Tooling](_Working_with_Connectors_via_Tooling_)
-   [Using a Connector](_Using_a_Connector_)

!!! info

In addition to the above methods, you can enable a connector by creating
a configuration file in the
`         <EI_HOME>/repository/deployment/server/synapse-configs/default/imports        `
directory with the following configurations.

!!! tip

Replace the value of the `         name        ` property with the name
of your connector, and name the configuration file
`         {org.wso2.carbon.connector}<CONNECTOR_NAME>.xml        `
(e.g., `         {org.wso2.carbon.connector}salesforce.xml        ` ).


``` xml
    <import xmlns="http://ws.apache.org/ns/synapse"
            name="salesforce"
            package="org.wso2.carbon.connector"
            status="enabled"/>
```

