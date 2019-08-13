# Synapse Handlers

Synapse Handlers can be used to process requests in a scenario where you have multiple requests and each request needs be processed in a specific manner. A Handler defines the interface that is required to handle the request and concreteHandlers are to handle requests in a specific manner based on what needs to be done with regard to each type of request. The diagram below illustrates this.

![](attachments/119131623/119131624.png)

Synapse handler is the interface used to register server response callbacks. Synapse handler provides the abstract handler implementation
that executes the request in flow, request out flow, response in flow and response out flow. The diagram below is an illustration of how the specified flows execute in the abstract handler implementation.

![](attachments/119131623/119131625.png)

-   **Request in flow**
    This executes when the request reaches the synapse engine.

    ``` java
    public boolean handleRequestInFlow(MessageContext synCtx);
    ```

-   **Request out flow**
    This executes when the request goes out of the synapse engine.
    ``` java
    public boolean handleRequestOutFlow(MessageContext synCtx);
    ```

-   **Response in flow**
    This executes when the response reaches the synapse engine.
    ``` java
    public boolean handleResponseInFlow(MessageContext synCtx);
    ```

-   **Response out flow**
    This executes when the response goes out of the synapse engine.

    ``` java
    public boolean handleResponseOutFlow(MessageContext synCtx);
    ```

The diagram below illustrates the basic component structure of WSO2 Micro Integrator and how the flows mentioned above execute in the request path and the response path.

![](attachments/119131623/119131626.png)

Now that you understand what a handler is, let's see how you can write a concrete Synapse handler.

## Writing a concrete Synapse handler

The easiest way to write a concrete Synapse handler is to extend the `org.apache.synapse.AbstractSynapseHandler` class. You can also write a concrete Synapse handler by implementing `org.apache.synapse.SynapseHandler`, which is the SynapseHandler interface.

Following is an example Synapse handler implementation that extends the `org.apache.synapse.AbstractSynapseHandler` class:

```
public class TestHandler extends AbstractSynapseHandler {
    
    private static final Log log = LogFactory.getLog(TestHandler.class);
    
    @Override
    public boolean handleRequestInFlow(MessageContext synCtx) {
            log.info("Request In Flow");
            return true;
    }
    
    @Override
    public boolean handleRequestOutFlow(MessageContext synCtx) {
            log.info("Request Out Flow");
            return true;
    }
    
    @Override
    public boolean handleResponseInFlow(MessageContext synCtx) {
            log.info("Response In Flow");
            return true;
    }
    
    @Override
    public boolean handleResponseOutFlow(MessageContext synCtx) {
            log.info("Response Out Flow");
            return true;
    }
}
```

## Deploying the Synapse handler

To deploy your custom synapse handler in WSO2 Micro Integrator, bundle the artifact as a JAR file (with either the .jar or .xar format), and add it to the `MI_HOME/lib/` directory. Be sure to restart the server after adding the files.

## Engaging the Synapse handler

To engage the deployed Synapse handler, you need to add the following configuration to the `MI_HOME/conf/synapse-handlers.xml` file.

```
<handlers>
  <handler name="TestHandler" class="package.TestHandler"/>
</handlers>
```
