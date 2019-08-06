# Using Log Files

Logging is one of the most important aspects of a production-grade
server. A properly configured logging system is vital for identifying
errors, security threats, and usage patterns.

!!! info
    **Java logging and Log4j integration:** I n addition to the logs from libraries that use Log4j, all logs from libraries (such as, Tomcat, Hazelcast and more) that use Java logging framework are also
        visible in the same log files. That is, when Java logging is enabled
        in Carbon, only the Log4j appenders will write to the log files. If
        the Java Logging Handlers have logs, these logs will be delegated to
        the log events of the corresponding Log4j appenders. A Pub/Sub
        registry pattern implementation has been used in the latter
        mentioned scenario to plug the handlers and appenders. The following
        default log4j appenders in the
        `           log4j.properties          ` file are used for this
        implementation:

    - org.wso2.carbon.logging.appenders.CarbonConsoleAppender
    - org.wso2.carbon.logging.appenders.CarbonDailyRollingFileAppender

Listed below are the various log types used in WSO2 Micro Integrator.

!!! Info
    Separate log files are created in the `MI_HOME/repository/logs` directory for each of the log types given below.

## Carbon logs

The Carbon log file (`wso2carbon.log`) covers all the management features of a product. Carbon logs are configured in the `log4j.properties` file (stored in the `MI_HOME/conf` directory).

The Carbon log file is enabled in the product by default as shown below. You can configure the details that is captured in this log file by [configuring the log4j properties](../logs/configuring_log4j_properties.md).

```
# CARBON_LOGFILE is set to be a DailyRollingFileAppender using a PatternLayout.
log4j.appender.CARBON_LOGFILE=org.wso2.carbon.utils.logging.appenders.CarbonDailyRollingFileAppender

# Log file will be overridden by the configuration setting in the DB. This path should be relative to WSO2 Carbon Home
log4j.appender.CARBON_LOGFILE.File=${carbon.home}/repository/logs/${instance.log}/wso2carbon${instance.log}.log
log4j.appender.CARBON_LOGFILE.Append=true
log4j.appender.CARBON_LOGFILE.layout=org.wso2.carbon.utils.logging.TenantAwarePatternLayout

# ConversionPattern will be overridden by the configuration setting in the DB
log4j.appender.CARBON_LOGFILE.layout.ConversionPattern=[%d] [%T] [%S] [%t] %P%5p {%c} - %x %m%n
log4j.appender.CARBON_LOGFILE.layout.TenantPattern=%U%@%D [%T] [%S]
log4j.appender.CARBON_LOGFILE.threshold=DEBUG
```

## Audit logs

Audit logs are used for tracking the sequence of actions that affect a particular task carried out on the server. These are also configured in the `log4j.properties` file (stored in the `MI_HOME/conf` directory).

Audit logs are enabled in the product by default as shown below. You can configure the details that are captured in this log file by [configuring the log4j properties](../logs/configuring_log4j_properties.md).

```
log4j.logger.AUDIT_LOG=INFO, AUDIT_LOGFILE
         
# Appender config to AUDIT_LOGFILE
log4j.appender.AUDIT_LOGFILE=org.wso2.carbon.utils.logging.appenders.CarbonDailyRollingFileAppender
log4j.appender.AUDIT_LOGFILE.File=${carbon.home}/repository/logs/audit.log
log4j.appender.AUDIT_LOGFILE.Append=true
log4j.appender.AUDIT_LOGFILE.layout=org.wso2.carbon.utils.logging.TenantAwarePatternLayout
log4j.appender.AUDIT_LOGFILE.layout.ConversionPattern=[%d] %P%5p {%c}- %x %m %n
log4j.appender.AUDIT_LOGFILE.layout.TenantPattern=%U%@%D [%T] [%S]
log4j.appender.AUDIT_LOGFILE.threshold=INFO
log4j.additivity.AUDIT_LOG=false
```

## Patch logs 

These logs contain details related to patches applied to the product. Patch logs cannot be customized.

## Wire logs 

You can enable wire logs to monitor HTTP messages that flow through the Micro Integrator. 

!!! Warning
    Please note that wire logs should be enabled for the troubleshooting purposes only. Running productions systems with wire logs enabled is not recommended.

In order to read the wire logs, you must first identify message direction.
<table>
    <tr>
        <td><b>DEBUG - wire >></b></td>
        <td>Represents the message coming into the Micro Integrator from the wire.</td>
    </tr>
    <tr>
        <td><b>DEBUG - wire <<</b></td>
        <td>Represents the message that goes to the wire from the Micro Integrator.</td>
    </tr>
