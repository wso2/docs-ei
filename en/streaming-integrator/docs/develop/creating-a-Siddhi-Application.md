# Creating a Siddhi Application

Siddhi applications are files that define the Siddhi logic to process
the events sent to WSO2 SP. They are written in the [Siddhi Query
Language](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/)
using the [Stream Processor Studio
tool](_Stream_Processor_Studio_Overview_) shipped with WSO2 SP.

A Siddhi file contains the following configurations:

<table>
<thead>
<tr class="header">
<th>Configuration</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Stream</td>
<td>A logical series of events ordered in time with a uniquely identifiable name, and set of defined attributes with specific data types defining its schema.</td>
</tr>
<tr class="even">
<td>Source</td>
<td>This consumes data from external sources (such as <code>             TCP            </code> , <code>             Kafka            </code> , <code>             HTTP            </code> , etc) in the form of events, then converts each event (that can be in <code>             XML            </code> , <code>             JSON            </code> , <code>             binary            </code> , etc. format) to a Siddhi event, and passes that to a stream for processing.</td>
</tr>
<tr class="odd">
<td>Sink</td>
<td>This takes events arriving at a stream, maps them to a predefined data format (such as <code>             XML            </code> , <code>             JSON,            </code> <code>             binary            </code> , etc), and publishes them to external endpoints (such as <code>             E-mail            </code> , <code>             TCP            </code> , <code>             Kafka            </code> , <code>             HTTP            </code> , etc).</td>
</tr>
<tr class="even">
<td>Executional Element</td>
<td><p>An executional element can be one of the following:</p>
<ul>
<li>Stateless query: Queries that only consider currently incoming events when generating an output. e.g., filters</li>
<li>Stateful query: Queries that consider both currently incoming events as well as past events when generating an output. e.g., windows, sequences, patterns, etc.</li>
<li>Partitions: Collections of stream definitions and Siddhi queries separated from each other within a Siddhi application for the purpose of processing events in parallel and in isolation</li>
</ul></td>
</tr>
</tbody>
</table>

A Siddhi application can be created from the source view or the design
view of the WSO2 SP Stream Processor Studio.

### Creating a Siddhi application in the source view

To create a Siddhi application via the source view of the WSO2 SP Stream
Processor Studio, follow the steps below:

