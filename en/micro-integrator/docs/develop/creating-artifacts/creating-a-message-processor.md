# Creating a Message Processor

Follow the instructions given below to create a new Message Processor artifact in WSO2 Integration Studio.

## Instructions

1.  If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right-click the project and go to **New → Message Processor** to open the **New Message Processor Artifact** dialog.
2.  Select **Create a new message-processor artifact** and click **Next**.
3.  Type a unique name for this message processor, specify the type of processor you're creating. 
4.  Specify values for the required parameter for the selected message processor.

	!!! Info
        See the list of configurable properties for a [Message Sampling Processor](../../references/synapse-properties/message-processors/msg-sampling-processor-properties.md), [Scheduled Message Forwarding Processorr](../../references/synapse-properties/message-processors/msg-sched-forwarding-processor-properties.md), or a [Scheduled Failover Message Forwarding Processor](../../references/synapse-properties/message-processors/msg-sched-failover-forwarding-processor-properties.md).

4.  Do one of the following:  
    -   To save the endpoint in an existing ESB Config project in your workspace, click **Browse** and select that project.
    -   To save the endpoint in a new ESB Config project, click **Create new Project** and create the new project.
5.  Click **Finish**. The message processor is created in the `src/main/synapse-config/message-processors` folder under the ESB Config project you specified.
6.  Open the new artifact from the project explorer, and update any optional message processor properties.

	!!! Info
        See the list of configurable properties for a [Message Sampling Processor](../../references/synapse-properties/message-processors/msg-sampling-processor-properties.md), [Scheduled Message Forwarding Processorr](../../references/synapse-properties/message-processors/msg-sched-forwarding-processor-properties.md), or a [Scheduled Failover Message Forwarding Processor](../../references/synapse-properties/message-processors/msg-sched-failover-forwarding-processor-properties.md).

## Examples

<ul>
	<li>
		<a href="../../../../use-cases/examples/message_store_processor_examples/using-message-forwarding-processor">Using the Message Forwarding Processor</a>
	</li>
	<li>
		<a href="../../../../use-cases/examples/message_store_processor_examples/using-message-sampling-processor">Using the Message Sampling Processor</a>
	</li>
	<li>
		<a href="../../../../use-cases/examples/message_store_processor_examples/securing-message-processor">Securing the Message Forwarding Processor</a>
	</li>
	<li>
		<a href="../../../../use-cases/examples/message_store_processor_examples/loadbalancing-with-message-processor">Load Balancing with Message Forwarding Processor</a>
	</li>
</ul>

## Tutorials

-	See the tutorial on [using a message processor](../../../../use-cases/tutorials/storing-and-forwarding-messages).