</table>

There are two incoming messages and two outgoing messages in the above log. The first part of the message log contains the HTTP headers and is followed by the message payload. As shown in this example, wire logs are very useful for troubleshooting unexpected issues that occur while integrating systems. You can verify whether a message payload is correctly going out from the Micro Integrator, whether HTTP headers such as Content-Type is properly set in the outgoing message, etc. by looking at the wire logs.

Enable wire logs by setting by uncommenting the following parameter and [updating the log level](../configuring_log4j_properties/#setting-the-log-level) (which is DEBUG by default).

``` java
log4j.logger.org.apache.synapse.transport.http.wire=DEBUG
```

## Service/Event Tracing logs 

These are logs that are enabled in some WSO2 Micro Integrator for tracing services and events using a separate log file (`wso2-<product>-trace.log`). If server/event tracing logs are used in your product, you can configure them in the `log4j.properties` file (stored in the `MI_HOME/conf` directory).

A separate log file for tracing services/events are enabled for certain WSO2 products in the `           log4j.properties          `file using a specific appender. These logs are published to a file named `           wso2-<product>-trace.log          ` . 

See instructions on [configuring the log4j properties](../logs/configuring_log4j_properties.md).

## HTTP Access logs

HTTP requests/responses are logged to monitor the activities related to an application's usage. HTTP access logs help you monitor information such as the persons who access the product, how many hits are received, what the errors are, etc. This information is useful for troubleshooting errors.

In the Micro Integrator, access logs can be generated for the PassThrough and NIO transports. The PassThrough/NIO transports works on 8280/8243 ports and is used for API/Service invocations. By default, the access logs from both the servlet transport and the PassThrough/NIO transports are written to a common access log file located in the `MI_HOME/repository/logs/` directory.

<!--
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
-->

### Configuring access logs for the PassThrough and NIO transports (Service/API invocation)

!!! note
    Access logs for the PassThrough/NIO transports log the request and the response on **two** separate log lines.

By default, access logs related to service/API invocation are disabled for performance reasons in the above products. You should enable these access log only for troubleshooting errors. Follow the steps given below to enable access logs for the PassThrough transport:

1.  Change the log level from `           WARN          ` to `           INFO          ` for the following entry in the
    `           MI_HOME/conf/log4j.properties          ` configuration file.

    ``` xml
    log4j.logger.org.apache.synapse.transport.http.access=INFO
    ```

2.  You can customize the format of this access log by changing the following property values in the `MI_HOME/conf/access-log.properties` configuration file. If this file does not exist in the product by default, you can create a new file with the following parameters.

    <table>
    <tbody>
      <tr class="odd">
         <td>access_log_directory</td>
         <td>Add this property ONLY if you want to change the default location of the log file. By default, the product is configured to store access logs in the <code>MI_HOME/repository/logs</code> directory.</td>
      </tr>
      <tr class="even">
         <td>access_log_prefix</td>
         <td>
            <div class="content-wrapper">
               <p>The prefix added to the log file's name. The default value is as follows:</p>
               <div class="code panel pdl" style="border-width: 1px;">
                  <div class="codeContent panelContent pdl">
                     <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence">
                        <pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>access_log_prefix=http_access_</span></code></pre>
                     </div>
                  </div>
               </div>
            </div>
         </td>
      </tr>
      <tr class="odd">
         <td>access_log_suffix</td>
         <td>
            <div class="content-wrapper">
               <p>The suffix added to the log file's name. The default value is as follows:</p>
               <div class="code panel pdl" style="border-width: 1px;">
                  <div class="codeContent panelContent pdl">
                     <div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence">
                        <pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a>access_log_suffix=.<span class="fu">log</span></span></code></pre>
                     </div>
                  </div>
               </div>
            </div>
         </td>
      </tr>
      <tr class="even">
         <td>access_log_file_date_format</td>
         <td>
            <div class="content-wrapper">
               <p>The date format used in access logs. The default value is as follows:</p>
               <div class="code panel pdl" style="border-width: 1px;">
                  <div class="codeContent panelContent pdl">
                     <div class="sourceCode" id="cb3" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence">
                        <pre class="sourceCode java"><code class="sourceCode java"><span id="cb3-1"><a href="#cb3-1"></a>access_log_file_date_format=yyyy-MM-dd</span></code></pre>
                     </div>
                  </div>
               </div>
            </div>
         </td>
      </tr>
      <tr class="odd">
         <td>access_log_pattern</td>
         <td>
            <div class="content-wrapper">
               <p>The attribute defines the format for the log pattern, which consists of the information fields from the requests and responses that should be logged. The pattern format is created using the following attributes:</p>
               <ul>
                  <li>
                     <p>A standard value to represent a particular string. For example, "%h" represents the remote host name in the request. Note that all the <a href="https://tomcat.apache.org/tomcat-7.0-doc/api/org/apache/catalina/valves/AccessLogValve.html">string replacement values supported by Tomcat</a> are NOT supported for the PassThrough transport's access logs. The list of supported values are <a href="#supported-log-pattern-formats-for-the-passthrough-and-nio-transports">given below</a> .</p>
                  </li>
                  <li><strong>%{xxx}i</strong> is used to represent the header in the incoming request (xxx=header value).</li>
                  <li><strong>%{xxx}o</strong> is used to represents the header in the outgoing request (xxx=header value).</li>
               </ul>
               <p>While you can use the above attributes to define a custom pattern, the standard patterns shown below can be used.</p>
               <ul>
                  <li>
                     <p><strong>common</strong> ( <a href="http://httpd.apache.org/docs/1.3/logs.html#common">Apache common log pattern</a> ):</p>
                     <div class="code panel pdl" style="border-width: 1px;">
                        <div class="codeContent panelContent pdl">
                           <div class="sourceCode" id="cb4" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence">
                              <pre class="sourceCode java"><code class="sourceCode java"><span id="cb4-1"><a href="#cb4-1"></a>access_log_pattern=%h %l %u %t <span class="st">&quot;%r&quot;</span> %s %b</span></code></pre>
                           </div>
                        </div>
                     </div>
                  </li>
                  <li>
                     <p><strong>combined</strong> ( <a href="http://httpd.apache.org/docs/1.3/logs.html#combined">Apache combined log pattern</a> ):</p>
                     <div class="code panel pdl" style="border-width: 1px;">
                        <div class="codeContent panelContent pdl">
                           <div class="sourceCode" id="cb5" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence">
                              <pre class="sourceCode java"><code class="sourceCode java"><span id="cb5-1"><a href="#cb5-1"></a>access_log_pattern=%h %l %u %t <span class="st">&quot;%r&quot;</span> %s %b <span class="st">&quot;%{Referer}i&quot;</span> <span class="st">&quot;%{User-Agent}i&quot;</span></span></code></pre>
                           </div>
                        </div>
                     </div>
                  </li>
               </ul>
               <p>By default, a modified version of the <a href="http://httpd.apache.org/docs/1.3/logs.html#combined">Apache combined log format</a> is enabled in the ESB as shown below. Note that the "X-Forwarded-For" header is appended to the beginning of the usually <strong>combined</strong> log format. This correctly identifies the original node that sent the request (in situations where requests go through a proxy such as a load balancer). The "X-Forwarded-For" header must be present in the incoming request for this to be logged.</p>
               <div class="code panel pdl" style="border-width: 1px;">
                  <div class="codeContent panelContent pdl">
                     <div class="sourceCode" id="cb6" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence">
                        <pre class="sourceCode java"><code class="sourceCode java"><span id="cb6-1"><a href="#cb6-1"></a>access_log_pattern=%{X-Forwarded-For}i %h %l %u %t \<span class="st">&quot;%r</span><span class="sc">\&quot;</span><span class="st"> %s %b </span><span class="sc">\&quot;</span><span class="st">%{Referer}i</span><span class="sc">\&quot;</span><span class="st"> </span><span class="sc">\&quot;</span><span class="st">%{User-Agent}i</span><span class="sc">\&quot;</span></span></code></pre>
                     </div>
                  </div>
               </div>
            </div>
         </td>
      </tr>
    </tbody>
    </table>

3.  Restart the server.

4.  Invoke a proxy service or REST API that is deployed in the Micro. For testing purposes, use the artifacts in the [quick start guide](../../quick-start-guide/quick-start-guide.md). The access log file for the service/API will be created in the
    `MI_HOME/repository/logs` directory. The default name of the log file is `http_access_.log` .

    !!! Tip
        Note that there will be delay in printing the logs to the log file.
    

### Supported log pattern formats for the PassThrough and NIO transports

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