1.  Start WSO2 SP in the editor mode and access the Stream Processor
    Studio. For detailed instructions, see [Starting Stream Processor
    Studio](Stream-Processor-Studio-Overview_112390916.html#StreamProcessorStudioOverview-StartingStreamProcessorStudio)
    . The Stream Processor Studio opens as shown below.  
    ![](attachments/112390796/112390825.png){width="845"}
2.  Click **New** to start defining a new Siddhi application. A new file
    opens as shown below.  
    ![](attachments/112390796/112390836.png){height="250"}
3.  Add the following sample Siddhi application to the file.

    ``` sql
        @App:name("SweetProductionAnalysis")
    
        @Source(type = 'tcp', context='SweetProductionData', @map(type='binary'))
        define stream SweetProductionStream (name string, amount double);
    
        @sink(type='log', @map(type='json'))
        define stream ProductionAlertStream (name string, amount double);
    
        from SweetProductionStream
        select *
        insert into ProductionAlertStream;
    ```

        !!! info
    
        Note the following in this Siddhi application
    
        <table>
        <thead>
        <tr class="header">
        <th>Configuration</th>
        <th>Description</th>
        </tr>
        </thead>
        <tbody>
        <tr class="odd">
        <td>Stream</td>
        <td><div class="content-wrapper">
        <p>This stream contains two stream configurations:</p>
        <ul>
        <li><p><code>                   SweetProductionStream                  </code></p>
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb1-1"><a href="#cb1-1"></a>define stream SweetProductionStream (name string, amount <span class="dt">double</span>);</span></code></pre></div>
        </div>
        </div>
        <p>This is the input stream that defines the schema based on which events are selected to be processed by the <code>                   SweetProductionAnalysis                  </code> Siddhi application. Events received via the source in this application are directed to this stream.</p></li>
        <li><p><code>                   ProductionAlertStream                                     </code></p>
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb2-1"><a href="#cb2-1"></a>define stream ProductionAlertStream (name string, amount <span class="dt">double</span>);</span></code></pre></div>
        </div>
        </div>
        <p>This is the output stream from which the sink configured in this application takes events to be published as the output.</p></li>
        </ul>
        </div></td>
        </tr>
        <tr class="even">
        <td>Source</td>
        <td><div class="content-wrapper">
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb3" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb3-1"><a href="#cb3-1"></a><span class="ot">@Source(type = &#39;tcp&#39;, context=&#39;SweetProductionData&#39;, @map(type=&#39;binary&#39;))</span></span></code></pre></div>
        </div>
        </div>
        <p><br />
        This source configuration has the following sections:</p>
        <ul>
        <li><pre><code>@Source(type = ‘tcp’, context=&#39;SweetProductionData&#39;</code></pre>
        <p>This configuration defines <code>                   tcp                  </code> as the transport via which events are received to be processed by the <code>                   SweetProductionAnalysis                  </code> Siddhi application.</p></li>
        <li><pre><code>@map(type=&#39;binary&#39;))
                              </code></pre>
        <p>This configuration defines the input mapping. In this scenario, Binary Mapper is used which converts input events into binary events and feeds them into siddhi.</p></li>
        </ul>
        !!! tip
        <p>The source types and map types are available as Siddhi extensions, and you can find via the operator finder as follows:</p>
        <ol>
        <li><p>Click the <strong>Operator Finder</strong> icon to open the Operator Finder.</p>
        <p><img src="attachments/112390796/112390801.png" height="250" /></p></li>
        <li><p>Move the cursor to the location in the Siddhi application where you want to add the source.<br />
        <img src="attachments/112390796/112390800.png" height="250" /></p></li>
        <li><p>Search for the required transport type. Once it appears in the search results, click the <strong>Add to Source</strong> icon on it.<br />
        <img src="attachments/112390796/112390799.png" height="250" /></p></li>
        <li><p>Similarly, search for the mapping type you want to include in the source configuration, and add it.</p>
        <p><img src="attachments/112390796/112390798.png" width="376" /></p></li>
        <li><p>The source annotation is now displayed as follows. You can add the other properties as required, and save your changes.<br />
        <img src="attachments/112390796/112390797.png" height="250" /></p></li>
        </ol>

    </div></td>
    </tr>
    <tr class="odd">
    <td>Sink</td>
    <td><div class="content-wrapper">
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb6" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb6-1"><a href="#cb6-1"></a><span class="ot">@sink(type=&#39;log&#39;, @map(type=&#39;json&#39;))</span></span></code></pre></div>
    </div>
    </div>
    <p>This sink configuration has the following sections:</p>
    <ul>
    <li><pre><code>@sink(type=&#39;log&#39;)</code></pre>
    <p>This configuration defines <code>                   log                  </code> as the transport via which the processed events are published from the <code>                   ProductionAlertStream                  </code> output stream. Log sink simply publishes events into the console.</p></li>
    <li><pre><code>@map(type=&#39;json&#39;))</code></pre>
    <p>This configuration defines the output mapping. Events are published with the <code>                  json                 </code> mapping type. Json mapper converts the events in the <code>                  ProductionAlertStream                 </code> to the Json format.</p></li>
    </ul>
        !!! tip
        <p>You can select the sink type and the map type from the <strong>Operator Finder</strong> .</p>

    </div></td>
    </tr>
    <tr class="even">
    <td>Executional Elements</td>
    <td><div class="content-wrapper">
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb9" data-syntaxhighlighter-params="brush: sql; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: sql; gutter: false; theme: Confluence"><pre class="sourceCode sql"><code class="sourceCode sql"><span id="cb9-1"><a href="#cb9-1"></a><span class="kw">from</span> SweetProductionStream</span>
    <span id="cb9-2"><a href="#cb9-2"></a><span class="kw">select</span> <span class="op">*</span></span>
    <span id="cb9-3"><a href="#cb9-3"></a><span class="kw">insert</span> <span class="kw">into</span> ProductionAlertStream;</span></code></pre></div>
    </div>
    </div>
    <p>This is where the logic of the siddhi app is defined. In this scenario, all the events received in the <code>                 SweetProductionStream                </code> input stream are inserted into the <code>                 ProductionAlertStream                </code> output stream.</p>
    </div></td>
    </tr>
    </tbody>
    </table>


4.  To save this Siddhi application, click **File** , and then click
    **Save** . By default siddhi applications are saved in the
    `           <SP_HOME>/wso2/editor/deployment/workspace          `
    directory.

5.  To export the Siddhi application to your preferred location, click
    **File** , and then click **Export File** .
6.  To see a graphical view of the event flow you defined in your Siddhi
    application, click **Design View** .  
    ![](attachments/112390796/112390846.png)  
    The event flow is displayed as follows.  
    ![](attachments/112390796/112390821.png){width="900"}

### Creating a Siddhi application in the design view

To create a Siddhi application via the design view of the WSO2 SP Stream
Processor Studio, follow the steps below:

1.  Start WSO2 SP in the editor mode and access the Stream Processor
    Studio. For detailed instructions, see [Starting Stream Processor
    Studio](Stream-Processor-Studio-Overview_112390916.html#StreamProcessorStudioOverview-StartingStreamProcessorStudio)
    . The Stream Processor Studio opens as shown below.  
    ![](attachments/112390796/112390825.png){width="845"}
2.  Click **New** to start defining a new Siddhi application. A new file
    opens as shown below.  
    ![](attachments/112390796/112390836.png){height="250"}
3.  To open the design view, click **Design View** .
4.  To define the input stream into which the events to be processed via
    the Siddhi application should be received, drag and drop the stream
    icon (shown below) into the grid.  
    ![](attachments/112390796/112390808.png){width="76"}  
    Once the stream component is added to the grid, move the cursor over
    it, and then click on the settings icon as shown below.  
    ![](attachments/112390796/119116002.png){width="112"}  
    As as result, the Stream Configuration form opens as follows.  
    ![](attachments/112390796/112390811.png){height="250"}  
    Fill this form as follows to define a stream named
    `          SweetProductionStream         ` with two attributes named
    `          name         ` and `          amount         ` :  
      
    1.  In the **Name** field, enter
        `            SweetProductionStream.           `
    2.  In the **Attributes** table, enter two attributes as follows.
        You can click **+Attribute** to add a new row in the table to
        define a new attribute.

        | Attribute Name                            | Attribute Type                            |
        |-------------------------------------------|-------------------------------------------|
        | `                 name                `   | `                 string                ` |
        | `                 amount                ` | `                 double                ` |

    3.  Click **Submit** to save the new stream definition. As a result,
        the stream is displayed on the grid with the
        `            SweetProductionStream           ` label as shown
        below.  
        ![](attachments/112390796/119116005.png){width="485"}

5.  To define the output stream to which the processed events need to be
    directed, drag and drop the stream icon again. Place it after the
    `           SweetProductionStream          ` stream. This stream
    should be named `           ProductionAlertStream          ` and
    have the following attributes.

    | Attribute Name                                 | Attribute Type                        |
    |------------------------------------------------|---------------------------------------|
    | `               name              `            | `               string              ` |
    | `               totalProduction              ` | `               long              `   |

6.  To add the source from which events are received, drag and drop the
    source icon (shown below) into the grid. The source is an input to
    the **SweetProductionStream** input stream component. Therefore,
    place this source component to the left of the input stream
    component in the grid.
    ![](attachments/112390796/112390814.png){width="76" height="76"}  
    Once you add the source component, draw a line from it to the
    **SweetProductionStream** input stream component by dragging the
    cursor as demonstrated below.  
    ![](attachments/112390796/119116026.gif)  
    Click the settings icon on the source component you added to open
    the **Source Configuration** form. Then enter information as
    follows.  
    ![](attachments/112390796/112390816.png){width="900"}  
    1.  In the **Source Type** field, select **tcp** .

    2.  For this example, assume that events are received in the
        `             binary            ` format. To indicate that
        events are expected to be converted from this format, select
        **binary** in the **Map Type** field.

    3.  To indicate the context, select the **context** check box and
        enter `            SweetProductionData           ` in the field
        that appears below.
    4.  Click **Submit.**

7.  To add a query that defines the execution logic, drag and drop the
    projection query icon (shown below) to the grid.  
    ![](attachments/112390796/112390830.png){width="76" height="74"}  
    The query uses the events in the
    `          SweetProductionStream         ` input stream as inputs
    and directs the processed events (which are its output) to the
    `          ProductionAlertStream         ` output stream. Therefore,
    create two connections as demonstrated below.  
    ![](attachments/112390796/119116042.gif)
8.  To define the execution logic, move the cursor over the query in the
    grid, and click on the settings icon that appears. This opens the
    **Query Configuration** form. Enter information in it as follows:  
    ![](attachments/112390796/119116053.png){width="900"}
    1.  Enter a name for the query in the **Name** field. In this
        example, let's enter `            query           ` as the name.
    2.  In order to specify how each user defined attribute in the input
        stream is converted to generate the output events, select **User
        Defined Attributes** in the **Select** field. As a result, the
        **User Defined Attributes** table appears. The **As** column of
        this table displays the attributes of the ouitput stream. To
        derive the value for each attribute, enter required
        expressions/values in the Expressioncolumn as explained below.
        1.  The value for `              name             ` can be
            derived from the input stream without any further
            processing. Therefore, enter
            `              name             ` as the expression for the
            `              name             ` attribute.
        2.  To derive the value for the
            `              totalProduction             ` attribute, the
            sum of the values for the amount attribute of input events
            need to be calculate. Therefore, enter the expression as
            follows to apply the `              sum()             `
            Siddhi function to the `              amount             `
            attribute.  
            `              sum(amount)             `
        3.  Leave the default values of the **Output** section
            unchanged.
    3.  Click **Submit** to save the information.  
          

9.  To add a sink to publish the output events that are directed to the
    `          ProductionAlertStream         ` output stream, drag and
    drop the sink icon (shown below) into the grid.  
    ![](attachments/112390796/112390817.png){width="77" height="78"}  
    Draw an arrow from the `          ProductionAlertStream         `
    output stream to the sink component to connect them.  
      
    Click the settings icon on the sink component you added to open the
    **Sink Configuration** form. Then enter information as follows.  
    ![](attachments/112390796/119118104.png){height="250"}  
    1.  In this example, let's assume that output needs to be generated
        as logs in the console. To indicate this, select
        `             log            ` in the **Sink Type** field.

    2.  In the **Map Type** field, select the format in which the output
        must be generated. For this example, let's select
        `            json           ` .
    3.  Click **Submit** to save the information.

10. To allign the Siddhi components that you have added to the grid,
    click **Edit** and then click **Auto-Align** . As a result, all the
    components are horizontally aligned as shown below.  
    ![](attachments/112390796/112390821.png){width="900"}
11. Click **Source View** . The siddhi application is displayed as
    follows.  
    ![](attachments/112390796/112390820.png)
12. Click **File** and then click **Save as** . The **Save to
    Workspace** dialog box appears. In the **File Name** field, enter
    `          SweetProductionAnalysis         ` and click **Save** .

![](attachments/112390796/112390826.png)
