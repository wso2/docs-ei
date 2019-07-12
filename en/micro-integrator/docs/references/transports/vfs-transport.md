# VFS Transport

**The Virtual File System (VFS) transport** is used by WSO2 Enterprise
Integrator(WSO2 EI) to process files in the specified source directory.
After processing the files, it moves them to a specified location or
deletes them. Note that files cannot remain in the source directory
after processing or they will be processed again, so if you need to
maintain these files or keep track of which files have been processed,
specify the option to move them instead of deleting them after
processing. If you want to move files into a database, use the VFS
transport and the [DBReport
mediator](https://docs.wso2.com/display/EI650/DB+Report+Mediator) (for
an example, see [Sample 271: File
Processing](https://docs.wso2.com/display/EI620/Sample+271%3A+File+Processing)
).

**Note** that the VFS transport is enabled by default from WSO2 EI
version 6.5.0 onwards.

!!! tip

**Tip** : When you transfer a file to a remote FTP location via VFS, the
ESB tries to detect the FTP location by navigating from the root folder
first. If the ESB does not have **at least list permission** to the root
(/), the file transfer fails.


------------------------------------------------------------------------

[Configuring the endpoint](#VFSTransport-Configuringtheendpoint) \|
[Security](#VFSTransport-Security) \| [Failure
tracking](#VFSTransport-Failuretracking) \| [Transferring large
files](#VFSTransport-Transferringlargefiles) \| [VFS service-level
parameters](#VFSTransport-VFSservice-levelparameters) \| [VFS URL
parameters](#VFSTransport-VFSURLparameters) \|
[Samples](#VFSTransport-Samples)

### Configuring the endpoint

To configure a VFS endpoint, use the `         vfs:file        ` prefix
in the URI. For example:

``` html/xml
    <endpoint>
        <address uri="vfs:file:///home/user/test/out"/>
    </endpoint>
```

### Security

The VFS transport supports the **SFTP protocol** with **Secure Sockets
Layer (SSL)** . The configuration is identical to other protocols with
the only difference being the URL prefixes and parameters. For more
information, see [VFS URL parameters](#VFSTransport-URLparams) below.

#### Securing VFS password in a proxy service

The following instructions describe how to secure the VFS password in a
proxy service configuration.

1.  Provide configuration for the password decryption by adding the
    following lines in
    `           <EI_HOME>/conf/axis2/axis2.xml          ` file:

    ``` java
            <transportReceiver class="org.apache.synapse.transport.vfs.VFSTransportListener" name="vfs">
            <parameter locked="false" name="keystore.identity.location">repository/resources/security/wso2carbon.jks</parameter>
            <parameter locked="false" name="keystore.identity.type">JKS</parameter>
            <parameter locked="false" name="keystore.identity.store.password">wso2carbon</parameter>
            <parameter locked="false" name="keystore.identity.key.password">wso2carbon</parameter>
            <parameter locked="false" name="keystore.identity.alias">wso2carbon</parameter>
            </transportReceiver>
    ```

        !!! info
    
        If you need to secure the passwords provided in the above
        configuration, you can do so using [secure
        vault.](https://docs.wso2.com/display/ADMIN44x/Carbon+Secure+Vault+Implementation)
        The secured configuration would look like this:
    
    ``` java
            <transportReceiver class="org.apache.synapse.transport.vfs.VFSTransportListener" name="vfs">
            <parameter locked="false" name="keystore.identity.location">repository/resources/security/wso2carbon.jks</parameter>
            <parameter locked="false" name="keystore.identity.type">JKS</parameter>
            <parameter locked="false" name="keystore.identity.store.password" svns:secretAlias="vfs.transport.keystore.password">password</parameter>
            <parameter locked="false" name="keystore.identity.key.password" svns:secretAlias="vfs.transport.key.password">password</parameter>
            <parameter locked="false" name="keystore.identity.alias">wso2carbon</parameter>
            </transportReceiver>
    ```
        

2.  Manually encrypt the password using Cipher Tool. See, [Encrypting
    Passwords in Cipher
    Tool](https://docs.wso2.com/display/ADMIN44x/Encrypting+Passwords+with+Cipher+Tool#EncryptingPasswordswithCipherTool-Encryptingpasswordsmanually)
    in the administration guide.
3.  Provide the encrypted password value in your proxy configuration by
    adding the following parameter:

    ``` java
        <parameter name="transport.vfs.FileURI">smb://{wso2:vault-decrypt('encryptedValue')}</parameter>
    ```

### Failure tracking

To track failures in file processing, which can occur when a resource
becomes unavailable, the VFS transport creates and maintains a failed
records file. This text file contains a list of files that failed to be
processed. When a failure occurs, an entry with the failed file name and
the timestamp is logged in the text file. When the next polling
iteration occurs, the VFS transport checks each file against the failed
records file, and if a file is listed as a failed record, it will skip
processing and schedule a move task to move that file.

### Transferring large files

If you need to transfer large files using the VFS transport, you can
avoid out-of-memory failures by taking the following steps:

1.  In `           <EI_HOME>/conf/axis2/axis2.xml          ` , in the
    `           messageBuilders          ` section, add the binary
    message builder as follows:

    ``` html/xml
            <messageBuilder contentType="application/binary" class="org.apache.axis2.format.BinaryBuilder"/>
    ```

    and in the `           messageFormatters          ` section, add the
    binary message formatter as follows:

    ``` html/xml
            <messageFormatter contentType="application/binary" class="org.apache.axis2.format.BinaryFormatter"/>
    ```

2.  In the proxy service where you use the VFS transport, add the
    following parameter to enable streaming (see [VFS service-level
    parameters](#VFSTransport-parameters) below for more information):

    ``` html/xml
            <parameter name="transport.vfs.Streaming">true</parameter>
    ```

3.  In the same proxy service, before the Send mediator, add the
    following property:

        !!! info
    
        Note
    
        You also need to add the following property if you want to use the
        VFS transport to transfer files from VFS to VFS.
    

    ``` html/xml
        <property name="ClientApiNonBlocking" value="true" scope="axis2" action="remove"/>
    ```

    For more information, see Example 3 of the [Send
    Mediator](https://docs.wso2.com/display/EI650/Send+Mediator#SendMediator-blocking)
    .

### VFS service-level parameters

The VFS transport does not have any global parameters to be configured.
Rather, it has a set of service-level parameters that must be specified
for each proxy service that uses the VFS transport.

<table>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
<th><p>Possible Values</p></th>
<th><p>Default Value</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="content-wrapper">
<p>transport.vfs.FileURI</p>
</div></td>
<td><div class="content-wrapper">
<p>The URI of the location of your files. This should be the source location of the files (i f you are configuring the ESB to read files) or the destination of the files ( if you are configuring the ESB to send files). You can specify connection-level parameters on the URL (see <a href="#VFSTransport-URLparams">VFS URL parameters</a> below).</p>
<p>When you need to access the absolute path of the URL, you can define the URL with <code>               sftpPathFromRoot              </code> as shown below. Also, note that <a href="#VFSTransport-avoid_permissions">transport.vfs.AvoidPermissionCheck</a> is a mandatory parameter for this URL when SFTP is used.</p>
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
<tr class="even">
<td><p>transport.vfs.ContentType</p></td>
<td><p>Content type of the files processed by the transport. To specify the encoding, follow the content type with a semi-colon and the character set. For example:</p>
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
<td><p>A valid content type for the files (e.g., <code>              text/xml             </code> ). You can specify the encoding after the content type, such as <code>              text/plain;charset=UTF-32             </code></p></td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><p>transport.vfs.<br />
FileNamePattern</p></td>
<td><p>If the VFS listener should process only a subset of the files available at the specified file URI location, use this parameter to select those files by name using a regular expression.</p></td>
<td><p>No</p></td>
<td><p>A regular expression to select files by name (e.g., <code>              *\.xml             </code> )</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p>transport.<br />
PollInterval</p></td>
<td><p>The polling interval for the transport receiver to poll the file URI location. The value is expressed in seconds unless you add "ms" for milliseconds, e.g., "2" or "2000ms" to specify 2 seconds.</p></td>
<td><p>No</p></td>
<td><p>A positive integer.</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><p>transport.vfs.<br />
ActionAfterProcess</p></td>
<td><p>Whether to move, delete or take no action on the files after the transport has processed them.</p></td>
<td><p>No</p></td>
<td><p><code>              MOVE             </code> , <code>              DELETE             </code> or <code>              NONE             </code></p></td>
<td><p><code>              DELETE             </code></p></td>
</tr>
<tr class="even">
<td><p>transport.vfs.<br />
ActionAfterFailure</p></td>
<td><p>Whether to move, delete or take no action on the files if a failure occurs.</p></td>
<td><p>No</p></td>
<td><p><code>              MOVE             </code> , <code>              DELETE             </code> or <code>              NONE             </code></p></td>
<td><p><code>              DELETE             </code></p></td>
</tr>
<tr class="odd">
<td><p>transport.vfs.<br />
MoveAfterProcess</p></td>
<td><p>Where to move the files after processing if ActionAfterProcess is MOVE.</p></td>
<td><p>Yes, if<br />
ActionAfterProcess<br />
is MOVE</p></td>
<td><p>A valid file URI</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p>transport.vfs.<br />
MoveAfterFailure</p></td>
<td><p>Where to move the files after processing if ActionAfterFailure is MOVE.</p></td>
<td><p>Yes, if<br />
ActionAfterFailure<br />
is MOVE</p></td>
<td><p>A valid file URI</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><p>transport.vfs.<br />
ReplyFileURI</p></td>
<td><p>The location where reply files should be written by the transport.</p></td>
<td><p>No</p></td>
<td><p>A valid file URI</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p>transport.vfs.<br />
ReplyFileName</p></td>
<td><p>The name for reply files written by the transport.</p></td>
<td><p>No</p></td>
<td><p>A valid file name</p></td>
<td><p><code>              response.xml             </code></p></td>
</tr>
<tr class="odd">
<td><p>transport.vfs.<br />
MoveTimestampFormat</p></td>
<td><p>The pattern/format of the timestamps added to file names as prefixes when moving files.</p></td>
<td><p>No</p></td>
<td><p>A valid <a href="http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html">timestamp pattern</a><br />
(e.g., <code>              yyyy-MM-dd'T'HH:mm:ss.SSSZ             </code> )</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p>transport.vfs.<br />
Streaming</p></td>
<td><p>Whether files should be transferred in streaming mode, which is useful when transferring large files</p></td>
<td><p>No</p></td>
<td><p><code>              true             </code> or <code>              false             </code></p></td>
<td><p><code>              false             </code></p></td>
</tr>
<tr class="odd">
<td><p>transport.vfs.<br />
ReconnectTimeout</p></td>
<td><p>Reconnect timeout value in seconds to be used in case of an error when transferring files</p></td>
<td><p>No</p></td>
<td><p>A positive integer</p></td>
<td><p>30 sec</p></td>
</tr>
<tr class="even">
<td><p>transport.vfs.<br />
MaxRetryCount</p></td>
<td><p>Maximum number of retry attempts to carry out in case of errors.</p></td>
<td><p>No</p></td>
<td><p>A positive integer</p></td>
<td><p>3</p></td>
</tr>
<tr class="odd">
<td><p>transport.vfs.Append</p></td>
<td><p>When writing the response to a file, whether the response should be appended to the response file instead of overwriting the file. This value should be defined as a query parameter in the out/reply<br />
file URI. For example:<br />
<code>              "                             vfs:file:///home/user/test/out                            ?             </code><br />
<code>              transport.vfs.Append=true"             </code></p>
<p>or:</p>
<p><code>              &lt;parameter name="             </code><br />
<code>              transport.vfs.ReplyFileURI"&gt;             </code><br />
<code>              file:///home/user/test/out?             </code><br />
<code>              transport.vfs.Append=true             </code><br />
<code>              &lt;/parameter&gt;             </code></p></td>
<td><p>No</p></td>
<td><p><code>              true             </code> or <code>              false             </code></p></td>
<td><p><code>              false             </code> (the response file will be completely overwritten).</p></td>
</tr>
<tr class="even">
<td><p>transport.vfs.<br />
MoveAfterFailedMove</p></td>
<td><p>Where to move the failed file.</p></td>
<td><p>No</p></td>
<td><p>A valid file URI</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><p>transport.vfs.<br />
FailedRecordsFileName</p></td>
<td><p>The name of the file that maintains<br />
the list of failed files.</p></td>
<td><p>No</p></td>
<td><p>A valid file name</p></td>
<td><p><code>              vfs-move-failed-records.             </code><br />
<code>              properties             </code></p></td>
</tr>
<tr class="even">
<td><p>transport.vfs.<br />
FailedRecordsFile<br />
Destination</p></td>
<td><p>Where to store the failed records file.</p></td>
<td><p>No</p></td>
<td><p>A folder URI</p></td>
<td><p><code>                           </code></p></td>
</tr>
<tr class="odd">
<td><p>transport.vfs.<br />
MoveFailedRecord<br />
TimestampFormat</p></td>
<td><p>Entries in the failed records file include the name of the file that failed and the timestamp of its failure. This property configures the time stamp format.</p></td>
<td><p>No</p></td>
<td><p>A valid <a href="http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html">timestamp pattern</a><br />
(e.g., <code>              yyyy-MM-dd'T'HH:mm:ss.SSSZ             </code> )</p></td>
<td><p><code>              dd-MM-yyyy HH:mm:ss             </code></p></td>
</tr>
<tr class="even">
<td><p>transport.vfs.<br />
FailedRecordNext<br />
RetryDuration</p></td>
<td><p>The time in milliseconds to wait before retrying the move task.</p></td>
<td><p>No</p></td>
<td><p>A positive integer</p></td>
<td><p>3000 milliseconds</p></td>
</tr>
<tr class="odd">
<td><p>transport.vfs.Locking</p></td>
<td><p>By default, file locking is enabled in the VFS transport. This parameter lets you configure the locking behavior on a per service basis. You can also disable locking globally by specifying the parameter at the receiver level and selectively enable locking only for a set of services.</p></td>
<td><p>No</p></td>
<td><p><code>              enable             </code> or <code>              disable             </code></p></td>
<td><p><code>              enable             </code></p></td>
</tr>
<tr class="even">
<td>transport.vfs.<br />
FileProcessCount</td>
<td>This setting allows you to throttle the VFS listener by processing files in batches. Specify the number of files you want to process in each batch.</td>
<td>No</td>
<td>A positive integer, such as 10</td>
<td>N/A</td>
</tr>
<tr class="odd">
<td>transport.vfs.<br />
FileProcessInterval</td>
<td>The interval in milliseconds between two file processes.</td>
<td>No</td>
<td>A positive integer, such as 1000</td>
<td>N/A</td>
</tr>
<tr class="even">
<td>transport.vfs.ClusterAware</td>
<td>Whether VFS coordination support is enabled in a clustered deployment or not.</td>
<td>No</td>
<td><code>             true            </code> or <code>             false            </code></td>
<td><code>             false            </code></td>
</tr>
<tr class="odd">
<td>transport.vfs.FileSizeLimit</td>
<td>Only file sizes that are less than or equal to the defined limit are processed.</td>
<td>No</td>
<td>File size in bytes</td>
<td><p>-1(unlimited file size)</p></td>
</tr>
<tr class="even">
<td>transport.vfs.AutoLockReleaseInterval</td>
<td><p>The timeout value for stale locks where the VFS transport will ignore those file locks once the defined time period is reached. (The time period is calculated from the time the lock is created to the time you attempt to access it.)</p>
<p>If you need stale locks to never timeout provide -1 as the timeout value.</p></td>
<td>No</td>
<td>Time in milliseconds</td>
<td>20000</td>
</tr>
<tr class="odd">
<td>transport.vfs.SFTPIdentities</td>
<td>Location of the private key</td>
<td>No</td>
<td>A valid file path</td>
<td>N/A</td>
</tr>
<tr class="even">
<td>transport.vfs.SFTPIdentityPassPhrase</td>
<td>Passphrase of the private key</td>
<td>No</td>
<td>A valid passphrase</td>
<td>N/A</td>
</tr>
<tr class="odd">
<td>transport.vfs.SFTPUserDirIsRoot</td>
<td>If the SFTP user directory should be treated as root</td>
<td>No</td>
<td><code>             true            </code> or <code>             false            </code></td>
<td>true</td>
</tr>
<tr class="even">
<td>transport.vfs.ResolveHostsDynamically</td>
<td><div class="content-wrapper">
<p>Whether hostnames should be resolved at the time of deployment or whether it is necessary to resolve hostnames dynamically at runtime. By default hostnames are resolved at the time of deployment. If you want to resolve hostnames at runtime, set this parameter to <code>               true              </code> .</p>
!!! note
<p>Note</p>
<p>Reolving hostnames at runtime is only possible for the Server Message Block (SMB) protocol.</p>

</div></td>
<td>No</td>
<td><code>             true            </code> or <code>             false            </code></td>
<td><code>             false            </code></td>
</tr>
</tbody>
</table>

### VFS URL parameters

When you use the [transport.vfs.FileURI](#VFSTransport-file_URL)
parameter, you can set connection-specific VFS parameters as URL query
parameters. For example, to use SFTP with SSL, you could specify the URL
as shown below. Note that
[transport.vfs.AvoidPermissionCheck](#VFSTransport-avoid_permissions) is
a mandatory parameter for this URL when SFTP is used.

``` html/xml
    <parameter name="transport.vfs.FileURI">vfs:ftps://test:test123@10.200.2.63/vfs/in?vfs.ssl.keystore=/home/user/openssl/keystore.jks&amp;vfs.ssl.truststore=/home/user/openssl/vfs-truststore.jks&amp;vfs.ssl.kspassword=importkey&amp;vfs.ssl.tspassword=wso2vfs&amp;vfs.ssl.keypassword=importkey;transport.vfs.AvoidPermissionCheck=true</parameter>
```

Following are details on the URL parameters you can set. To configure
the proxy over ftp/sftp click
[here](https://docs.wso2.com/display/EI650/Configuring+File+Inbound+Protocol+for+FTP%2C+SFTP+and+FILE+Connections#ConfiguringFileInboundProtocolforFTP,SFTPandFILEConnections-ConfigSFTP)
.

<table>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Possible Values</p></th>
<th>Default Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="content-wrapper">
<p>transport.vfs.AvoidPermissionCheck</p>
</div></td>
<td><p><strong>Be sure</strong> to set this parameter to <strong>true</strong> for an SFTP connection. This is because (by default) the VFS transport checks whether the user has permission to access the location of the files (the source location or the destination). However, since the system is reading files in an external server through the SFTP connection, this permission check is not required and should be avoided.</p></td>
<td>true | false</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><p>vfs.passive</p></td>
<td><p>Enable FTP passive mode. This is required when the FTP client and server are not in the same network.</p></td>
<td>true | false</td>
<td>false</td>
</tr>
<tr class="odd">
<td>transport. vfs . Append</td>
<td>If file with same name exists, this parameter tells whether to create a new file and write content or append content to existing file</td>
<td><p>true | false</p></td>
<td>false</td>
</tr>
<tr class="even">
<td>vfs.protection</td>
<td><p>Set data channel protection level using FTP PROT command</p></td>
<td><ul>
<li>C - Clear</li>
<li>S - Safe(SSL protocol only)</li>
<li>E - Confidential(SSL protocol only)</li>
<li>P - Private</li>
</ul></td>
<td>C</td>
</tr>
<tr class="odd">
<td>vfs.ssl.keystore</td>
<td>Private key store to use for mutual SSL. Your keystore must be signed by a certificate authority. For more information, see <a href="index">http://docs.oracle.com/cd/E19509-01/820-3503/ggfen/index.html</a> .</td>
<td>String - Path of keystore</td>
<td><br />
</td>
</tr>
<tr class="even">
<td>vfs.ssl.kspassword</td>
<td>Private key store password</td>
<td>String</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td>vfs.ssl.keypassword</td>
<td>Private key password</td>
<td>String</td>
<td><br />
</td>
</tr>
<tr class="even">
<td>vfs.ssl.truststore</td>
<td>Trust store to use for SFTP</td>
<td>String - Path of keystore</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td>vfs.ssl.tspassword</td>
<td>Trust store password</td>
<td>String</td>
<td><br />
</td>
</tr>
<tr class="even">
<td>transport.vfs.CreateFolder</td>
<td>If the directory does not exists create and write the file</td>
<td>true | false</td>
<td>false</td>
</tr>
<tr class="odd">
<td>transport. vfs.SendFileSynchronously</td>
<td>Whether to send files synchronously to the file host. When this parameter is set to <code>             true            </code> , files will be sent one after another to the file host. This synchronous write can be configured on a per host basis.</td>
<td>true | false</td>
<td>false</td>
</tr>
</tbody>
</table>

### Samples

-   [Sample 254: Using the File System as Transport Medium
    (VFS)](https://docs.wso2.com/pages/viewpage.action?pageId=85369151)
-   [Sample 255: Switching from FTP Transport Listener to Mail Transport
    Sender](https://docs.wso2.com/display/EI6xx/Sample+255%3A+Switching+from+FTP+Transport+Listener+to+Mail+Transport+Sender)
-   [Sample 265: Accessing a Windows Share Using the VFS
    Transport](https://docs.wso2.com/display/EI6xx/Sample+265%3A+Accessing+a+Windows+Share+Using+the+VFS+Transport)
-   [Sample 271: File
    Processing](https://docs.wso2.com/display/EI6xx/Sample+271%3A+File+Processing)
-   [Sample 654: Smooks
    Mediator](https://docs.wso2.com/display/EI6xx/Sample+654%3A+Smooks+Mediator)
