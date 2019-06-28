# Best Practices

Here are the guidelines and recommendations to design, develop, test,
and deploy WSO2 integration solutions:

-   [Design](#BestPractices-Design)
    -   [Implementing enterprise integration patterns
        (EIPs)](#BestPractices-Implementingenterpriseintegrationpatterns(EIPs))
-   [Development](#BestPractices-Development)
    -   [Creating a tooling
        project](#BestPractices-Creatingatoolingproject)
    -   [Creating artifacts](#BestPractices-Creatingartifacts)
    -   [Naming
        artifacts](#BestPractices-NamingGuidelinesNamingartifacts)
    -   [Using mediators](#BestPractices-Usingmediators)
        -   [Using the Log mediator](#BestPractices-UsingtheLogmediator)
        -   [Using the Class/Custom
            mediators](#BestPractices-UsingtheClass/Custommediators)
        -   [Using the Call vs. Callout vs.
            Send mediators](#BestPractices-UsingtheCallvs.Calloutvs.Sendmediators)
        -   [Using the In and
            Out mediators](#BestPractices-UsingtheInandOutmediators)
        -   [Using
            the ForEach mediator](#BestPractices-UsingtheForEachmediator)
        -   [Using the Clone
            mediator](#BestPractices-UsingtheClonemediator)
        -   [Using the Loopback 
            mediator](#BestPractices-UsingtheLoopbackmediator)
        -   [Using the Send
            mediator](#BestPractices-UsingtheSendmediator)
        -   [Mediators for message
            transformation](#BestPractices-Mediatorsformessagetransformation)
        -   [General mediator best
            practices](#BestPractices-Generalmediatorbestpractices)
    -   [Working with proxy
        services](#BestPractices-Workingwithproxyservices)
    -   [Working with APIs](#BestPractices-WorkingwithAPIs)
    -   [Working with endpoints](#BestPractices-Workingwithendpoints)
    -   [Error handling](#BestPractices-HandlingErrorsErrorhandling)
        -   [Behaviour of the
            FaultSequence](#BestPractices-BehaviouroftheFaultSequence)
        -   [Best practices](#BestPractices-Bestpractices)
-   [Troubleshooting](#BestPractices-Troubleshooting)
-   [Testing](#BestPractices-Testing)
-   [Deployment](#BestPractices-Deployment)
    -   [General guidelines](#BestPractices-Generalguidelines)
    -   [Recommended deployment
        patterns](#BestPractices-Recommendeddeploymentpatterns)

### Design

#### Implementing enterprise integration patterns (EIPs)

See [Enterprise Integration Patterns with WSO2 Enterprise
Integrator](https://docs.wso2.com/display/EIP/Enterprise+Integration+Patterns+with+WSO2+Enterprise+Integrator)
on how to simulate the patterns in the [Enterprise Integration Patterns
catalog](http://www.enterpriseintegrationpatterns.com/) using various
constructs of WSO2 Enterprise Integrator.

### Development

#### Creating a tooling project

-   When you start [WSO2 Enterprise Integrator
    Tooling](https://docs.wso2.com/enterprise-integrator/WSO2+Enterprise+Integrator+Tooling)
    , a default workspace is created to store your projects. Change the
    default workspace location to a preferred location that you can
    easily access. For information on how to change the default
    workspace, see [Switching
    workspaces](https://help.eclipse.org/neon/index.jsp?topic=%252Forg.eclipse.platform.doc.user%252Freference%252Fref-workspaceswitch.htm)
    .
-   Change the default location that your new project is created in your
    machine in the same way that you change the default workspace.
-   Create a [Maven Multi Module (MMM)
    project](https://docs.wso2.com/developer-studio/Working+with+Maven+Multi+Module+Projects)
    to group the projects of a specific use case.
-   When you create the project structure, add it to source control such
    as
    [SVN](http://www.eclipse.org/subversive/documentation/teamSupport/actions.php)
    or [Git](http://wiki.eclipse.org/EGit/User_Guide) .
-   Use the source control command line tools to interact with files.
-   When you create projects, ensure that you follow the standard Maven
    naming convention. For more information, see [Naming
    Guidelines](#BestPractices-NamingGuideli) .
-   When you define `          groupid         ` ,
    `          artifactid,         ` and `          version         `
    , ensure that you follow the proper [naming
    convention](https://maven.apache.org/guides/mini/guide-naming-conventions.html)
    . For example, you can specify a name such as
    `          org.acme.integration.sampleesb-demo-project1.0.0         `
    .

#### Creating artifacts

-   When you create an artifact, use the proper naming convention from
    the start.
-   If an ESB project contains many artifacts related to multiple use
    cases, name the artifacts by prefixing or post fixing the use case
    name.
-   Create specific ESB projects for specific use cases. This makes it
    easy to manage the code/project.
-   Design the ESB logic into [highly
    cohesive](https://en.wikipedia.org/wiki/Cohesion_(computer_science))
    and [loosely
    coupled](https://en.wikipedia.org/wiki/Coupling_(computer_programming))
    modules.
-   Avoid creating large, complex mediation code as much as possible.
    Always follow the [separation of
    concerns](https://en.wikipedia.org/wiki/Separation_of_concerns)
    design principle and split lengthy logic into separate mediation
    components.
-   When you have a common set of code, implement it in a sequence or a
    template that can be reused. For more information, see [Sequence
    Template](https://docs.wso2.com/enterprise-integrator/Sequence+Template)
    .
-   Externalize endpoint and policy references using the Registry. For
    more information, see [Managing ESB projects across
    environments](https://docs.wso2.com/display/Governance460/Governing+External+References+Across+Environments#GoverningExternalReferencesAcrossEnvironments-ManagingESBprojectsacrossenvironments)
    .
-   Hide sensitive information such as passwords in configuration files
    using [Secure
    Vault](https://docs.wso2.com/display/ADMIN44x/Encrypting+Passwords+with+Cipher+Tool)
    .

#### Naming artifacts

When you create a project structure, be sure to follow the naming
convention described in the table below:

!!! info

Note

The runtime server of your project is shared with other projects.
Therefore, give unique names for artifacts to avoid name conflicts when
you have multiple projects.


<table style="width:100%;">
<colgroup>
<col style="width: 10%" />
<col style="width: 34%" />
<col style="width: 28%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Type</th>
<th>Description</th>
<th>Format</th>
<th>Sample</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Project name</td>
<td>The root directory of the project's source code.</td>
<td><code>             &lt;ProjectName&gt;            </code></td>
<td><code>             StudentInformationSystem            </code></td>
</tr>
<tr class="even">
<td>Sub modules (common)</td>
<td>ESB/DSS projects.</td>
<td><code>             &lt;Project&gt;&lt;ModuleName&gt;            </code></td>
<td><code>             StudentInformationSystemDataService            </code></td>
</tr>
<tr class="odd">
<td>Folders</td>
<td>Custom folders that you create to categorize resources in registry resource projects.</td>
<td><p><code>              &lt;Project&gt;&lt;FolderName&gt;             </code><br />
Specify the folder structure for registry resources<br />
<code>              &lt;Project&gt;/&lt;ResourceType&gt;/&lt;subResourceType&gt;             </code></p></td>
<td><p><code>              StudentInformationSystemLibraries             </code><br />
<br />
<code>              StudentInformationSystem             </code><br />
<code>              ---Schema             </code><br />
<code>              ---wsdl             </code><br />
<code>              ---xsd             </code></p></td>
</tr>
<tr class="even">
<td>Source files</td>
<td>Source files such as sequences and proxy services.</td>
<td><code>             &lt;FileName&gt;&lt;Type&gt;.&lt;Extension&gt;            </code></td>
<td><code>             GetStudentSequence, SimpleStockQuoteProxyService            </code></td>
</tr>
</tbody>
</table>

Listed below are the best practices for working with the source code:

-   Follow a standard file naming convention to improve the readability
    of the code. Naming conventions change depending on the programming
    language.

-   Given below is the naming convention for WSO2 source files:  

    ``` java
        <ProjectName>_<FileName>_<FileType>.<Extension>
    
        Example: 
        Student_Student_Sequence.xml, Stocks_StockQuote_FileInbound.xml
    ```

    You can use a general naming convention for other known source
    files. If applicable, you can use a class naming convention that
    includes the business domain so that it is self explanatory. For
    example, `           BubbleSortMediator.java          ` . For more
    information on naming conventions, see the library article [WSO2
    Developer Studio - Development and Deployment Best
    Practices](http://wso2.com/library/articles/2015/10/article-wso2-developer-studio-development-and-deployment-best-practices/)
    .

#### Using mediators

##### Using the [Log](https://docs.wso2.com/enterprise-integrator/Log+Mediator) mediator

-   When a server encounters errors, you should have enough information
    about the errors in the logs.

-   To log mediated messages, the best option is to use the Log
    mediator.  

-   In a production setup, it is not recommended to use the Log mediator
    within sequences and proxy services other than in the Fault
    sequences.

-   Using logs with information such as ERROR\_CODE is generally useful
    to understand the issue.

-   Put the Log mediators within the Fault sequences, which capture
    information about errors.  
-   In the development phase, you should use Log mediators in
    the message flow as checkpoints.

-   If you are using the Log mediator to track and troubleshoot the
    message flow, set the log category to `           DEBUG          ` .

-   If you are in a development environment, ensure that you set the
    global log level of
    `           org.apache.synapse.mediators.builtin.LogMediator          `
    to `           DEBUG          ` .

-   If you are in a production environment, ensure that you set the
    global log level of
    `           org.apache.synapse.mediators.builtin.LogMediator          `
    to INFO. [Setting the log level to
    INFO](https://docs.wso2.com/display/ESB490/Setting+Up+Logging)
    allows you to troubleshoot the message flow in production whenever
    required without having to go through the synapse configuration.

-   You should use Log mediators in the
    `           FaultSequence          ` to capture information about
    errors that occur. The logs entries printed via the
    `           FaultSequences          ` will be available in the
    wso2carbon.log that is in the main log file.

##### Using the [Class](https://docs.wso2.com/enterprise-integrator/Class+Mediator) /Custom mediators

-   You should not write a Class mediator if the intended functionality
    can be achieved using the enterprise service bus capabilities of
    WSO2 Enterprise Integrator. Following this practice avoids
    maintenance overhead. If you want to see detailed information on the
    functionality of each built-in mediator of WSO2 Enterprise
    Integrator, see the [Mediator
    catalog](https://docs.wso2.com/enterprise-integrator/Mediators) .

-   If you are writing a Class mediator, ensure that you have a good
    understanding of the performance impact and possible memory leaks so
    that you can take these into consideration.

-   Be sure to specify a [proper package
    name](http://www.oracle.com/technetwork/java/codeconventions-135099.html)
    for Class  mediators
-   Be sure to apply all [java naming conventions and code best
    practices](http://www.oracle.com/technetwork/java/codeconventions-135099.html)
    when you write the code for Class mediators.

##### Using the [Call](https://docs.wso2.com/enterprise-integrator/Call+mediator) vs. [Callout](https://docs.wso2.com/enterprise-integrator/Callout+mediator) vs. [Send](https://docs.wso2.com/enterprise-integrator/Send+mediator) mediators

-   You can use either the Call mediator, Callout mediator or Send
    mediator to send a message to an endpoint.

-   If you use the Send mediator to send a message, the response goes to
    the `          OutSequence         ` (or to the specified receiving
    sequence).
-   If you use the Call mediator to send a message, the response goes to
    the next mediator, which is placed right after the send mediator in
    the mediation flow.
-   The use of Call mediator is recommended for service chaining
    scenarios.
-   Both the Send mediator and Call mediator use the [non blocking
    transport](https://docs.wso2.com/enterprise-integrator/HTTP+PassThrough+Transport)
    . Therefore, there is no difference between the performance of the
    Send and Call mediators.
-   Behaviour of the Callout mediator is similar to the Call mediator,
    but it uses the [blocking
    transport](https://axis.apache.org/axis2/java/core/docs/http-transport.html)
    to send the message out. Therefore , in terms of performance,
    Callout mediator is not as good as the Call mediator or Send
    mediator. If there are scenarios where blocking behaviour is
    required, you can use the Callout mediator. For example, see [JMS
    Transactions](https://docs.wso2.com/enterprise-integrator/JMS+Transactions)
    .

        !!! info
    
        Note
    
        You should not use the Callout mediator unless there is a specific
        requirement for blocking behaviour in the underlying transport
        implementation.
    
        !!! tip
    
        The Callout mediator functionality has been merged into the Call
        mediator in WSO2 Enterprise Integrator. Therefore, you can use the
        Call mediator and set the blocking flag appropriately to switch
        between non-blocking and blocking behaviour.
    

    The following diagram illustrates a proxy/REST API with a Send
    mediator:

![](attachments/119133370/119133372.png){width="700"}  
The following diagram illustrates a proxy/REST API with a Call/Callout
mediator:  
![](attachments/119133370/119133371.png){width="700"}

##### Using the [In and Out](https://docs.wso2.com/enterprise-integrator/In+and+Out+Mediators) mediators

A Proxy service or REST API resource has a `         InSequence        `
to handle the request message flow and has an
`         OutSequence        ` to handle the response message flow.
Therefore, the In mediator and Out mediator should not be used in proxy
services or REST API resources. However, you should use the In mediator
and Out mediator in the main sequence.

##### Using the [ForEach](https://docs.wso2.com/display/EI650/ForEach+Mediator) mediator

[Iterate Mediator](https://docs.wso2.com/display/EI650/Iterate+Mediator)
is quite similar to the ForEach mediator. You can use complex XPath
expressions to conditionally select elements to iterate over in both
mediators. Following are the main difference between ForEach and Iterate
mediators:

-   Use the ForEach mediator only for message transformations. If you
    need to make back-end calls from each iteration, then use the
    iterate mediator.
-   ForEach supports modifying the original payload. You can use Iterate
    for situations where you send the split messages to a target and
    collect them by an Aggregate in a different flow
-   You need to always accompany an Iterate with an Aggregate mediator.
    ForEach loops over the sub-messages and merges them back to the same
    parent element of the message.
-   In Iterate you need to send the split messages to an endpoint to
    continue the message flow. However, ForEach does not allow using
    [Call](https://docs.wso2.com/display/EI650/Call+Mediator) ,
    [Send](https://docs.wso2.com/display/EI650/Send+Mediator) and
    [Callout](https://docs.wso2.com/display/EI650/Callout+Mediator)
    mediators in the sequence.
-   ForEach does not split the message flow, unlike Iterate Mediator. It
    guarantees to execute in the same thread until all iterations are
    complete.

When you use ForEach mediator, you can only loop through segments of the
message and do changes to a particular segment. For example, you can
change the payload using payload factory mediator. But you cannot send
the split message out to a service. Once you exit from the ForEach loop,
it automatically aggregates the split segments. This replaces the
ForEach function of the complex XSLT mediators using a ForEach mediator
and a Payload Factory mediator. However, to implement the
split-aggregate pattern, you still need to use Iterate mediator.

##### Using the [Clone](https://docs.wso2.com/display/EI650/Clone+Mediator) mediator

When using a Clone mediator, use a Call mediator in the target
sequence to bring the responses back into the In-Sequence. This
continues the mediation since the Continuation Stack gets pushed into
the Synapse Message Context via the `         handleMessage        `
method in the `         SynapseCallbackReceiver        ` class.

Otherwise, the Continuation Stack becomes empty in the Synapse Message
Context if you do not use a Call mediator in the target sequence.

##### Using the [Loopback](https://docs.wso2.com/enterprise-integrator/Loopback+Mediator) mediator

Do not include the Loopback mediator in the
`         OutSequence        ` .

!!! tip

When a message passes from the `         InSequence        ` (request
path) to the `         OutSequence        ` (response path), you cannot
use the Loopback mediator to move the message back to the
`         OutSequence        ` again.


The following diagram illustrates a proxy/REST API with a Loopback
mediator:

![](attachments/119133370/119133377.png)

##### Using the [Send](https://docs.wso2.com/enterprise-integrator/Send+Mediator) mediator

-   You should not specify any mediator after the [Send
    Mediator](https://docs.wso2.com/enterprise-integrator/Send+Mediator)
    [or the](https://docs.wso2.com/enterprise-integrator/Send+Mediator)
    [Respond
    Mediator](https://docs.wso2.com/enterprise-integrator/Respond+Mediator)
    .  
      
    A message flow must end from these two mediators. Here, the *message
    flow* does not mean the current sequence. If you have these two
    mediators in a sub-sequence that gets called from a parent sequence,
    then once the message returns from the sub-sequence to the parent,
    the parent sequence should not include any mediator after the call
    to the sub-sequence. If you include a mediator after these two
    mediators, it can cause unusual behaviour in the message flow.  
      
    The following diagram illustrates an incorrect use of the Send
    mediator:  
      
    ![](attachments/119133370/119133374.png){width="650"}  
      

    The following diagram illustrates the correct use of the Send
    mediator:

    ![](attachments/119133370/119133375.png){width="700"}

##### Mediators for message transformation

Follow the guidelines below to use mediators in scenarios that require
message transformation:

-   The most common message transformation use case is reconstructing
    the entire message payload according to the required format of the
    back end service. You can use the [PayloadFactory
    Mediator](https://docs.wso2.com/enterprise-integrator/PayloadFactory+Mediator)
    to do the transformation in this use case, if you know that the
    structure of the new payload is simple, which means the message
    format is fixed, and only a few parameters are extracted from the
    original message.
-   If the original message has repetitive segments and if you want to
    transform each of those segment into a new format with repetitive
    segments, you can use the [For-Each
    Mediator](https://docs.wso2.com/enterprise-integrator/ForEach+Mediator)
    together with the PayloadFactory mediator. The For-Each mediator
    iterates through the repetitive segments and the PayloadFactory
    mediator constructs the segments of the message to a new format. The
    combination of For-Each and PayloadFactory gives the functionality
    provided by the [For-Each function in
    XSLT](https://www.w3schools.com/xml/xsl_for_each.asp) .
-   If only a small part of the message needs to be modified (e.g., you
    need to add/remove an element) you should use the [Enrich
    Mediator](https://docs.wso2.com/eterprise-integrator/Enrich+Mediator)
    .
-   If the transformation logic is complex (i.e., the logic includes
    repetitive segments, conditional transformations, etc), you can use
    the [XSLT
    Mediator](https://docs.wso2.com/enterprise-integrator/XSLT+Mediator)
    to do the transformation.

        !!! note
    
        Note
    
        When you use the XSLT mediator, a third party engine does the
        message transformation. Therefore, this can impact the performance.
    

-   If you want to change the structure of the data in a message or
    convert and transform one data format to another, use the [Data
    Mapper
    Mediator](https://docs.wso2.com/enterprise-integrator/Data+Mapper+Mediator)
    .
-   If you want to work with large file transformations, use the [Smooks
    Mediator](https://docs.wso2.com/enterprise-integrator/Smooks+Mediator)
    .

##### General mediator best practices

-   Use the
    [Iterate](https://docs.wso2.com/display/EI650/Iterate+Mediator)
    mediator in association with the
    [Aggregate](https://docs.wso2.com/display/EI650/Aggregate+Mediator)
    mediator.

-   Do not do any configuration after the Send mediator.

-   Do proper error handling to handle mediation errors as well as
    endpoint errors.

-   Use appropriate intervals for tasks.

-   Use the ForEach mediator only for message transformations. If you
    need to make back-end calls from each iteration, then use the
    iterate mediator.

-   Do not use the DB mediators (
    [DBReport](https://docs.wso2.com/enterprise-integrator/DB+Report+Mediator)
    and
    [DBLookup](https://docs.wso2.com/enterprise-integrator/DBLookup+Mediator)
    ) with complex SQL queries or in scenarios where you need to
    simultaneously retrieve multiple rows. Instead, use the data
    services functionality of WSO2 Enterprise Integrator. For
    information on how to use the data services functionality, see the
    [Tutorials](https://docs.wso2.com/enterprise-integrator/Data+Integration+Tutorials)
    .

-   Use dollar context (i.e.,
    [$ctx](https://docs.wso2.com/enterprise-integrator/Synapse+XPath+Variables#SynapseXPathVariables-$ctx)
    ) instead of
    [get-property()](https://docs.wso2.com/enterprise-integrator/XPath+Extension+Functions#XPathExtensionFunctions-get-property()functionfunc)
    . This is because the get - property methods search even in Registry
    if the value is not available in the message context. Thus, it
    affects performance as Registry search is an expensive operation.
    However, `           $          `
    `                       ctx                     ` only checks in the
    message context.  
    If you need to retrieve a property that you have set on a message,
    use the predefined XPath variables such as $ctx instead of the
    get-property() function for better performance. For example, use
    `           $ctx:          ` [proxy.name](http://proxy.name/)
    instead of `           get-property('          `
    [proxy.name](http://proxy.name/) `           ')          ` .  
    For more information on the predefined XPath variables that you can
    use to retrieve a property, and for examples of XPath variable
    usage, see [Synapse XPath
    Variables](https://docs.wso2.com/enterprise-integrator/Synapse+XPath+Variables)
    .

        !!! note
    
        Note
    
        The use of the `           get-property()          ` function can
        have a lower performance because it does a registry lookup when the
        value is not available in the message context. Therefore, the
        recommended approach is to use predefined XPath variables when you
        need to retrieve a property.
    
        You will encounter this performance issue only if you are using WSO2
        ESB 4.9.0 or below.
    

-   Reusing a defined sequence  
    If you want to repeatedly use the same mediation sequence, you can
    define it and save it either in the Synapse configuration or in the
    Registry, with a unique name. Then you can call the mediation
    sequence from the main sequence as well as from multiple proxy
    services and REST APIs. The saved sequence can be called via the
    [Sequence
    mediator](https://docs.wso2.com/enterprise-integrator/Sequence+Mediator)
    or can be selected as the `           InSequence          ` ,
    `           OutSequence          ` , or
    `           FaultSequence          ` when you define a [proxy
    service](https://docs.wso2.com/enterprise-integrator/Working+with+Proxy+Services)
    or a [REST
    API](https://docs.wso2.com/enterprise-integrator/Working+with+APIs)
    .

    The following diagram illustrates how a saved sequence can be called
    using the Sequence mediator:

![](attachments/119133370/119133373.png){width="700"}

  

When you are adding the last mediator in a sequence, make sure to use
one of the following mediators depending on the scenario. Any mediator
added after one of the following mediators will not be applied.

-   Use the [Respond
    mediator](https://docs.wso2.com/enterprise-integrator/Respond+Mediator)
    as the last mediator if you want the message to be sent back to the
    client.

-   Use the [Send
    Mediator](https://docs.wso2.com/enterprise-integrator/Send+Mediator)
    as the last mediator if you want the message to be sent to a
    specific endpoint. If you want to mediate the message after it is
    sent, you can use the [Clone
    mediator](https://docs.wso2.com/enterprise-integrator/Clone+Mediator)
    to [make two copies of the message and process them
    separately](https://docs.wso2.com/display/IntegrationPatterns/Scatter-Gather)
    in order to avoid conflicts.

-   Use the [Drop
    mediator](https://docs.wso2.com/enterprise-integrator/Drop+Mediator)
    as the last mediator if you want to stop the mediation flow at a
    particular point (e.g., when a filter condition is not met).

-   Use the [Loopback
    mediator](https://docs.wso2.com/enterprise-integrator/Loopback+Mediator)
    as the last mediator if you want the message to be moved from the
    `            InSequence           ` to the
    `            OutSequence           ` .

        !!! info
    
        Note
    
        The Loopback mediator prevents the execution of subsequent mediators
        in the `            In           ` Sequence.
    

-   Use the [Store
    mediator](https://docs.wso2.com/enterprise-integrator/Store+Mediator)
    as the last mediator if you want to enqueue messages to a [message
    store](https://docs.wso2.com/enterprise-integrator/Message+Stores) .

#### Working with proxy services

Use [REST
APIs](https://docs.wso2.com/enterprise-integrator/Using+REST+with+APIs)
instead of proxy services for RESTful service development.

Configure `                   FaultSequences                 `
appropriately. For more information on how the
`         FaultSequence        ` works, see the section on [Handling
errors](#BestPractices-HandlingErrors) .

#### Working with APIs

The following are some [best
practices](https://s3.amazonaws.com/tfpearsonecollege/bestpractices/RESTful+Best+Practices.pdf)
to keep in mind when designing your APIs for use with REST.

-   **Use meaningful resource names** to clarify what a given request
    does. A RESTful URI should refer to a resource that is a thing
    instead of an action. The name and structure of URIs should convey
    meaning to those consumers.
-   **Use plurals in node names** to keep your API URIs consistent
    across all HTTP methods.
-   **Use HTTP methods appropriately** . Use `          POST         ` ,
    `          GET         ` , `          PUT         ` ,
    `          DELETE         ` , `          OPTIONS         ` and
    `          HEAD         ` in requests to clarify the purpose of the
    request. The `          POST         ` , `          GET         ` ,
    `          PUT         ` and `          DELETE         ` methods map
    to the CRUD methods Create, Read, Update, and Delete, respectively.
    Each resource should have at least one method.
-   **Create at most only one default resource** (a resource with
    neither a uri-template nor a url-mapping) for each API.
-   **Offer both XML and JSON** whenever possible.
-   **Use abstraction when it's helpful** . The API implementation does
    not need to mimic the underlying implementation.
-   **Implement resource discoverability through links (HATEOAS)** . As
    mentioned in the previous section, the application state should be
    communicated via hypertext. The API should be usable and
    understandable given an initial URI without prior knowledge or
    out-of-band information.
-   **Version your APIs** as early as possible in the development cycle.
    At present, the ESB profile identifies each API by its unique
    context name. If you introduce a version in the API context (e.g.,
    /Service/1.0.0), you can update it when you upgrade the same API
    (e.g., /Service/1.0.1).
-   **Secure your services** using OAuth2, OpenID, or another
    authentication/authorization mechanism. See also
    [Securing APIs](https://docs.wso2.com/display/EI650/Applying+Security+to+an+API)
    .

#### Working with endpoints

-   Do not use anonymous endpoints. Always use [named
    endpoints](https://docs.wso2.com/enterprise-integrator/Working+with+Endpoints#WorkingwithEndpoints-named_endpoints)
    . As anynymous endpoints have auto-generated names in the synapse
    configuration, it is difficult to identify which endpoint is causing
    the error in case of an error.

-   Configure timeout settings appropriately. Timeout configurations are
    required before you go into production with the system.  
      
    The diagram below illustrates the typical message flow when a proxy
    service is involved in a client-server communication. The two
    connectores, `           Client to Proxy connection          ` and
    `           Proxy to Backend connection          ` *, a* re two
    separate connections that do not depend on each other. Even if one
    connections times out, the other is unaffected.  
    ![](attachments/119133370/119133384.png){width="650"}  
    Here are the important timeout parameters you should configure
    before going into production:

    <table>
    <thead>
    <tr class="header">
    <th>Parameter</th>
    <th>Description</th>
    <th>Configuration File</th>
    <th>Default Value</th>
    <th>Recommended Value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><code>               http.socket.timeout              </code></td>
    <td>The socket timeout of the <a href="https://docs.wso2.com/enterprise-integrator/HTTP+PassThrough+Transport">Passthrough</a> http/https transport sender and listener. You can find the <a href="https://docs.wso2.com/enterprise-integrator/Tuning+the+HTTP+Transport">passthru-http.properties</a> file in the <code>               &lt;EI_HOME&gt;/conf              </code> directory.</td>
    <td><code>               passthru-http.properties              </code></td>
    <td>180000</td>
    <td>180000</td>
    </tr>
    <tr class="even">
    <td>Endpoint timeout</td>
    <td><div class="content-wrapper">
    <p>The timeout parameter that you should configure at the endpoint level. You can configure timeout values as required for specific endpoints.</p>
    <p>Here's a sample endpoint configuration that is configured with timeout parameters. Here, <code>                 &lt;duration&gt;                </code> is the timeout value, and <code>                 &lt;responseAction&gt;                </code> is the action to be taken on timeout. In this example, it is invoking the <code>                 FaultSequence                </code> .</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;endpoint&gt;</span>
    <span id="cb1-2"><a href="#cb1-2"></a>   &lt;address uri=<span class="st">&quot;http://localhost:8281/services/SimpleStockQuoteService&quot;</span>&gt;</span>
    <span id="cb1-3"><a href="#cb1-3"></a>       &lt;timeout&gt;</span>
    <span id="cb1-4"><a href="#cb1-4"></a>          &lt;duration&gt;<span class="dv">120000</span>&lt;/duration&gt;</span>
    <span id="cb1-5"><a href="#cb1-5"></a>          &lt;responseAction&gt;fault&lt;/responseAction&gt;</span>
    <span id="cb1-6"><a href="#cb1-6"></a>       &lt;/timeout&gt;</span>
    <span id="cb1-7"><a href="#cb1-7"></a>   &lt;/address&gt;</span>
    <span id="cb1-8"><a href="#cb1-8"></a> &lt;/endpoint&gt;</span></code></pre></div>
    </div>
    </div>
        !!! tip
        <p>Follow the formula <code>                 Socket Timeout &gt; max(Global endpoint timeout, Timeout of individual endpoints)                </code> , and make sure that you set the <code>                 http.socket.timeout                </code> to a value higher than all other endpoint timeout values.</p>

    </div></td>
    <td>Endpoint configuration files</td>
    <td>synapse.global_timeout_interval</td>
    <td>Depends on the use case, Typically 120000</td>
    </tr>
    <tr class="odd">
    <td><code>               synapse.global_timeout_interval              </code></td>
    <td><div class="content-wrapper">
    <p>Global timeout value for endpoints. Can be overwritten by individual endpoint timeout values.</p>
    <p>Synapse, which is the underlying mediation engine of WSO2 Enterprise Integrator, is a complete asynchronous messaging engine that does not block its worker threads on network I/O. Instead, it registers a call-back for a particular request and returns the threads without waiting for a response. When a response is available, the registered call-back is used to correlate it with the relevant request so that further processing can be done.<br />
    If the backend server does not respond, it is required to clear the registered call-backs after a particular <em>duration</em> to prevent possible memory leaks. This <em>duration</em> is set via a timer task called <code>                 TimeoutHandler                </code> . The <code>                 synapse.global_timeout_interval                </code> parameter represents the <em>duration</em> that a call-back should be kept in the call-back store.</p>
        !!! tip
        <p>If you have configured a timeout value at the endpoint level, the global timeout value is not taken into consideration for that endpoint. For all the other endpoints that do not have a timeout value configured, the global value is considered as the timeout value.</p>

    <p>You can configure the <code>                 synapse.global_timeout_interval                </code> parameter in the <code>                 &lt;EI_HOME&gt;/conf/synapse.properties                </code> file. The default value is 120 seconds. If you want to support endpoint timeout values that are greater than 120 seconds, set the <code>                 synapse.global_timeout_interval                </code> to a value more than 120 seconds. However, the need to set such large timeout values for endpoints is extremely unlikely.</p>
    </div></td>
    <td><code>               synapse.properties              </code></td>
    <td>120000</td>
    <td>120000</td>
    </tr>
    <tr class="even">
    <td><code>               synapse.timeout_handler_interval              </code></td>
    <td>Duration between two <code>               TimeoutHandler              </code> executions.The <code>               TimeoutHandler              </code> is executed every 15 seconds by default. Therefore, the time that call-backs get cleared can deviate up to 15 seconds from the configured value.<br />
    You can configure the <code>               TimeoutHandler              </code> execution interval by specifying a required value for <code>               synapse.timeout_handler_interval              </code> in the <code>               &lt;EI_HOME&gt;/conf/synapse.properties              </code> file.</td>
    <td><code>               synapse.properties              </code></td>
    <td>15000</td>
    <td>15000</td>
    </tr>
    </tbody>
    </table>

-   Set the socket timeout value and individual endpoint timeout values
    appropriately. Use this formula to set timeout values:

    `           Socket Timeout > max(Global          `
    `           endpoint          `
    `           timeout, Timeout          ` `           of          `
    `           individual          ` `           endpoints)          `

-   Be sure to set proper values to advanced configuration parameters,
    although they are optional.  
    The happy path should work with the default values, but you might
    encounter issues in production when the system does not follow the
    happy path. For example, if you use the default configurations and
    as an error occurs in your sequence, the endpoint gets suspended
    immediately and subsequent messages to that endpoint get rejected
    without being sent to the backend service. This might not be the
    expected behaviour in every use case. Therefore, it is important to
    perform [endpoint error
    handling](https://docs.wso2.com/enterprise-integrator/Endpoint+Error+Handling)
    based on the use case.

-   Use the [HTTP
    endpoint](https://docs.wso2.com/enterprise-integrator/HTTP+Endpoint)
    for RESTful service invocations. The HTTP endpoint is especially
    designed to make RESTful service integration easy. For example, it
    supports `           url-templates          ` , which is an option
    to set the http method.

-   For RESTful service integration, use either [REST
    APIs](https://docs.wso2.com/enterprise-integrator/Using+REST+with+APIs)
    or HTTP endpoints. You can use REST APIs to expose an integration
    solution as a RESTful service, and use HTTP endpoints to logically
    represent a RESTful backend service.

#### Error handling

##### Behaviour of the `         FaultSequence        `

-   If a sequence explicitly defines a fault handler using the
    [onError](https://docs.wso2.com/enterprise-integrator/Sample+4%3A+Specifying+a+Fault+Sequence+with+a+Regular+Mediation+Sequence)
    attribute, WSO2 Enterprise Integrator invokes that specific onError
    sequence whenever an error occurs in the sequence. This is true even
    if the sequence is invoked by a [proxy
    service](https://docs.wso2.com/enterprise-integrator/Working+with+Proxy+Services)
    or in an
    [API](https://docs.wso2.com/enterprise-integrator/Working+with+APIs)
    .  
      
    ![](attachments/119133370/119133380.png){width="650"}
-   If a request arrives via the [main
    sequence](https://docs.wso2.com/enterprise-integrator/Mediation+Sequences#MediationSequences-mainSequence)
    and fails within a sequence that does not explicitly define a fault
    handler, the default
    `                     FaultSequence                   ` is
    invoked.  
      
    ![](attachments/119133370/119133378.png){width="680"}
-   If a request arrives via a proxy service or an API, and fails within
    a sequence that does not explicitly define a fault handler, the
    `          FaultSequence         ` of the proxy service is
    invoked.  
      
    ![](attachments/119133370/119133379.png){width="650"}
-   If the proxy service does not have a
    `           FaultSequence          ` defined, the default fault
    handler sequence is invoked.

        !!! tip
    
        This is only applicable to WSO2 ESB 4.9.0 and above.
    

    ![](attachments/119133370/119133376.png){width="650"}  
    ![](attachments/119133370/119133385.png){width="650"}

-   If there is a `           FaultSequence          ` defined at the
    proxy service-level, and the onError sequence is referenced in the
    `           InSequence          ` /
    `           OutSequence          ` , then the onError sequence of
    the relevant `           InSequence          ` /
    `           OutSequence          ` is invoked in the event of an
    error. In this case the proxy service
    `           FaultSequence          ` is ignored. For more
    information on how this works, see [Specifying a Fault Sequence with
    a Regular Mediation
    Sequence](https://docs.wso2.com/enterprise-integrator/Sample+4:+Specifying+a+Fault+Sequence+with+a+Regular+Mediation+Sequence)
    .

-   If you send a request to a REST API, and there is no matching
    resource defined in the API for the request (i.e., REST API resource
    mismatch), an [HTTP status
    code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) 200
    is returned. You should define a default error sequence called
    `           _resource_mismatch_handler_          ` to handle such
    requests.

##### Best practices

-   Whenever an error occurs in WSO2 Enterprise Integrator, the
    mediation engine attempts to provide as much information as possible
    about the
    [error](https://docs.wso2.com/enterprise-integrator/Error+Handling)
    . This is done by initializing a set of property values on the
    erroneous message. Following are the properties:
    -   ERROR\_CODE
    -   ERROR\_MESSAGE
    -   ERROR\_DETAIL
    -   ERROR\_EXCEPTION

The above properties can be logged using the log mediator as follows
inside a `          FaultSequence         ` path.  

``` java
    <log level="custom">
        <property name="text" value="An unexpected error occurred"/>
        <property name="message" expression="$ctx:ERROR_MESSAGE"/>
        <property name="code" expression="$ctx:ERROR_CODE"/>
        <property name="detail" expression="$ctx:ERROR_DETAIL"/>
        <property name="exception" expression="$ctx:ERROR_EXCEPTION"/>
    </log>
```

  

-   When you use the above properties, be sure to log the sequence name
    and proxy service/API name as well if you want to make debugging
    issues easier.
-   If the configuration is complex, you need to separate the
    configuration into multiple sequences. When the configuration is
    separated into multiple sequences, you need to define an [error
    handler (onError
    sequence)](https://docs.wso2.com/enterprise-integrator/Sample+4%3A+Specifying+a+Fault+Sequence+with+a+Regular+Mediation+Sequence)
    in each sequence.

### Troubleshooting

For a complete guide on troubleshooting issues that you may come across
when working with enterprise service bus capabilities, see the
[Enterprise Service Bus Troubleshoot
Guide](https://wso2.com/files/WSO2ESB-ATroubleshootGuide.pdf) . For
troubleshooting with tooling, see [Troubleshooting WSO2 Integration
Studio](https://docs.wso2.com/display/EI650/Troubleshooting+WSO2+Integration+Studio)
.

### Testing

-   Define a test strategy to plan what should be tested in a specific
    project.
-   Create a test plan covering all functional scenarios and performance
    tests.
-   Write Jmeter, SoapUI test cases. If applicable always write
    automation tests.
-   Write java integration tests whenever applicable to automate the
    test scenarios.
-   Automate web application testing using Selenium.
-   If you have an integration solution, isolate test scenarios of each
    product as much as possible and test the scenarios as separate
    units. Then you can write integration tests for the integration
    scenarios including one or more products using java tools such as
    Jmeter and Soap UI.
-   For an application development project, you should perform tests
    throughout the Software Development Life Cycle (SDLC). Make sure
    that your testing environment is identical to the production
    environment when you execute test plans. (For example, resource
    allocation, VM sizes, VMs used for DBs, DB performance tuning,
    network resources etc in the production environment should be
    identical in the test environment)
-   Do not rely on test results of a single node test.
-   Test all custom solutions and artifacts that you develop. Never use
    default ports of the servers when testing custom features.
-   Before you move into production, run load tests on the development
    and test environments using samples that replicate the production
    use case.
-   Before you move into production, run a penetration test on the
    production setup. You need to suggest this to the customer and make
    arrangements for it beforehand.
-   Run a round of User Acceptance Tests (UAT) against the requirements
    to ensure that the system satisfies the customer's acceptance
    criteria.
-   Document all functional test results and performance test results.
    Create a document explaining the steps followed, samples used, and
    the outcome of the tests for both functional test results as well as
    performance test results. Create a separate document including the
    summary of all the test results (e.g number of test cases that you
    executed, observations, summary etc).  
    Following is a sample template for functional test results:

    <table>
    <thead>
    <tr class="header">
    <th>Test Case ID</th>
    <th>Function</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Description</td>
    <td><br />
    </td>
    </tr>
    <tr class="even">
    <td>Test Steps</td>
    <td><br />
    </td>
    </tr>
    <tr class="odd">
    <td>Expected Results</td>
    <td><br />
    </td>
    </tr>
    <tr class="even">
    <td>Actual Results</td>
    <td><br />
    </td>
    </tr>
    <tr class="odd">
    <td>Status</td>
    <td><br />
    </td>
    </tr>
    <tr class="even">
    <td>Comments</td>
    <td><br />
    </td>
    </tr>
    </tbody>
    </table>

    Following is a sample template for performance test results:

    <table>
    <thead>
    <tr class="header">
    <th>Test Case ID</th>
    <th>Function</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Description</td>
    <td><br />
    </td>
    </tr>
    <tr class="even">
    <td>Test Steps</td>
    <td><br />
    </td>
    </tr>
    <tr class="odd">
    <td>Expected Results</td>
    <td><br />
    </td>
    </tr>
    <tr class="even">
    <td>Actual Results</td>
    <td><br />
    </td>
    </tr>
    <tr class="odd">
    <td>Duration</td>
    <td><br />
    </td>
    </tr>
    <tr class="even">
    <td>CPU Usage</td>
    <td><br />
    </td>
    </tr>
    <tr class="odd">
    <td>Memory Usage</td>
    <td><br />
    </td>
    </tr>
    <tr class="even">
    <td>Status</td>
    <td><br />
    </td>
    </tr>
    <tr class="odd">
    <td>Comments</td>
    <td><br />
    </td>
    </tr>
    </tbody>
    </table>

-   Never test on a single node.
-   Conduct proper developer tests.
-   Test use cases with all possible scenarios.
-   It is not sufficient to test only the happy path.
-   Do performance tests.

### Deployment

#### General guidelines

-   Ensure that you tune the deployment environment based on the
    [performance tuning
    guidelines](https://docs.wso2.com/enterprise-integrator/Performance+Tuning)
    .
-   [Install all patches using WSO2 Update
    Manager(WUM)](https://docs.wso2.com/display/ADMIN44x/Updating+WSO2+Products)
    in [all
    environments](https://docs.wso2.com/display/ADMIN44x/WSO2+Patch+Application+Process)
    .

        !!! info
    
        Note
    
        If you are using WSO2 ESB 4.9.0 or above, ensure that you have all
        [WUM](https://docs.wso2.com/display/ADMIN44x/Updating+WSO2+Products)
        updates installed at any given time.
    

-   Pre-test patches in a test environment before going into the
    production environment. You should use an automated test suite to do
    this. For example, you can use [JMeter
    Automation](http://www.testautomationguru.com/how-to-test-rest-api-using-jmeter/)
    or [SOAP
    UI](https://www.soapui.org/tutorials/rest-sample-project.html) .
-   Pre-test artifacts in a test environment before deploying into the
    production environment.
-   Pre-test configuration changes before applying them in the
    production environment.
-   Automate the process of change deployment. You can use puppet for
    this purpose. For more information, see the tutorial on [How To Use
    WSO2 Puppet Modules to Deploy WSO2
    Products](http://wso2.com/library/tutorials/2016/03/tutorial-how-to-use-wso2-puppet-modules-to-deploy-wso2-products/)
    .
-   Make sure you follow the [production hardening
    guidelines](https://docs.wso2.com/display/ADMIN44x/Production+Deployment+Guidelines)
    before going into production or exposing instances to live traffic.
-   To port your artifacts from one environment to another, first
    package the artifacts into a Composite Application (C-App) archive
    using WSO2 EI Tooling, and then [deploy the C-App in the new
    environment](https://docs.wso2.com/display/EI650/Running+the+Product#RunningtheProduct-RunningtheESBprofileviaTooling)
    .
-   It is not recommended to run more than one server instance inside a
    docker container.

!!! note

You can implement high availability in your deployment via strategies
like Blue-Green, Canary, or Rolling deployments. Select one of those
strategies based on your requirements. This enables you to perform load
balancing to maintain availability of services for consumers while they
are being undeployed.


#### Recommended deployment patterns

**Push model**

-   All WSO2 Enterprise Integrator artifacts reside in a version control
    system such as SVN or Git.
-   The CI/CD server checks out the artifacts, builds and pushes the
    artifacts to individual WSO2 Enterprise Integrator nodes.

  

![](attachments/119133370/119133381.png){width="600"}

**Sync model**

Uses a file share or rsync to keep deployment artifacts in-sync across a
set of nodes.

-   Linux file mount
    -   All WSO2 Enterprise Integrator nodes mount the same remote file
        system as \<
        `            EI_HOME>/repository/deployment/server           `
        directory.
    -   If one node makes a change to that location by updating an
        artifact, the rest of the nodes see the change through remote
        mount and update the runtime.

![](attachments/119133370/119133383.png)

-   rsync
    -   One node acts as a master.
    -   Artifact modification should be done only in the master node.
    -   The master node runs rsync on a schedule(cron job) to copy \<
        `            EI_HOME>/repository/deployment/server           `
        to remote nodes.

![](attachments/119133370/119133382.png){height="250"}

-   Hazelcast clustering
    -   If a WSO2 Enterprise Integrator deployment requires task
        coordination/inbound endpoint coordination, hazelcast clustering
        should be enabled.

                !!! note
        
                Note
        
                Hazelcast is known to be unstable when the network is
                unreliable. Therefore, do not enable Hazelcast clustering unless
                coordination is required.
        

    -   Hazelcast clustering can be enabled with any of the above
        deployment patterns.
    -   When you use Hazelcast, it can result in cluster-wide failure if
        the inter-node communication is interrupted. To recover from
        such an issue, you have to restart the entire cluster at
        once. Round robin restart does not work in this situation. Be
        sure to keep customers informed of this limitation.
