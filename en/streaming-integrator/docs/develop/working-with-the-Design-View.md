# Working with the Design View

This section provides an overview of the design view of the Stream
Processor Studio.

-   [Accesing the Design
    View](#WorkingwiththeDesignView-AccesingtheDesignView)
-   [Adding Siddhi
    components](#WorkingwiththeDesignView-AddingSiddhicomponents)
-   [Connecting Siddhi
    components](#WorkingwiththeDesignView-ConnectingSiddhicomponents)
-   [Saving, running and debugging Siddhi
    applications](#WorkingwiththeDesignView-Saving,runninganddebuggingSiddhiapplications)

## Accesing the Design View

To open the design view of the Stream Processor Studio:

1.  Start the Stream Processor Studio and log in with your credentials.
    For detailed instructions, see [Stream Processor Studio Overview -
    Starting Stream Processor
    Studio](Stream-Processor-Studio-Overview_112390916.html#StreamProcessorStudioOverview-StartingStreamProcessorStudio)
    .
2.  Click **New** and open a new Siddhi file, or click **Open** and open
    an existing Siddhi file.
3.  Click **Design View** to open the Design View.  
    ![](attachments/112390944/112390975.png)  
    The design view opens as shown in the example below. It consists of
    a grid to which you can drag and drop the Siddhi components
    represented by icons displayed in the left panel to design a Siddhi
    application.  
    ![](attachments/112390944/112390985.png){width="790"}

## Adding Siddhi components

To add a Siddhi component to the Siddhi application that you are
creating/editing in the design view, click on the relevant icon in the
left pane, and then drag and drop it to the grid as demonstrated in the
example below.

![](attachments/112390944/119112441.gif){width="791"}

Once you add a Siddhi component, you can configure it as required. To
configure a Siddhi component, click the settings icon on the component.
This opens a form with parameters related to the relevant component.

![](attachments/112390944/119112466.png){width="100"}

The following is the complete list of Siddhi components that you can add
to the grid of the design view when you create a Siddhi application.

### Stream

<table>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390952.png" width="78" height="78" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>A stream represents a logical series of events ordered in time. For a detailed description of streams, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#stream">Siddhi Query Guide - Stream</a> .</td>
</tr>
<tr class="odd">
<td>Form</td>
<td><div class="content-wrapper">
<p>To configure the stream, click the settings icon on the stream component you added to the grid. Then enter values as follows:</p>
<p><strong>Stream Name</strong> <strong>:</strong> A unique name for the stream. This should be specified in title caps, and without spaces (e.g., <code>               ProductionDataStream              </code> ).</p>
<p><strong>Attributes</strong> : Attributes of streams are specified as name and type pairs in the <strong>Attributes</strong> table.</p>
</div></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390955.png" width="706" /></p>
<p>The details entered in the above <strong></strong> form creates a stream configuration as follows:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a>define stream SweetProductionStream (amount <span class="dt">double</span>, name string);</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td><ul>
<li>Sources</li>
<li>Projection queries</li>
<li>Filter queries</li>
<li>Window queries</li>
<li>Join queries</li>
</ul></td>
</tr>
<tr class="even">
<td>Target</td>
<td><ul>
<li>Sinks</li>
<li>Projection queries</li>
<li>Filter queries</li>
<li>Window queries</li>
<li>Join queries</li>
</ul></td>
</tr>
</tbody>
</table>

  

### Source

<table>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390954.png" width="78" height="78" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>A source receives events in the specified transport and in the specified format. For more information, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#source">Siddhi Query Guide - Source</a> .</td>
</tr>
<tr class="odd">
<td>Form</td>
<td><div class="content-wrapper">
<p>To configure the source, click the settings icon on the source component you added to the grid. This opens a form where you can enter the following information:<br />
</p>
!!! info
<p>To access the form in which you can configure a source, you must first connect the source as the source (input) object to a stream component.</p>

<p><br />
</p>
<ul>
<li><strong>Source Type</strong> : This specifies the transport type via which the events are received. The value should be entered in lower case (e.g., t <code>                cp               </code> ). The other parameters displayed for the source depends on the source type selected.</li>
<li><strong>Map Type</strong> : This specifies the format in which you want to receive the events (e.g., <code>                xml               </code> ). The other parameters displayed for the map depends on the map type selected. If you want to add more configurations to the mapping, click <strong>Customized Options</strong> and set the required properties and key value pairs.<br />
</li>
<li><p><strong>Map Attribute as Key/Value Pairs</strong> : If this check box is selected, you can define custom mapping by entering key value pairs. You can add as many key value pairs as required under this check box.</p></li>
</ul>
</div></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/119112474.png" width="900" /></p>
<p>The details entered in the above <strong></strong> form creates a source configuration as follows:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a><span class="at">@source</span>(type = &#39;tcp&#39;, </span>
<span id="cb1-2"><a href="#cb1-2"></a>    <span class="at">@map</span>(type = &#39;json&#39;, </span>
<span id="cb1-3"><a href="#cb1-3"></a>        <span class="at">@attributes</span>(name = <span class="st">&quot;$.sweet&quot;</span>, amount = <span class="st">&quot;$.batch.count&quot;</span>)))</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td>No connection can start from another Siddhi component and link to a source because a source is the point from which events selected into the event flow of the Siddhi application start.</td>
</tr>
<tr class="even">
<td>Target</td>
<td><p>Streams</p></td>
</tr>
</tbody>
</table>

  

### Sink

<table>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390956.png" width="77" height="78" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>A sink publishes events via the specified transport and in the specified format. For more information, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#sink">Siddhi Query Guide - Sink</a> .</td>
</tr>
<tr class="odd">
<td>Form</td>
<td><div class="content-wrapper">
<p>To configure the sink, click the settings icon on the sink component you added to the grid.<br />
</p>
!!! info
<p>To access the form in which you can configure a sink, you must first connect the sinke as the target object to a stream component.</p>

<ul>
<li><strong>Sink Type</strong> : This specifies the transport via which the sink publishes processed events. The value should be entered in lower case (e.g., <code>                log               </code> ).<br />
</li>
<li><strong>Map Type</strong> : This specifies the format in which you want to publish the events (e.g., <code>                passThrough               </code> ). The other parameters displayed for the map depends on the map type selected. If you want to add more configurations to the mapping, click <strong>Customized Options</strong> and set the required properties and key value pairs.</li>
<li><p><strong>Map Attribute as Key/Value Pairs</strong> : If this check box is selected, you can define custom mapping by entering key value pairs. You can add as many key value pairs as required under this check box.<br />
</p></li>
</ul>
</div></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390959.png" width="900" /></p>
<p>The details entered in the above <strong></strong> form creates a sink configuration as follows:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a><span class="ot">@sink(type = &#39;log&#39;, prefix = &quot;Sweet Totals:&quot;)</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td>Streams</td>
</tr>
<tr class="even">
<td>Target</td>
<td><p>N/A</p>
<p>A sink cannot be followed by another Siddhi component because it represents the last stage of the event flow where the results of the processing carried out by the Siddhi application are communicated via the required interface.</p></td>
</tr>
</tbody>
</table>

### Table

<table>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390949.png" width="77" height="77" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>A table is a stored version of an stream or a table of events. For more information, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#table">Siddhi Query Guide - Table</a> .</td>
</tr>
<tr class="odd">
<td>Form</td>
<td><p>To configure the table, click the settings icon on the table component you added to the grid.</p>
<p><strong>Name</strong> <strong>:</strong> This field specifies unique name for the table. This should be specified in title caps, and without spaces (e.g., <code>              ProductionDataTable             </code> ).</p>
<p><strong>Attributes</strong> : Attributes of tables are specified as name and type pairs in the <strong>Attributes</strong> table. To add a new attribute, click <strong>+Attribute</strong> .</p>
<p><strong>Store Type</strong> : This specifies the specific database type in which you want to stopre data or whether the data is to be stored in-memory. Once the store type is selected, select an option to indicate whether the datastore needs to be defined inline, whether you want to use a datasource defined in the <code>              &lt;SP_HOME&gt;/conf/worker/deployment.yaml             </code> file, or connected to a JNDI resource. For more information, see <a href="https://docs.wso2.com/display/SP440/Defining+Tables+for+Physical+Stores">Defining Tables for Physical Stores</a> . The other parameters configured under <strong>Store Type</strong> depend on the store type you select.</p>
<p><strong>Annotations</strong> : This section allows you to specify the table attributes you want to use as the primary key and indexes via the <code>              @primarykey             </code> and <code>              @index             </code> annotations. For more information, see <a href="https://docs.wso2.com/display/SP440/Defining+Data+Tables">Defining Data Tables</a> . If you want to add any other custom annotations to your table definition, click <strong>+Annotation</strong> to define them.</p></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390953.png" height="250" /></p>
<p>The details entered in the above <strong></strong> form creates a table definition as follows:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a><span class="ot">@store(type = &#39;rdbms&#39;, datasource = &quot;SweetProductionDB&quot;)</span></span>
<span id="cb1-2"><a href="#cb1-2"></a>define <span class="kw">table</span> ShipmentDetails (name string, supplier string, amount <span class="dt">double</span>);</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td><ul>
<li>Projection queries</li>
<li>Window queries</li>
<li>Filter queries</li>
<li>Join queries</li>
</ul></td>
</tr>
<tr class="even">
<td>Target</td>
<td><ul>
<li>Projection queries</li>
<li>Window queries</li>
<li>Filter queries</li>
<li>Join queries</li>
</ul></td>
</tr>
</tbody>
</table>

### Window

<table>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390947.png" width="78" height="77" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This icon represents a window definition that can be shared across multiple queries. For more information, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#defined-window">Siddhi Query Guide - (Defined) Window</a> .</td>
</tr>
<tr class="odd">
<td>Form</td>
<td><p>To configure the window, <a href="#WorkingwiththeDesignView-Settings">click the settings icon</a> on the window component you added to the grid, and update the following information.</p>
<ul>
<li><strong>Name</strong> : This field specifies a unique name for the window. <code>               PascalCase              </code> is used for window names as a convention.</li>
<li><strong>Attributes</strong> : Attributes of windows are specified as name and type pairs in the <strong>Attributes</strong> table.</li>
<li><strong>Window Type</strong> : This specifies the function of the window (i.e., the window type such as <code>               time              </code> , <code>               length              </code> , <code>               frequent              </code> etc.). The window types supported include <a href="https://siddhi-io.github.io/siddhi/api/latest/#time-window">time</a> , <a href="https://siddhi-io.github.io/siddhi/api/latest/#timebatch-window">timeBatch</a> , <a href="https://siddhi-io.github.io/siddhi/api/latest/#timelength-window">timeLength</a> , <a href="https://siddhi-io.github.io/siddhi/api/latest/#length-window">length</a> , <a href="https://siddhi-io.github.io/siddhi/api/latest/#lengthbatch-window">lengthBatch</a> , <a href="https://siddhi-io.github.io/siddhi/api/latest/#sort-window">sort</a> , <a href="https://siddhi-io.github.io/siddhi/api/latest/#frequent-window">frequent</a> , <a href="https://siddhi-io.github.io/siddhi/api/latest/#lossyfrequent-window">lossyFrequent</a> , <a href="https://siddhi-io.github.io/siddhi/api/latest/#cron-window">cron</a> , <a href="https://siddhi-io.github.io/siddhi/api/latest/#externaltime-window">externalTime</a> , <a href="https://siddhi-io.github.io/siddhi/api/latest/#externaltimebatch-window">externalTimeBatch</a> .</li>
<li><strong>Parameters</strong> : This section allows you to define one or more parameters for the window definition based on the window type you entered in the <strong>Window Type</strong> field.</li>
<li><strong>Annotations</strong> : If you want to add any other custom annotations to your window definition, click <strong>+Annotation</strong> to define them.</li>
</ul></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390946.png" width="900" /></p>
<p>The details entered in the above form creates a window definition as follows:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a>define window FiveMinTempWindow (roomNo <span class="dt">int</span>, temp <span class="dt">double</span>) <span class="dt">time</span>(<span class="dv">5</span> <span class="fu">min</span>) output <span class="kw">all</span> <span class="kw">events</span>;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td><ul>
<li>Projection queries</li>
<li>Window queries</li>
<li>Filter queries</li>
<li>Join queries</li>
</ul></td>
</tr>
<tr class="even">
<td>Target</td>
<td><ul>
<li>Projection queries</li>
<li>Window queries</li>
<li>Filter queries</li>
<li>Join queries</li>
</ul></td>
</tr>
</tbody>
</table>

### Trigger

<table>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390951.png" width="76" height="78" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>A trigger allows you to generate events periodically. For more information, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#trigger">Siddhi Query Guide - Trigger</a> .</td>
</tr>
<tr class="odd">
<td>Form</td>
<td><div class="content-wrapper">
<p>To configure the trigger, <a href="#WorkingwiththeDesignView-Settings">click the settings icon</a> on the trigger component you added to the grid, and update the following information.</p>
<ul>
<li><strong>Name</strong> : A unique name for the trigger.</li>
<li><strong>Trigger Criteria</strong> : This specifies the criteria based on which the trigger is activated. Possible values are as follows:
<ul>
<li><strong>start</strong> : Select this to trigger events when the SP server has started.</li>
<li><strong>every</strong> : Select this to specify a time interval at which events should be triggered.</li>
<li><strong>cron-expression</strong> : Select this to enter a cron expression based on which the events can be triggered. For more information about cron expressions, see the <a href="http://www.quartz-scheduler.org/documentation/quartz-2.2.x/tutorials/tutorial-lesson-06">quartz-scheduler</a> .</li>
</ul></li>
</ul>
</div></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><br />
</p>
<img src="attachments/112390944/112390950.png" width="461" />
<p>The details entered in the above orm creates a trigger definition as follows:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a>define <span class="kw">trigger</span> FiveMinTriggerStream <span class="kw">at</span> every <span class="dv">5</span> <span class="fu">min</span>;</span></code></pre></div>
</div>
</div>
<p><br />
</p>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td>N/A</td>
</tr>
<tr class="even">
<td>Target</td>
<td><ul>
<li>Projection queries</li>
<li>Window queries</li>
<li>Filter queries</li>
<li>Join queries</li>
</ul></td>
</tr>
</tbody>
</table>

### Aggregation

<table>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390988.png" width="77" height="76" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><div class="content-wrapper">
<p>Incremental aggregation allows you to obtain aggregates in an incremental manner for a specified set of time periods. For more information, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#incremental-aggregation">Siddhi Query Guide - Incremental Aggregation</a> .</p>
!!! tip
<p>Before you add an aggregation:</p>
<p>Make sure that you have already added the stream with the events to which the aggregation is applied is already defined.</p>

</div></td>
</tr>
<tr class="odd">
<td>Form</td>
<td><div class="content-wrapper">
<p>To configure the aggregation, <a href="#WorkingwiththeDesignView-Settings">click the settings icon</a> on the aggregation component you added to the grid, and update the following information.</p>
<ul>
<li><strong>Aggregation Meta Information</strong> : In this section, define a unique name for the aggregation in the <strong>Name</strong> field, and specify the stream from which the input information is taken to perform the aggregations. You can also select the optional annotations you want to use in the aggregation definition by selecting the relevant check boxes. For more information about configuring the annotations once you select them, see <a href="https://docs.wso2.com/display/SP440/Incremental+Analysis#IncrementalAnalysis-annotation">Incremental Analysis</a> .</li>
<li><strong>Projection</strong> : This section specifies the attributes to be included in the aggregation query. In the <strong>Select</strong> field, you can select <strong>All</strong> attributes to perform the aggregation for all the attributes of the stream specified under <strong>Input</strong> , or select <strong>User Defined Attributes</strong> to select specific attributes. If you select <strong>User Defined Attributes</strong> , you can add attributes to be selected to be inserted into the output stream. Here, you can enter the names of specific attributes in the input stream, or enter expressions to convert input stream attribute values as required to generate output events. You can also specify the attribute(s) by which you want to group the output.</li>
<li><strong>Aggregation Criteria</strong> : Here, you can specify the time values based on which the aggregates are calculated.</li>
</ul>
</div></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390990.png" width="900" /></p>
<p>The details entered in the above form creates an aggregation definition as follows:<br />
</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a>define aggregation TradeAggregation</span>
<span id="cb1-2"><a href="#cb1-2"></a><span class="kw">from</span> TradeStream</span>
<span id="cb1-3"><a href="#cb1-3"></a><span class="kw">select</span> symbol, <span class="fu">avg</span>(price) <span class="kw">as</span> avgPrice, <span class="fu">sum</span>(price) <span class="kw">as</span> total</span>
<span id="cb1-4"><a href="#cb1-4"></a>    <span class="kw">group</span> <span class="kw">by</span> symbol</span>
<span id="cb1-5"><a href="#cb1-5"></a>    aggregate <span class="kw">by</span> <span class="dt">timestamp</span> every seconds<span class="op">..</span>.years;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td>N/A</td>
</tr>
<tr class="even">
<td>Target</td>
<td>Join queries</td>
</tr>
</tbody>
</table>

### Function

<table>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390979.png" width="77" height="78" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>The function icon represents <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#script">Script in Siddhi Query Language</a> . It allows you to write functions in other programming languages and execute them within Siddhi queries. A function component in a Siddhi application is not connected to ther Siddhi components in the design UI. However, the configuration of one or more Query components can include a reference to it.</td>
</tr>
<tr class="odd">
<td>Form</td>
<td><p>To configure the function, <a href="#WorkingwiththeDesignView-Settings">click the settings icon</a> on the function component you added to the grid, and update the following information.</p>
<ul>
<li><strong>Name</strong> : A unique name for the function.</li>
<li><strong>Script Type</strong> : The language in which the function is written.</li>
<li><strong>Return Value</strong> : The data format of the value that is generated as the output via the function.<br />
</li>
<li><strong>Script Body</strong> : This is a free text field to write the function in the specified script type.<br />
</li>
</ul></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/119112626.png" width="800" /></p>
<p>The details entered in the above form creates a function definition as follows:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a>define <span class="kw">function</span> concatFN[JAVASCRIPT] <span class="kw">return</span> string {</span>
<span id="cb1-2"><a href="#cb1-2"></a>    var str1 <span class="op">=</span> <span class="kw">data</span>[<span class="dv">0</span>];</span>
<span id="cb1-3"><a href="#cb1-3"></a>    var str2 <span class="op">=</span> <span class="kw">data</span>[<span class="dv">1</span>];</span>
<span id="cb1-4"><a href="#cb1-4"></a>    var str3 <span class="op">=</span> <span class="kw">data</span>[<span class="dv">2</span>];</span>
<span id="cb1-5"><a href="#cb1-5"></a>    var responce <span class="op">=</span> str1 <span class="op">+</span> str2 <span class="op">+</span> str3;</span>
<span id="cb1-6"><a href="#cb1-6"></a>    <span class="kw">return</span> responce;</span>
<span id="cb1-7"><a href="#cb1-7"></a>};</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

### Projection Query

<table>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390967.png" width="78" height="76" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><div class="content-wrapper">
!!! tip
<p>Before you add a projection query:</p>
<p>You need to add and configure the following:</p>
<ul>
<li>The input stream with the events to be processed by the query.</li>
<li>The output stream to which the events processed by the query are directed.</li>
</ul>

<p>This icon represents a query to project the events in an input stream to an output stream. This invoves selectng the attributes to be included in the output, renaming attributes, introducing constant values, and using mathematical and/or logical expressions. For more information, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#query-projection">Siddhi Query Guide - Query Projection</a> .</p>
</div></td>
</tr>
<tr class="odd">
<td>Form</td>
<td><div class="content-wrapper">
<p>Once you connect the query to an input stream (source) and an output stream (target), you can configure it. To configure the projection query, <a href="#WorkingwiththeDesignView-Settings">click the settings icon</a> on the projection query component you added to the grid, and update the following information.</p>
<ul>
<li><strong>Query Meta Information</strong> : This section specifies the stream to be considered as the input stream with the events to which the query needs to be applied. The input stream connected to the query as the source is automatically displayed.</li>
<li><strong>Projection</strong> : This section specifies the attributes to be included in the output. In the <strong>Select</strong> field, you can select <strong>All Attributes</strong> to select all the attributes of the events, or select <strong>User Defined Attributes</strong> to select specific attributes from the input stream. If you select <strong>User Defined Attributes</strong> , you can add attributes to be selected to be inserted into the output stream. Here, you can enter the names of specific attributes in the input stream, or enter expressions to convert input stream attribute values as required to generate output events. You can also specify the attribute(s) by which you want to group the output.</li>
<li><strong>Output</strong> : This section specifies the action to be performed on the output event. The fields to be configured in this section are as follows:
<ul>
<li><strong>Operation</strong> : This field specifies the operation to be performed on the generated output event (e.g., <code>                  Insert                 </code> to insert events to a selected stream/table/window).</li>
<li><strong>Into</strong> : This field specifies the stream/table/window in which the operation specified need to be performed.</li>
<li><strong>Event Type</strong> This field specifies whether the operation needs to be performed for all output events, only current events or for only expired events.|</li>
</ul></li>
</ul>
</div></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390963.png" width="900" /></p>
<p>The details entered in the above form creates a query as follows:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">from</span> TradeStream</span>
<span id="cb1-2"><a href="#cb1-2"></a><span class="kw">select</span> symbol, <span class="fu">avg</span>(price) <span class="kw">as</span> averagePrice, <span class="fu">sum</span>(volume) <span class="kw">as</span> total</span>
<span id="cb1-3"><a href="#cb1-3"></a><span class="kw">insert</span> <span class="kw">all</span> <span class="kw">events</span> <span class="kw">into</span> OutputStream;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td><ul>
<li>Streams</li>
<li>Tables</li>
<li>Triggers</li>
<li>Windows</li>
</ul></td>
</tr>
<tr class="even">
<td>Target</td>
<td><ul>
<li>Streams</li>
<li>Tables</li>
<li>Windows</li>
</ul></td>
</tr>
</tbody>
</table>

### Filter Query

<table>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390981.png" width="76" height="77" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><div class="content-wrapper">
!!! tip
<p>Before you add a filter query:</p>
<p>You need to add and configure the following:</p>
<ul>
<li>The input stream with the events to be processed by the query.</li>
<li>The output stream to which the events processed by the query are directed.</li>
</ul>

<p>A filter query filters information in an input stream based on a given condition. For more information, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#filter">Siddhi Query Guide - Filters</a> .</p>
</div></td>
</tr>
<tr class="odd">
<td>Form</td>
<td><div class="content-wrapper">
<p>Once you connect the query to an input stream (source) and an output stream (target), you can configure it. To configure the filter query, <a href="#WorkingwiththeDesignView-Settings">click the settings icon</a> on the filter query component you added to the grid, and update the following information.</p>
<ul>
<li><p>By default, the <strong>Stream Handler</strong> check box is selected, and a stream handler of the <code>                 filter                </code> type is available under it to indicate that the query is a filter. Expand this stream handler, and enter the condition based on which the information needs to be filtered.</p>
<p>!!! info</p>
<p>A Siddhi application can have multiple stream handlers. To add another stream handler, click the <strong>+ Stream Handler</strong> . Multiple functions, filters and windows can be defined within the same form as stream handlers.</p>
</p></li>
<li><strong>Projection</strong> : This section specifies the attributes to be included in the output. In the <strong>Select</strong> field, you can select <strong>All Attributes</strong> to select all the attributes of the events, or select <strong>User Defined Attributes</strong> to select specific attributes from the input stream. If you select <strong>User Defined Attributes</strong> , you can add attributes to be selected to be inserted into the output stream. Here, you can enter the names of specific attributes in the input stream, or enter expressions to convert input stream attribute values as required to generate output events. You can also specify the attribute(s) by which you want to group the output.</li>
<li><strong>Output</strong> : This section specifies the action to be performed on the output event. The fields to be configured in this section are as follows:
<ul>
<li><strong>Operation</strong> : This field specifies the operation to be performed on the generated output event (e.g., <code>                  Insert                 </code> to insert events to a selected stream/table/window).</li>
<li><strong>Into</strong> : This field specifies the stream/table/window in which the operation specified need to be performed.</li>
<li><strong>Event Type</strong> This field specifies whether the operation needs to be performed for all output events, only current events or for only expired events.</li>
</ul></li>
</ul>
</div></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390983.png" width="900" /></p>
<p>The details entered in the above form creates a query with a filter as follows:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">from</span> TradeStream[<span class="fu">sum</span>(amount) <span class="op">&gt;</span> <span class="dv">10000</span>]</span>
<span id="cb1-2"><a href="#cb1-2"></a><span class="kw">select</span> symbol, <span class="fu">avg</span>(price) <span class="kw">as</span> averagePrice, <span class="fu">sum</span>(amount) <span class="kw">as</span> total</span>
<span id="cb1-3"><a href="#cb1-3"></a><span class="kw">insert</span> <span class="kw">all</span> <span class="kw">events</span> <span class="kw">into</span> OutputStream;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td><ul>
<li>Streams</li>
<li>Tables</li>
<li>Triggers</li>
<li>Windows</li>
</ul></td>
</tr>
<tr class="even">
<td>Target</td>
<td><ul>
<li>Streams</li>
<li>Tables</li>
<li>Windows</li>
</ul></td>
</tr>
</tbody>
</table>

### Window Query

<table>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390948.png" width="76" height="77" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><div class="content-wrapper">
!!! tip
<p>Before you add a window query:</p>
<p>You need to add and configure the following:</p>
<ul>
<li>The input stream with the events to be processed by the query.</li>
<li>The output stream to which the events processed by the query are directed.</li>
</ul>

<p>Window queries include a window to select a subset of events to be processed based on a specific criterion. For more information, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#defined-window">Siddhi Query Guide - (Defined) Window</a> .</p>
</div></td>
</tr>
<tr class="odd">
<td>Form</td>
<td><div class="content-wrapper">
<p>Once you connect the query to an input stream (source) and an output stream (target), you can configure it. To configure the window query, <a href="#WorkingwiththeDesignView-Settings">click the settings icon</a> on the window query component you added to the grid, and update the following information.</p>
<ul>
<li><p>By default, the <strong>Stream Handler</strong> check box is selected, and a stream handler of the <code>                 window                </code> type is available under it to indicate that the query is a filter. Expand this stream handler, and enter details to determine the window including the window type and the basis on which the subset of events considered by the window is determined (i.e., based on the window type selected).</p>
<p>!!! info</p>
<p>A Siddhi application can have multiple stream handlers. To add another stream handler, click the <strong>+ Stream Handler</strong> . Multiple functions, filters and windows can be defined within the same form as stream handlers.</p>
</p></li>
<li><strong>Projection</strong> : This section specifies the attributes to be included in the output. In the <strong>Select</strong> field, you can select <strong>All Attributes</strong> to select all the attributes of the events, or select <strong>User Defined Attributes</strong> to select specific attributes from the input stream. If you select <strong>User Defined Attributes</strong> , you can add attributes to be selected to be inserted into the output stream. Here, you can enter the names of specific attributes in the input stream, or enter expressions to convert input stream attribute values as required to generate output events. You can also specify the attribute(s) by which you want to group the output.</li>
<li><strong>Output</strong> : This section specifies the action to be performed on the output event. The fields to be configured in this section are as follows:
<ul>
<li><strong>Operation</strong> : This field specifies the operation to be performed on the generated output event (e.g., <code>                  Insert                 </code> to insert events to a selected stream/table/window).</li>
<li><strong>Into</strong> : This field specifies the stream/table/window in which the operation specified need to be performed.</li>
<li><strong>Event Type</strong> This field specifies whether the operation needs to be performed for all output events, only current events or for only expired events.</li>
</ul></li>
</ul>
</div></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390983.png" width="900" /></p>
<p>The details entered in the above <strong>Query Configuration</strong> form creates a query with a window as follows:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">from</span> TradeStream#window.<span class="dt">time</span>(<span class="dv">1</span> <span class="dt">month</span>)</span>
<span id="cb1-2"><a href="#cb1-2"></a><span class="kw">select</span> symbol, <span class="fu">avg</span>(price) <span class="kw">as</span> averagePrice, <span class="fu">sum</span>(amount) <span class="kw">as</span> total</span>
<span id="cb1-3"><a href="#cb1-3"></a><span class="kw">insert</span> <span class="kw">all</span> <span class="kw">events</span> <span class="kw">into</span> OutputStream;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td><div class="content-wrapper">
!!! info
<p>A window query can have only one source at a given time.</p>

<ul>
<li>Streams</li>
<li>Tables</li>
<li>Triggers</li>
<li>Windows</li>
</ul>
</div></td>
</tr>
<tr class="even">
<td>Target</td>
<td><ul>
<li>Streams</li>
<li>Tables</li>
<li>Windows</li>
</ul></td>
</tr>
</tbody>
</table>

### Join Query

<table>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390976.png" width="76" height="76" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>A join query derives a combined result from two streams in real-time based on a specified condition. For more information, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#join-stream">Siddhi Query Guide - Join</a> .</td>
</tr>
<tr class="odd">
<td>Form</td>
<td><div class="content-wrapper">
<p>Once you connect two Siddhi components to the join query as sources and another Siddhi component as the target, you can configure the join query. To configure the join query, <a href="#WorkingwiththeDesignView-Settings">click the settings icon</a> on the join query component you added to the grid and update the following information.</p>
<ul>
<li><strong>Query Meta Information</strong> : In this section, enter a unique name for the query and any annotations that you want to include in the query. The <code>                @dist               </code> annotation is supported by default to use the query in a fully distributed deployment if required (for more information, see <a href="https://docs.wso2.com/display/SP440/Converting+to+a+Distributed+Streaming+Application">Converting to a Distributed Streaming Application</a> ). You can also add customized annotations.<br />
</li>
<li><strong>Input</strong> : Here, you can specify the input sources, the references, the join type, join condition, and stream handlers for the left source and right source of the join. For a detailed explanation of the join concept, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#join-stream">Siddhi Query Guide - Joins</a> .</li>
<li><strong>Projection</strong> : This section specifies the attributes to be included in the output. In the <strong>Select</strong> field, you can select <strong>All Attributes</strong> to select all the attributes of the events, or select <strong>User Defined Attributes</strong> to select specific attributes from the input stream. If you select <strong>User Defined Attributes</strong> , you can add attributes to be selected to be inserted into the output stream. Here, you can enter the names of specific attributes in the input stream, or enter expressions to convert input stream attribute values as required to generate output events. You can also specify the attribute(s) by which you want to group the output.</li>
<li><strong>Output</strong> : This section specifies the action to be performed on the output event. The fields to be configured in this section are as follows:
<ul>
<li><strong>Operation</strong> : This field specifies the operation to be performed on the generated output event (e.g., <code>                  Insert                 </code> to insert events to a selected stream/table/window).</li>
<li><strong>Into</strong> : This field specifies the stream/table/window in which the operation specified need to be performed.</li>
<li><strong>Event Type</strong> This field specifies whether the operation needs to be performed for all output events, only current events or for only expired events.</li>
</ul></li>
</ul>
</div></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p>A join query is configured as follows:</p>
<p><strong><img src="attachments/112390944/119115456.png" width="900" /><br />
</strong> The above configurations result in creating the following join query.</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">from</span> TempStream[temp <span class="op">&gt;</span> <span class="fl">30.0</span>]#window.<span class="dt">time</span>(<span class="dv">1</span> <span class="fu">min</span>) <span class="kw">as</span> T</span>
<span id="cb1-2"><a href="#cb1-2"></a>  <span class="kw">join</span> RegulatorStream[isOn <span class="op">==</span> <span class="kw">false</span>]#window.<span class="fu">length</span>(<span class="dv">1</span>) <span class="kw">as</span> R</span>
<span id="cb1-3"><a href="#cb1-3"></a>  <span class="kw">on</span> T.roomNo <span class="op">==</span> R.roomNo</span>
<span id="cb1-4"><a href="#cb1-4"></a><span class="kw">select</span> T.roomNo, R.deviceID, <span class="st">&#39;start&#39;</span> <span class="kw">as</span> action</span>
<span id="cb1-5"><a href="#cb1-5"></a><span class="kw">insert</span> <span class="kw">into</span> RegulatorActionStream;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td><div class="content-wrapper">
!!! info
<p>A join query must always be connected to two sources, and at least one of them must be a defined stream/trigger/window.</p>

<ul>
<li>Streams</li>
<li>Tables</li>
<li>Aggregations</li>
<li>Windows</li>
</ul>
</div></td>
</tr>
<tr class="even">
<td>Target</td>
<td><div class="content-wrapper">
!!! info
<p>A join query must always be connected to a single target.</p>

<ul>
<li>Streams</li>
<li>Tables</li>
<li>Windows</li>
</ul>
</div></td>
</tr>
</tbody>
</table>

### Pattern Query

<table style="width:100%;">
<colgroup>
<col style="width: 7%" />
<col style="width: 92%" />
</colgroup>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390965.png" width="76" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><div class="content-wrapper">
!!! tip
<p>Before you add a pattern query:</p>
<p>You need to add and configure the following:</p>
<ul>
<li>The input stream with the events to be processed by the query.</li>
<li>The output stream to which the events processed by the query are directed.</li>
</ul>

<p>A pattern query detects patterns in events that arrive overtime. For more information, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#pattern">Siddhi Query Guide - Patterns</a> .</p>
</div></td>
</tr>
<tr class="odd">
<td>Form</td>
<td><div class="content-wrapper">
<p>Once you connect the query to an input stream (source) and an output stream (target), you can configure it. To configure the pattern query, <a href="#WorkingwiththeDesignView-Settings">click the settings icon</a> on the pattern query component you added to the grid and update the following information.<br />
</p>
<ul>
<li><strong>Query Meta Information</strong> : In this section, enter a unique name for the query and any annotations that you want to include in the query. The <code>                @dist               </code> annotation is supported by default to use the query in a fully distributed deployment if required (for more information, see <a href="https://docs.wso2.com/display/SP440/Converting+to+a+Distributed+Streaming+Application">Converting to a Distributed Streaming Application</a> ). You can also add customized annotations.</li>
<li><strong>Input</strong> : This section defines the conditions based on which patterns are identified. This involves specifying a unique ID and the input stream considered for each condition. Multiple conditions can be added. Each condition is configured in a separate tab within this section. For more information about the Pattern concept, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#pattern">Siddhi Query Guide - Patterns</a> .</li>
<li><strong>Projection</strong> : This section specifies the attributes to be included in the output. In the <strong>Select</strong> field, you can select <strong>All Attributes</strong> to select all the attributes of the events, or select <strong>User Defined Attributes</strong> to select specific attributes from the input stream. If you select <strong>User Defined Attributes</strong> , you can add attributes to be selected to be inserted into the output stream. Here, you can enter the names of specific attributes in the input stream, or enter expressions to convert input stream attribute values as required to generate output events. You can also specify the attribute(s) by which you want to group the output.</li>
<li><strong>Output</strong> : This section specifies the action to be performed on the output event. The fields to be configured in this section are as follows:
<ul>
<li><strong>Operation</strong> : This field specifies the operation to be performed on the generated output event (e.g., <code>                  Insert                 </code> to insert events to a selected stream/table/window).</li>
<li><strong>Into</strong> : This field specifies the stream/table/window in which the operation specified need to be performed.</li>
<li><strong>Event Type</strong> This field specifies whether the operation needs to be performed for all output events, only current events or for only expired events.</li>
</ul></li>
</ul>
</div></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390969.png" width="900" /></p>
<p>The above configuration results in creating the following query.</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">from</span> every (e1<span class="op">=</span>MaterialSupplyStream) <span class="op">-&gt;</span> <span class="kw">not</span> MaterialConsumptionStream[name <span class="op">==</span> e1.name <span class="kw">and</span> amount <span class="op">==</span> e1.amount]</span>
<span id="cb1-2"><a href="#cb1-2"></a>    <span class="cf">for</span> <span class="dv">15</span> sec</span>
<span id="cb1-3"><a href="#cb1-3"></a><span class="kw">select</span> e1.name, e1.amount</span>
<span id="cb1-4"><a href="#cb1-4"></a><span class="kw">insert</span> <span class="kw">into</span> ProductionDelayAlertStream;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td><ul>
<li>Streams</li>
<li>Tables</li>
<li>Triggers</li>
<li>Windows</li>
</ul></td>
</tr>
<tr class="even">
<td>Target</td>
<td><ul>
<li>Streams</li>
<li>Tables</li>
<li>Windows</li>
</ul></td>
</tr>
</tbody>
</table>

  

### Sequence Query

<table style="width:100%;">
<colgroup>
<col style="width: 7%" />
<col style="width: 92%" />
</colgroup>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390961.png" width="76" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><div class="content-wrapper">
!!! tip
<p>Before you add a sequence query:</p>
<p>You need to add and configure the following:</p>
<ul>
<li>The input stream with the events to be processed by the query.</li>
<li>The output stream to which the events processed by the query are directed.</li>
</ul>

<p>A sequence query detects sequences in event occurrences over time. For more information, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#sequence">Siddhi Query Guide - Sequence</a> .</p>
</div></td>
</tr>
<tr class="odd">
<td>Form</td>
<td><div class="content-wrapper">
<p>Once you connect the query to an input stream (source) and an output stream (target), you can configure it. To configure the sequence query, <a href="#WorkingwiththeDesignView-Settings">click the settings icon</a> on the sequence query component you added to the grid and update the following information.</p>
<ul>
<li><strong>Query Meta Information</strong> : In this section, enter a unique name for the query and any annotations that you want to include in the query. The <code>                @dist               </code> annotation is supported by default to use the query in a fully distributed deployment if required (for more information, see <a href="https://docs.wso2.com/display/SP440/Converting+to+a+Distributed+Streaming+Application">Converting to a Distributed Streaming Application</a> ). You can also add customized annotations.</li>
<li><strong>Input</strong> : This section defines the conditions based on which sequences are identified. This involves specifying a unique ID and the input stream considered for each condition. Multiple conditions can be added. Each condition is configured in a separate tab within this section. For more information about the Sequence concept, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#sequence">Siddhi Query Guide - Sequences</a> .</li>
<li><strong>Projection</strong> : This section specifies the attributes to be included in the output. In the <strong>Select</strong> field, you can select <strong>All Attributes</strong> to select all the attributes of the events, or select <strong>User Defined Attributes</strong> to select specific attributes from the input stream. If you select <strong>User Defined Attributes</strong> , you can add attributes to be selected to be inserted into the output stream. Here, you can enter the names of specific attributes in the input stream, or enter expressions to convert input stream attribute values as required to generate output events. You can also specify the attribute(s) by which you want to group the output.</li>
<li><strong>Output</strong> : This section specifies the action to be performed on the output event. The fields to be configured in this section are as follows:
<ul>
<li><strong>Operation</strong> : This field specifies the operation to be performed on the generated output event (e.g., <code>                  Insert                 </code> to insert events to a selected stream/table/window).</li>
<li><strong>Into</strong> : This field specifies the stream/table/window in which the operation specified need to be performed.</li>
<li><strong>Event Type</strong> This field specifies whether the operation needs to be performed for all output events, only current events or for only expired events.</li>
</ul></li>
</ul>
</div></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390960.png" width="900" /></p>
<p>The above configuration results in creating the following query.</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">from</span> every e1<span class="op">=</span>SweetProductionStream, </span>
<span id="cb1-2"><a href="#cb1-2"></a>e2<span class="op">=</span>SweetProductionStream[e1.amount <span class="op">&gt;</span> amount <span class="kw">and</span> (<span class="dt">timestamp</span> <span class="op">-</span> e1.<span class="dt">timestamp</span>) <span class="op">&lt;</span> <span class="dv">10</span> <span class="op">*</span> <span class="dv">60000</span>]<span class="op">*</span>,</span>
<span id="cb1-3"><a href="#cb1-3"></a>e3<span class="op">=</span>SweetProductionStream[<span class="dt">timestamp</span> <span class="op">-</span> e1.<span class="dt">timestamp</span> <span class="op">&gt;</span> <span class="dv">10</span> <span class="op">*</span> <span class="dv">60000</span> <span class="kw">and</span> e1.amount <span class="op">&gt;</span> amount]</span>
<span id="cb1-4"><a href="#cb1-4"></a><span class="kw">select</span> e1.name, e1.amount <span class="kw">as</span> initialAmount, e2.amount <span class="kw">as</span> finalAmount, e2.<span class="dt">timestamp</span></span>
<span id="cb1-5"><a href="#cb1-5"></a><span class="kw">insert</span> <span class="kw">into</span> DecreasingTrendAlertStream;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td><ul>
<li>Streams</li>
<li>Tables</li>
<li>Triggers</li>
<li>Windows</li>
</ul></td>
</tr>
<tr class="even">
<td>Target</td>
<td><ul>
<li>Streams</li>
<li>Tables</li>
<li>Windows</li>
</ul></td>
</tr>
</tbody>
</table>

### Partitions

<table style="width:100%;">
<colgroup>
<col style="width: 7%" />
<col style="width: 92%" />
</colgroup>
<tbody>
<tr class="odd">
<td>Icon</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390970.png" width="76" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><div class="content-wrapper">
!!! tip
<p>Before you add a partition:</p>
<p>You need to add the stream to be partitioned.</p>

<p>Partitions divide streams and queries into isolated groups in order to process them in parallel and in isolation. For more information, see <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#partition">Siddhi Query Guide - Partition</a> .</p>
</div></td>
</tr>
<tr class="odd">
<td>Form</td>
<td><div class="content-wrapper">
<p>Once the stream to be partitioned is connected as a source to the partition, you can configure the partition. In order to do so, move the cursor over the partition and <a href="#WorkingwiththeDesignView-Settings">click the settings icon</a> on the partition component. This opens the <strong>Partition Configuration</strong> form. In this form, you can enter expressions to convert the attributes of the stream that is selected to be partitioned.</p>
</div></td>
</tr>
<tr class="even">
<td>Example</td>
<td><div class="content-wrapper">
<p><img src="attachments/112390944/112390972.png" height="250" /></p>
<p>The above configuration creates the following partition query.</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">partition</span> <span class="kw">with</span> ( roomNo <span class="op">&gt;=</span> <span class="dv">1030</span> <span class="kw">as</span> <span class="st">&#39;serverRoom&#39;</span> <span class="kw">or</span> </span>
<span id="cb1-2"><a href="#cb1-2"></a>                 roomNo <span class="op">&lt;</span> <span class="dv">1030</span> <span class="kw">and</span> roomNo <span class="op">&gt;=</span> <span class="dv">330</span> <span class="kw">as</span> <span class="st">&#39;officeRoom&#39;</span> <span class="kw">or</span> </span>
<span id="cb1-3"><a href="#cb1-3"></a>                 roomNo <span class="op">&lt;</span> <span class="dv">330</span> <span class="kw">as</span> <span class="st">&#39;lobby&#39;</span> <span class="kw">of</span> TempStream)</span>
<span id="cb1-4"><a href="#cb1-4"></a><span class="cf">begin</span></span>
<span id="cb1-5"><a href="#cb1-5"></a>    <span class="kw">from</span> TempStream#window.<span class="dt">time</span>(<span class="dv">10</span> <span class="fu">min</span>)</span>
<span id="cb1-6"><a href="#cb1-6"></a>    <span class="kw">select</span> roomNo, deviceID, <span class="fu">avg</span>(temp) <span class="kw">as</span> avgTemp</span>
<span id="cb1-7"><a href="#cb1-7"></a>    <span class="kw">insert</span> <span class="kw">into</span> AreaTempStream</span>
<span id="cb1-8"><a href="#cb1-8"></a><span class="cf">end</span>;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Source</td>
<td>Streams</td>
</tr>
<tr class="even">
<td>Target</td>
<td>N/A</td>
</tr>
</tbody>
</table>

  

## Connecting Siddhi components

In order to define how the Siddhi components in a Siddhi application
interact with each other to process events, you need to define
connections between Siddhi components. A connection is defined by
drawing an arrow from one component to another by dragging the cursor as
demonstrated below.

![](attachments/112390944/119112618.gif)

## Saving, running and debugging Siddhi applications

To save a Siddhi application that you created in the design view, you
need to switch to the source view. You also need to switch to the source
view to run or debug a Siddhi application. For more information, see the
following sections:

-   [Stream Processor Studio
    Overview](_Stream_Processor_Studio_Overview_)
-   [Debugging a Siddhi Application](_Debugging_a_Siddhi_Application_)
