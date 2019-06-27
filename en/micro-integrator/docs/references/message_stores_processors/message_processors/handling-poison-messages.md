# Handling Poison Messages

A poison message is a message sent by the client to the server that has
failed a specified number of times. When a poison message is received,
the message processor shuts down. You can manage the poison message by
popping it out of the queue or redirecting it to another message store.

!!! tip

Before you begin:

If you want to redirect a poison message to a specific message store,
that store needs to be already defined. For instructions to define a
message, see [Creating a Message Store](_Creating_a_Message_Store_) .


To handle a poison message, follow the steps below:

1.  In the **Main** tab, click **Message Processors** to open the
    **Manage Message Processors** page. When a message processor is
    deactivated after receiving a poison message, it is assigned the
    **Inactive** status, and a **View Message** link is displayed for it
    as shown below.  
      
    ![](attachments/119138306/119138310.png){width="1087" height="250"}
2.  To view the message payload, click **View Message.  
      
    ![](attachments/119138306/119138311.png){width="900" height="367"}**
3.  If you want to pop the message out of the queue, click **Pop
    Message** . A message appears for you to confirm whether you want to
    proceed. Click **Yes** . Once you confirm, you are redirected to the
    **Manage Message Processors** page. If you want to reactivate the
    message processor, click **Activate** .
4.  If you want to redirect the messages, click **Redirect Message** . A
    dialog box named **Select a store to redirect the message** opens.
    In the **Redirect to** field, select the message processor you want
    to redirect the message to.

To handle a poison message, follow the steps below:

1.  In the **Main** tab, click **Message Processors** to open the
    **Manage Message Processors** page. When a message processor is
    deactivated after receiving a poison message, it is assigned the
    **Inactive** status, and a **View Message** link is displayed for it
    as shown below.  
      
    ![](attachments/119138306/119138310.png){width="1087" height="250"}
2.  To view the message payload, click **View Message.  
      
    ![](attachments/119138306/119138311.png){width="900" height="367"}**
3.  If you want to pop the message out of the queue, click **Pop
    Message** . A message appears for you to confirm whether you want to
    proceed. Click **Yes** . Once you confirm, you are redirected to the
    **Manage Message Processors** page.
4.  If you want to redirect the messages, click **Redirect Message** . A
    dialog box named **Select a store to redirect the message** opens.
    In the **Redirect to** field, select the message processor you want
    to redirect the message to. Then click **Redirect** .

        !!! info
    
        Only the pre-defined message stores appear in the list.
    

5.  If there are more then one poison messages, repeat steps 2-4 to pop
    them out of the queue and redirect tham as required. Once all the
    poison messages are handled, the following message is displayed
    instead of a message payload when you click **View Message** for the
    message processor (i.e., step 2).  
      
    ![](attachments/119138306/119138319.png){width="900" height="362"}
