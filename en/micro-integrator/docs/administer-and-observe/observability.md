# Working with Observability

Product observability enables rapid debugging of product issues. The ESB
profile of WSO2 Enterprise Integrator (WSO2 EI) enables observability
using correlation logs. Correlation logs allow you to monitor individual
HTTP requests from the point that a message is received by the ESB until
the corresponding response message is sent back to the original message
sender. That is, the complete round trip of an HTTP message (client →
ESB → back-end → ESB → client) can be tracked and anlyzed using a log
file.

When correlation logs are enabled for the ESB server, a separate log
file named `         correlation.log        ` is created in the
`         <EI_HOME>/repository/logs/        ` directory. Every HTTP
message that flows through the ESB and between the ESB and external
clients undergoes several state changes. A new log entry is created in
the `         correlation.log        ` file corresponding to the state
changes in the round trip of a single HTTP request. A correlation ID
assigned to the incoming HTTP request is assigned to all the log entries
corresponding to the request. Therefore, you can use this correlation ID
to easily locate the logs relevant to the round trip of a specific HTTP
request and, thereby, analyze the behaviour of the message flow.

!!! note

-   By default, product observability is not enabled as it impacts on
    the product's performance.
-   In order to use this feature, apply the WUM update that is released
    on 2018-11-24.

    !!! warning

    If you want to deploy a WUM update into production, you need to have
    a paid subscription. If you do not have a paid subscription, you can
    use this feature with the next version of WSO2 Enterprise Integrator
    when it is released. For more information on updating WSO2 products
    using WUM, see [Getting Started with
    WUM](https://docs.wso2.com/display/ADMIN44x/Getting+Started+with+WUM)
    .



See the following topics for details:

-   [Configuring correlation
    logs](#WorkingwithObservability-Configuringcorrelationlogs)
-   [Enabling correlation logs in the
    ESB](#WorkingwithObservability-EnablingcorrelationlogsintheESB)
-   [Sending an HTTP request with a correlation
    ID](#WorkingwithObservability-SendinganHTTPrequestwithacorrelationID)
-   [Accessing the correlation
    logs](#WorkingwithObservability-Accessingthecorrelationlogs)
-   [Reading correlation
    logs](#WorkingwithObservability-Readingcorrelationlogs)

### Configuring correlation logs

Follow the steps given below to configure correlation logs in the ESB
server.

1.  Add the following parameters to the
    `           log4j.properties          ` file (stored in the
    `           <EI_HOME>/conf/          ` directory):

    ``` java
        # correlation logs
            log4j.logger.correlation=INFO, CORRELATION
            log4j.additivity.correlation=false
            # Appender config for correlation logs
            log4j.appender.CORRELATION=org.apache.log4j.RollingFileAppender
            log4j.appender.CORRELATION.File=${carbon.home}/repository/logs/${instance.log}/correlation.log
            log4j.appender.CORRELATION.MaxFileSize=10MB
            log4j.appender.CORRELATION.layout=org.apache.log4j.PatternLayout
            log4j.appender.CORRELATION.Threshold=INFO
            log4j.appender.CORRELATION.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS}|%X{Correlation-ID}|%t|%m%n
    ```

2.  Note that the maximum file size of the correlation log is set to
    10MB in the above configuration. That is, when the size of the file
    exceeds 10MB, a new log file is created. If required, you can change
    this file size.

3.  **If required** , you can change the default HTTP header (which is
    'activity\_id') that is used to carry the correlation ID by adding
    the following property to the
    `           passthru-http.properties          ` file (stored in the
    `           <EI_HOME>/conf/          ` directory). Replace
    `           <correlation_id>          ` with a value of your choice.

    ``` java
            correlation_header_name=<correlation_id>
    ```

Once the logs are configured, correlation logging should be enabled in
the ESB as explained in the next section.

### Enabling correlation logs in the ESB

You can enable correlation logging by passing a system property.

-   If you want correlation logs to be enabled every time the server
    starts, add the following system property to the product start-up
    script (stored in the `           <EI_HOME>/bin/          `
    directory) and set it to `           true          ` .

    ``` java
            -DenableCorrelationLogs=true \
    ```

-   Alternatively, you can pass the system property at the time of
    starting the server by executing the following command:

    |                       |                                                                                                                                             |
    |-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
    | On Linux/MacOS/CentOS | `               sh integrator.sh              ` `               -DenableCorrelationLogs=              ` `               true              ` |
    | On Windows            | `               integrator.bat              ` `               -DenableCorrelationLogs=              ` `               true              `   |

Now when you start the ESB server, the
`         correlation.log        ` file is created in the
`         <EI_HOME>/repository/logs/        ` directory.

### Sending an HTTP request with a correlation ID

When the client sends an HTTP request to the ESB, a correlation ID for
the request can be passed using the correlation header that is
configured in the ESB. By default, the correlation header is
'activity\_id'. If you want to change the default correlation header,
wee the topic on [configuring correlation
logs](#WorkingwithObservability-Configuringcorrelationlogs) . If the
client does not pass a correlation ID in the request, the ESB will
generate an internal value and assign it to the request. The correlation
ID assigned to the incoming request is assigned to all the log entries
that are related to the same request.

Shown below is the POST request that is sent using the CURL client in
the [quick start
guide](https://docs.wso2.com/display/EI650/Quick+Start+Guide) . Note
that the correlation ID is set in this request.

``` java
    curl -X POST --data @request.json http://localhost:8280/healthcare/categories/surgery/reserve -H "Content-Type:application/json" -H "activityid:correlationID"
```

### Accessing the correlation logs

If you know the correlation ID of the HTTP request that you want to
analyze, you can isolate the relevant logs as explained below.

1.  Open a terminal and navigate to the
    `          <EI_HOME>/repository/logs/         ` directory where the
    `          correlation.log         ` file is saved.
2.  Execute the following command with the required correlation ID.
    Replace \<correlation\_ID\> with the required value.

    ``` java
            cat correlation.log | grep "<correlation_ID>"
    ```

Shown below is an example of correlation log entries corresponding to
the round trip of a single HTTP request.

``` java
    2018-11-30 15:27:27,262|correlationID|HTTP-Listener I/O dispatcher-5|0|HTTP State Transition|http-incoming-17|POST|/healthcare/categories/surgery/reserve|REQUEST_HEAD
    2018-11-30 15:27:27,262|correlationID|HTTP-Listener I/O dispatcher-5|0|HTTP State Transition|http-incoming-17|POST|/healthcare/categories/surgery/reserve|REQUEST_BODY
    2018-11-30 15:27:27,263|correlationID|HTTP-Listener I/O dispatcher-5|1|HTTP State Transition|http-incoming-17|POST|/healthcare/categories/surgery/reserve|REQUEST_DONE
    2018-11-30 15:27:27,265|correlationID|HTTP-Sender I/O dispatcher-4|42173|HTTP State Transition|http-outgoing-4|POST|http://localhost:9090/grandoaks/categories/surgery/reserve|REQUEST_HEAD
    2018-11-30 15:27:27,265|correlationID|HTTP-Sender I/O dispatcher-4|0|HTTP State Transition|http-outgoing-4|POST|http://localhost:9090/grandoaks/categories/surgery/reserve|REQUEST_DONE
    2018-11-30 15:27:27,267|correlationID|HTTP-Sender I/O dispatcher-4|2 |HTTP|sourhttp://localhost:9090/grandoaks/categories/surgery/reserve|BACKEND LATENCY
    2018-11-30 15:27:27,267|correlationID|HTTP-Sender I/O dispatcher-4|2|HTTP State Transition|http-outgoing-4|POST|http://localhost:9090/grandoaks/categories/surgery/reserve|RESPONSE_HEAD
    2018-11-30 15:27:27,267|correlationID|HTTP-Sender I/O dispatcher-4|0|HTTP State Transition|http-outgoing-4|POST|http://localhost:9090/grandoaks/categories/surgery/reserve|RESPONSE_BODY
    2018-11-30 15:27:27,267|correlationID|HTTP-Sender I/O dispatcher-4|0|HTTP State Transition|http-outgoing-4|POST|http://localhost:9090/grandoaks/categories/surgery/reserve|RESPONSE_DONE
    2018-11-30 15:27:27,269|correlationID|HTTP-Listener I/O dispatcher-5|6|HTTP State Transition|http-incoming-17|POST|/healthcare/categories/surgery/reserve|RESPONSE_HEAD
    2018-11-30 15:27:27,269|correlationID|HTTP-Listener I/O dispatcher-5|0|HTTP State Transition|http-incoming-17|POST|/healthcare/categories/surgery/reserve|RESPONSE_BODY
    2018-11-30 15:27:27,269|correlationID|HTTP-Listener I/O dispatcher-5|0|HTTP State Transition|http-incoming-17|POST|/healthcare/categories/surgery/reserve|RESPONSE_DONE
    2018-11-30 15:27:27,269|correlationID|HTTP-Listener I/O dispatcher-5|7|HTTP|http-incoming-17|POST|/healthcare/categories/surgery/reserve|ROUND-TRIP LATENCY
```

### Reading correlation logs

The pattern/format of a correlation log is shown below along with an
example log entry.

-   [**Log Pattern**](#fae88ac6b61247e8b1a061b8b1a51674)
-   [**Example Log**](#9a817fd43cb54fdfa1f22d39985f63b5)

``` java
    Time Stamp|Correlation ID|Thread name|Duration|Call type|Connection name|Method type|Connection URL|HTTP state
```

``` java
    2018-10-26 17:34:40,464|de461a83-fc74-4660-93ed-1b609ecfac23|HTTP-Listener I/O dispatcher-3|535|HTTP|http-incoming-3|GET|/api/querydoctor/surgery|ROUND-TRIP LATENCY
```

The detail recorded in a log entry is described below.

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 86%" />
</colgroup>
<tbody>
<tr class="odd">
<td>Time Stamp</td>
<td><div class="content-wrapper">
<p>The time at which the log is created.</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
<strong>Example</strong>
</div>
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a><span class="dv">2018</span>-<span class="dv">10</span>-<span class="dv">26</span> <span class="dv">17</span>:<span class="dv">34</span>:<span class="dv">40</span>,<span class="dv">464</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="even">
<td>Correlation ID</td>
<td><div class="content-wrapper">
<p>Each log contains a correlation ID, which is unique to the HTTP request. A client can send the correlation ID in the header of the HTTP request. If this correlation ID is missing in the incoming request, the ESB will generate one for the request.</p>
<p>The HTTP header that carries the correlation ID is configured in the ESB.</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
<strong>Example</strong>
</div>
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a>de461a83-fc74-<span class="dv">4660</span>-93ed-1b609ecfac23</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Thread name</td>
<td><div class="content-wrapper">
<p>The identifier of the thread.</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
<strong>Example</strong>
</div>
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb3" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb3-1"><a href="#cb3-1"></a>HTTP-Listener I/O dispatcher-<span class="dv">3</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="even">
<td>Duration</td>
<td><div class="content-wrapper">
<p>The duration (given in milliseconds) depends on the type of log entry:</p>
<ul>
<li>If the state in the log entry is ROUND-TRIP LATENCY, the duration corresponds to the time gap between the REQUEST_HEAD state and the ROUND-TRIP LATENCY state. That is, the total time of the round trip.</li>
<li>If the state in the log entry is BACKEND LATENCY, the duration corresponds to the total time taken by the backend to process the message.</li>
<li>For all other log entries, the duration corresponds to the time gap between the current log entry and the immediately previous log entry. That is, the time taken for the HTTP request to move from one state to another.</li>
</ul>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
<strong>Example</strong>
</div>
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb4" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb4-1"><a href="#cb4-1"></a><span class="dv">535</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Call type</td>
<td><p>There are two possible call types:</p>
<ul>
<li><strong>HTTP</strong> call type identifies logs that correspond to either back-end latency or round-trip latency states. That is, in the case of an individual request, one log will be recorded to identify back-end latency, and another log for round-trip latency. Since these logs relate to HTTP calls between the ESB and external clients, these logs are categorized using the HTTP call type.</li>
<li><strong>HTTP State Transition</strong> call type identifies logs that correspond to the state transitions in the HTTP transport related to a particular message.</li>
</ul></td>
</tr>
<tr class="even">
<td>Connection name</td>
<td><div class="content-wrapper">
<p>This is a name that is generated to identify the connection between the ESB and the external client (back-end or message sender).</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
<strong>Example</strong>
</div>
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb5" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb5-1"><a href="#cb5-1"></a>http-incoming-<span class="dv">3</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>Method type</td>
<td><div class="content-wrapper">
<p>The HTTP method used for the request.</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
<strong>Example</strong>
</div>
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb6" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb6-1"><a href="#cb6-1"></a>GET</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="even">
<td>Connection URL</td>
<td><div class="content-wrapper">
<p>The connection URL of the external client with which the message is being communicated. For example, if the message is being read from the client, the connection URL corresponds to the client sending the message. However, if the message is being written to the backend, the URL corresponds to the backend client.</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
<strong>Example</strong>
</div>
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb7" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb7-1"><a href="#cb7-1"></a>/api/querydoctor/surgery</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="odd">
<td>HTTP state</td>
<td><p>Listed below are the state changes that a message goes through when it flows through the ESB, and when the message flows between the ESB and external clients. Typically, a new log entry is generated for each of the states. However, there can be two separate log entries created for one particular state (e xcept for BACKEND LATENCY and ROUND-TRIP LATENCY) depending on whether the message is being read or written. You can identify the two separate log entries from the <a href="#WorkingwithObservability-ConnectionURL">connection URL</a> explained above.</p>
<ul>
<li><strong>REQUEST_HEAD:</strong> All HTTP headers in the incoming request are being read/or being written to the backend.</li>
<li><strong>REQUEST_BODY</strong> : The body of the incoming request is being read/or being written to the backend.</li>
<li><strong>REQUEST_DONE</strong> : The request is completely read (content decoded)/ or is completely written to the backend.</li>
<li><strong>BACKEND LATENCY</strong> : The response message is received by the ESB. This status corresponds to the time total time taken by the backend to process the message.</li>
<li><strong>RESPONSE_HEAD</strong> : All HTTP headers in the response message are being read/or being written to the client.</li>
<li><strong>RESPONSE_BODY</strong> : The body of the response message is being read/or being written to the client.</li>
<li><strong>RESPONSE_DONE</strong> : The response is completely read/ or completely written to the client.</li>
<li><strong>ROUND-TRIP LATENCY</strong> : The response message is completely written to the client. This status corresponds to the total time taken by the HTTP request to compete the round trip (from the point of receiving the HTTP request from a client until the response message is sent back to the client)</li>
</ul></td>
</tr>
</tbody>
</table>

