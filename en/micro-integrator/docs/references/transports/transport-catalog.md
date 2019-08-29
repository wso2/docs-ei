# Transport Catalog

Given below is the list of transport parameters that can be configured for services.

## VFS Service-Level Parameters

The VFS transport does not have any global parameters to be configured. Rather, it has a set of service-level parameters that must be specified for each proxy service that uses the VFS transport.

<table>
   <thead>
      <tr>
         <th>
          Parameter
         </th>
         <th>
          Description
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td id="vfs-transport-file_url">
            transport.vfs.FileURI
         </td>
         <td>
            <div class="content-wrapper">
               The URI of the location of your files. This should be the source location of the files (if you are configuring the Micro Integrator to read files) or the destination of the files (if you are configuring the Micro Integrator to send files). You can specify connection-level parameters on the URL (see <a href="#vfs-url-parameters">VFS URL parameters</a> below).</p>
               When you need to access the absolute path of the URL, you can define the URL with <code>sftpPathFromRoot</code> as shown below. Also, note that <a href="#vfs-transport-avoid_permissions">transport.vfs.AvoidPermissionCheck</a> is a mandatory parameter for this URL when SFTP is used.
               <div class="code panel pdl" style="border-width: 1px;">
                  <div class="codeContent panelContent pdl">
                     <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence">
                        <pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;parameter name=<span class="st">&quot;transport.vfs.FileURI&quot;</span>&gt;sftp:<span class="co">//[ username[: password]@] hostname[: port][ absolute-path]?sftpPathFromRoot=true;transport.vfs.AvoidPermissionCheck=true&lt;/parameter&gt;</span></span></code></pre>
                     </div>
                  </div>
               </div>
            </div>
            You need to specify a valid file URI in the following form: <code>file://path</code>.
         </td>
      </tr>
      <tr>
         <td>
           transport.vfs.ContentType
         </td>
         <td>
            Content type of the files processed by the transport. To specify the encoding, follow the content type with a semi-colon and the character set. For example: <code>parameter name="transport.vfs.ContentType“text/plain;charset=UTF-32/parameter</code>.</br></br>
            When writing a file, you can set a different encoding with the <code>CHARACTER_SET_ENCODING</code> property: <code>property name="CHARACTER_SET_ENCODING" value="UTF-8" scope="axis2" type="STRING"/</code>.</br>
            Specify a valid content type. For example, <code>text/xml</code>. You can specify the encoding after the content type, such as <code>text/plain;charset=UTF-32</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.FileNamePattern
         </td>
         <td>
            If the VFS listener should process only a subset of the files available at the specified file URI location, use this parameter to select those files by name using a regular expression. Specify a regular expression. For example: <code>*\.xml</code>.
         </td>
      </tr>
      <tr>
         <td>
          transport.PollInterval
         </td>
         <td>
            The polling interval for the transport receiver to poll the file URI location. The value is expressed in seconds unless you add "ms" for milliseconds. For example, "2" or "2000ms" to specify 2 seconds. Specify a positive integer.
         </td>
      </tr>
      <tr>
         <td>
           transport.vfs.ActionAfterProcess
         </td>
         <td>
           Whether to move, delete or take no action on the files after the transport has processed them. Possible values are <code>MOVE</code>, <code>DELETE</code>, or <code>NONE</code>. The default value is <code>DELETE</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.ActionAfterFailure
         </td>
         <td>
            Whether to move, delete or take no action on the files if a failure occurs. Possible values are <code>MOVE</code>, <code>DELETE</code>, or <code>NONE</code>. The default value is <code>DELETE</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.MoveAfterProcess
         </td>
         <td>
            Where to move the files after processing if **ActionAfterProcess** is MOVE. This parameter is required if the <b>ActionAfterProcess</b> is <code>MOVE</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.MoveAfterFailure
         </td>
         <td>
            Where to move the files after processing if **ActionAfterFailure** is MOVE. This parameter is required if the <b>ActionAfterFailure</b> is <code>MOVE</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.ReplyFileURI
         </td>
         <td>
           The location where reply files should be written by the transport.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.ReplyFileName
         </td>
         <td>
            The name for reply files written by the transport. The default value is <code>response.xml</code>.
         </td>
      </tr>
      <tr>
         <td>
           transport.vfs.MoveTimestampFormat
         </td>
         <td>
            The pattern/format of the timestamps added to file names as prefixes when moving files. Specify a valid <a href="http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html">timestamp pattern</a>. For example, <code>yyyy-MM-dd'T'HH:mm:ss.SSSZ</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.Streaming
         </td>
         <td>
            Whether files should be transferred in streaming mode, which is useful when transferring large files. The default setting is <code>false</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.ReconnectTimeout
         </td>
         <td>
            Reconnect timeout value in seconds to be used in case of an error when transferring files. Specify a positive integer. The default values is <code>30</code> seconds.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.MaxRetryCount
         </td>
         <td>
            Maximum number of retry attempts to carry out in case of errors. Specify a positive integer. The default values is <code>3</code>.
         </td>
      </tr>
      <tr>
         <td>
           transport.vfs.Append
         </td>
         <td>
            When writing the response to a file, whether the response should be appended to the response file instead of overwriting the file. This value should be defined as a query parameter in the out/reply file URI. For example:<br />
               <code>vfs:file:///home/user/test/out?transport.vfs.Append=true</code><br />
            or,
            <code>parameter name="transport.vfs.ReplyFileURI"file:///home/user/test/out?transport.vfs.Append=true/parameter</code>.<br />
            By default, the parameter is set to <code>false</code> (the response file will be completely overwritten).
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.MoveAfterFailedMove
         </td>
         <td>
           Where to move the failed file.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.FailedRecordsFileName
         </td>
         <td>
            The name of the file that maintains the list of failed files. Example file name: vfs-move-failed-records.properties.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.FailedRecordsFileDestination
         </td>
         <td>
            Where to store the failed records file.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.MoveFailedRecordTimestampFormat
         </td>
         <td>
            Entries in the failed records file include the name of the file that failed and the timestamp of its failure. This property configures the time stamp format. Specify a valid <a href="http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html">timestamp pattern</a>. For example, <code>yyyy-MM-dd'T'HH:mm:ss.SSSZ</code>. By default, the parameter is set to <code>dd-MM-yyyy HH:mm:ss</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.FailedRecordNextRetryDuration
         </td>
         <td>
           The time in milliseconds to wait before retrying the move task. Specify a positive integer. The default value is <code>3000</code> miliseconds.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.Locking
         </td>
         <td>
            By default, file locking is enabled in the VFS transport. This parameter lets you configure the locking behavior on a per service basis. You can also disable locking globally by specifying the parameter at the receiver level and selectively enable locking only for a set of services. The setting is enabled by default.
         </td>
      </tr>
      <tr>
         <td>transport.vfs.FileProcessCount
         </td>
         <td>This setting allows you to throttle the VFS listener by processing files in batches. Specify the number of files you want to process in each batch. Specify a positive integer such as <code>10</code>.</br></br>
          <b>Note</b>: If you specify the <code>transport.vfs.FileProcessCount</code> parameter, you do not need to specify the <code>transport.vfs.FileProcessInterval</code> parameter in a configuration, and vice versa. These two parameters cannot be used at the same time.</td>
      </tr>
      <tr>
         <td>transport.vfs.FileProcessInterval
         </td>
         <td>The interval in milliseconds between two file processes. Specify a positive integer, such as <code>10</code></td>
      </tr>
      <tr>
         <td>transport.vfs.ClusterAware</td>
         <td>Whether VFS coordination support is enabled in a clustered deployment or not. By default, this setting is set to <code>false</code>.</td>
      </tr>
      <tr>
         <td>transport.vfs.FileSizeLimit</td>
         <td>Only file sizes that are less than or equal to the defined limit are processed. Specify the file size in bytes. The default value is <code>-1</code>(unlimited file size).</td>
      </tr>
      <tr>
         <td>transport.vfs.AutoLockReleaseInterval</td>
         <td>
            The timeout value for stale locks where the VFS transport will ignore those file locks once the defined time period is reached. The time period is calculated from the time the lock is created to the time you attempt to access it. If you need stale locks to never timeout provide -1 as the timeout value. Specify the time in miliseconds. The default value is <code>20000</code>.
         </td>
      </tr>
      <tr>
         <td>transport.vfs.SFTPIdentities</td>
         <td>Location of the private key.</td>
      </tr>
      <tr>
         <td>transport.vfs.SFTPIdentityPassPhrase</td>
         <td>Passphrase of the private key.</td>
      </tr>
      <tr>
         <td>transport.vfs.SFTPUserDirIsRoot</td>
         <td>If the SFTP user directory should be treated as root. By default, this parameter is set to <code>true</code>.</td>
      </tr>
      <tr>
         <td>transport.vfs.ResolveHostsDynamically</td>
         <td>
            Whether hostnames should be resolved at the time of deployment or whether it is necessary to resolve hostnames dynamically at runtime. By default hostnames are resolved at the time of deployment. If you want to resolve hostnames at runtime, set this parameter to <code>true</code>.
            <b>Note</b>: Resolving hostnames at runtime is only possible for the Server Message Block (SMB) protocol. </br>
            By default, this setting is <code>false</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.vfs.DistributedLock
         </td>
         <td>
            This applies only in cluster deployments. Set to <code>true</code> if you need to avoid multiple servers trying to process the same file simultaneously.
         </td>
      </tr>
      <tr>
         <td>
          transport.vfs.DistributedTimeout
         </td>
         <td>
            The timeout period in seconds for the distributed lock. Specify a positive integer, such as <code>10</code>.
         </td>
      </tr>
      <tr>
         <td>
          transport.vfs.AutoLockRelease
         </td>
         <td>
            Set to <code>true</code> if you need to release locking in order to avoid files not being processed due to faulty locking. This works together with the <code>transport.vfs.AutoLockReleaseInterval</code> and <code>transport.vfs.LockReleaseSameNode</code> parameters.
         </td>
      </tr>
      <tr>
         <td>
          transport.vfs.LockReleaseSameNode
         </td>
         <td>Set to <code>true</code> if you need to release the locks only accrued by the same worker node.<br />
            If this is set to <code>false</code>, locks accrued by other nodes will be released according to the value specified in <code>transport.vfs.AutoLockReleaseInterval</code>. By default, this setting is <code>true</code>.
         </td>
      </tr>
      <tr>
         <td>transport.vfs.FileSortAttribute</td>
         <td>The attribute by which the files should be sorted and processed. The possible values are <code>NONE</code>, <code>Size</code>, or <code>Lastmodifiedtimestamp</code>.</td>
      </tr>
      <tr>
         <td>transport.vfs.FileSortAsscending</td>
         <td>
            The sort order to sort and process the files. If set to <code>true</code>, files will be sorted in ascending order based on the attribute you specify in <code>transport.vfs.FileSortAttribute</code>. By default, this setting is <code>true</code>.
         </td>
      </tr>
      <tr>
         <td>transport.vfs.CreateFolder</td>
         <td>
            Set to <code>true</code> to create a folder if a folder does not exist when moving files. By default, this setting is <code>false</code>.
         </td>
      </tr>
      <tr>
         <td>transport.vfs.SubFolderTimestampFormat</td>
         <td>
            The pattern/format of the timestamps added to the folder structure when moving files. You need to set <code>transport.vfs.CreateFolder</code> to <code>true</code> in order to specify a value for this parameter. Specify a valid <a href="http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html">timestamp pattern</a>. For example, <code>yyyy-MM-dd'T'HH:mm:ss.SSSZ</code>.
         </td>
      </tr>
      <tr>
         <td>transport.vfs.Build</td>
         <td>
            Set to <code>true</code> if you need to build the content inside the file before injecting the file to the mediation engine. If there is a build error, the file will not be injected to the mediation engine. By default, this setting is <code>false</code>.
         </td>
      </tr>
   </tbody>
