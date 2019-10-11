# Creating a Message Store

Follow the instructions given below to create a new Message Store artifact in WSO2 Integration Studio.

## Instructions

1. If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right-click the project and go to **New → Message Store** to open the **New Message Store Artifact** dialog.
2. Select the **Create a new message-store artifact** option and click **Next**.
3. Type a unique name for the message store, and then select the type of message store you are creating.
4. Specify values for the required properties for the selected message store.

	!!! Info
        See the list of configurable properties for a [JMS](../../references/synapse-properties/message-stores/jms-msg-store-properties.md), [JDBC](../../references/synapse-properties/message-stores/jdbc-msg-store-properties.md), [RabbitMQ](../../references/synapse-properties/message-stores/rabbitmq-msg-store-properties.md), [Resequence](../../references/synapse-properties/message-stores/resequence-msg-store-properties.md), [WSO2 MB](../../references/synapse-properties/message-stores/wso2mb-msg-store-properties.md), [In-Memory](../../references/synapse-properties/message-stores/in-memory-msg-store-properties.md), or [Custom](../../references/synapse-properties/message-stores/custom-msg-store-properties.md) message store.

5. Do one of the following:  
	-   To save the endpoint in an existing ESB Config project in your workspace, click **Browse** and select that project.
	-   To save the endpoint in a new ESB Config project, click **Create new Project** and create the new project.
6. Click **Finish**. The message store is created in the `src/main/synapse-config/message-stores` folder under the ESB Config project you specified.
7. Open the new artifact from the project explorer, and update any optional message store properties.

	!!! Info
        See the list of configurable properties for a [JMS](../../references/synapse-properties/message-stores/jms-msg-store-properties.md), [JDBC](../../references/synapse-properties/message-stores/jdbc-msg-store-properties.md), [RabbitMQ](../../references/synapse-properties/message-stores/rabbitmq-msg-store-properties.md), [Resequence](../../references/synapse-properties/message-stores/resequence-msg-store-properties.md), [WSO2 MB](../../references/synapse-properties/message-stores/wso2mb-msg-store-properties.md), [In-Memory](../../references/synapse-properties/message-stores/in-memory-msg-store-properties.md), or [Custom](../../references/synapse-properties/message-stores/custom-msg-store-properties.md) message store.

## Examples

-   [Introduction to Message Stores and Processors](../../../use-cases/examples/message_store_processor_examples/intro-message-stores-processors)
-   [JDBC Message Store](../../../use-cases/examples/message_store_processor_examples/using-jdbc-message-store)
-   [JMS Message Store](../../../use-cases/examples/message_store_processor_examples/using-jms-message-stores)
-   [RabbitMQ Message Store](../../../use-cases/examples/message_store_processor_examples/using-rabbitmq-message-stores)

## Tutorials

-	See the tutorial on [using message stores and processors](../../../../use-cases/tutorials/storing-and-forwarding-messages)
