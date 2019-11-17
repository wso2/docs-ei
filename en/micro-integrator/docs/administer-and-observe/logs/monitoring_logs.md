# Using Log Files

Logging is one of the most important aspects of a production-grade
server. A properly configured logging system is vital for identifying
errors, security threats, and usage patterns.

!!! Info
    **Java logging and Log4j2 integration:** In addition to the logs from libraries that use Log4j2, all logs from libraries that use the Java logging framework are also visible in the same log files. That is, when Java logging is enabled
        in Carbon, only the Log4j2 appenders will write to the log files. If
        the Java Logging Handlers have logs, these logs will be delegated to
        the log events of the corresponding Log4j2 appenders. A Pub/Sub
        registry pattern implementation has been used in the latter
        mentioned scenario to plug the handlers and appenders. The following
        default log4j2 appenders in the
        `           log4j2.properties          ` file are used for this
        implementation:

    - org.wso2.carbon.logging.appenders.CarbonConsoleAppender
    - org.wso2.carbon.logging.appenders.CarbonDailyRollingFileAppender

Listed below are the various log types used in WSO2 Micro Integrator.

!!! Info
    Separate log files are created in the `MI_HOME/repository/logs` directory for each of the log types given below.

## Carbon logs

The Carbon log file (`wso2carbon.log`) covers all the management features of a product. Carbon logs are configured in the `log4j2.properties` file (stored in the `MI_HOME/conf` directory).

The Carbon log file is enabled in the product by default as shown below. You can configure the details that are captured in this log file by [configuring the log4j2 properties](../logs/configuring_log4j_properties.md).

```xml
# CARBON_LOGFILE is set to be a DailyRollingFileAppender using a PatternLayout.
appender.CARBON_LOGFILE.type = RollingFile
appender.CARBON_LOGFILE.name = CARBON_LOGFILE
appender.CARBON_LOGFILE.fileName = ${sys:carbon.home}/repository/logs/wso2carbon.log
appender.CARBON_LOGFILE.filePattern = ${sys:carbon.home}/repository/logs/wso2carbon-%d{MM-dd-yyyy}.log
appender.CARBON_LOGFILE.layout.type = PatternLayout
appender.CARBON_LOGFILE.layout.pattern = [%d] %5p {%c} - %m%ex%n
appender.CARBON_LOGFILE.policies.type = Policies
appender.CARBON_LOGFILE.policies.time.type = TimeBasedTriggeringPolicy
appender.CARBON_LOGFILE.policies.time.interval = 1
appender.CARBON_LOGFILE.policies.time.modulate = true
appender.CARBON_LOGFILE.policies.size.type = SizeBasedTriggeringPolicy
appender.CARBON_LOGFILE.policies.size.size=10MB
appender.CARBON_LOGFILE.strategy.type = DefaultRolloverStrategy
appender.CARBON_LOGFILE.strategy.max = 20
appender.CARBON_LOGFILE.filter.threshold.type = ThresholdFilter
appender.CARBON_LOGFILE.filter.threshold.level = DEBUG
```

## Audit logs

Audit logs are used for tracking the sequence of actions that affect a particular task carried out on the server. These are also configured in the `log4j2.properties` file (stored in the `MI_HOME/conf` directory).

Audit logs are enabled in the product by default as shown below. You can configure the details that are captured in this log file by [configuring the log4j2 properties](../logs/configuring_log4j_properties.md).

```xml
# Appender config to AUDIT_LOGFILE
appender.AUDIT_LOGFILE.type = RollingFile
appender.AUDIT_LOGFILE.name = AUDIT_LOGFILE
appender.AUDIT_LOGFILE.fileName = ${sys:carbon.home}/repository/logs/audit.log
appender.AUDIT_LOGFILE.filePattern = ${sys:carbon.home}/repository/logs/audit-%d{MM-dd-yyyy}.log
appender.AUDIT_LOGFILE.layout.type = PatternLayout
appender.AUDIT_LOGFILE.layout.pattern = TID: [%d] %5p {%c} - %m%ex%n
appender.AUDIT_LOGFILE.policies.type = Policies
appender.AUDIT_LOGFILE.policies.time.type = TimeBasedTriggeringPolicy
appender.AUDIT_LOGFILE.policies.time.interval = 1
appender.AUDIT_LOGFILE.policies.time.modulate = true
appender.AUDIT_LOGFILE.policies.size.type = SizeBasedTriggeringPolicy
appender.AUDIT_LOGFILE.policies.size.size=10MB
appender.AUDIT_LOGFILE.strategy.type = DefaultRolloverStrategy
appender.AUDIT_LOGFILE.strategy.max = 20
appender.AUDIT_LOGFILE.filter.threshold.type = ThresholdFilter
appender.AUDIT_LOGFILE.filter.threshold.level = INFO
```

