# Scheduling ESB Tasks

The ESB profile of WSO2 EI can be configured to execute tasks
periodically. You can schedule a task to run after a time interval of
't' for an 'n' number of times, or you can schedule the task to run once
when the ESB server starts. Alternatively, you can use cron expressions
to have more control over how the task should be scheduled; for example,
you can use a Cron expression to schedule the task to run at 10 pm on
the 20th day of every month.

You can schedule a Task in the ESB profile via the following methods:

-   [Scheduling a Task Using the Default
    Implementation](_Scheduling_a_Task_Using_the_Default_Implementation_)
-   [Scheduling a Task Using a Custom
    Implementation](_Scheduling_a_Task_Using_a_Custom_Implementation_)

!!! info

In a clustered environment, tasks are distributed among server nodes
according to the **round-robin** method, by default. If required, you
can change this default task handling behaviour so that tasks are
distributed **randomly** , or according to a **specific rule** . This is
a server-level setting that is configured in the
`         tasks-config.xml        ` file.

-   See [Configuring the Task Scheduling
    Component](https://docs.wso2.com/display/ADMIN44x/Configuring+the+Task+Scheduling+Component)
    for instructions on configuring the task handling behaviour at
    server-level.
-   You can also configure the task handling behaviour at task-level, by
    specifying the [Pinned Servers](#SchedulingESBTasks-pinned_servers)
    for a task. Note that this setting overrides the server-level
    configuration.

Also, note that a scheduled task will only run on one of the nodes (at a
given time) in a clustered environment. The task will fail over to
another node, only if the first node fails.

## Scheduling a Task Using the Default Implementation

According to the default task implementation in the ESB profile, a task
can be configured to inject messages, either to a defined endpoint, to a
proxy service or a specific sequence defined in the ESB. The sections
below demonstrate an example of scheduling a task using the default
implementation, to inject an XML message and to print it in the logs of
the server.

-   [Creating the
    Task](#SchedulingaTaskUsingtheDefaultImplementation-CreatingtheTask)
    -   [Creating the ESB
        Project](#SchedulingaTaskUsingtheDefaultImplementation-CreatingtheESBProject)
    -   [Creating the
        Sequence](#SchedulingaTaskUsingtheDefaultImplementation-CreatingtheSequence)
    -   [Creating the Scheduled
        Task](#SchedulingaTaskUsingtheDefaultImplementation-CreatingtheScheduledTask)
    -   [Defining the properties of the
        Task](#SchedulingaTaskUsingtheDefaultImplementation-DefiningthepropertiesoftheTask)
-   [Deploying the
    Task](#SchedulingaTaskUsingtheDefaultImplementation-DeployingtheTask)
-   [Viewing the
    output](#SchedulingaTaskUsingtheDefaultImplementation-Viewingtheoutput)

### Creating the Task

Follow the steps below to create the task, which you want to schedule.

#### Creating the ESB Project

1.  Open **WSO2 Integration Studio** and click **ESB Project → Create
    New ** in the **Getting Started** tab as shown below.  
    ![](attachments/119130430/119130431.png){width="700" height="336"}

2.  Enter **ScheduleDefaultTask** as the **ESB Project Name** and click
    **Finish** .  
    ![](attachments/119130430/119130440.png){width="500" height="515"}

3.  The new project will be listed in the project explorer.

#### Creating the Sequence

1.  In the **Project Explorer** , right click the
    **ScheduleDefaultTask** project, and click **New** → **Sequence**
    .  
    ![](attachments/119130430/119130439.png){width="500" height="502"}
2.  Click **Create New Sequence** and click **Next** .
3.  Enter the **InjectXMLSequence** as the sequence name and click
    **Finish** .  
    ![](attachments/119130430/119130438.png){width="500" height="497"}  
4.  Drag and drop a **Log** mediator and a **Drop** mediator from the
    **Mediators** Palette.  
    ![](attachments/119130430/119130437.png){width="550"}  
5.  Click on the **Log** mediator, and in the Properties section enter
    the following details.  
    -   **Log Category:** `            INFO           `
    -   **Log Level:** `            CUSTOM           `

    ![](images/icons/grey_arrow_down.png){.expand-control-image}
    Configuration of the Sequence

    The below is the complete source configuration of the Sequence
    (i.e., the `             InjectXMLSequence.xml            ` file).

    ``` xml
        <?xml version="1.0" encoding="UTF-8"?>
        <sequence name="ScheduleCustomSequence" trace="disable" xmlns="http://ws.apache.org/ns/synapse">
            <log level="custom" />
            <drop />
        </sequence>
    ```

#### Creating the Scheduled Task

1.  Right click the **ScheduleDefaultTask** project and click **New** →
    **Scheduled Task** .  
    ![](attachments/119130430/119130436.png){width="500" height="370"}
2.  Select **Create a** **New Scheduled Task Artifact** and click
    **Next** .  
    ![](attachments/119130430/119130435.png){width="500" height="269"}

        !!! tip
    
        Importing a task?
    
        If you already have a task created, you have the option of importing
        the XML configuration. Select **Import Scheduled Task Artifact** and
        follow the instructions on the UI. To create a new task from
        scratch, continue with the following steps.
    

3.  Enter the below details and click **Finish** .  

    -   **Task Name:** `            InjectXMLTask           `
    -   **Count:** `            -1           `
    -   I **nterval (in seconds):** 5

    ![](attachments/119130430/119130434.png){width="500" height="452"}

    ![](images/icons/grey_arrow_down.png){.expand-control-image} More
    information of the above parameters

    <table>
    <thead>
    <tr class="header">
    <th>Parameter</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Task Name</td>
    <td>Name of a scheduled task.</td>
    </tr>
    <tr class="even">
    <td>Task Group</td>
    <td>The <code>                 synapse.simple.quartz                </code> group will be selected by default.</td>
    </tr>
    <tr class="odd">
    <td>Task Implementation</td>
    <td>The default task implementation class ( <code>                 org.apache.synapse.startup.tasks.MessageInjector                </code> ) of the ESB will be selected by default. This class simply injects a specified message into the Synapse environment of the ESB when the server starts.<br />
    If you are want to use a custom task implementation, see the instructions in <a href="#SchedulingaTaskUsingtheDefaultImplementation-custom">Writing Tasks</a> .</td>
    </tr>
    <tr class="even">
    <td>Trigger Type</td>
    <td><div class="content-wrapper">
    <p>The trigger type determines the task execution schedule.</p>
    <ul>
    <li><p><strong>Simple Trigger:</strong> Schedules the task to run a specified number of times at specified intervals. In the <strong>Count</strong> field, enter the number of time the task should be executed, and in the <strong>Interval</strong> field, enter the time interval (in seconds) between consecutive executions of the task.</p>
    <p>See the following examples for simple triggers:</p>
    <div class="localtabs-macro">
    <div class="aui-tabs horizontal-tabs" data-aui-responsive="true" role="application">
    <ul>
    <li><a href="#4703a6be07a54b79af66ebe01fab2cb9"><strong>Only once</strong></a></li>
    <li><a href="#0ef990d2713b4ec2bc9faffda54498ab"><strong>Every 5 seconds continuously</strong></a></li>
    <li><a href="#1aec3cd57f7044aa97b4bd094760ac79"><strong>Every 5 seconds 10 times</strong></a></li>
    </ul>
    <div id="4703a6be07a54b79af66ebe01fab2cb9" class="tabs-pane active-pane" name="Only once">
    <p>To run only once after WSO2 EI starts:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;task name=<span class="st">&quot;CheckPrice&quot;</span> <span class="kw">class</span>=<span class="st">&quot;org.wso2.esb.tutorial.tasks.PlaceStockOrderTask&quot;</span>&gt;</span>
    <span id="cb1-2"><a href="#cb1-2"></a>&lt;trigger once=<span class="st">&quot;true&quot;</span>/&gt;</span>
    <span id="cb1-3"><a href="#cb1-3"></a>&lt;/task&gt;</span></code></pre></div>
    </div>
    </div>
    </div>
    <div id="0ef990d2713b4ec2bc9faffda54498ab" class="tabs-pane" name="Every 5 seconds continuously">
    <p>To run every 5 seconds continuously:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a>&lt;task name=<span class="st">&quot;CheckPrice&quot;</span> <span class="kw">class</span>=<span class="st">&quot;org.wso2.esb.tutorial.tasks.PlaceStockOrderTask&quot;</span>&gt;</span>
    <span id="cb2-2"><a href="#cb2-2"></a>&lt;trigger interval=<span class="st">&quot;5&quot;</span>/&gt;</span>
    <span id="cb2-3"><a href="#cb2-3"></a>&lt;/task&gt;</span></code></pre></div>
    </div>
    </div>
    </div>
    <div id="1aec3cd57f7044aa97b4bd094760ac79" class="tabs-pane" name="Every 5 seconds 10 times">
    <p>To run every 5 seconds for 10 times:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb3" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb3-1"><a href="#cb3-1"></a>&lt;task name=<span class="st">&quot;CheckPrice&quot;</span> <span class="kw">class</span>=<span class="st">&quot;org.wso2.esb.tutorial.tasks.PlaceStockOrderTask&quot;</span>&gt;</span>
    <span id="cb3-2"><a href="#cb3-2"></a>&lt;trigger interval=<span class="st">&quot;5&quot;</span> count=<span class="st">&quot;10&quot;</span>/&gt;</span>
    <span id="cb3-3"><a href="#cb3-3"></a>&lt;/task&gt;</span></code></pre></div>
    </div>
    </div>
    </div>
    </div>
    </div></li>
    <li><p><strong>Cron Trigger:</strong> Schedules the task according to a Cron expression. See the following example for acron trigger where the task is scheduled to run at 1:30 AM:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb4" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb4-1"><a href="#cb4-1"></a>&lt;task name=<span class="st">&quot;CheckPrice&quot;</span> <span class="kw">class</span>=<span class="st">&quot;org.wso2.esb.tutorial.tasks.PlaceStockOrderTask&quot;</span>&gt;</span>
    <span id="cb4-2"><a href="#cb4-2"></a>&lt;trigger cron=<span class="st">&quot;0 30 1 * * ?&quot;</span>/&gt;</span>
    <span id="cb4-3"><a href="#cb4-3"></a>&lt;/task&gt;</span></code></pre></div>
    </div>
    </div></li>
    </ul>
    </div></td>
    </tr>
    <tr class="odd">
    <td>Pinned Servers</td>
    <td><div class="content-wrapper">
    <p>The list of ESB server nodes that will run the task. You can specify the IP addresses of the required nodes.</p>
        !!! info
        <p>This setting can be used if you want the task to run on a selected set of nodes in an ESB cluster. Note that the task will only run on one of the nodes at a time. It will fail over to another node, only if the first node fails. Pinned servers will override the default task handling behavior defined at server-level (for this particular task). However, if <strong>rule-based</strong> task handling is specified at server-level, you need to ensure that the same server nodes you specify as pinned servers for the task are also specified for the task handling rule at server-level.</p>

    </div></td>
    </tr>
    </tbody>
    </table>

    In the **Package Explorer** , you view the
    `           InjectXMLTask          ` created task created in the
    `           src/main/synapse-config/tasks          ` directory under
    the **ScheduleDefaultTask** project.

#### Defining the properties of the Task

1.  In the **Form View** of the `          InjectXMLTask.xml         `
    file, click the **Task Implementation Properties** button.  
    ![](attachments/119130430/119130433.png){width="650" height="281"}
2.  Select **XML** as the **Parameter Type** of the **message**
    parameter, enter
    `           <abc>This is a scheduled task of the default implementation.</abc>          `
    as the XML message in the **Value/Expression** field and click
    **OK** .  
    ![](attachments/119130430/119130451.png)

    ![](images/icons/grey_arrow_down.png){.expand-control-image} More
    information on the above properties

    <table>
    <thead>
    <tr class="header">
    <th>Parameter Name</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>message</td>
    <td><div class="content-wrapper">
    <p>Specify the body of the request that should be sent when the task is executed.</p>
        !!! tip
        <p>It is mandatory to provide a value for the message property. Therefore, even If you do not want to send a message body, you have to provide an empty payload as the value to avoid an exception being thrown.</p>

    </div></td>
    </tr>
    <tr class="even">
    <td>soapAction</td>
    <td>This is the SOAP action to use when sending the message to the endpoint.</td>
    </tr>
    <tr class="odd">
    <td>to</td>
    <td><div class="content-wrapper">
    <p>If the task should send the message directly to the endpoint through the <strong>main</strong> sequence, the endpoint address should be specified. For example, if the address of the endpoint is <a href="http://localhost:9000/services/SimpleStockQuoteService">http://localhost:9000/services/SimpleStockQuoteService</a> , the Synapse configuration of the scheduled task will be as follows:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;task <span class="kw">class</span>=<span class="st">&quot;org.apache.synapse.startup.tasks.MessageInjector&quot;</span> group=<span class="st">&quot;synapse.simple.quartz&quot;</span> name=<span class="st">&quot;CheckPrice&quot;</span>&gt;        &lt;property name=<span class="st">&quot;to&quot;</span> value=<span class="st">&quot;http://localhost:9000/services/SimpleStockQuoteService&quot;</span>/&gt;</span>
    <span id="cb1-2"><a href="#cb1-2"></a>        &lt;property name=<span class="st">&quot;soapAction&quot;</span> value=<span class="st">&quot;urn:getQuote&quot;</span>/&gt;</span>
    <span id="cb1-3"><a href="#cb1-3"></a>        &lt;property name=<span class="st">&quot;message&quot;</span>&gt;</span>
    <span id="cb1-4"><a href="#cb1-4"></a>            &lt;m0:getQuote xmlns:m0=<span class="st">&quot;http://services.samples&quot;</span> xmlns=<span class="st">&quot;http://ws.apache.org/ns/synapse&quot;</span>&gt;</span>
    <span id="cb1-5"><a href="#cb1-5"></a>                &lt;m0:request&gt;</span>
    <span id="cb1-6"><a href="#cb1-6"></a>                    &lt;m0:symbol&gt;IBM&lt;/m0:symbol&gt;</span>
    <span id="cb1-7"><a href="#cb1-7"></a>                &lt;/m0:request&gt;</span>
    <span id="cb1-8"><a href="#cb1-8"></a>            &lt;/m0:getQuote&gt;</span>
    <span id="cb1-9"><a href="#cb1-9"></a>        &lt;/property&gt;</span>
    <span id="cb1-10"><a href="#cb1-10"></a>        &lt;trigger interval=<span class="st">&quot;5&quot;</span>/&gt;</span>
    <span id="cb1-11"><a href="#cb1-11"></a>    &lt;/task&gt;</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    <tr class="even">
    <td>injectTo</td>
    <td>If the task is not sending the message directly to the endpoint (through the main sequence), it should be injected to proxy service or a sequence. Specify <strong>sequence</strong> , or <strong>proxy</strong> .</td>
    </tr>
    <tr class="odd">
    <td>sequenceName</td>
    <td><div class="content-wrapper">
    <p>If the task should inject the message to a sequence ( <strong>injectTo</strong> parameter is <strong>sequence</strong> ), enter the name of the sequence. For example, if the name of the sequence is 'SampleSequence', the synapse configuration of the scheduled task will be as follows:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a>&lt;task name=<span class="st">&quot;SampleInjectToSequenceTask&quot;</span></span>
    <span id="cb2-2"><a href="#cb2-2"></a>         <span class="kw">class</span>=<span class="st">&quot;org.apache.synapse.startup.tasks.MessageInjector&quot;</span></span>
    <span id="cb2-3"><a href="#cb2-3"></a>         group=<span class="st">&quot;synapse.simple.quartz&quot;</span>&gt;</span>
    <span id="cb2-4"><a href="#cb2-4"></a>      &lt;trigger count=<span class="st">&quot;2&quot;</span> interval=<span class="st">&quot;5&quot;</span>/&gt;</span>
    <span id="cb2-5"><a href="#cb2-5"></a></span>
    <span id="cb2-6"><a href="#cb2-6"></a>      &lt;property xmlns:task=<span class="st">&quot;http://www.wso2.org/products/wso2commons/tasks&quot;</span></span>
    <span id="cb2-7"><a href="#cb2-7"></a></span>
    <span id="cb2-8"><a href="#cb2-8"></a>                name=<span class="st">&quot;injectTo&quot;</span></span>
    <span id="cb2-9"><a href="#cb2-9"></a>                value=<span class="st">&quot;sequence&quot;</span>/&gt;</span>
    <span id="cb2-10"><a href="#cb2-10"></a></span>
    <span id="cb2-11"><a href="#cb2-11"></a>      &lt;property xmlns:task=<span class="st">&quot;http://www.wso2.org/products/wso2commons/tasks&quot;</span> name=<span class="st">&quot;message&quot;</span>&gt;</span>
    <span id="cb2-12"><a href="#cb2-12"></a>         &lt;m0:getQuote xmlns:m0=<span class="st">&quot;http://services.samples&quot;</span>&gt;</span>
    <span id="cb2-13"><a href="#cb2-13"></a>            &lt;m0:request&gt;</span>
    <span id="cb2-14"><a href="#cb2-14"></a>               &lt;m0:symbol&gt;IBM&lt;/m0:symbol&gt;</span>
    <span id="cb2-15"><a href="#cb2-15"></a>            &lt;/m0:request&gt;</span>
    <span id="cb2-16"><a href="#cb2-16"></a>         &lt;/m0:getQuote&gt;</span>
    <span id="cb2-17"><a href="#cb2-17"></a>      &lt;/property&gt;</span>
    <span id="cb2-18"><a href="#cb2-18"></a></span>
    <span id="cb2-19"><a href="#cb2-19"></a>      &lt;property xmlns:task=<span class="st">&quot;http://www.wso2.org/products/wso2commons/tasks&quot;</span></span>
    <span id="cb2-20"><a href="#cb2-20"></a></span>
    <span id="cb2-21"><a href="#cb2-21"></a>                name=<span class="st">&quot;sequenceName&quot;</span></span>
    <span id="cb2-22"><a href="#cb2-22"></a>                value=<span class="st">&quot;SampleSequence&quot;</span>/&gt;</span>
    <span id="cb2-23"><a href="#cb2-23"></a></span>
    <span id="cb2-24"><a href="#cb2-24"></a>   &lt;/task&gt;</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    <tr class="even">
    <td>proxyName</td>
    <td><div class="content-wrapper">
    <p>If the task should inject the message to a proxy service ( <strong>injectTo</strong> parameter is <strong>proxy</strong> ), enter the name of the proxy service. For example, if the name of the proxy service is 'SampleProxy', the synapse configuration of the scheduled task will be as follows:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb3" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb3-1"><a href="#cb3-1"></a> &lt;task name=<span class="st">&quot;SampleInjectToProxyTask&quot;</span></span>
    <span id="cb3-2"><a href="#cb3-2"></a>         <span class="kw">class</span>=<span class="st">&quot;org.apache.synapse.startup.tasks.MessageInjector&quot;</span></span>
    <span id="cb3-3"><a href="#cb3-3"></a>         group=<span class="st">&quot;synapse.simple.quartz&quot;</span>&gt;</span>
    <span id="cb3-4"><a href="#cb3-4"></a>      &lt;trigger count=<span class="st">&quot;2&quot;</span> interval=<span class="st">&quot;5&quot;</span>/&gt;</span>
    <span id="cb3-5"><a href="#cb3-5"></a>      &lt;property xmlns:task=<span class="st">&quot;http://www.wso2.org/products/wso2commons/tasks&quot;</span> name=<span class="st">&quot;message&quot;</span>&gt;</span>
    <span id="cb3-6"><a href="#cb3-6"></a>         &lt;m0:getQuote xmlns:m0=<span class="st">&quot;http://services.samples&quot;</span>&gt;</span>
    <span id="cb3-7"><a href="#cb3-7"></a>            &lt;m0:request&gt;</span>
    <span id="cb3-8"><a href="#cb3-8"></a>               &lt;m0:symbol&gt;IBM&lt;/m0:symbol&gt;</span>
    <span id="cb3-9"><a href="#cb3-9"></a>            &lt;/m0:request&gt;</span>
    <span id="cb3-10"><a href="#cb3-10"></a>         &lt;/m0:getQuote&gt;</span>
    <span id="cb3-11"><a href="#cb3-11"></a>      &lt;/property&gt;</span>
    <span id="cb3-12"><a href="#cb3-12"></a></span>
    <span id="cb3-13"><a href="#cb3-13"></a>      &lt;property xmlns:task=<span class="st">&quot;http://www.wso2.org/products/wso2commons/tasks&quot;</span></span>
    <span id="cb3-14"><a href="#cb3-14"></a></span>
    <span id="cb3-15"><a href="#cb3-15"></a>                name=<span class="st">&quot;proxyName&quot;</span></span>
    <span id="cb3-16"><a href="#cb3-16"></a>                value=<span class="st">&quot;SampleProxy&quot;</span>/&gt;</span>
    <span id="cb3-17"><a href="#cb3-17"></a></span>
    <span id="cb3-18"><a href="#cb3-18"></a>      &lt;property xmlns:task=<span class="st">&quot;http://www.wso2.org/products/wso2commons/tasks&quot;</span></span>
    <span id="cb3-19"><a href="#cb3-19"></a>                </span>
    <span id="cb3-20"><a href="#cb3-20"></a>                name=<span class="st">&quot;injectTo&quot;</span></span>
    <span id="cb3-21"><a href="#cb3-21"></a>                value=<span class="st">&quot;proxy&quot;</span>/&gt;</span>
    <span id="cb3-22"><a href="#cb3-22"></a></span>
    <span id="cb3-23"><a href="#cb3-23"></a>   &lt;/task&gt;</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

        !!! info
        **Injecting messages to RESTful Endpoints**
    
        In order to use the Message Injector to inject a message to a
        RESTful endpint, we can specify the injector with the required
        payload and inject the message to sequence or proxy service as
        defined above. The sample below shows a RESTful message injection
        through a ProxyService.
    
        -   [**Task Configuration**](#2b774f3ecd594a939ac35967e7f0ae1b)
        -   [**Proxy Service**](#09ee85d851ca4b25b5dfc400524fda0b)
    
    ``` java
            <task name="SampleInjectToProxyTask"
                     class="org.apache.synapse.startup.tasks.MessageInjector"
                     group="synapse.simple.quartz">
                  <trigger count="2" interval="5"/>
                  <property xmlns:task="http://www.wso2.org/products/wso2commons/tasks"
                            name="injectTo"
                            value="proxy"/>
                  <property xmlns:task="http://www.wso2.org/products/wso2commons/tasks" name="message">
                     <request xmlns="">
                        <location>
                           <city>London</city>
                           <country>UK</country>
                        </location>
                     </request>
                  </property>
                  <property xmlns:task="http://www.wso2.org/products/wso2commons/tasks"
                            name="proxyName"
                            value="SampleProxy"/>
               </task>
    ```
        
    ``` java
            <proxy name="SampleProxy"
                      transports="https http"
                      startOnLoad="true"
                      trace="disable">
                  <description/>
                  <target>
                     <inSequence>
                        <property name="uri.var.city" expression="//request/location/city"/>
                        <property name="uri.var.cc" expression="//request/location/country"/>
                        <log>
                           <property name="Which city?" expression="get-property('uri.var.city')"/>
                           <property name="Which country?" expression="get-property('uri.var.cc')"/>
                        </log>
                        <send>
                           <endpoint name="EP">
                              <http method="get"
                                    uri-template="http://api.openweathermap.org/data/2.5/weather?q={uri.var.city},{uri.var.cc}"/>
                           </endpoint>
                        </send>
                     </inSequence>
                     <outSequence>
                        <log level="full"/>
                        <drop/>
                     </outSequence>
                  </target>
               </proxy>
    ```
        

    ![](images/icons/grey_arrow_down.png){.expand-control-image}
    Configuration of the Scheduled Task

    The below is the complete source configuration of the Scheduled Task
    (i.e., the `             InjectXMLTask.xml            ` file).

    ``` xml
        <?xml version="1.0" encoding="UTF-8"?>
        <task class="com.example.post.scheduleTask1.ESBTask" group="synapse.simple.quartz" name="PrintParameterTask" xmlns="http://ws.apache.org/ns/synapse">
            <trigger interval="5" />
            <property name="parameter" value="<abc>This is a scheduled task of the default implementation.</abc>" xmlns:task="http://www.wso2.org/products/wso2commons/tasks"/>
        </task>
    ```

### Deploying the Task

1.  Open the `          pom.xml         ` file of the Composite
    Application Project and select the artifacts that need to be
    deployed.  
    ![](attachments/119130430/119130432.png){width="700" height="306"}
2.  Start the ESB profile by adding the
    **ScheduleDefaultTaskCompositeApplication** . For instructions, see
    [Running the ESB profile via WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/Running+the+Product#RunningtheProduct-RunningtheESBprofileviaTooling)
    .  
    ![](attachments/119130430/119130444.png)

### Viewing the output

You view the XML message you injected (i.e.,
`         <abc>This is a scheduled task of the default implementation.</abc>        `
) getting printed in the logs of the ESB Profile every 5 seconds.

![](attachments/119130430/119130443.png)

## Scheduling a Task Using a Custom Implementation

When you create a task using the default task implementation of the ESB,
the task can inject messages to a proxy service, or to a sequence. If
you have a specific task-handling requirement, you can write your own
task-handling implementation by creating a custom Java Class that
implements the `         org.apache.synapse.startup.Task        `
interface.

For example, the below sections demonstrate how you can create and
schedule a task to receive stock quotes by invoking a back-end service,
which exposes stock quotes. The scheduled task will read stock order
information from a text file, and print the stock quotes.

-   [Starting the Back End
    Service](#SchedulingaTaskUsingaCustomImplementation-StartingtheBackEndService)
-   [Creating the custom Task
    implementation](#SchedulingaTaskUsingaCustomImplementation-CreatingthecustomTaskimplementation)
    -   [Creating the Maven
        Project](#SchedulingaTaskUsingaCustomImplementation-CreatingtheMavenProject)
    -   [Creating the Java
        Package](#SchedulingaTaskUsingaCustomImplementation-CreatingtheJavaPackage)
    -   [Creating the Java
        Class](#SchedulingaTaskUsingaCustomImplementation-CreatingtheJavaClass)
-   [Deploying the custom Task
    implementation](#SchedulingaTaskUsingaCustomImplementation-DeployingthecustomTaskimplementation)
-   [Creating the
    Task](#SchedulingaTaskUsingaCustomImplementation-CreatingtheTask)
    -   [Creating the ESB
        Project](#SchedulingaTaskUsingaCustomImplementation-CreatingtheESBProject)
    -   [Creating the
        Sequence](#SchedulingaTaskUsingaCustomImplementation-CreatingtheSequence)
    -   [Creating the Scheduled
        Task](#SchedulingaTaskUsingaCustomImplementation-CreatingtheScheduledTask)
    -   [Defining the properties of the
        Task](#SchedulingaTaskUsingaCustomImplementation-DefiningthepropertiesoftheTask)
-   [Deploying the
    Task](#SchedulingaTaskUsingaCustomImplementation-DeployingtheTask)
-   [Creating the text
    file](#SchedulingaTaskUsingaCustomImplementation-Creatingthetextfile)
-   [Viewing the
    output](#SchedulingaTaskUsingaCustomImplementation-Viewingtheoutput)

### Starting the Back End Service

Follow the steps below to start the backend service.

1.  Download the backend service from [Git
    Hub](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/stockquote-deployable-jar-2.2.2.jar)
    .

2.  Copy the JAR file you downloaded to the
    `          <EI_HOME>/wso2/msf4j/deployment/microservices         `
    directory.
3.  Start the MSF4J profile. For instructions , see [Starting the
    product
    profiles](https://docs.wso2.com/display/EI650/Running+the+Product#RunningtheProduct-Startingtheproductprofiles)
    .

### Creating the custom Task implementation

Follow the steps below to create the implementation of the custom Task.

#### Creating the Maven Project

1.  Create a Maven Project using the following information. For
    instructions, see [Creating the Maven
    project](#SchedulingaTaskUsingaCustomImplementation-CreatingtheMavenproject)
    .

        !!! tip
    
        You can skip step 5 since you do not need to add external JAR files
        in this example.
    

    -   **Group Id:** `             org.wso2.task            `

    -   **Artifact Id** :
        `             StockQuoteTaskMavenProject            `

        `                                                            `

    #### Creating the Java Package

2.  Create a Java Package inside the Maven Project using the following
    information. For instructions, see [Creating the Java Package for
    the
    Maven Project](https://docs.wso2.com/display/EI650/Working+with+Service+Tasks#WorkingwithServiceTasks-CreatingtheJavaPackagefortheMavenProject)
    .

    -   **Name:** `             org.wso2.task.stockquote.v1            `

        ![](attachments/119130458/119130467.png)

    #### Creating the Java Class

3.  Create a Java Class inside the Maven Project using the following
    information. For instructions, see [Creating the Java Class for the
    Maven
    Project](https://docs.wso2.com/display/EI650/Working+with+Service+Tasks#WorkingwithServiceTasks-CreatingtheJavaClassfortheMavenProject)
    .  
    -   **Name:** `            StockQuoteTaskV1           `

    `                                                  `
4.  In the **Project Explorer** , double-click on the
    **StockQuoteTaskV1.java** file and replace its source with the below
    content.  

    ``` java
        package org.wso2.task.stockquote.v1;
    
        import java.io.BufferedReader;
        import java.io.File;
        import java.io.FileReader;
        import java.io.IOException;
    
        import org.apache.axiom.om.OMAbstractFactory;
        import org.apache.axiom.om.OMElement;
        import org.apache.axiom.om.OMFactory;
        import org.apache.axiom.om.OMNamespace;
        import org.apache.axis2.addressing.EndpointReference;
        import org.apache.commons.logging.Log;
        import org.apache.commons.logging.LogFactory;
        import org.apache.synapse.ManagedLifecycle;
        import org.apache.synapse.MessageContext;
        import org.apache.synapse.SynapseException;
        import org.apache.synapse.core.SynapseEnvironment;
        import org.apache.synapse.startup.Task;
        import org.apache.synapse.util.PayloadHelper;
    
        public class StockQuoteTaskV1 implements Task, ManagedLifecycle {
            private Log log = LogFactory.getLog(StockQuoteTaskV1.class);
            private String to;
            private String stockFile;
            private SynapseEnvironment synapseEnvironment;
    
            public void execute() {
                log.debug("PlaceStockOrderTask begin");
    
                if (synapseEnvironment == null) {
                    log.error("Synapse Environment not set");
                    return;
                }
    
                if (to == null) {
                    log.error("to not set");
                    return;
                }
    
                File existFile = new File(stockFile);
    
                if (!existFile.exists()) {
                    log.debug("waiting for stock file");
                    return;
                }
    
                try {
    
                    // file format IBM,100,120.50
    
                    BufferedReader reader = new BufferedReader(new FileReader(stockFile));
                    String line = null;
    
                    while ((line = reader.readLine()) != null) {
                        line = line.trim();
    
                        if (line == "") {
                            continue;
                        }
    
                        String[] split = line.split(",");
                        String symbol = split[0];
                        String quantity = split[1];
                        String price = split[2];
                        MessageContext mc = synapseEnvironment.createMessageContext();
                        mc.setTo(new EndpointReference(to));
                        mc.setSoapAction("urn:placeOrder");
                        mc.setProperty("OUT_ONLY", "true");
                        OMElement placeOrderRequest = createPlaceOrderRequest(symbol, quantity, price);
                        PayloadHelper.setXMLPayload(mc, placeOrderRequest);
                        synapseEnvironment.injectMessage(mc);
                        log.info("placed order symbol:" + symbol + " quantity:" + quantity + " price:" + price);
                    }
    
                    reader.close();
                } catch (IOException e) {
                    throw new SynapseException("error reading file", e);
                }
    
                File renamefile = new File(stockFile);
                renamefile.renameTo(new File(stockFile + "." + System.currentTimeMillis()));
                log.debug("PlaceStockOrderTask end");
            }
    
            public static OMElement createPlaceOrderRequest(String symbol, String qty, String purchPrice) {
                OMFactory factory = OMAbstractFactory.getOMFactory();
                OMNamespace ns = factory.createOMNamespace("http://services.samples/xsd", "m0");
                OMElement placeOrder = factory.createOMElement("placeOrder", ns);
                OMElement order = factory.createOMElement("order", ns);
                OMElement price = factory.createOMElement("price", ns);
                OMElement quantity = factory.createOMElement("quantity", ns);
                OMElement symb = factory.createOMElement("symbol", ns);
                price.setText(purchPrice);
                quantity.setText(qty);
                symb.setText(symbol);
                order.addChild(price);
                order.addChild(quantity);
                order.addChild(symb);
                placeOrder.addChild(order);
                return placeOrder;
            }
    
            public void destroy() {}
    
            public void init(SynapseEnvironment synapseEnvironment) {
                this.synapseEnvironment = synapseEnvironment;
            }
    
            public SynapseEnvironment getSynapseEnvironment() {
                return synapseEnvironment;
            }
    
            public void setSynapseEnvironment(SynapseEnvironment synapseEnvironment) {
                this.synapseEnvironment = synapseEnvironment;
            }
    
            public String getTo() {
                return to;
            }
    
            public void setTo(String to) {
                this.to = to;
            }
    
            public String getStockFile() {
                return stockFile;
            }
    
            public void setStockFile(String stockFile) {
                this.stockFile = stockFile;
            }
        }
    ```

    ![](attachments/119130458/119130464.png)

5.  In the **Project Explorer** , double-click on the **pom.xml** file
    and replace its source with the below content.

    **pom.xml**

    ``` xml
            <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
                <modelVersion>4.0.0</modelVersion>
                <groupId>org.wso2.task</groupId>
                <artifactId>StockQuoteTask</artifactId>
                <version>0.0.1-SNAPSHOT</version>
                <repositories>
                    <repository>
                        <id>wso2.releases</id>
                        <name>WSO2 internal Repository</name>
                        <url>http://maven.wso2.org/nexus/content/repositories/releases/</url>
                        <releases>
                            <enabled>true</enabled>
                            <updatePolicy>daily</updatePolicy>
                            <checksumPolicy>ignore</checksumPolicy>
                        </releases>
                    </repository>
                </repositories>
                <dependencies>
                    <dependency>
                        <groupId>org.apache.synapse</groupId>
                        <artifactId>synapse-core</artifactId>
                        <version>2.1.7-wso2v65</version>
                    </dependency>
                </dependencies>
            </project>
    ```

    ![](attachments/119130458/119130465.png)  

    ![](images/icons/grey_arrow_down.png){.expand-control-image} Writing
    the custom Task

    ##### Step 1: Writing the Task Class

    You can create a custom task class, which implements the
    `             org.apache.synapse.startup.Task            ` interface
    as follows. This interface has a single
    `             execute()            ` method, which contains the code
    that will be executed according to the defined schedule.

    The `             execute()            ` method contains the
    following actions:

    1.  Check whether the file exists at the desired location.
    2.  If it does, then read the file line by line composing place
        order messages for each line in the text file.
    3.  Individual messages are then injected to the synapse environment
        with the given `              To             `
        [endpoint](https://docs.wso2.com/display/EI6xx/Working+with+Endpoints)
        reference.
    4.  Set each message as `              OUT_ONLY             `
        since it is not expected any response for messages.

    In addition to the `             execute()            ` method, it
    is also possible to make the class implement a
    `             JavaBean            ` interface.

        !!! info
        When creating customized task schedules, if the injecting sequence
        of the message flow contains **Publish Event** mediators, set the
        following property in the Synapse message context:
    
    ``` java
            mc.setProperty("CURRENT_TASK_EXECUTING_TENANT_IDENTIFIER",PrivilegedCarbonContext.getThreadLocalCarbonContext().getTenantId());
    ```
        
            Also, add the following dependency to the POM file of the custom
            task project:
            `             WSO2 Carbon - Utilities bundle            ` (symbolic
            name: `             org.wso2.carbon.utils            ` )
        
        !!! info
        When creating customized task schedules, if the injecting sequence
        of the message flow contains **Publish Event** mediators, set the
        following property in the Synapse message context:
    
    ``` java
            mc.setProperty("CURRENT_TASK_EXECUTING_TENANT_IDENTIFIER",PrivilegedCarbonContext.getThreadLocalCarbonContext().getTenantId());
    ```
        
            Also, add the following dependency to the POM file of the custom
            task project:
            `             WSO2 Carbon - Utilities bundle            ` (symbolic
            name: `             org.wso2.carbon.utils            ` )
        
    This is a bean implementing two properties: To and StockFile. These
    are used to configure the task.

    **Implementing `              ManagedLifecycle             ` for
    Initialization and Clean-up**

    Since a task implements `             ManagedLifecyle            `
    interface, the ESB profile will call the
    `             init()            ` method at the initialization of a
    `             Task            ` object and
    `             destroy()            ` method when a
    `             Task            ` object is destroyed:

    ``` java
        public interface ManagedLifecycle {
        public void init(SynapseEnvironment se);
        public void destroy();
        }
    ```

    The `             PlaceStockOrderTask            ` stores the
    Synapse environment object reference in an instance variable for
    later use with the `             init()            ` method. The
    `             SynapseEnvironment            ` is needed for
    injecting messages into the ESB.

    ##### Step 2: Customizing the Task

    It is possible to pass values to a task at runtime using property
    elements. In this example, the location of the stock order file and
    its address was given using two properties within the
    `             Task            ` object:

    -   **String type**
    -   **OMElement type**

        !!! info
        Tip
    
        For **OMElement** type, it is possible to pass XML elements as
        values in the configuration file.
    
    When creating a `             Task            ` object, the ESB will
    initialize the properties with the given values in the configuration
    file.

    ``` java
        public String getStockFile() {
        return stockFile;
        }
        public void setStockFile(String stockFile) {
        this.stockFile = stockFile;
        }
    ```

    For example, the following properties in the
    `             Task            ` class are initialized with the given
    values within the property element of the task in the configuration.

    ``` java
            <syn:property xmlns="http://ws.apache.org/ns/synapse" name="stockFile"value="/home/upul/test/stock.txt"/>
    ```

    For those properties given as XML elements, properties need to be
    defined within the `             Task            ` class using the
    format given below. OMElement comes from [Apache
    AXIOM](http://ws.apache.org/commons/axiom/) , which is used by the
    ESB profile. AXIOM is an object model similar to DOM. To learn more
    about AXIOM, see the tutorial in the [AXIOM user
    guide](http://ws.apache.org/axiom/userguide/userguide.html) .

    ``` java
            public void setMessage(OMElement elem) {
            message = elem;}
    ```

    It can be initialized with an XML element as follows:

    ``` java
            <property name="message">
            <m0:getQuote xmlns:m0="http://services.samples/xsd">
            <m0:request>
            <m0:symbol>IBM</m0:symbol>
            </m0:request>
            </m0:getQuote>
            </property>
    ```

### Deploying the custom Task implementation

For instructions on deploying the custom Task implementation, see
[Deploying artifacts of the Maven
Project](https://docs.wso2.com/display/EI650/Working+with+Service+Tasks#WorkingwithServiceTasks-DeployingartifactsoftheMavenProject)
.

!!! tip

You can skip step 1 since you already added the dependencies above.


### Creating the Task

Follow the steps below to create the task and schedule it.

#### Creating the ESB Project

1.  Create a ESB Solution Project using the following information. For
    instructions, see [Creating the ESB
    Project](Scheduling-a-Task-Using-the-Default-Implementation_119130430.html#SchedulingaTaskUsingtheDefaultImplementation-CreatingtheESBProject)
    .

    -   **Name:** `            PrintStockQuote           `

    `                                                  `

    #### Creating the Sequence

2.  Create a Sequence using the following information. For instructions,
    see [Creating the
    Sequence](Scheduling-a-Task-Using-the-Default-Implementation_119130430.html#SchedulingaTaskUsingtheDefaultImplementation-CreatingtheSequence)
    .

    -   **Name:** `            PrintStockQuoteSequence           `  
        `                                                            `

3.  Add a **Log Mediator** and a **Drop Mediator** to the sequence and
    configure them. For instructions, see [Creating the
    Sequence](Scheduling-a-Task-Using-the-Default-Implementation_119130430.html#SchedulingaTaskUsingtheDefaultImplementation-CreatingtheSequence)
    .  
    ![](attachments/119130458/119130461.png)

    ![](images/icons/grey_arrow_down.png){.expand-control-image}
    Configuration of the Sequence

    The below is the complete source configuration of the Sequence
    (i.e., the `             PrintStockQuoteSequence.xml            `
    file).

    ``` xml
        <?xml version="1.0" encoding="UTF-8"?>
        <sequence name="PrintStockQuoteSequence" trace="disable" xmlns="http://ws.apache.org/ns/synapse">
            <log level="custom"/>
            <drop/>
        </sequence>
    ```

    #### Creating the Scheduled Task

4.  Create a Scheduled Task using the following information. For
    instructions, see [Creating the Scheduled
    Task](Scheduling-a-Task-Using-the-Default-Implementation_119130430.html#SchedulingaTaskUsingtheDefaultImplementation-CreatingtheScheduledTask)
    .

    -   **Task Name:**
        `            PrintStockQuoteScheduledTask           `
    -   **Count:** `            1           `
    -   I **nterval (in seconds):** 5  
        ![](attachments/119130458/119130460.png)

    #### Defining the properties of the Task

-   In the **Project Explorer** , double-click on the **Print
    StockQuoteScheduledTask.xml** file and replace its source with the
    below content.

    ``` xml
            <task class="org.apache.synapse.startup.tasks.MessageInjector" group="synapse.simple.quartz" name="PrintStockQuoteScheduledTask" xmlns="http://ws.apache.org/ns/synapse">
                <trigger count="1" interval="5" />
                <property name="to" value="http://localhost:9000/soap/SimpleStockQuoteService" xmlns:task="http://www.wso2.org/products/wso2commons/tasks" />
                <property name="stockFile" value="/Users/praneesha/Desktop/stockfile.txt" xmlns:task="http://www.wso2.org/products/wso2commons/tasks" />
                <property name="synapseEnvironment" value="" xmlns:task="http://www.wso2.org/products/wso2commons/tasks" />
            </task>
    ```

    The task properties will change according to the custom
    implementation. Therefore, you need to enter values for your custom
    properties. This sets the below properties.

    <table>
    <thead>
    <tr class="header">
    <th>Parameter Name</th>
    <th>Value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>to</td>
    <td><a href="http://localhost:9000/soap/SimpleStockQuoteService">http://localhost:9000/soap/SimpleStockQuoteService</a></td>
    </tr>
    <tr class="even">
    <td><pre><code>stockFile</code></pre></td>
    <td>The directory path to the <code>               stockfile.txt              </code> file.</td>
    </tr>
    <tr class="odd">
    <td><pre><code>synapseEnvironment</code></pre></td>
    <td>Do not enter a value. This will be used during runtime.</td>
    </tr>
    </tbody>
    </table>

        !!! note
    
        Currently, you cannot set properties of a custom task using the
        **Design View** due to a [known
        issue](https://github.com/wso2/product-ei/issues/2551) , which will
        be fixed in the future WSO2 EI versions.
    

    ![](images/icons/grey_arrow_down.png){.expand-control-image}
    Configuration of the Scheduled Task

    The below is the complete source configuration of the Sequence
    (i.e., the
    `             PrintStockQuoteScheduledTask.xml            ` file).

    ``` xml
        <?xml version="1.0" encoding="UTF-8"?>
        <task class="org.apache.synapse.startup.tasks.MessageInjector" group="synapse.simple.quartz" name="PrintStockQuoteScheduledTask" xmlns="http://ws.apache.org/ns/synapse">
            <trigger interval="3" />
            <property name="to" value="http://localhost:9000/soap/SimpleStockQuoteService" xmlns:task="http://www.wso2.org/products/wso2commons/tasks" />
            <property name="stockFile" value="/Users/praneesha/Desktop/stockfile.txt" xmlns:task="http://www.wso2.org/products/wso2commons/tasks" />
        </task>
    ```

### Deploying the Task

For instructions on deploying the Task, see [Deploying the
Task](Scheduling-a-Task-Using-the-Default-Implementation_119130430.html#SchedulingaTaskUsingtheDefaultImplementation-DeployingtheTask)
.

### Creating the text file

Create a text file named `         stockfile.txt        ` with the
following content and save it to a preferred location on your
machine. This will include the information to be read by the scheduled
task to pass to the backend service.

**stockfile.txt**

``` java
    IBM,100,120.50
    MSFT,200,70.25
    SUN,400,60.758
```

!!! info

Each line in the text file contains details for a stock order:

-   `          symbol         `
-   `          quantity         `
-   `          price         `

A task that is scheduled using this custom implementation will read the
text file, a line at a time, and create orders using the given values to
be sent to the back-end service. The text file will then be tagged as
processed to include a system time stamp. The task will be scheduled to
run every 15 seconds.


### Viewing the output

You will view the stock quotes sent by the backend service printed every
3 seconds by the scheduled task in the below format.

``` text
    INFO - StockQuoteTask placed order symbol:IBM quantity:100 price:120.50
```

![](attachments/119130458/119130459.png)
  
