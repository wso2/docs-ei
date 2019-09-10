# Sample 8: Introduction to Static and Dynamic Registry Resources and Using XSLT Transformations

!!! warning

Note that WSO2 EI is shipped with the following changes to what is
mentioned in this documentation :

-   `           <PRODUCT_HOME>/          `
    `           repository/samples/          ` directory that includes
    all Integration profile samples is changed to
    `           <EI_HOME>/          `
    `           samples/service-bus/          ` .
    `                     `
-   `           <PRODUCT_HOME>/          `
    `           repository/samples/resources/          ` directory that
    includes all artifacts related to the Integration profile samples is
    changed to `           <EI_HOME>/          `
    `           samples/service-bus/resources/          ` .


-   [Introduction](#Sample8:IntroductiontoStaticandDynamicRegistryResourcesandUsingXSLTTransformations-Introduction)
-   [Prerequisites](#Sample8:IntroductiontoStaticandDynamicRegistryResourcesandUsingXSLTTransformations-Prerequisites)
-   [Building the
    sample](#Sample8:IntroductiontoStaticandDynamicRegistryResourcesandUsingXSLTTransformations-Buildingthesample)
-   [Executing the
    sample](#Sample8:IntroductiontoStaticandDynamicRegistryResourcesandUsingXSLTTransformations-Executingthesample)
-   [Analyzing the
    output](#Sample8:IntroductiontoStaticandDynamicRegistryResourcesandUsingXSLTTransformations-Analyzingtheoutput)

### Introduction

This sample demonstrates the functionality of static and dynamic
registry resources and the XSLT Mediator . Here a message is sent from
the sample client to the back-end service through the ESB using the XSLT
Mediator to perform the transformations. The XSLT transformations are
specified as registry resources.

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

``` html/xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <!-- the SimpleURLRegistry allows access to a URL based registry (e.g. file:/// or http://) -->
        <registry provider="org.wso2.carbon.mediation.registry.ESBRegistry">
            <!-- the root property of the simple URL registry helps resolve a resource URL as root + key -->
            <parameter name="root">file:repository/samples/resources/</parameter>
            <!-- all resources loaded from the URL registry would be cached for this number of milli seconds -->
            <parameter name="cachableDuration">15000</parameter>
        </registry>
        <!-- define the request processing XSLT resource as a static URL source -->
        <localEntry key="xslt-key-req" src="file:repository/samples/resources/transform/transform.xslt"/>
        <sequence name="main">
            <in>
                <!-- transform the custom quote request into a standard quote requst expected by the service -->
                <xslt key="xslt-key-req"/>
                <send/>
            </in>
            <out>
                <!-- transform the standard response back into the custom format the client expects -->
                <!-- the key is looked up in the remote registry and loaded as a 'dynamic' registry resource -->
                <xslt key="transform/transform_back.xslt"/>
                <send/>
            </out>
        </sequence>
    </definitions>
```

This configuration file `         synapse_sample_8.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start the Axis2 server. For instructions on starting the Axis2
    server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

2.  Deploy the back-end service **SimpleStockQuoteService** . For
    instructions on deploying sample back-end services, see [Deploying
    sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

    Now you have a running ESB instance and a back-end service deployed.
    In the next section, we will send a message to the back-end service
    through the ESB using a sample client.

### Executing the sample

The sample client used here is the **Stock Quote Client** , which can
operate in several modes. For further details on this sample client and
its operation modes, see [Stock Quote
Client](https://docs.wso2.com/display/EI650/Using+the+Sample+Clients#UsingtheSampleClients-StockQuoteClient)
.

According to the configuration file
`         synapse_sample_8.xml,        ` the first resource
*xslt-key-req* is specified as a local registry entry. Local entries do
not place the resource on the registry, but simply make it available to
the local configuration. If a local entry is defined with a key that
already exists in the remote registry, the local entry will get a higher
preference and will override the remote resource.

In this example you will notice the new *registry* definition.

ESB comes with a simple URL-based registry
implementation SimpleURLRegistry. During initialization of the registry,
the SimpleURLRegistry expects to find a property named root, which
specifies a prefix for the registry keys used later. When
the SimpleURLRegistry is used, this root is prefixed to the entry keys
to form the complete URL for the resource being looked up. The registry
caches a resource once requested, and caches it internally for a
specified duration. Once this period expires, if necessary, it will
reload the meta information about the resource and reload its cached
copy, the next time the resource is requested.

Therefore, the second XSLT resource
key transform/transform\_back.xslt concatenated with the root of
the SimpleURLRegistry
[file:repository/samples/resources/](http://filerepository) forms the
complete URL of the resource as
[file:repository/samples/resources/transform/transform\_back.xslt](http://filerepository)
and caches its value for a period of 15000 ms.

**To execute the sample client**

1.  Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          ` directory.

    ``` bash
            ant stockquote -Daddurl=http://localhost:9000/services/SimpleStockQuoteService -Dtrpurl=http://localhost:8280/ -Dmode=customquote
    ```

2.  Run the client again immediately using the above command (within 15
    seconds of the first request).
3.  Leave the system idle for more than 15 seconds and run the client
    again using the command specified in step 1.
4.  Now edit the
    `          <ESB_HOME>/repository/samples/resources/transform/transform_back.xslt         `
    file and add a blank line at the end
5.  Run the client again using the command specified in step 1.

### Analyzing the output

When you analyze the debug log output on the ESB console, you will see
that the incoming message is transformed into a standard stock quote
request as expected by the SimpleStockQuoteService deployed on the local
Axis2 instance by the XSLT Mediator. The XSLT Mediator uses Xalan-J to
perform the transformations. It is possible to configure the underlying
transformation engine using properties where necessary.

You will also see that the response from the SimpleStockQuoteService is
converted back into the custom format as expected by the client during
the out message processing.

During the response processing, the SimpleURLRegistry fetches the
resource.

When the client is run for the second time, you will not see the
resource being reloaded by the registry as the cached value would be
still valid.

When the client is run after leaving the system idle for more than 15
seconds, you will see that the registry detects the expiry of the cached
resource and that it checks the meta information about the resource to
determine if the resource itself has changed and requires a fresh fetch
from the source URL. If the metadata/version number indicates that a
reload of the cached resource is not necessary (unless the resource
itself actually changed), the updated meta information is used and the
cache lease is extended as appropriate.

``` java
    [HttpClientWorker-1] DEBUG AbstractRegistry - Cached object has expired for key : transform/transform_back.xslt
    [HttpClientWorker-1] DEBUG SimpleURLRegistry - Perform RegistryEntry lookup for key : transform/transform_back.xslt
    [HttpClientWorker-1] DEBUG AbstractRegistry - Expired version number is same as current version in registry
    [HttpClientWorker-1] DEBUG AbstractRegistry - Renew cache lease for another 15s
```

Once the transform\_back.xslt file is edited and the client is run
again, If the cache is expired, you will see the following debug log
messages which shows that the resource is re-fetched from its URL by the
registry.

``` java
    [HttpClientWorker-1] DEBUG AbstractRegistry - Cached object has expired for key : transform/transform_back.xslt
    [HttpClientWorker-1] DEBUG SimpleURLRegistry - Perform RegistryEntry lookup for key : transform/transform_back.xslt
```

The `         SimpleURLRegistry        ` allows the resource to be
cached and detects updates so that the changes can be reloaded without
restarting the ESB instance.
