# Custom Message Store

**Custom Message Store** allows users to create a message store with
their own message store implementation. It can be configured using
configuration by giving the fully qualified class name of the message
store implementation as the class value.

Messages will be stored as specified in the underlying message store
implementation. Parameter configuration can be used to pass any
configuration parameters that is needed by the message store
implementation class.

## Parameters

-   **Name** - Unique name of the message store
-   **Provider Class** - Fully qualified name of the message store
    implementation class