## Patch logs 

These logs contain details related to patches applied to the product. Patch logs cannot be customized.

## Wire logs 

You can enable wire logs to monitor HTTP messages that flow through the Micro Integrator. 

!!! Warning
    Please note that wire logs should be enabled for troubleshooting purposes only. It is not recommended to run production systems with wire logs enabled.

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

Enable wire logs by uncommenting the following parameter and [updating the log level](../configuring_log4j_properties/#setting-the-log-level) (which is DEBUG by default).

```xml
logger.synapse-transport-http-wire.name=org.apache.synapse.transport.http.wire
logger.synapse-transport-http-wire.level=DEBUG
```

Then, add to the loggers as a comma-separated list:
```xml
loggers = synapse-transport-http-wire, AUDIT_LOG, SERVICE_LOGGER, trace-messages,
```

## Service/Event Tracing logs 

These are logs that are enabled in WSO2 Micro Integrator for tracing services and events using a separate log file (`wso2carbon-trace-messages.log`). If server/event tracing logs are used in your product, you can configure them in the `log4j2.properties` file (stored in the `MI_HOME/conf` directory).

A separate log file for tracing services/events are enabled for certain WSO2 products in the `log4j2.properties` file using a specific appender. These logs are published to a file named `wso2carbon-trace-messages.log`.

```xml
# Appender config to CARBON_TRACE_LOGFILE
appender.CARBON_TRACE_LOGFILE.type = RollingFile
appender.CARBON_TRACE_LOGFILE.name = CARBON_TRACE_LOGFILE
appender.CARBON_TRACE_LOGFILE.fileName = ${sys:carbon.home}/repository/logs/wso2carbon-trace-messages.log
appender.CARBON_TRACE_LOGFILE.filePattern = ${sys:carbon.home}/repository/logs/wso2carbon-trace-messages-%d{MM-dd-yyyy}.log
appender.CARBON_TRACE_LOGFILE.layout.type = PatternLayout
appender.CARBON_TRACE_LOGFILE.layout.pattern = [%d] %5p {%c} - %m%ex%n
appender.CARBON_TRACE_LOGFILE.policies.type = Policies
appender.CARBON_TRACE_LOGFILE.policies.time.type = TimeBasedTriggeringPolicy
appender.CARBON_TRACE_LOGFILE.policies.time.interval = 1
appender.CARBON_TRACE_LOGFILE.policies.time.modulate = true
appender.CARBON_TRACE_LOGFILE.policies.size.type = SizeBasedTriggeringPolicy
appender.CARBON_TRACE_LOGFILE.policies.size.size=10MB
appender.CARBON_TRACE_LOGFILE.strategy.type = DefaultRolloverStrategy
appender.CARBON_TRACE_LOGFILE.strategy.max = 20
```

See instructions on [configuring the log4j2 properties](../logs/configuring_log4j_properties.md).

## HTTP Access logs

HTTP requests/responses are logged to monitor the activities related to an application's usage. HTTP access logs help you monitor information such as the persons who access the product, how many hits are received, what the errors are, etc. This information is useful for troubleshooting errors.

In the Micro Integrator, access logs can be generated for the PassThrough transport. The PassThrough transport works on 8290/8253 ports and is used for API/Service invocations. By default, the access logs from the PassThrough transport are written to a common access log file located in the `MI_HOME/repository/logs/` directory.

### Configuring access logs for the PassThrough transport (Service/API invocation)

!!! Note
    Access logs for the PassThrough transport logs the request and the response on **two** separate log lines.

By default, access logs related to service/API invocations are enabled. You can disable these access logs if required. Follow the steps given below to enable access logs for the PassThrough transport:

1.  Change the log level to `WARN` for the following entry in the `MI_HOME/conf/log4j2.properties` configuration file.

    ``` xml
    logger.PassThroughAccess.name = org.apache.synapse.transport.http.access
    logger.PassThroughAccess.level = INFO
    ```
    
    Then add the loggers as a comma-separated list:
    ```xml
    loggers = PassThroughAccess, AUDIT_LOG, SERVICE_LOGGER, trace-messages,
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
4.  Invoke a proxy service or REST API that is deployed in the Micro Integrator. The access log file for the service/API will be created in the `MI_HOME/repository/logs` directory. The default name of the log file is `http_access_.log` .

    !!! Tip
        Note that there will be a delay in printing the logs to the log file.

### Supported log pattern formats for the PassThrough transport

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
