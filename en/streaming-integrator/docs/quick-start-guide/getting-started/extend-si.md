# Extend the Streaming Integrator

The Streaming Integrator is by default shipped with most of the available Siddhi extensions by default. If a Siddhi extension you require is not shipped by default, you can download and install it.

In this scenario, let's assume that the laboratories require the siddhi-execution-extrema extension to carry out more advanced calculations for different types of time windows. To download and install it, follow the procedure below:

1. Open the [Siddhi Extensions page](https://store.wso2.com/store/assets/analyticsextension/list). The available Siddhi extensions are displayed as follows.
    ![Extensions Home Page](../../images/quick-start-guide-101/siddhi-extensions.png)

2. Search for the siddhi-execution-extrema extension.
    ![Searched Extension](../../images/quick-start-guide-101/search-for-extension.png)

3. Click on the **V4.1.1** for this scenario. As a result, the following page opens.
    ![Extrema Extension Page](../../images/quick-start-guide-101/extension-page.png)

4. To download the extension, click **Download Extension**. Then enter your email address in the dialog box that appears, and click **Submit**. As a result, a JAR fil is downloaded to a location in your machine (the location depends on your browser settings).

5. To install the siddhi-execution-extrema extension in your Streaming Integrator, place the JAR file you downloaded in the `<SI_HOME>/lib directory`.

## What's Next?

- To learn more about the key concepts of WSO2 Streaming Integrator, see [Key Concepts](../../concepts/concepts.md).

- For more hands-on experience with WSO2 Streaming Integrator, try the [Tutorials](../../examples/tutorials-overview.md).

- For more guidance as you use WSO2 Streaming Integrator for your Streaming Integration use cases, see [Use Cases](../guides/use-cases.md)

- To learn how to the Streaming Integrator can trigger an integration flow to act on the results it generates, see [Getting SI Running with MI in Five Minutes](../hello-world-with-mi.md).