# Working with Proxy Servers

When using the ESB profile of WSO2 Enterprise Integrator (WSO2 EI),
there can be scenarios where you need to configure the ESB to route
messages through a proxy server. For example, if the ESB is behind a
firewall, your proxy service might need to talk to a server through a
proxy server as illustrated in the following diagram:

![](attachments/119130243/119130245.png)

## Routing messages through a proxy server

See the instructions given below.

### For non-blocking service calls

To configure the ESB to route messages through a proxy server
(for non-blocking service calls), add the parameters given below to the esb.toml file and update the
values. This configuration ensures that all HTTP requests pass through
the configured proxy server.

``` java
[config_section]
non-blocking=true
http.proxyHost=""
http.proxyPort=""
```

The parameters are described below.

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 85%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre><code>non-blocking</code></pre></td>
<td>Specifies whether or not 'non-blocking' mode is enabled for the transport sender. Be sure that this parameter is set to <code>             true            </code> .</td>
</tr>
<tr class="even">
<td><pre><code>http.proxyHost</code></pre></td>
<td>The host name of the proxy server.</td>
</tr>
<tr class="odd">
<td><pre><code>http.proxyPort</code></pre></td>
<td>The port (number) in the proxy server.</td>
</tr>
</tbody>
</table>

### For blocking service calls

To configure the ESB profile to route messages through a proxy server
(for blocking service calls), add the parameters given below to the esb.toml file, and update the
values. This configuration ensures that all HTTP requests pass through
the configured proxy server.

``` java
[config_heading]
ProxyHost=""
ProxyPort=""
ProxyUser=""
ProxyPassword=""
```

The parameters are described below.

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 84%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre><code>ProxyHost</code></pre></td>
<td>The host name of the proxy server.</td>
</tr>
<tr class="even">
<td><pre><code>ProxyPort</code></pre></td>
<td>The port (number) in the proxy server.</td>
</tr>
<tr class="odd">
<td><pre><code>ProxyUser</code></pre></td>
<td>The user name for connecting to the proxy server.</td>
</tr>
<tr class="even">
<td><pre><code>ProxyPassword</code></pre></td>
<td>The password for connecting to the proxy server.</td>
</tr>
</tbody>
</table>

>**Bypass the proxy server for blocking calls?**

> In the case of blocking service calls, you can apply a system property
in the ESB profile to bypass the proxy server and route messages
directly to the hosts that should receive the messages. Explained below
are two methods of applying the system property:
> - Set the system property in the product startup script that is
    located in the `           <PRODUCT_HOME>/bin/          ` directory
    as shown below. Note that the list of host names are separated by
    the pipe symbol ('\|').
    ``` java
        -Dhttp.nonProxyHosts =10.|localhost|127.0.0.1|.\.domain.com \
    ```
> -   Pass the system property when you start the server as shown below.
    ``` java
            ./integrator.sh -Dhttp.nonProxyHosts =10.|localhost|127.0.0.1|.\.domain.com
    ```
        
> A proxy server might require HTTP basic authentication before it handles
communication from the ESB profile.


## Configuring proxy profiles in the ESB profile

When using the ESB profile, there can be scenarios where you need to
configure multiple proxy servers to route messages to different
endpoints as illustrated in the following diagram.

![proxy profile](attachments/119130243/119130244.png "proxy profile")

When you need to route messages to different endpoints through multiple
proxy servers, you can configure proxy profiles.

To configure proxy profiles in the ESB, open the esb.toml file and define multiple profiles based on the number of
    proxy servers you need to have.

> When you define a profile, it is mandatory to specify the `targetHosts`, `proxyHost` and `proxyPort` parameters for each profile.   

The following is a sample proxy profile configuration that you can have
in the `         <transportSender>        ` configuration of the HTTP
transport:

``` Java
[config_heading]
targetHosts=example.com, .*.sample.com
proxyHost=localhost
proxyPort=3128
proxyUserName=squidUser
proxyPassword=password
bypass=xxx.sample.com

targetHosts=localhost
proxyHost=localhost
proxyPort=7443

targetHosts="*"
proxyHost=localhost
proxyPort="7443"
bypass="test.com, direct.com"

```

When you configure a proxy profile, following are details of the
parameters that you need to define in a `         <profile>        ` :

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Required</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             targetHosts            </code></td>
<td><p>A host name or a comma-separated list of host names for a target endpoint.<br />
Host names can be specified as regular expressions that match a pattern. When <code>              targetHosts             </code> is specified as an asterisks (*), it will match all the hosts in the profile</p></td>
<td>Yes</td>
</tr>
<tr class="even">
<td><code>             proxyHost            </code></td>
<td>The host name of the proxy server.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><code>             proxyPort            </code></td>
<td>The port number of the proxy server.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><code>             proxyUserName            </code></td>
<td>The user name for the proxy server authentication.</td>
<td>No</td>
</tr>
<tr class="odd">
<td><code>             proxyPassword            </code></td>
<td>The password for the proxy server authentication.</td>
<td>No</td>
</tr>
<tr class="even">
<td><code>             bypass            </code></td>
<td><p>A host name or a comma-separated list of host names that should not be sent via the proxy server.</p>
<p>For example, if you want all requests to <code>              *.                             sample.com                           </code> to be sent via a proxy server, but need to directly send requests to <code>                             hello.sample.com                           </code> , instead of going through the proxy server, you can add <code>                             hello.sample.com                           </code> as a bypass host name.</p>
<p>You can specify host names as regular expressions that match a pattern.</p></td>
<td>No</td>
</tr>
</tbody>
</table>