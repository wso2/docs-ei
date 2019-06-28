# Custom Message Store

**Custom Message Store** allows users to create a message store with
their own message store implementation. It can be configured using
configuration by giving the fully qualified class name of the message
store implementation as the class value.

Messages will be stored as specified in the underlying message store
implementation. Parameter configuration can be used to pass any
configuration parameters that is needed by the message store
implementation class.

### UI Configuration

1\. In the "Add Message Stores" tab, click "Custom Message Store" ( For
instructions, see [Creating a Message Store](_Creating_a_Message_Store_)
). The "Add Custom Message Store" page appears with its default view.

![](attachments/30540655/30705361.png){width="800"}

Custom Message Store parameters:

-   **Name** - Unique name of the message store
-   **Provider Class** - Fully qualified name of the message store
    implementation class

2\. Parameters can be added to the message store by giving a "Name" and
"Value" to "Message Store Parameters" and clicking Add Parameter button.

![](attachments/30540655/30705359.png){width="800"}

3\. Added parameters will be listed as follows.

![](attachments/30540655/30705360.png){width="800" height="278"}
