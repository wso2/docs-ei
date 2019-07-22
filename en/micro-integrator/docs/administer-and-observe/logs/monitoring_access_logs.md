# Monitoring Access Logs

HTTP access logs help you monitor information such as the persons who
access the product, how many hits are received, what the errors are,
etc. This information is useful for troubleshooting errors.

All WSO2 products can enable access logs for the HTTP servlet transport.
This servlet transport works on 9443/9763 ports, and
it receives admin/operation requests. Therefore, access logs for the
servert transport is useful for analysing operational/admin-level access
details. Additionally, in products such as WSO2 Enterprise Service Bus,
WSO2 Enterprise Integrator (and WSO2 API Manager), access logs can be
generated for the PassThrough and NIO transports. The PassThrough/NIO
transports works on 8280/8243 ports and is used for API/Service
invocations. By default, the access logs from both the servlet transport
and the PassThrough/NIO transports are written to a common access log
file located in the `         <EI_HOME>/repository/logs/        `
directory.

See the topics given below to configure the default behaviour of HTTP
access logs in the ESB.

-   [Configuring access logs for the HTTP servlet
    transport](#MonitoringAccessLogs-ConfiguringaccesslogsfortheHTTPservlettransport)
-   [Configuring access logs for the PassThrough and NIO transports
    (Service/API
    invocation)](#MonitoringAccessLogs-ConfiguringaccesslogsforthePassThroughandNIOtransports(Service/APIinvocation))
    -   [Supported log pattern formats for the PassThrough and NIO
        transports](#MonitoringAccessLogs-SupportedlogpatternformatsforthePassThroughandNIOtransports)

### Configuring access logs for the HTTP servlet transport

!!! note

**Note** that access logs for the HTTP servlet transport logs details of
the request as well as the response on a single log line.


As the runtime of WSO2 products is based on Apache Tomcat, you can use
the `         Access_Log_Valve        ` variable in Tomcat as explained
below to configure access logs for the HTTP servlet transport:

1.  Open the \<
    `           EI_HOME>/conf/tomcat/catalina-server.xml          ` file
    (which is the server descriptor file for the embedded Tomcat
    integration)

2.  Customize the attributes for the
    `           Access_Log_Valve          ` variable shown below.

    ``` java
        <Valve className="org.apache.catalina.valves.AccessLogValve" 
                       directory="${carbon.home}/repository/logs"
                       prefix="http_access_management_console_" 
                       suffix=".log"
                       pattern="combined" />
    ```

    The attributes that are used by default are explained below. See the
    descriptions of the Tomcat-supported [Access Log
    Valve attributes](http://tomcat.apache.org/tomcat-7.0-doc/config/valve.html#Access_Log_Valve/Attributes)
    and customize the required values.

    <table style="width:100%;">
    <colgroup>
    <col style="width: 5%" />
    <col style="width: 94%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>directory</td>
    <td>The path to the directory that will store the access log file. By default, this location is set to " <code>               ${carbon.home}/repository/logs"              </code> in all WSO2 products.</td>
    </tr>
    <tr class="even">
    <td>prefix</td>
    <td>The prefix added to the log file's name. By default, this is "http_access_management_console_".</td>
    </tr>
    <tr class="odd">
    <td>suffix</td>
    <td>The suffix added to the log file's name. By default, this is ".log".</td>
    </tr>
    <tr class="even">
    <td>pattern</td>
    <td><div class="content-wrapper">
    <p>The attribute defines the format for the log pattern, which consists of the information fields from the requests and responses that should be logged. The pattern format is created using the following attributes:</p>
    <ul>
    <li><p>A standard value to represent a particular string. For example, "%h" represents the remote host name in the request. See the list of <a href="https://tomcat.apache.org/tomcat-7.0-doc/api/org/apache/catalina/valves/AccessLogValve.html">string replacement values supported by the Tomcat valve</a> .</p></li>
    <li><strong>%{xxx}i</strong> is used to represent the header in the incoming request (xxx=header value).</li>
    <li><strong>%{xxx}o</strong> is used to represents the header in the outgoing request (xxx=header value).</li>
    </ul>
    <p>While you can use the above attributes to define a custom pattern, the standard patterns shown below can be used.</p>
    <ul>
    <li><p><strong>common</strong> ( <a href="http://httpd.apache.org/docs/1.3/logs.html#common">Apache common log pattern</a> ):</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>pattern=%h %l %u %t <span class="st">&quot;%r&quot;</span> %s %b</span></code></pre></div>
    </div>
    </div></li>
    <li><p><strong>combined</strong> ( <a href="http://httpd.apache.org/docs/1.3/logs.html#combined">Apache combined log pattern</a> ):</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a>pattern=%h %l %u %t <span class="st">&quot;%r&quot;</span> %s %b <span class="st">&quot;%{Referer}i&quot;</span> <span class="st">&quot;%{User-Agent}i&quot;</span></span></code></pre></div>
    </div>
    </div></li>
    </ul>
    <p>Note that, by default, the "combined" pattern is enabled in the ESB.</p>
    </div></td>
    </tr>
    </tbody>
    </table>

3.  Restart the server. According to the default configurations, a log
    file named `           http_access_management_console          ` \_
    `           .{DATE}.log          ` is created inside the \<
    `           EI_HOME>/repository/logs          ` directory. The log
    is rotated on a daily basis.

### Configuring access logs for the PassThrough and NIO transports (Service/API invocation)

!!! note

**Note** that access logs for the PassThrough/NIO transports log the
request and the response on **two** separate log lines.


By default, access logs related to service/API invocation are disabled
for performance reasons in the above products. You should enable these
access log only for troubleshooting errors. Follow the steps given below
to enable access logs for the PassThrough transport:

1.  Change the log level from `           WARN          ` to
    `           INFO          ` for the following entry in the
    `           <EI_HOME>/conf/log4j.properties          `
    configuration file.

    ``` xml
        log4j.logger.org.apache.synapse.transport.http.access=INFO
    ```

2.  You can customize the format of this access log by changing the
    following property values in the
    `           <EI_HOME>/conf/access-log.properties          `
    configuration file. If this file does not exist in the product by
    default, you can create a new file with the following parameters.

    <table>
    <tbody>
    <tr class="odd">
    <td>access_log_directory</td>
    <td>Add this property ONLY if you want to change the default location of the log file. By default, the product is configured to store access logs in the <code>               &lt;EI_HOME&gt;/repository/logs              </code> directory.</td>
    </tr>
    <tr class="even">
    <td>access_log_prefix</td>
    <td><div class="content-wrapper">
    <p>The prefix added to the log file's name. The default value is as follows:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>access_log_prefix=http_access_</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    <tr class="odd">
    <td>access_log_suffix</td>
    <td><div class="content-wrapper">
    <p>The suffix added to the log file's name. The default value is as follows:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a>access_log_suffix=.<span class="fu">log</span></span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    <tr class="even">
    <td>access_log_file_date_format</td>
    <td><div class="content-wrapper">
    <p>The date format used in access logs. The default value is as follows:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb3" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb3-1"><a href="#cb3-1"></a>access_log_file_date_format=yyyy-MM-dd</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    <tr class="odd">
    <td>access_log_pattern</td>
    <td><div class="content-wrapper">
    <p>The attribute defines the format for the log pattern, which consists of the information fields from the requests and responses that should be logged. The pattern format is created using the following attributes:</p>
    <ul>
    <li><p>A standard value to represent a particular string. For example, "%h" represents the remote host name in the request. Note that all the <a href="https://tomcat.apache.org/tomcat-7.0-doc/api/org/apache/catalina/valves/AccessLogValve.html">string replacement values supported by Tomcat</a> are NOT supported for the PassThrough transport's access logs. The list of supported values are <a href="#MonitoringAccessLogs-Supportedlogpatternformatsforthepassthroughtransport">given below</a> .</p></li>
    <li><strong>%{xxx}i</strong> is used to represent the header in the incoming request (xxx=header value).</li>
    <li><strong>%{xxx}o</strong> is used to represents the header in the outgoing request (xxx=header value).</li>
    </ul>
    <p>While you can use the above attributes to define a custom pattern, the standard patterns shown below can be used.</p>
    <ul>
    <li><p><strong>common</strong> ( <a href="http://httpd.apache.org/docs/1.3/logs.html#common">Apache common log pattern</a> ):</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb4" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb4-1"><a href="#cb4-1"></a>access_log_pattern=%h %l %u %t <span class="st">&quot;%r&quot;</span> %s %b</span></code></pre></div>
    </div>
    </div></li>
    <li><p><strong>combined</strong> ( <a href="http://httpd.apache.org/docs/1.3/logs.html#combined">Apache combined log pattern</a> ):</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb5" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb5-1"><a href="#cb5-1"></a>access_log_pattern=%h %l %u %t <span class="st">&quot;%r&quot;</span> %s %b <span class="st">&quot;%{Referer}i&quot;</span> <span class="st">&quot;%{User-Agent}i&quot;</span></span></code></pre></div>
    </div>
    </div></li>
    </ul>
    <p>By default, a modified version of the <a href="http://httpd.apache.org/docs/1.3/logs.html#combined">Apache combined log format</a> is enabled in the ESB as shown below. Note that the "X-Forwarded-For" header is appended to the beginning of the usually <strong>combined</strong> log format. This correctly identifies the original node that sent the request (in situations where requests go through a proxy such as a load balancer). The "X-Forwarded-For" header must be present in the incoming request for this to be logged.</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb6" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb6-1"><a href="#cb6-1"></a>access_log_pattern=%{X-Forwarded-For}i %h %l %u %t \<span class="st">&quot;%r</span><span class="sc">\&quot;</span><span class="st"> %s %b </span><span class="sc">\&quot;</span><span class="st">%{Referer}i</span><span class="sc">\&quot;</span><span class="st"> </span><span class="sc">\&quot;</span><span class="st">%{User-Agent}i</span><span class="sc">\&quot;</span></span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

3.  Restart the server.

4.  Invoke a proxy service or REST API that is deployed in the ESB. For
    testing purposes, use the artifacts in the [quick start
    guide](https://docs.wso2.com/display/EI650/Quick+Start+Guide) .  
    The access log file for the service/API will be created in the
    `           <EI_HOME>/repository/logs          ` directory. The
    default name of the log file is
    `           http_access_.log          ` .

        !!! tip
    
        Note that there will be delay in printing the logs to the log file.
    

#### Supported log pattern formats for the PassThrough and NIO transports

<table>
<thead>
<tr class="header">
<th>Attribute</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>%a</code></pre></td>
<td><p>Remote IP address</p></td>
</tr>
<tr class="even">
<td><pre><code>%A</code></pre></td>
<td><p>Local IP address</p></td>
</tr>
<tr class="odd">
<td><pre><code>%b</code></pre></td>
<td><p>Bytes sent, excluding HTTP headers, or '-' if zero</p></td>
</tr>
<tr class="even">
<td><pre><code>%B</code></pre></td>
<td><p>Bytes sent, excluding HTTP headers</p></td>
</tr>
<tr class="odd">
<td><pre><code>%c</code></pre></td>
<td><p>Cookie value</p></td>
</tr>
<tr class="even">
<td><pre><code>%C</code></pre></td>
<td><p>Accept header</p></td>
</tr>
<tr class="odd">
<td><pre><code>%e</code></pre></td>
<td><p>Accept Encoding</p></td>
</tr>
<tr class="even">
<td><pre><code>%E</code></pre></td>
<td><p>Transfer Encoding</p></td>
</tr>
<tr class="odd">
<td><pre><code>%h</code></pre></td>
<td><p>Remote host name (or IP address if enableLookups for the connector is false)</p></td>
</tr>
<tr class="even">
<td><pre><code>%l</code></pre></td>
<td><p>Remote logical username from identd (always returns '-')</p></td>
</tr>
<tr class="odd">
<td><pre><code>%L</code></pre></td>
<td><p>Accept Language</p></td>
</tr>
<tr class="even">
<td><pre><code>%k</code></pre></td>
<td><p>Keep Alive</p></td>
</tr>
<tr class="odd">
<td><pre><code>%m</code></pre></td>
<td><p>Request method (GET, POST, etc.)</p></td>
</tr>
<tr class="even">
<td><pre><code>%n</code></pre></td>
<td><p>Content Encoding</p></td>
</tr>
<tr class="odd">
<td><pre><code>%r</code></pre></td>
<td><p>Request Element</p></td>
</tr>
<tr class="even">
<td><pre><code>%s</code></pre></td>
<td><p>HTTP status code of the response</p></td>
</tr>
<tr class="odd">
<td><pre><code>%S</code></pre></td>
<td><p>Accept Chatset</p></td>
</tr>
<tr class="even">
<td><pre><code>%t</code></pre></td>
<td><p>Date and time, in Common Log Format</p></td>
</tr>
<tr class="odd">
<td><pre><code>%T</code></pre></td>
<td><p>Time taken to process the request in seconds.</p></td>
</tr>
<tr class="even">
<td><pre><code>%u</code></pre></td>
<td><p>Remote user that was authenticated (if any), else '-'</p></td>
</tr>
<tr class="odd">
<td><pre><code>%U</code></pre></td>
<td><p>Requested URL path</p></td>
</tr>
<tr class="even">
<td><pre><code>%v</code></pre></td>
<td><p>Local server name</p></td>
</tr>
<tr class="odd">
<td><pre><code>%V</code></pre></td>
<td><p>Vary Header</p></td>
</tr>
<tr class="even">
<td><pre><code>%x</code></pre></td>
<td><p>Connection Header</p></td>
</tr>
<tr class="odd">
<td><pre><code>%Z</code></pre></td>
<td><p>Server Header</p></td>
</tr>
</tbody>
</table>
