# Message Transformations

Message Transformation is a common use case in WSO2 Enterprise
Integrator (WSO2 EI). When it comes to message transformations, you can
use the [FastXSLT
mediator](https://docs.wso2.com/display/EI650/FastXSLT+Mediator) , [XSLT
mediator](https://docs.wso2.com/display/EI650/XSLT+Mediator) , [Script
mediator](https://docs.wso2.com/display/EI650/Script+Mediator) as well
as the [PayloadFactory
mediator](https://docs.wso2.com/display/EI650/PayloadFactory+Mediator) .
This section describes different message transformation use cases and
parameters you can configure for optimal performance.

## Comparison of the fast XSLT mediator and the XSLT mediator

The FastXSLT Mediator and XSLT mediator can be used to do XSLT based
message transformations in WSO2 EI. Both mediators can be used to
perform complex message transformations. The FastXSLT mediator has
significant performance over the XSLT mediator as it uses the streaming
XPath parser and applies the XSLT transformation to the message stream
instead of applying it to the XML message payload, but with the FastXSLT
mediator you cannot specify the source, properties, features, or
resources as you can with the XSLT mediator.

> Always use the FastXSLT mediator instead of the default XSLT mediator
for better performance in implementations where the original message
remains unmodified.

In implementations where the message payload needs to be pre-processed,
use the XSLT mediator instead of the FastXSLT mediator. Any
pre-processing performed on the message payload is not be visible to the
FastXSLT mediator, because the transformation logic is applied to the
original message stream instead of the message payload.


## Optimizing the XSLT Mediator

In real world scenarios, you might come across use cases where you do
pre-processing before doing the XSLT message transformation using the
XSLT mediator. In such use cases, you have to use the XSLT mediator
instead of the FastXSLT mediator. You can specify the source,
properties, features, or resources with the XSLT mediator, but the
downside with the XSLT mediator is that it does not perform well when
the message size is larger.

It is possible to tune the XSLT mediator configuration to perform better
for large message sizes.  
In the XSLT mediator, the following two parameters control the memory
usage of the server. 

Open the esb.toml file and update the following:

``` java
[mediation]
synapse.temp_data.chunk.size=3072
synapse.temp_data.chunk.threshold=8
```

These parameters decide when to write to the file system when the
message size is large. The default values for the parameters allow WSO2
EI to process messages of size 3072\*8 = 24K with the XSLT mediator
without writing to the file system. This means that there is no
performance drop in the XSLT mediator when the message size is less than
24K, but when the message size is larger than that, you see a clear
performance degradation.

You can improve the performance of the XSLT mediator by allowing the use
of more memory when processing large messages with the XSLT mediator.  
For example if you know that your message size is going to be less than
512k (i.e., 16384 \* 32), you can set the values of the above parameters
as follows:

``` java
[mediation]
synapse.temp_data.chunk.size=16384
synapse.temp_data.chunk.threshold=32
```

Then the XSLT transformation happens in memory without writing data to
disk. Therefore, performance is increased.

### Optimizing the fastXSLT Mediator

If you want to perform transformation on the original message, you can
use the FastXSLT mediator. Any pre-processing that applies to the
message payload is not be visible to the FastXSLT mediator as the
transformation logic is applied to the message stream instead of the
message payload.

If you cannot perform your transformation with the FastXSLT mediator,
then it is recommended to write a custom class mediator to do your
transformation, since it will be much faster than the XSLT mediator.

Open the esb.toml file and update the following:

``` java
[mediation]
// Enable streaming XPath for fastXSLT mediator.
synapse.streaming.xpath.enabled=true

synapse.temp_data.chunk.size=3072 
synapse.temp_data.chunk.threshold=32
```

> To enable the FastXSLT mediator, your XSLT script must include the
following parameter in the XSL output.

``` xml
    omit-xml-declaration="yes"
```
    
    For example,
    
``` xml
    <xsl:output method="xml" omit-xml-declaration="yes" encoding="UTF-8" indent="yes"/>
```
    
