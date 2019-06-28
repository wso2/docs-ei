# File Inbound Protocol

The file inbound protocol is a multi-tenant capable alternative to the
VFS transport. The file inbound protocol uses the [VFS
transport](https://docs.wso2.com/display/EI650/VFS+Transport) to process
files in a specified source directory. After processing the files, it
moves them to a specified location or deletes them. Note that files
cannot remain in the source directory after processing or they will be
processed again, so if you need to maintain these files or keep track of
the files that are processed, specify the option to move them instead of
deleting them after processing.

### Security

The file inbound protocol supports the **FTPS protocol** with **Secure
Sockets Layer (SSL)** . The configuration is identical to other
protocols. The only difference is the URL prefixes and parameters.  For
more information, see [VFS URL
parameters](https://docs.wso2.com/display/EI650/VFS+Transport#VFSTransport-URLparams)
.

### Failure tracking

To track failures in file processing that can occur when a resource
becomes unavailable, the VFS transport creates and maintains a failed
records file. This text file contains a list of files that failed to
processed. When a failure occurs, an entry with the failed file name and
timestamp is logged in the text file. When the next polling iteration
occurs, the VFS transport checks each file against the failed records
file, and if a file is listed as a failed record, it will skip
processing and schedule a move task to move that file.

``` xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" 
                     name="file" sequence="request" 
                     onError="fault" 
                     protocol="file" 
                     suspend="false">
       <parameters>
          <parameter name="interval">1000</parameter>
          <parameter name="sequential">true</parameter> 
          <parameter name="coordination">true</parameter> 
          <parameter name="transport.vfs.ActionAfterProcess">MOVE</parameter>
          <parameter name="transport.vfs.MoveAfterProcess">file:///home/user/test/out</parameter>
          <parameter name="transport.vfs.FileURI">file:///home/user/test/in</parameter>
          <parameter name="transport.vfs.MoveAfterFailure">file:///home/user/test/failed</parameter>
          <parameter name="transport.vfs.FileNamePattern">.*.txt</parameter>
          <parameter name="transport.vfs.ContentType">text/plain</parameter>
          <parameter name="transport.vfs.ActionAfterFailure">MOVE</parameter>
       </parameters>
    </inboundEndpoint>
```

### VFS service-level parameters

The VFS transport does not have any global parameters to be configured.
Rather, it has a set of service-level parameters that must be specified
for each proxy service that uses the VFS transport. For information on
how to configure the file inbound protocol for FTP, SFTP and FILE
connections , see [Configuring File Inbound Protocol for FTP, SFTP and
FILE
Connections](_Configuring_File_Inbound_Protocol_for_FTP_SFTP_and_FILE_Connections_)
.

<table>
<thead>
<tr class="header">
<th><p>Parameter</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
<th><p>Possible Values</p></th>
<th><p>Default Value</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             interval            </code></td>
<td>The time duration in milliseconds between two file scans that checks for updates.</td>
<td>Yes</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>             sequential            </code></td>
<td>Files will be processed sequentially when this parameter is set to true.</td>
<td>Yes</td>
<td><br />
</td>
<td>true</td>
</tr>
<tr class="odd">
<td><code>             coordination            </code></td>
<td><p>This should be true for clustered deployments in order to prevent two nodes from retrieving the same file.</p></td>
<td>Yes</td>
<td><br />
</td>
<td>true</td>
</tr>
<tr class="even">
<td><p><code>              transport.vfs.             </code> <code>              FileURI             </code></p></td>
<td><div class="content-wrapper">
<p>The URI of the location of your files. This should be the source location of the files (i f you are configuring the ESB to read files) or the destination of the files ( if you are configuring the ESB to send files). You can specify connection-level parameters on the URL (see <a href="https://docs.wso2.com/display/EI650/VFS+Transport#VFSTransport-VFSURLparameters">VFS URL parameters</a> ).</p>
<p>When you need to access the absolute path of the URL, you can define the URL with <code>               sftpPathFromRoot              </code> as shown below. Also, note that <a href="https://docs.wso2.com/display/EI650/VFS+Transport#VFSTransport-avoid_permissions">transport.vfs.AvoidPermissionCheck</a> is a mandatory parameter for this URL when FTPS is used.</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;parameter name=<span class="st">&quot;transport.vfs.FileURI&quot;</span>&gt;sftp:<span class="co">//[ username[: password]@] hostname[: port][ absolute-path]?sftpPathFromRoot=true;transport.vfs.AvoidPermissionCheck=true&lt;/parameter&gt;</span></span></code></pre></div>
</div>
</div>
</div></td>
<td><p>Yes</p></td>
<td><p>A valid file URI in the following form:<br />
<code>              file://&lt;path&gt;             </code></p></td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.vfs.             </code> <code>              ContentType             </code></p></td>
<td><p>Content type of the files processed by the transport. To specify the encoding when reading a file, follow the content type with a semi-colon and the character set. For example:</p>
<div>
<div>
<code>               &lt;parameter name="transport.vfs.ContentType“&gt;text/plain;charset=UTF-32&lt;/parameter&gt;              </code>
</div>
<div>
When writing a file, you can set a different encoding with the <code>               CHARACTER_SET_ENCODING              </code> property:
</div>
<div>
<code>               &lt;property name="CHARACTER_SET_ENCODING" value="UTF-8" scope="axis2" type="STRING"/&gt;              </code>
</div>
</div></td>
<td><p>Yes</p></td>
<td><p>A valid content type for the files (e.g., <code>              text/xml             </code> ). You can specify the encoding after the content type, such as: <code>              text/plain;charset=UTF-32             </code></p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.vfs.             </code> <code>              FileNamePattern             </code></p></td>
<td><p>If the VFS listener should process only a subset of the files available at the specified file URI location, use this parameter to select those files by name using a regular expression.</p></td>
<td><p>No</p></td>
<td><p>A regular expression to select files by name (e.g., <code>              *\.xml             </code> )</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><p>t <code>              ransport.vfs.             </code> <code>              ActionAfterProcess             </code></p></td>
<td><p>Whether to move or delete the files after the transport has processed them.</p></td>
<td><p>No</p></td>
<td><p><code>              MOVE             </code> or <code>              DELETE             </code></p></td>
<td><p><code>              DELETE             </code></p></td>
</tr>
<tr class="even">
<td><p><code>              transport.vfs.             </code> <code>              ActionAfterFailure             </code></p></td>
<td><p>Whether to move or delete the files if a failure occurs.</p></td>
<td><p>No</p></td>
<td><p><code>              MOVE             </code> or <code>              DELETE             </code></p></td>
<td><p><code>              DELETE             </code></p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.vfs.             </code> <code>              MoveAfterProcess             </code></p></td>
<td><p>Where to move the files after processing if the value specified as <code>              transport.vfs.ActionAfterProcess             </code> is <code style="line-height: 1.4285715;">              MOVE             </code> .</p></td>
<td><p>Yes, if<br />
ActionAfterProcess<br />
is MOVE</p></td>
<td><p>A valid file URI</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.vfs.             </code> <code>              MoveAfterFailure             </code></p></td>
<td><p>Where to move the files after processing if the value specified as <code>              transport.vfs.ActionAfterFailure             </code> is <code>              MOVE             </code> .</p></td>
<td><p>Yes, if<br />
ActionAfterFailure<br />
is MOVE</p></td>
<td><p>A valid file URI</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.vfs.             </code> <code>              ReplyFileURI             </code></p></td>
<td><p>The location where reply files should be written by the transport.</p></td>
<td><p>No</p></td>
<td><p>A valid file URI</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.vfs.             </code> <code>              ReplyFileName             </code></p></td>
<td><p>The name for reply files written by the transport.</p></td>
<td><p>No</p></td>
<td><p>A valid file name</p></td>
<td><p><code>              response.xml             </code></p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.vfs.             </code> <code>              MoveTimestampFormat             </code></p></td>
<td><p>The pattern/format of the timestamp added to file names as prefixes when moving files.</p></td>
<td><p>No</p></td>
<td><p>A valid <a href="http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html">timestamp pattern</a><br />
(e.g., <code>              yyyy-MM-dd'T'HH:mm:ss.SSSZ             </code> )</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.vfs.             </code> <code>              Streaming             </code></p></td>
<td><p>Whether files should be transferred in streaming mode, which is useful when transferring large files.</p></td>
<td><p>No</p></td>
<td><p><code>              true             </code> or <code>              false             </code></p></td>
<td><p><code>              false             </code></p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.vfs.             </code> <code>              ReconnectTimeout             </code></p></td>
<td><p>Reconnect timeout value in seconds to be used in case of an error when transferring files.</p></td>
<td><p>No</p></td>
<td><p>A positive integer</p></td>
<td><p>30 sec</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.vfs.             </code> <code>              MaxRetryCount             </code></p></td>
<td><p>Maximum number of retry attempts in case of errors.</p></td>
<td><p>No</p></td>
<td><p>A positive integer</p></td>
<td><p>3</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.vfs.             </code> <code>              MoveAfterFailedMove             </code></p></td>
<td><p>The location to move a failed file.</p></td>
<td><p>No</p></td>
<td><p>A valid file URI</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.vfs.             </code> <code>              FailedRecordsFileName             </code></p></td>
<td><p>The name of the file that maintains the list of failed files.</p></td>
<td><p>No</p></td>
<td><p>A valid file name</p></td>
<td><p><code>              vfs-move-failed-records.             </code><br />
<code>              properties             </code></p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.vfs.             </code> <code>              FailedRecordsFile             </code> <code>              Destination             </code></p></td>
<td><p>The location to store the failed records file.</p></td>
<td><p>No</p></td>
<td><p>A folder URI</p></td>
<td><p><code>              repository/conf/             </code></p></td>
</tr>
<tr class="even">
<td><p><code>              transport.vfs.             </code> <code>              MoveFailedRecord             </code> <code>              TimestampFormat             </code></p></td>
<td><p>The time stamp format for entries in the failed records file. The failed records file maintains the name of the file that failed and the timestamp of its failure.</p></td>
<td><p>No</p></td>
<td><p>A valid <a href="http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html">timestamp pattern</a><br />
(e.g., <code>              yyyy-MM-dd'T'HH:                             mm:ss.SSSZ                           </code> )</p></td>
<td><p><code>              dd-MM-yyyy HH:mm:ss             </code></p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.vfs.             </code> <code>              FailedRecordNext             </code> <code>              RetryDuration             </code></p></td>
<td><p>The time in milliseconds to wait before retrying the move task.</p></td>
<td><p>No</p></td>
<td><p>A positive integer</p></td>
<td><p>3000 milliseconds</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.vfs.Locking             </code></p></td>
<td><p>By default, file locking is enabled in the VFS transport. This parameter lets you configure the locking behavior on a per service basis. You can also disable locking globally by specifying the parameter at the receiver level and selectively enabling locking only for a set of services.</p></td>
<td><p>No</p></td>
<td><p><code>              enable             </code> or <code>              disable             </code></p></td>
<td><p><code>              enable             </code></p></td>
</tr>
<tr class="odd">
<td><code>             transport.vfs.            </code> <code>             FileProcessCount            </code></td>
<td>This parameter allows you to throttle the VFS listener by processing files in batches. Specify the number of files you want to process in each batch. If you specify a value for this</td>
<td>No</td>
<td>A positive integer, such as 10</td>
<td>N/A</td>
</tr>
<tr class="even">
<td><p><code>              transport.vfs.             </code> FileProcessInterval</p></td>
<td>The interval in milliseconds between file processing batches.</td>
<td>No</td>
<td>A positive integer, such as 1000</td>
<td>N/A</td>
</tr>
<tr class="odd">
<td><p><code>              transport.vfs.DistributedLock             </code></p></td>
<td>This applies only in cluster deployments. Set to <code>             true            </code> if you need to avoid multiple servers trying to process the same file simultaneously.</td>
<td>No</td>
<td>true or false</td>
<td>N/A</td>
</tr>
<tr class="even">
<td><p><code>              transport.vfs.DistributedTimeout             </code></p></td>
<td>The timeout period in seconds for the distributed lock.</td>
<td>No</td>
<td>A positive integer, such as 10</td>
<td>N/A</td>
</tr>
<tr class="odd">
<td><p><code>              transport.vfs.AutoLockRelease             </code></p></td>
<td>Set to true if you need to release locking in order to avoid files not being processed due to faulty locking. This works together with the <code>             transport.vfs.AutoLockReleaseInterval            </code> and <code>             transport.vfs.LockReleaseSameNode            </code> parameters.</td>
<td>No</td>
<td>true or false</td>
<td>N/A</td>
</tr>
<tr class="even">
<td><p><code>              transport.vfs.AutoLockReleaseInterval             </code></p></td>
<td>Lock release interval in milliseconds.</td>
<td>No</td>
<td>A positive integer, such as 1000</td>
<td>N/A</td>
</tr>
<tr class="odd">
<td><p><code>              transport.vfs.LockReleaseSameNode             </code></p></td>
<td>Set to <code>             true            </code> if you need to release the locks only accrued by the same worker node.<br />
If this is set to <code>             false            </code> , locks accrued by other nodes will be released according to the value specified in <code>             t             ransport.vfs.AutoLockReleaseInterval            </code> .</td>
<td>No</td>
<td>true or false</td>
<td>true</td>
</tr>
<tr class="even">
<td><code>             transport.vfs.FileSortAttribute            </code></td>
<td>The attribute by which the files should be sorted and processed.</td>
<td>No</td>
<td>NONE, Name, Size and Lastmodifiedtimestamp</td>
<td>N/A</td>
</tr>
<tr class="odd">
<td><code>             transport.vfs.FileSortAsscending            </code></td>
<td>The sort order to sort and process the files. If set to <code>             true            </code> files will be sorted in ascending order based on the attribute you specify in <code>             transport.vfs.FileSortAttribute.            </code></td>
<td>No</td>
<td>true or false</td>
<td>true</td>
</tr>
<tr class="even">
<td><code>             transport.vfs.CreateFolder            </code></td>
<td>Set to <code>             true            </code> to create a folder if a folder does not exist when moving files.</td>
<td>No</td>
<td>true or false</td>
<td>false</td>
</tr>
<tr class="odd">
<td><code>             transport.vfs.SubFolderTimestampFormat            </code></td>
<td>The pattern/format of the timestamps added to the folder structure when moving files. You need to set <code>             transport.vfs.CreateFolder            </code> to <code>             true            </code> to in order to specify a value for this parameter.</td>
<td>No</td>
<td>A valid <a href="http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html">timestamp pattern</a><br />
(e.g., <code>             yyyy-MM-dd'T'HH:                           mm:ss.SSSZ                         </code> )</td>
<td>N/A</td>
</tr>
<tr class="even">
<td><code>             transport.vfs.Build            </code></td>
<td>Set to <code>             true            </code> if you need to build the content inside the file before injecting the file to the mediation engine. If there is a build error, the file will not be injected to the mediation engine.</td>
<td>No</td>
<td>true or false</td>
<td>false</td>
</tr>
</tbody>
</table>

!!! info

Note

If you specify the `         transport.vfs.FileProcessCount        `
parameter you do not need to specify the
`         transport.vfs.FileProcessInterval        ` parameter in a
configuration, and vice versa. This is because the
`         transport.vfs.FileProcessCount        ` parameter and t he
`         transport.vfs.FileProcessInterval        ` parameter cannot be
used at the same time.


### Samples

For a sample that demonstrates how to use the file system as an input
medium via the inbound file listener, see [Sample 900: Inbound Endpoint
File Protocol Sample
(VFS)](https://docs.wso2.com/pages/viewpage.action?pageId=85377137) .

## Configuring File Inbound Protocol for FTP, SFTP and FILE Connections

The following section describes how to configure the file inbound
protocol for FTP, SFTP and FILE connections.

-   To configure the file inbound protocol for FTP connections, you
    should specify the URL as
    `           ftp://{username}:{password}@{hostname/ip_address}/{source_filepath}                     `

    ``` html/xml
        ftp://admin:pass@localhost/orders
    ```

-   To configure the file inbound protocol for SFTP connections, you
    should specify the URL as
    `           sftp://{username}:{password}@{hostname/ip_address}/{source_filepath}                     `

    ``` html/xml
            sftp://admin:pass@localhost/orders
    ```

!!! tip

If the password contains special characters, these characters will need
to be replaced with their hexadecimal representation.


-   To configure the file inbound protocol for FILE connections, you
    should specify the URL as
    `           file://{local_file_system_path}                     `

    ``` html/xml
        file:///home/user/test/in
    ```

### Configuring a Proxy Server over FTP and SFTP Connections

Proxy server specific parameters can be set as URL query parameters. For
example, to use Proxy over FTP, you could specify the URL as follows:

``` html/xml
    ftp://username:password@127.0.0.1/home/wso2/res?proxyServer=127.0.0.1&proxyPort=3128&proxyUsername=proxyuser&proxyPassword=proxyPass&timeout=2500&retryCount=3
```

Following are the URL parameters you can set:

<table>
<thead>
<tr class="header">
<th><p>Parameter</p></th>
<th><p>Description</p></th>
<th><p>Possible Values</p></th>
<th>Default Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>              proxyServer             </code></p></td>
<td><p>The IP address of the proxy server.</p></td>
<td>127.0.0.1</td>
<td>false</td>
</tr>
<tr class="even">
<td><p><code>              proxyPort             </code></p></td>
<td>The port number on which the proxy server is listening for requests.</td>
<td>1328</td>
<td>false</td>
</tr>
<tr class="odd">
<td><code>             proxyType            </code></td>
<td><div class="content-wrapper">
<p>The proxy server type. This can either be <code>               HTTP              </code> or <code>               SOCKS              </code> .</p>
!!! info
<p>Note</p>
<p>In a configuration, if the proxy server type is not specified or an unknown proxy server type is specified, the <code>               proxyType              </code> will be considered as <code>               HTTP              </code> .</p>

</div></td>
<td><p><br />
</p>
<p><code>              HTTP             </code> or <code>              SOCKS             </code></p></td>
<td><p><br />
</p>
<p><code>              HTTP             </code></p></td>
</tr>
<tr class="even">
<td><p><code>              proxyUsername             </code></p></td>
<td>The user name of the proxy server.</td>
<td><br />
</td>
<td>false</td>
</tr>
<tr class="odd">
<td><p><code>              proxyPassword             </code></p></td>
<td>The password of the proxy server.</td>
<td><br />
</td>
<td>false</td>
</tr>
<tr class="even">
<td><p><code>              timeout             </code></p></td>
<td>The connection timeout in milliseconds.</td>
<td>1000</td>
<td>5000</td>
</tr>
<tr class="odd">
<td><p><code>              retryCount             </code></p></td>
<td>The number of retry attempts in case of a connection timeout.</td>
<td>3</td>
<td>5</td>
</tr>
</tbody>
</table>