</table>

The following service-level parameters are required for Inbound Endpoints.

<table>
   <tr>
    <th>
      Parameter
    </th>
    <th>
      Description
    </th>
  </tr>
   <tbody>
      <tr>
         <td>interval</td>
         <td>The time duration in milliseconds between two file scans that checks for updates.</td>
      </tr>
      <tr>
         <td>sequential</td>
         <td>Files will be processed sequentially when this parameter is set to true.</td>
      </tr>
      <tr>
         <td>coordination</td>
         <td>
            <p>This should be true for clustered deployments in order to prevent two nodes from retrieving the same file.</p>
         </td>
      </tr>
   </tbody>
</table>

## VFS URL Parameters

When you use the [transport.vfs.FileURI](#vfs-transport-file_url) parameter, you can set connection-specific VFS parameters as URL query parameters. For example, to use SFTP with SSL, you could specify the URL as shown below. Note that [transport.vfs.AvoidPermissionCheck](#vfs-transport-avoid_permissions) is a mandatory parameter for this URL when SFTP is used.

```
<parameter name="transport.vfs.FileURI">vfs:ftps://test:test123@10.200.2.63/vfs/in?vfs.ssl.keystore=/home/user/openssl/keystore.jks&amp;vfs.ssl.truststore=/home/user/openssl/vfs-truststore.jks&amp;vfs.ssl.kspassword=importkey&amp;vfs.ssl.tspassword=wso2vfs&amp;vfs.ssl.keypassword=importkey;transport.vfs.AvoidPermissionCheck=true</parameter>
```
<table>
   <thead>
      <tr>
         <th>
           Parameter
         </th>
         <th>
           Description
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td id="vfs-transport-avoid_permissions">
            transport.vfs.AvoidPermissionCheck
         </td>
         <td>
            <b>Be sure</b> to set this parameter to <strong>true</strong> for an SFTP connection. This is because (by default) the VFS transport checks whether the user has permission to access the location of the files (the source location or the destination). However, since the system is reading files in an external server through the SFTP connection, this permission check is not required and should be avoided.
         </td>
      </tr>
      <tr>
         <td>
           vfs.passive
         </td>
         <td>
           Enable FTP passive mode. This is required when the FTP client and server are not in the same network. By default, this setting is <code>false</code>.
         </td>
      </tr>
      <tr>
         <td>transport.vfs.Append</td>
         <td>
            If a file with same name exists, this parameter tells whether to create a new file and write content or append content to existing file. By default, this setting is <code>false</code>.
         </td>
      </tr>
      <tr>
         <td>vfs.protection</td>
         <td>
            Set data channel protection level using FTP PROT command. Possible values are as follows:
            <ul>
               <li>C - Clear</li>
               <li>S - Safe(SSL protocol only)</li>
               <li>E - Confidential(SSL protocol only)</li>
               <li>P - Private</li>
            </ul>
            The default value is <code>C</code>.
         </td>
         <td>
      </tr>
      <tr>
         <td>vfs.ssl.keystore</td>
         <td>Private key store to use for mutual SSL. Your keystore must be signed by a certificate authority. For more information, see <a href="index">http://docs.oracle.com/cd/E19509-01/820-3503/ggfen/index.html</a>. Possible value: String (Path of keystore).</td>
      </tr>
      <tr>
         <td>vfs.ssl.kspassword</td>
         <td>Private key store password.</td>
      </tr>
      <tr>
         <td>vfs.ssl.keypassword</td>
         <td>Private key password</td>
      </tr>
      <tr>
         <td>vfs.ssl.truststore</td>
         <td>Trust store to use for SFTP</td>
      </tr>
      <tr>
         <td>vfs.ssl.tspassword</td>
         <td>Trust store password</td>
      </tr>
      <tr>
         <td>transport.vfs.CreateFolder</td>
         <td>If the directory does not exists create and write the file. The default setting is <code>false</code>.</td>
      </tr>
      <tr>
         <td>transport. vfs.SendFileSynchronously</td>
         <td>
            Whether to send files synchronously to the file host. When this parameter is set to <code>true</code>, files will be sent one after another to the file host. This synchronous write can be configured on a per host basis. The default setting is <code>false</code>.
         </td>
      </tr>
   </tbody>
</table>

**Configuring a Proxy Server over FTP and SFTP Connections**

Proxy server specific parameters can be set as URL query parameters. For example, to use Proxy over FTP, you could specify the URL as follows:

```
ftp://username:password@127.0.0.1/home/wso2/res?proxyServer=127.0.0.1&proxyPort=3128&proxyUsername=proxyuser&proxyPassword=proxyPass&timeout=2500&retryCount=3
```

Following are the URL parameters you can set:

<table>
   <thead>
      <tr>
         <th>
            Parameter
         </th>
         <th>
            Description
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>
            <p>proxyServer</p>
         </td>
         <td>
            The IP address of the proxy server. Possible value: <code>127.0.0.1</code>. This parameter is set to <code>false</code> by default.
         </td>
      </tr>
      <tr>
         <td>
            <p>proxyPort</p>
         </td>
         <td>The port number on which the proxy server is listening for requests. Possible value: <code>1328</code>. This parameter is set to <code>false</code> by default.</td>
      </tr>
      <tr>
         <td>proxyType</td>
         <td>
            The proxy server type. This can either be <code>HTTP</code> or <code>SOCKS</code>. The default is <code>SOCKS</code>.
            <b>Note</b>: In a configuration, if the proxy server type is not specified or an unknown proxy server type is specified, the <code>proxyType</code> will be considered as <code>HTTP</code>.
         </td>
      </tr>
      <tr>
         <td>
            proxyUsername
         </td>
         <td>The user name of the proxy server.</td>
      </tr>
      <tr>
         <td>
            proxyPassword
         </td>
         <td>The password of the proxy server.</td>
      </tr>
      <tr>
         <td>
           timeout
         </td>
         <td>
            The connection timeout in milliseconds. Possible value: <code>1000</code>. The default value is <code>5000</code>.
         </td>
      </tr>
      <tr>
         <td>
           retryCount
         </td>
         <td>The number of retry attempts in case of a connection timeout. Possible value: <code>3</code>. The default value is <code>5</code>.</td>
      </tr>
   </tbody>
</table>

## FIX Parameters

<table>
      <tr>
         <th>
          Parameter
         </th>
         <th>
           Description
         </th>
      </tr>
   <tbody>
      <tr>
         <td>
			transport.fix.AcceptorConfigURL
         </td>
         <td>
            URL to the Quickfix/J acceptor configuration file (see notes below).</br></br> This is required for receiving messages over FIX.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.InitiatorConfigURL
         </td>
         <td>
            URL to the Quickfix/J initiator configuration file (see notes below).</br></br>
            Required for sendingmessages over FIX.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.AcceptorLogFactory
         </td>
         <td>
            Log factory implementation to be used for the FIX acceptor (Determines how logging is done at the acceptor level).</br></br>
            Possible values are <code>console</code>, <code>file</code>, and <code>jdbc</code>.</br></br>
            Logging is disabled by default.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.InitiatorLogFactory
         </td>
         <td>
            Log factory implementation to be used for the FIX acceptor (Determines how logging is done at the acceptor level).</br></br>
            Possible values are <code>console</code>, <code>file</code>, and <code>jdbc</code>.</br></br>
            Logging is disabled by default.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.AcceptorMessageStore
         </td>
         <td>
            Message store mechanism to be used with the acceptor (determines how the FIX message store is maintained).</br></br>
            Possible values are <code>memory</code>, <code>file</code>, <code>sleepycat</code>, and <code>jdbc</code>.</br></br>
            The default value is <code>memory</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.InitiatorMessageStore
         </td>
         <td>
            Message store mechanism to be used with the initiator (determines how the FIX message store is maintained).</br></br>
            Possible values are <code>memory</code>, <code>file</code>, <code>sleepycat</code>, and <code>jdbc</code>.</br></br>
            The default value is <code>memory</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.ResponseDeliverToCompID
         </td>
         <td>
            If the response FIX messages should be delivered to a location different from the location from where the request originated, use this property to set the DeliverToCompID field of the FIX messages.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.ResponseDeliverToSubID
         </td>
         <td>
            If the response FIX messages should be delivered to a location different from the location from where the request originated, use this property to set the DeliverToSubID field of the FIX messages.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.ResponseDeliverToLocationID
         </td>
         <td>
            If the response FIX messages should be delivered to a location different from the location from where the request was originated, use this property to set the DeliverToLocationID field of the FIX messages.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.endAllToInSequence
         </td>
         <td>
            By default, all received FIX messages (including responses) will be directed to the in sequence of the proxy service. Use this property to override that behavior.<br/><br/>
            By default, this setting is <code>true</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.BeginStringValidation
         </td>
         <td>
            Whether the transport should validate <code>BeginString</code> values when forwrding FIX messages across sessions.<br/><br/>
            By default, this setting is <code>true</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.DropExtraResponses
         </td>
         <td>
            In situations where the FIX recipient sends multiple responses per request, use this parameter to drop excessive responses and use only the first one.<br/><br/>
            By default, this setting is <code>false</code>.
         </td>
      </tr>
   </tbody>
</table>

## Service-level RabbitMQ Parameters

### Required Parameters (Receiving Messages)

<table>
  <tr>
    <th>Parameter</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>rabbitmq.connection.factory</td>
    <td>The name of the connection factory.</td>
  </tr>
  <tr>
    <td>rabbitmq.exchange.name</td>
    <td>
      Name of the RabbitMQ exchange to which the queue is bound. Use this parameter instead of <code>rabbitmq.queue.routing.key</code> if you need to use the default exchange and publish to a queue.
    </td>
  </tr>
  <tr>
    <td>rabbitmq.queue.name</td>
    <td>The queue name to send or consume messages. If you do not specify this parameter, you need to specify the <code>rabbitmq.queue.routing.key</code> parameter.</td>
  </tr>
</table>

### Optional Parameters (Receiving Messages)

<table>
   <tr>
      <th>Parameter</th>
      <th>Description</th>
   </tr>
   <tbody>
      <tr>
         <td>rabbitmq.queue.auto.ack</td>
         <td>
            Defines how the message processor sends the acknowledgement when consuming messages recived from the RabbitMQ message store. If you set this to true, the message processor automatically sends the acknowledgement to the messages store as soon as it receives messages from it. This is called an auto acknowledgement.
            If you set it to <code>false</code>, the message processor waits until it receives the response from the backend to send the acknowledgement to the mssage store. This is called a client acknowledgement.
         </td>
      </tr>
      <tr>
         <td>rabbitmq.consumer.tag</td>
         <td>The client­ generated consumer tag to establish context.</td>
      </tr>
      <tr>
         <td>rabbitmq.channel.consumer.qos</td>
         <td>
            The consumer QoS value. You need to specify this parameter only if the <code>rabbitmq.queue.auto.ack</code> parameter is set to <code>false</code>.
         </td>
      </tr>
      <tr>
         <td>rabbitmq.queue.durable</td>
         <td>Whether the queue should remain declared even if the broker restarts.</td>
      </tr>
      <tr>
         <td>rabbitmq.queue.exclusive</td>
         <td>Whether the queue should be exclusive or should be consumable by other connections.</td>
      </tr>
      <tr>
         <td>rabbitmq.queue.auto.delete</td>
         <td>Whether to keep the queue even if it is not being consumed anymore.</td>
      </tr>
      <tr>
         <td>rabbitmq.queue.routing.key</td>
         <td>The routing key of the queue.</td>
      </tr>
      <tr>
         <td>rabbitmq.queue.autodeclare</td>
         <td>Whether to create queues if they are not present. However, you should set this parameter only if queues are not declared prior on the broker. Setting this parameter in the publish URL to <code>false</code> improves RabbitMQ transport performance.</td>
      </tr>
      <tr>
         <td>rabbitmq.exchange.autodeclare</td>
         <td>Whether to create exchanges if they are not present. However, you should set this parameter only if exchanges are not declared prior on the broker. Setting this parameter in the publish URL to <code>false</code> improves RabbitMQ transport performance.</td>
      </tr>
      <tr>
         <td>rabbitmq.exchange.type</td>
         <td>The type of the exchange.</td>
      </tr>
      <tr>
         <td>rabbitmq.exchange.durable></td>
         <td>Whether the exchange should remain declared even if the broker restarts.</td>
      </tr>
      <tr>
         <td>rabbitmq.exchange.auto.delete</td>
         <td>
            Whether to keep the exchange even if it is not bound to any queue anymore.
         </td>
      </tr>
      <tr>
         <td>rabbitmq.message.content.type</td>
         <td>
            The content type of the consumer. <b>Note</b>: If the content type is specified in the message, this parameter does not override the specified content type. The default value is <code>text/xml</code>.
         </td>
      </tr>
   </tbody>
</table>

### Required Parameters (Sending Messages)

<table>
  <tr>
    <th>Parameter</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>rabbitmq.server.host.name</td>
    <td>Host name of the server.</td>
  </tr>
  <tr>
    <td>rabbitmq.server.port</td>
    <td>Port number of the server.</td>
  </tr>
</table>

### Optional Parameters (Sending Messages)

<table>
   <tr>
    <th>Parameter</th>
    <th>Description</th>
   </tr>
   <tbody>
      <tr>
         <td>rabbitmq.exchange.name</td>
         <td>
            The name of the RabbitMQ exchange to which the queue is bound. Use this parameter instead of <code>rabbitmq.queue.routing.key</code>, if you need to use the default exchange and publish to a queue.
         </td>
      </tr>
      <tr>
         <td>rabbitmq.queue.routing.key</td>
         <td>The exchange and queue binding key that will be used to route messages.</td>
      </tr>
      <tr>
         <td>rabbitmq.replyto.name</td>
         <td>The name of the call back­ queue. Specify this parameter if you expect a response.</td>
      </tr>
      <tr>
         <td>rabbitmq.queue.delivery.mode</td>
         <td>
            The delivery mode of the queue. Possible values are 1 and 2.<br/>
               1 - Non­-persistent.<br/>
               2 - Persistent. This is the default value.
         </td>
      </tr>
      <tr>
         <td>rabbitmq.exchange.type</td>
         <td>The type of the exchange.</td>
      </tr>
      <tr>
         <td>rabbitmq.queue.name</td>
         <td>The queue name to send or consume messages. If you do not specify this parameter, you need to specify the <code>rabbitmq.queue.routing.key</code> parameter.</td>
      </tr>
      <tr>
         <td>rabbitmq.queue.durable</td>
         <td>Whether the queue should remain declared even if the broker restarts. The default value is <code>false</code>.</td>
      </tr>
      <tr>
         <td>rabbitmq.queue.exclusive</td>
         <td>Whether the queue should be exclusive or should be consumable by other connections. The default value is <code>false</code>.</td>
      </tr>
      <tr>
         <td>rabbitmq.queue.auto.delete</td>
         <td>Whether to keep the queue even if it is not being consumed anymore. The default value is <code>false</code>.</td>
      </tr>
      <tr>
         <td>rabbitmq.exchange.durable</td>
         <td>Whether the exchange should remain declared even if the broker restarts.</td>
      </tr>
      <tr>
         <td>rabbitmq.queue.autodeclare</td>
         <td>
            Whether to create queues if they are not present. However, you should set this parameter only if queues are not declared prior on the broker. Setting this parameter in the publish URL to <code>false</code> improves RabbitMQ transport performance.
         </td>
      </tr>
      <tr>
         <td>rabbitmq.exchange.autodeclare</td>
         <td>Whether to create exchanges if they are not present. However, you should set this parameter only if exchanges are not declared prior on the broker. Setting this parameter in the publish URL to <code>false</code> improves RabbitMQ transport performance.</td>
      </tr>
      <tr>
         <td>rabbitmq.message.correlation.id</td>
         <td>
            The correlation ID is required to identify a message that comes through one queue and requires a response back via another queue. This ID helps you map the messages and is unique for every request.
         </td>
      </tr>
      <tr>
         <td>
          rabbitmq.message.id
         </td>
         <td>Every message has its own unique message ID.</td>
      </tr>
      <tr>
        <td>CachedRabbitMQConnectionFactory</td>
        <td>
           This parameter increases the performance and provides higher throughput in message delivery.
        </td>
      </tr>
   </tbody>
</table>

### Optional Parameters (Connection Recovery)

In case of a network failure or broker shutdown, the Micro Integrator will try to recreate the connection.

<table>
  <tr>
    <th>Parameter</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>rabbitmq.connection.retry.interval</td>
    <td>
      The retry interval specifies how frequently (time interval) the Micro Integrator should retry to recreate a lost connection. The default value is <code>30000</code> ms. That is, the Micro Integrator retries to connect every 30000 miliseconds.
    </td>
  </tr>
  <tr>
    <td>rabbitmq.connection.retry.count</td>
    <td>
      The retry count specifies the number of times the Micro Integrator will try to recreate a lost connection. The default retry count is <code>3</code>. That is, the Micro Integrator retries only 3 times.
    </td>
  </tr>
  <tr>
    <td>rabbitmq.server.retry.interval</td>
    <td>
      This parameter is <b>optional</b>.</br>
      The parameters specified above sets the retry interval with which the RabbitMQ client tries to reconnect. Generally having this value less than the value specified as <code>rabbitmq.connection.retry.interval</code> will help synchronize the reconnection of the Micro Integrator and the RabbitMQ client.
    </td>
  </tr>
</table>

### Optional Parameters (SSL)

To enable SSL support in RabbitMQ, you need to configure the following parameters.

<table>
  <tr>
    <th>Parameter</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>rabbitmq.connection.ssl.enabled</td>
    <td>
       Specifies whether SSL is enabled for the connection.
    </td>
  </tr>
  <tr>
    <td>rabbitmq.connection.ssl.version</td>
    <td>
       The SSL protocols that are supported.
    </td>
  </tr>
  <tr>
    <td>rabbitmq.connection.ssl.keystore.location</td>
    <td>
       The location of the keystore that is used.
    </td>
  </tr>
  <tr>
    <td>rabbitmq.connection.ssl.keystore.type</td>
    <td>
       The type of keystore.
    </td>
  </tr>
  <tr>
    <td>rabbitmq.connection.ssl.keystore.password</td>
    <td>
       The keystore password.
    </td>
  </tr>
  <tr>
    <td>rabbitmq.connection.ssl.truststore.location</td>
    <td>
       The location of the truststore.
    </td>
  </tr>
  <tr>
    <td>rabbitmq.connection.ssl.truststore.type</td>
    <td>
       The type of the truststore.
    </td>
  </tr>
  <tr>
    <td>rabbitmq.connection.ssl.truststore.password</td>
    <td>
       The password of the keystore.
    </td>
  </tr>
</table>

## HTTP/HTTPS Servlet Parameters

Similar to the servlet HTTP transport, this transport is also based on Apache Tomcat's connector implementation. Please refer Tomcat *connector
configuration reference* for a complete list of supported parameters.

## JMS Parameters

### JMS connection factory parameters

Configuration parameters for the JMS receiver and the sender are XML fragments that represent JMS connection factories.

<table>
      <tr>
         <th>
            <p>Parameter Name</p>
         </th>
         <th>
            <p>Description</p>
         </th>
      </tr>
   <tbody>
      <tr>
         <td>
           java.naming.factory.initial
         </td>
         <td>
            JNDI initial context factory class. The class must implement the <code>java.naming.spi.InitialContextFactory</code> interface. The default value is <code>org.apache.activemq.jndi.ActiveMQInitialContextFactory</code>.
         </td>
      </tr>
      <tr>
         <td>
            java.naming.provider.url
         </td>
         <td>
            URL of the JNDI provider. By default, the value is <code>tcp://localhost:61616</code>.
         </td>
      </tr>
      <tr>
         <td>
            java.naming.security.principal
         </td>
         <td>
            <p>JNDI Username.</p>
         </td>
      </tr>
      <tr>
         <td>
            java.naming.security.credentials
         </td>
         <td>
            <p>JNDI password.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.Transactionality
         </td>
         <td>
            Preferred mode of transactionality.</br>
            <b>Note</b>: In the Micro Integrator, JMS transactions only work with either the Callout mediator or the Call mediator in blocking mode. The possible values are as follows:
            <ul>
               <li><b>none</b>: Disables transactions in the JMS transport.</li>
               <li><b>local</b>: Enables local JMS session transactions.</li>
               <li><b>jta</b>: Enables global JTA transactions.</li>
            </ul>
         </td>
      </tr>
      <tr>
         <td>
            transport.UserTxnJNDIName
         </td>
         <td>
           JNDI name to be used to require user transaction. The default value is <code>java:comp/UserTransaction</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.CacheUserTxn
         </td>
         <td>
            Whether caching for user transactions should be enabled or not. By default, this setting is <code>true</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.SessionTransacted
         </td>
         <td>
            Whether the JMS session should be transacted or not. By default, this setting is <code>true</code> (if transactionality is 'local').
         </td>
      </tr>
      <tr>
         <td>
           transport.jms.SessionAcknowledgement
         </td>
         <td>
           JMS session acknowledgment mode. The possible values are as follows:
           <ul>
               <li><em>AUTO_ACKNOWLEDGE:</em> The session automatically acknowledges the consumer receipt of messages when message processing has finished.</li>
               <li><em>CLIENT_ACKNOWLEDGE:</em> The consumer acknowledges all messages delivered so far by the session. If the consumer falls behind in its processing, a large number of unacknowledged messages can build up.</li>
               <li><em>DUPS_OK_ACKNOWLEDGE:</em> The session lazily acknowledges the delivery of messages to the consumer. Lazy means that the consumer can delay acknowledgement to the server until a convenient time. During a delay, the server might redeliver messages. This mode reduces session overhead but the consumer can receive duplicate messages should JMS fail,</li>
               <li><em>SESSION_TRANSACTED:</em> The session is a related group of consumed or produced messages that are treated as a single unit of work.</li>
            </ul>
            Also see <a href="http://wso2.com/library/articles/2013/01/jms-message-delivery-reliability-acknowledgement-patterns/">JMS Message Delivery Reliability and Acknowledgement Patterns</a>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.ConnectionFactoryJNDIName
         </td>
         <td>
            The JNDI name of the connection factory.</br></br>
            The possible values are as follows:
            <ul>
               <li>QueueConnectionFactory</li>
               <li>TopicConnectionFactory</li>
            </ul>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.ConnectionFactoryType
         </td>
         <td>
           Type of the connection factory. The possible values are <code>queue</code>, or <code>topic</code>. The default value is <code>queue</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.JMSSpecVersion
         </td>
         <td>
            JMS API version. Possible values are <code>1.1</code> or <code>1.0.2b</code>. The default value is <code>1.1</code>.
         </td>
      </tr> 
      <tr>
         <td>
            transport.jms.UserName
         </td>
         <td>
            The JMS connection username.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.Password
         </td>
         <td>
            The JMS connection password.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.Destination
         </td>
         <td>
            The JNDI name of the destination.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.DestinationType
         </td>
         <td>
            Type of the destination. Possible values are <code>queue</code>, or <code>topic</code>. The default setting is <code>queue</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.DefaultReplyDestination
         </td>
         <td>
            JNDI name of the default reply destination.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.DefaultReplyDestinationType
         </td>
         <td>
            Type of the reply destination. Possible values are <code>queue</code>, or <code>topic</code>. Defaults to the type of the destination.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.MessageSelector
         </td>
         <td>
            Message selector implementation.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.SubscriptionDurable
         </td>
         <td>
            Whether the connection factory is subscription durable or not. By default, this parameter is set to <code>false</code>.
         </td>
      </tr>
      <tr>
         <td>transport.jms.DurableSubscriberClientID</td>
         <td>The <code>ClientId</code> parameter when using durable subscriptions. This parameter is required if the value specified as <code>transport.jms.ubscriptionDurable</code> is <code>true</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.DurableSubscriberName
         </td>
         <td>
            The name of the durable subscriber. This parameter is required if the value specified as <code>transport.jms.SubscriptionDurable</code> is <code>true</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.PubSubNoLocal
         </td>
         <td>
            Whether the messages should be published by the same connection through which they were received. The default setting is <code>false</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.CacheLevel
         </td>
         <td>
            The cache level with which JMS objects should be cached at start up. You can configure this in the ei.toml file if Micro Integrator acts as a JMS producer. Example:
            <div class="code panel pdl" style="border-width: 1px;">
                  <div class="codeContent panelContent pdl">
                     <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence">
                        <pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;endpoint&gt;</span>
<span id="cb1-2"><a href="#cb1-2"></a>   &lt;address uri=<span class="st">&quot;jms:/example.MyQueue?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.wso2.andes.jndi.PropertiesFileInitialContextFactory&amp;java.naming.provider.url=repository/conf/jndi.properties&amp;transport.jms.DestinationType=queue&amp;transport.jms.CacheLevel=producer&quot;</span>/&gt;</span>
<span id="cb1-3"><a href="#cb1-3"></a>&lt;/endpoint&gt;</span></code></pre>
                     </div>
                  </div>
            </div>
            If the Micro Integrator is a JMS consumer, you can configure a proxy service. Following are the possible values for this parameter:
            <ul>
               <li><strong>none</strong>: None of the JMS objects will be cached.</li>
               <li><strong>connection</strong>: JMS connection objects will be cached.</li>
               <li><strong>session</strong>: JMS connection and session objects will be cached.</li>
               <li><strong>consumer</strong>: JMS connection, session, and consumer objects will be cached.</li>
               <li><strong>producer</strong>: JMS connection, session, and producer objects will be cached.</li>
               <li><strong>auto</strong>: An appropriate cache level will be used based on the transaction strategy.</li>
            </ul>
            </div>
         By default, this parameter is set to <code>auto</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.ReceiveTimeout
         </td>
         <td>
            Time to wait for a JMS message during polling. Set this parameter value to a negative integer to wait indefinitely. Set to zero to prevent waiting. The default value is <code>1000</code> milliseconds.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.ConcurrentConsumers
         </td>
         <td>
            Number of concurrent threads to be started to consume messages when polling. You can specify any positive integer. However, for topics, this parameters should always be set to <code>1</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.MaxConcurrentConsumers
         </td>
         <td>
            Maximum number of concurrent threads to use during polling. You can specify any positive integer. The default value is <code>1</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.IdleTaskLimit
         </td>
         <td>
            The number of idle runs per thread before it dies out. You can specify any positive integer. The default value is <code>10</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.MaxMessagesPerTask
         </td>
         <td>
            <p>The maximum number of successful message receipts per thread. You can specify any positive integer. The default value is <code>-1</code>. Use <code>-1</code> to indicate infinity.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.InitialReconnectDuration
         </td>
         <td>
           Initial reconnection attempts duration in milliseconds. You can specify any positive integer. The default value is <code>10000</code> milliseconds.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.ReconnectProgressFactor
         </td>
         <td>
            Factor by which the reconnection duration will be increased. You can specify any positive integer. The default value is <code>2</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.MaxReconnectDuration
         <td>
            Maximum reconnection duration in milliseconds. The default value is <code>3600000</code> milliseconds (1 hour).
         </td>
      </tr>
      <tr>
         <td>transport.jms.ReconnectInterval</td>
         <td>Reconnection interval in milliseconds. The default value is <code>3600000</code> milliseconds (1 hour).
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.MaxJMSConnections
         </td>
         <td>
            Maximum cached JMS connections in the producer level. You can specify any positive integer. The default value is <code>10</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.MaxConsumeErrorRetriesBeforeDelay
         </td>
         <td>
            Number of retries on consume errors before sleep delay kicks in. You can specify any positive integer. The default value is <code>20</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.ConsumeErrorDelay
         </td>
         <td>
            Sleep delay when a consume error is encountered (in milliseconds). You can specify any positive integer. The default value is <code>100</code> milliseconds.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.ConsumeErrorProgression
         </td>
         <td>
           Factor by which the consume error retry sleep will be increased. You can specify any positive integer. The default value is <code>2.0</code>.
         </td>
      </tr>
      <tr>
         <td>transport.jms.MaxConsumeErrorRetryCount</td>
         <td>
            The maximum number of times the consumer should retry upon receiving a consumer error. You need to introduce this parameter only if the Broker has issues in notifying the Exception Listeners about the exceptions occurred. You can specify any positive integer. The default value is <code>1</code>.
         </td>
      </tr>
   </tbody>
</table>

### Service-level JMS configuration parameters

<table>
      <tr>
         <th>
            Parameter Name
         </th>
         <th>
            Description
         </th>
      </tr>
   <tbody>
      <tr>
         <td>
            transport.jms.ConnectionFactory
         </td>
         <td>
            Name of the JMS connection factory the service should use. You can specify the name of an already defined connection factory
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.PublishEPR
         </td>
         <td>
            JMS EPR to be published in the WSDL. Specify a JMS EPR.
         </td>
      </tr>
      <tr>
         <td>transport.jms.ContentType</td>
         <td>Specifies how the transport listener should determine the content type of received messages. Specify a simple string value, in which case the transport listener assumes that the received messages always have the specified content type, or a set of rules. For more information, see <a href="http://axis.apache.org/axis2/java/transports/jms.html#Service_configuration">http://axis.apache.org/axis2/java/transports/jms.html#Service_configuration</a>.</td>
      </tr>
      <tr>
         <td>
            <code>transport.jms.MessagePropertyHyphens</code>
         </td>
         <td>Specifies the action to be taken when there are JMS Message property names that contain hyphens. The possible values are as follows:
            <ul>
               <li><b>none</b>: No action will be taken. This is the default value.</li>
               <li><b>replace</b>: Transport headers with hyphens will be replaced before adding them as JMS message properties, and if the Micro Integrator is the consumer, hyphens will be reintroduced on message retrieval.</li>
               <li>
                  <b>delete</b>: Transport headers with hyphens will be deleted.
               </li>
            </ul>
         </td>
      </tr>
   </tbody>
</table>

## MQTT Connection Factory Parameters

### Required Parameters

<table>
	<tr>
		<th>Property</th>
		<th>Description</th>
	</tr>
	<tr>
      <td>mqtt.server.host.name</td>
      <td>The name of the host.</td>
   </tr>
   <tr>
      <td>mqtt.server.port</td>
      <td>The port ID. The possible values are <code>1883</code> or <code>1885</code>.</td>
   </tr>
   <tr>
      <td>mqtt.client.id</td>
      <td>The client ID.</td>
   </tr>
   <tr>
      <td>mqtt.topic.name</td>
      <td>The name of the topic</td>
   </tr>
</table>

### Optional Parameters

<table>
   <tr>
      <th>
         Parameter
      </th>
      <th>
         Description
      </th>
   </tr>
   <tbody>
      <tr>
         <td>mqtt.subscription.qos</td>
         <td>The QoS value. The possible values are <code>0</code>, <code>1</code>, or <code>2</code>.</td>
      </tr>
      <tr>
         <td>mqtt.session.clean</td>
         <td>Whether session clean should be enabled or not.</td>
      </tr>
      <tr>
         <td>mqtt.ssl.enable</td>
         <td>Whether SSL should be enabled or not.</td>
      </tr>
      <tr>
         <td>mqtt.subscription.username</td>
         <td>The username for the subscription.</td>
      </tr>
      <tr>
         <td>mqtt.subscription.password</td>
         <td>The password for the subscription.</td>
      </tr>
      <tr>
         <td>mqtt.temporary.store.directory</td>
         <td>The path of the directory to be used as the persistent data store for quality of service purposes. The default value is the Micro Integrator temp path.</td>
      </tr>
      <tr>
         <td>mqtt.blocking.sender</td>
         <td>Whether blocking sender should be enabled or not.</td>
      </tr>
      <tr>
         <td>mqtt.content.type</td>
         <td>The content type. The default content type is <code>text/plain</code>.</td>
      </tr>
      <tr>
         <td>mqtt.message.retained</td>
         <td>Whether the messaging engine should retain a published message or not. This parameter can be used only in the transport sender. By default, this parameter is set to <code>false</code>.</td>
      </tr>
   </tbody>
</table>
