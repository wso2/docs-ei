---
template: templates/2-column.html
---

# Configuration Catalog
This document describes all the configuration parameters that are used in WSO2 Enterprise Integrator.

## Instructions for use

> Select the configuration sections, parameters, and values that are required for your use and add them to the .toml file. See the example .toml file given below.

```toml
# This is an example .toml file.

[server]
pattern="value"            
enable_port_forward=true

[[custom_transport.listener]]
class="value"
protocol = "value"

```

<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2 id="configuring-the-default-deployment-settings">Configuring the default deployment settings</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[server]</code>
                    <span class="badge-required">Required</span>
                    <p>
                        This toml header groups the parameters that are used for identifying a server node. You need need to update these values when you <a href="https://wso2docs-configurationcatalog-3.netlify.com/setup/deploying_wso2_ei">set up a deployment</a>.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>hostname</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"localhost"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The hostname of the WSO2 EI server instance.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>node_ip</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"10.100.1.80"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The IP address of the server node.</p>
                            </div>
                        </div>
                    </div>
                    <!--
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>base_path</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"127.0.0.1"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The............</p>
                            </div>
                        </div>
                    </div>
                    -->
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>enable_mtom</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Use this paramater to enable MTOM (Message Transmission Optimization Mechanism) for the product server.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>enable_swa</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Use this paramater to enable SwA (SOAP with Attachments) for the product server. When SwA is enabled, the ESB will process the files attached to SOAP messages.</p>
                            </div>
                        </div>
                    </div>
                    <!--
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>userAgent</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"WSO2 ${product.key} ${product.version}"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>serverDetails</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"WSO2 ${product.key} ${product.version}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.....</p>
                            </div>
                        </div>
                    </div>
                -->
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>offset</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>0</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Port offset allows you to run multiple WSO2 products, multiple instances of a WSO2 product, or multiple WSO2 product clusters on the same server or virtual machine (VM). Port offset defines the number by which all ports defined in the runtime such as the HTTP/S ports will be offset. For example, if the default HTTP port is 9443 and the portOffset is 1, the effective HTTP port will be 9444. Therefore, for each additional WSO2 product instance, set the port offset to a unique value (the default is 0) so that they can all run on the same server without any port conflicts.</p>
                            </div>
                        </div>
                    </div>
                    <!--
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>proxy_context_path</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>....</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.....</p>
                            </div>
                        </div>
                    </div>
                -->
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[server]
hostname="localhost"
node_ip = "10.100.1.80"
base_path = "127.0.0.1"
enable_mtom=false
enable_swa=false
userAgent = "WSO2 ${product.key} ${product.version}"
serverDetails = "WSO2 ${product.key} ${product.version}"
offset  = 0
proxy_context_path = ""</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2 id="configuring-the-cluster-settings">Configuring the cluster settings</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[clustering]</code>
                    <span class="badge-required">Required</span>
                    <p>
                        This config heading groups the parameters that connects the server to a <a href="https://wso2docs-configurationcatalog-3.netlify.com/setup/deploying_wso2_ei">clustered deployment</a>.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>members</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>["10.100.5.86:4000","10.100.5.86:4001"]</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Specify the well-known members in the cluster in both nodes as shown below. For example, when you configure one ESB node, you need to specify the other nodes in the cluster as well-known members as shown below. The port value for the WKA node must be the same value as it's localMemberPort (in this case it is 4000).
                                <details class="warning classes" open="open">
                                    <summary>Note</summary>
                                    <p>You can also use IP address ranges for the hostname (e.g., 192.168.1.2-10). However, you can define a range only for the last portion of the IP address. Smaller the range, faster the time it takes to discover members since each node has to scan a lesser number of potential members. The best practice is to add all the members (including itself) in all the nodes to avoid any conflicts in configurations.</p>
                                </details>
                                </p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>local_member_port</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>4000</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The port that is assigned to the server node in the deployment.
                                <details class="warning classes" open="open">
                                    <summary>Note</summary>
                                    <p>This port number is not affected by the port offset value specified under the [server] section. If this port number is already assigned to another server, the clustering framework automatically increments this port number.
                                    However, if there are two servers running on the same machine, ensure that a unique port is set for each server. For example, you can have port 4000 for node 1 and port 4001 for node 2.</p>
                                </details>
                                </p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>local_member_host</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"10.100.5.86"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The hostname of the server node. When you have multiple nodes in the deployment, this hostname will be used to identify the node during inter-node communications.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>membership_scheme</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string">string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>'wka'</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Add this parameter to change the default membership scheme. By default, the well-known address registration method (WKA) is used, which ensures that each node sends cluster initiation messages to the WKA members.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>domain</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"wso2.carbon.domain"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Add this parameter to change the default cluster (domain name) to which the server node joins. By default, wso2.carbon.domain is set. </p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[clustering]
members = ["10.100.5.86:4000","10.100.5.86:4001"]
local_member_port = 4000
local_member_host = "10.100.5.86"
membership_scheme =  'wka'
domain = "wso2.carbon.domain"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2 id="connecting-to-the-primary-data-store">Connecting to the primary data store</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[database.shared_db]</code>
                    <span class="badge-required">Required</span>
                    <p>
                        This config heading groups the parameters connecting the server to the primary database. This database stores the <b>user permissions</b> that apply to user roles. This also stores the <b>users</b> and <b>roles</b> unless a separate user store is configured with the [user_store] config section. Read more about how to use <a href="https://wso2docs-configurationcatalog-3.netlify.com/setup/deploying_wso2_ei">databases for your WSO2 EI deployment</a>.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"H2"</code></span>
                                </div> </br>
                                <div class="param-possible">
                                	<span class="param-possible-value"><code>"MySQL"</code>,<code>"Oracle"</code>,<code>"Postgre"</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The type of database that is used for the primary database. All the database types that are supported by this product is listed below as possible values.</p>
                                <details class="warning classes" open="open">
                                    <summary>Warning</summary>
                                    <p>"H2" (not recommended for production environments). So please change when going on production.</p>
                                </details>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>url</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"jdbc:h2:./repository/database/WSO2SHARED_DB;DB_CLOSE_ON_EXIT=FALSE;LOCK_TIMEOUT=60000"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The connection URL of the database. Note that this is specific to the database type you are using. Use the URL pattern for the database type.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>username</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>wso2carbon</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The user name for connecting to the database.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>wso2carbon</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The password for connecting to the database.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[database.shared_db]
type="H2"
url="jdbc:h2:./repository/database/WSO2SHARED_DB;DB_CLOSE_ON_EXIT=FALSE;LOCK_TIMEOUT=60000"
username=wso2carbon
password=wso2carbon</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2 id="tuning-the-primary-data-store-connection">Tuning the primary data store connection</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[database.shared_db.pool_options]</code>
                    <p>
                        This config heading in the ei.toml file groups the performance tuning parameters of the primary database that you configured using the [database.shared_db] section. Read more about <a href="https://wso2docs-configurationcatalog-3.netlify.com/setup/tuning_db_performance">tuning database performance in WSO2 EI</a>.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>max_active</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"50"</code></span>
                                </div> </br>
                                <div class="param-possible">
                                	<span class="param-possible-value"><code>"MySQL"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The maximum number of active connections that can be allocated at the same time from the connection pool. If there are too many active connections in the connection pool, some of them would be idle, which incurs an unnecessary system overhead. Depending on your environment, enter any negative value to denote an unlimited number of active connections.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>max_wait</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"6000"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The maximum number of milliseconds that the pool will wait (when there are no available connections) for a connection to be returned before throwing an exception. Depending on your environment, enter zero or a negative value to wait indefinitely.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>test_on_borrow</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code>,<code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Determines whether objects are validated before being borrowed from the pool. If the object fails to validate, it will be dropped from the pool, and another attempt will be made to borrow another.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>validation_interval</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"30000"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Connections are validated (at the most) at this frequency (time in milliseconds). If a connection is due for validation but has been validated previously within this interval, it will not be validated again. This avoids access validation.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>default_auto_commit</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code>,<code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Specifies whether each SQL statement should be automatically committed when the operation is completed. It is recommended to disable auto committing.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[database.shared_db_options]
maxActive="50"
maxWait="6000"
testOnBorrow=true
validationInterval="30000"
defaultAutoCommit=true
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Connecting to the user stores</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[user_store]</code>
                    <span class="badge-required">Required</span>
                    <p>
                        This config heading in the ei.toml connects the server to the user store. The user store holds the users and roles defined for the system. Read more about how to set up <a href="https://wso2docs-configurationcatalog-3.netlify.com/setup/deploying_wso2_ei">user stores for your WSO2 EI deployment</a>.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"database"</code></span>
                                </div>
                                <div class="param-possible">
                                	<span class="param-possible-value"><code>"database"</code>, <code>"JDBC"</code>, <code>"RW LDAP"</code>, <code>"RO LDAP"</code>, <code>"AD"</code>, <code>"Custom"</code>
                                	</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The type of user store:
                                    <ul>
                                        <li><code>"database"</code>: Refers the embedded H2 database with the settings defined under [database.shared_db].</li>
                                        <li><code>"JDBC"</code>: Refers an external RDBMS.</li>
                                        <li><code>"RW LDAP"</code>: Refers an external LDAP with both read/write access.</li>
                                        <li><code>"RO LDAP"</code>: Refers an external LDAP with read only access.</li>
                                        <li><code>"AD"</code>: Rrefers an active directory user store.</li>
                                        <li><code>"Custom"</code>: Refers a custom user store implementation.</li>
                                    </ul>
                                </p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap">
                            <code>connection_url</code>
                          </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                    <span class="badge-required">Required</span>
                                </p>
                            </div>
                            <div class="param-default">
                                    <span class="param-default-value"><code>"jdbc:h2:./repository/database/WSO2SHARED_DB;DB_CLOSE_ON_EXIT=FALSE;LOCK_TIMEOUT=60000"</code></span>
                            </div>
                            <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                            </div>
                            <div class="param-description">
                                <p>Add this parameter if you have changed the default user store type. You need to change the default connection URL by specifying the connection URL of your new user store (LDAP or AD user store).</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap">
                            <code>connection_name</code>
                          </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>wso2carbon</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Add this parameter if you have changed the default user store type. You need to change the default connection name (user name for connecting to the data store) by specifying the connection name of your new user store (LDAP or AD user store).</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap">
                            <code>connection_password</code>
                          </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>wso2carbon</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Add this parameter if you have changed the default user store type. You need to change the default password (password for connecting to the data store) by specifying the connection password of your new user store (LDAP or AD user store).</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>base_dn</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                            </div>
                            <div class="param-default">
                                    <span class="param-default-value"><code></code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[user_store]
type="database"
connection_url=""
connection_name="uid=admin,ou=wso2ei"
connection_password="$secret{ldap_password}"
base_dn="dc=example,dc=com"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2 id="configuring-the-system-administrator">Configuring the system administrator</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[super_admin]</code>
                    <span class="badge-required">Required</span>
                    <p>
                        The config heading in the ei.toml file that groups the parameters <a href="https://wso2docs-configurationcatalog-3.netlify.com/setup/deploying_wso2_ei">defining the system administrator</a>.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>username</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"admin"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The user name of the system administrator. This user has all permissions enabled by default.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                	<span class="param-default-value"><code>"admin"</code><span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The password of the system adminsitrator. You can use a plain text password such as 'admin'. However, if you want to use an encrypted password that is listed under the <code>[secrets]</code> configuration section, use the ... expression to include the encrypted password. Read more about encrypting plain text passwords.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>create_admin_account</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                    <span class="badge-required">Required</span>
                                </p>
                                <div class="param-default">
                                	<span class="param-default-value"><code>true</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Sets whether the system administrator credentials should be created in the system at the time of starting the server.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[super_admin]
username="admin"
password="admin"
create_admin_account=true
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the TLS keystore</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[keystore.tls]</code>
                    <p>
                        This config heading in the ei.toml file groups the parameters that connect the server to the primary keystore. This keystore is used for SSL handshaking (when the server communicates with another server) and for encrypting plain text information in configuration files. By default, this keystore is also used for encrypted data in internal datastores, unless you have configured a separate keystore for internal data encryption. Read more about <a href="https://wso2docs-configurationcatalog-3.netlify.com/security/configuring_keystores#configuring_the_primary_keystore">configuring the primary keystore</a>.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>file_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"wso2carbon.jks"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The name of the keystore file that is used for SSL communication and for encrypting/decrypting data in configuration files.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                	<span class="param-default-value">"JKS"<span>
                                </div>
                                <div class="param-possible">
                                	<span class="param-possible-value"><code>"JKS"</code> or <code>"PKCS12"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The type of the keystore file.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                	<span class="param-default-value"><code>"wso2carbon"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The password of the keystore file that is used for SSL communication and for encrypting/decrypting data in configuration files. The keystore password is used when accessing the keys in the keystore.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>alias</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                	<span class="param-default-value"><code>"wso2carbon"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The alias of the public key corresponding to the private key that is included in the keystore. The public key is used for encrypting data in the ESB server, which only the corresponding private key can decrypt. The public key is embedded in a digital certificate, and this certificate can be shared over the internet by storing it in a separate trust store file.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>key_password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                	<span class="param-default-value"><code>"wso2carbon"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The password of the private key that is included in the keystore. The private key is used to decrypt the data that has been encrypted using the keystore's public key.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[keystore.tls]
file_name="wso2carbon.jks"
type="JKS"
password="wso2carbon"
alias="wso2carbon"
key_password="wso2carbon"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the internal keystore</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[keystore.internal]</code>
                    <p>
                        Add this config heading to the ei.toml file to group the parameters that connect the server to the keystore used for encrypting/decrypting data in internal data stores. You may sometimes choose to configure a separate keystore for this purpose because the primary keystore that is used by the [keystore.tls] configuration needs to renew certificates frequently. However, for encrypting information in internal data stores, the keystore certificates should not be changed frequently because the data that is already encrypted will become unusable every time the certificate changes. Read more about <a href="https://wso2docs-configurationcatalog-3.netlify.com/security/configuring_keystores#configuring_the_internal_keystore">configuring the internal keystore</a>.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>file_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"wso2carbon.jks"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The name of the keystore file that is used for data encryption/decryption in internal data stores.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">"JKS"<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>"JKS"</code> or <code>"PKCS12"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The type of the keystore file.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"wso2carbon"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The password of the keystore file that is used for data encryption/decryption in internal data stores. The keystore password is used when accessing the keys in the keystore.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>alias</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"wso2carbon"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The alias of the public key corresponding to the private key that is included in the keystore. The public key is used for encrypting data in the ESB server, which only the corresponding private key can decrypt. The public key is embedded in a digital certificate, and this certificate can be shared over the internet by storing it in a separate trust store file.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>key_password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"wso2carbon"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The password of the private key that is included in the keystore. The private key is used to decrypt the data that has been encrypted using the keystore's public key.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[keystore.internal]
file_name="wso2carbon.jks"
type="JKS"
password="wso2carbon"
alias="wso2carbon"
key_password="wso2carbon"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the trust store</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[truststore]</code>
                    <p>
                        Add this config heading to the ei.toml file to group the parameters that connect the server to the keystore file (trust store) that is used to store the digital certificates that the server trusts for SSL communication. All keystore files used by this product should be stored in the EI_HOME/repository/resources/security/ directory. The product is configured to use the default trust store (wso2truststore.jks), which contains the self-signed digital certificate of the default keystore. Read more about asymetric encryption in WSO2 EI. Read more about <a href="https://wso2docs-configurationcatalog-3.netlify.com/security/configuring_keystores#configuring_the_primary_keystore">configuring the truststore</a>.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>file_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"wso2truststore.jks"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The name of the keystore file that is used for storing the trusted digital certificates.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">"JKS"<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>"JKS"</code> or <code>"PKCS12"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The type of the keystore file that is used as the trust store.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"wso2carbon"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The password of the keystore file that is used as the trust store.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>alias</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"symmetric.key.value"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The alias is the password of the digital certificate (which holds the public key) that is included in the trustore.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>algorithm</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>.....</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[truststore]
file_name="wso2truststore.jks"
type="JKS"
password="wso2carbon"
alias="symmetric.key.value"
algorithm=""
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the server request processor</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[[server.get_request_processor]]</code>
                    <p>
                        Add this config section to the ei.toml file to add processors that process special HTTP GET requests such as ?wsdl, ?policy etc. In order to plug in a processor to handle a special request, simply add an entry to this section.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>item</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"swagger.json"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The value of the Item element is the first parameter in the query string(e.g. ?wsdl) which needs special processing.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>class</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"org.wso2.appcloud.api.swagger.processors.json.SwaggerJsonProcessor"</code><span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The value of the Class element is a class that implements org.wso2.carbon.transport.HttpGetRequestProcessor.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[[server.get_request_processor]]
item = "swagger.json"
class = "org.wso2.appcloud.api.swagger.processors.json.SwaggerJsonProcessor"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the HTTP transport</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.http]</code>
                    <p>
                        Add this config heading to the ei.toml file to group the parameters for configuring the HTTP/S transports in the product.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>socket_timeout</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"3m"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>core_worker_pool_size</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>400</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>max_worker_pool_size</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>400</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>worker_pool_queue_length</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>-1</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>io_buffer_size</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>16384</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>max_http_connection_per_host_port</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>32767</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>preserve_http_user_agent</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>preserve_http_server_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>preserve_http_headers</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>["Content-Type"]</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>disable_connection_keepalive</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>enable_message_size_validation</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>max_message_size_bytes</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>81920</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>max_open_connections</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>-1</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>force_xml_validation</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>force_json_validation</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>    
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.port</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">"8280"<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The port on which this transport receiver should listen for incoming messages.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.wsdl_epr_prefix</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{server.hostname}"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>A URL prefix which will be added to all service EPRs and EPRs in WSDLs etc.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.bind_address</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{server.hostname}"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.secured_port</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"8243"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The secured port on which this transport receiver should listen for incoming messages.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.secured_wsdl_epr_prefix</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>"$ref{server.hostname}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>A URL prefix which will be added to all service EPRs and EPRs in WSDLs etc.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.secured_bind_address</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>"$ref{server.hostname}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.........</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.secured_protocols</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>"TLSv1,TLSv1.1,TLSv1.2"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.verify_client</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"require"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.ssl_profile.file_path</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"conf/sslprofiles/listenerprofiles.xml"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.ssl_profile.read_interval</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"1h"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.preferred_ciphers</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,TLS_DHE_RSA_WITH_AES_128_CBC_SHA256,TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,TLS_DHE_RSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,TLS_DHE_RSA_WITH_AES_128_GCM_SHA256"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.keystore.file_name</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.file_name}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.keystore.type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.type}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.keystore.password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.keystore.key_password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.key_password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.truststore.file_name</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.file_name}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.truststore.type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.type}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.truststore.password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.warn_on_http_500</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>If the outgoing messages should be sent through an HTTP proxy server, use this parameter to specify the target proxy.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.proxy_host</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"${deployement.node_ip}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>If the outgoing messages should be sent through an HTTP proxy server, use this parameter to specify the target proxy.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.proxy_port</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"3128"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The port through which the target proxy accepts HTTP traffic.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.non_proxy_hosts</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>["$ref{server.hostname}"]</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The list of hosts to which the HTTP traffic should be sent directly without going through the proxy. When trying to add multiple hostnames along with an asterisk in order to define a set of sub-domains for non-proxy hosts, you need to add a period before the asterisk when configuring proxy server.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.hostname_verifier</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>"AllowAll"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The list of hosts to which the HTTP traffic should be sent directly without going through the proxy. When trying to add multiple hostnames along with an asterisk in order to define a set of sub-domains for non-proxy hosts, you need to add a period before the asterisk when configuring proxy server.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.keystore.file_name</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.file_name}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.keystore.type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.type}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.keystore.password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.keystore.key_password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.key_password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.truststore.file_name</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.file_name}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.truststore.type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.type}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.truststore.password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>blocking_sender.enable_client_caching</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>This parameter is used to specify whether the HTTP client should save cache entries and the cached responses in the JVM memory or not.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>blocking_sender.transfer_encoding</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"chunked"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>"chunked"</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>This parameter enables you to specify whether the data sent should be chunked. It can be used instead of the Content-Length header if you want to upload data without having to know the amount of data to be uploaded in advance.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>blocking_sender.default_connections_per_host</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The maximum number of connections that will be created per host server by the client. If the backend server is slow, the connections in use at a given time will take a long time to be released and added back to the connection pool. As a result, connections may not be available for some requests. In such situations, it is recommended to increase the value for this parameter.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>blocking_sender.omit_soap12_action</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>If following is set to 'true', optional action part of the Content-Type will not be added to the SOAP 1.2 messages.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>blocking_sender.so_timeout</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"1m"</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>If following is set to 'true', optional action part of the Content-Type will not be added to the SOAP 1.2 messages.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.http]
socket_timeout = "3m"
core_worker_pool_size = 400
max_worker_pool_size = 400
worker_pool_queue_length = -1
io_buffer_size = 16384
max_http_connection_per_host_port = 32767
preserve_http_user_agent = false
preserve_http_server_name = true
preserve_http_headers = ["Content-Type"]
disable_connection_keepalive = false
enable_message_size_validation = false
max_message_size_bytes = 81920
max_open_connections = -1
force_xml_validation = false
force_json_validation = false
listener.port = 8280    #inferred  default: 8280
listener.wsdl_epr_prefix ="$ref{server.hostname}"
listener.bind_address = "$ref{server.hostname}"
listener.secured_port = 8243
listener.secured_wsdl_epr_prefix = "$ref{server.hostname}"
listener.secured_bind_address = "$ref{server.hostname}"
listener.secured_protocols = "TLSv1,TLSv1.1,TLSv1.2"
listener.verify_client = "require"
listener.ssl_profile.file_path = "conf/sslprofiles/listenerprofiles.xml"
listener.ssl_profile.read_interval = "1h"
listener.preferred_ciphers = "TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,TLS_DHE_RSA_WITH_AES_128_CBC_SHA256,TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,TLS_DHE_RSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,TLS_DHE_RSA_WITH_AES_128_GCM_SHA256"
listener.keystore.file_name ="$ref{keystore.tls.file_name}"
listener.keystore.type = "$ref{keystore.tls.type}"
listener.keystore.password = "$ref{keystore.tls.password}"
listener.keystore.key_password = "$ref{keystore.tls.key_password}"
listener.truststore.file_name = "$ref{truststore.file_name}"
listener.truststore.type = "$ref{truststore.type}"
listener.truststore.password = "$ref{truststore.password}"
sender.warn_on_http_500 = "*"
sender.proxy_host = "$ref{server.hostname}"
sender.proxy_port = 3128
sender.non_proxy_hosts = ["$ref{server.hostname}"]
sender.hostname_verifier = "AllowAll"
sender.keystore.file_name ="$ref{keystore.tls.file_name}"
sender.keystore.type = "$ref{keystore.tls.type}"
sender.keystore.password = "$ref{keystore.tls.password}"
sender.keystore.key_password = "$ref{keystore.tls.key_password}"
sender.truststore.file_name = "$ref{truststore.file_name}"
sender.truststore.type = "$ref{truststore.type}"
sender.truststore.password = "$ref{truststore.password}"
sender.ssl_profile.file_path = "conf/sslprofiles/senderprofiles.xml"
sender.ssl_profile.read_interval = "30s"
# common for http/https
blocking_sender.enable_client_caching = true
blocking_sender.transfer_encoding = "chunked"
blocking_sender.default_connections_per_host = 200
blocking_sender.omit_soap12_action = true
blocking_sender.so_timeout = "1m"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the HTTP proxy profile</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[[transport.http.proxy_profile]]</code>
                    <p>
                        This.....
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>target_hosts</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string">string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>proxy_hosts</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">""<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>""</code> or <code>""</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the local transport sender.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>proxy_port</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">""<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>""</code> or <code>""</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the local transport sender.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>proxy_username</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">""<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>""</code> or <code>""</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the local transport sender.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>proxy_password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">""<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>""</code> or <code>""</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the local transport sender.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>bypass_hosts</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">""<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>""</code> or <code>""</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the local transport sender.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[[transport.http.proxy_profile]]
target_hosts = [""]
proxy_host = ""
proxy_port = ""
proxy_username = ""
proxy_password = ""
bypass_hosts = [""]
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the Local transport</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.local]</code>
                    <p>
                        This parameter is used to enable the listeners and senders when the ESB server communicates through the local transport.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.enabled</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the local transport listener.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.enabled</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">false<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the local transport sender.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.local]
listener.enabled=false
sender.enabled=false
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the VFS transport</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.vfs]</code>
                    <span class="badge-required">Required</span>
                    <p>
                        Add this config heading to the ei.toml file to group the parameters that configure the ESB server to communicate through the VFS transport. Read more about file transfering in the ESB.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.enabled</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the VFS transport listener.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.keystore.file_name</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.file_name}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.keystore.type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.type}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.keystore.password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.keystore.key_password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.key_password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.keystore.alias</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.alias}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.enabled</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">true<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the VFS transport sender.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">
[transport.vfs]
listener.enable = true
listener.keystore.file_name = "$ref{keystore.tls.file_name}"
listener.keystore.type = "$ref{keystore.tls.type}"
listener.keystore.password = "$ref{keystore.tls.password}"
listener.keystore.key_password = "$ref{keystore.tls.key_password}"
listener.keystore.alias = "$ref{keystore.tls.alias}"
sender.enable = true
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the MAIL transport listener</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.mail.listener]</code>
                    <p>
                        Add this config heading to the ei.toml file to group the parameters that configure the ESB server to communicate through the MAIL transport. Read more about using the MAIL transport.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>enabled</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the MAIL transport listener in the ESB.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"mailto"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the MAIL transport listener in the ESB.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.mail.listener]
enable = true
name = "mailto"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the MAIL transport sender</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[[transport.mail.sender]]</code>
                    <p>
                        Add this config heading to the ei.toml file to group the parameters that configure the ESB server to communicate through the MAIL transport. Read more about using the MAIL transport.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"mailto"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the MAIL transport sender in the ESB.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.hostname</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"smtp.gmail.com"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.port</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>587</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.enable_tls</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.auth</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.username</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"demo_user"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.password</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"mailpassword"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.from</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"demo_user@wso2.com"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[[transport.mail.sender]]
name = "mailto"
parameter.hostname = "smtp.gmail.com"
parameter.port = "587"
parameter.enable_tls = true
parameter.auth = true
parameter.username = "demo_user"
parameter.password = "mailpassword"
parameter.from = "demo_user@wso2.com"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the JMS transport listener</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[[transport.jms.listener]]</code>
                    <p>
                        Add this config heading to the ei.toml file to group the parameters that configure the ESB server to communicate through the JMS transport. Read more about using the JMS transport.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"myTopicListener"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The ......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.initial_naming_factory</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.broker_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"artemis"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.provider_url</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"tcp://localhost:61616"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.connection_factory_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"TopicConnectionFactory"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.connection_factory_type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"topic"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.cache_level</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"consumer"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.naming_security_principal</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.naming_security_credential</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.transactionality</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.transaction_jndi_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.cache_user_transaction</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.session_transaction</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.session_acknowledgement</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"AUTO_ACKNOWLEDGE"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.jms_spec_version</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"1.1"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.username</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.password</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.destination</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.destination_type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"queue"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.default_reply_destination</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.default_destination_type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"queue"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.message_selector</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.subscription_durable</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.durable_subscriber_client_id</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.durable_subscriber_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.pub_sub_local</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.receive_timeout</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"1s"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.concurrent_consumer</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>1</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.max_concurrent_consumer</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>1</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.idle_task_limit</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>10</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.max_message_per_task</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>-1</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.initial_reconnection_duration</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"10s"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.reconnect_progress_factor</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>2</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.max_reconnect_duration</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"1h"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.reconnect_interval</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"1h"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.max_jsm_connection</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>10</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.max_consumer_error_retrieve_before_delay</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>20</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.consume_error_delay</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"100ms"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.consume_error_progression</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"2.0"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[[transport.jms.listener]]

name = "myTopicListener"
parameter.initial_naming_factory = "org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory"
parameter.broker_name = "artemis"
parameter.provider_url = "tcp://localhost:61616"
parameter.connection_factory_name = "TopicConnectionFactory"
parameter.connection_factory_type = "topic"
parameter.cache_level = "consumer"

parameter.naming_security_principal = ""
parameter.naming_security_credential = ""
parameter.transactionality = ""
parameter.transaction_jndi_name = ""
parameter.cache_user_transaction = true
parameter.session_transaction = true
parameter.session_acknowledgement = "AUTO_ACKNOWLEDGE"
parameter.jms_spec_version = "1.1"
parameter.username = ""
parameter.password = ""
parameter.destination = ""
parameter.destination_type = "queue"
parameter.default_reply_destination = ""
parameter.default_destination_type = "queue"
parameter.message_selector = ""
parameter.subscription_durable = false
parameter.durable_subscriber_client_id = ""
parameter.durable_subscriber_name = ""
parameter.pub_sub_local = false
parameter.receive_timeout = "1s"
parameter.concurrent_consumer = 1
parameter.max_concurrent_consumer = 1
parameter.idle_task_limit = 10
parameter.max_message_per_task = -1
parameter.initial_reconnection_duration = "10s"
parameter.reconnect_progress_factor = 2
parameter.max_reconnect_duration = "1h"
parameter.reconnect_interval = "1h"
parameter.max_jsm_connection = 10
parameter.max_consumer_error_retrieve_before_delay = 20
parameter.consume_error_delay = "100ms"
parameter.consume_error_progression = "2.0"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the JMS transport sender</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[[transport.jms.sender]]</code>
                    <p>
                        Add this config heading to the ei.toml file to group the parameters that configure the ESB server to communicate through the JMS transport. Read more about using the JMS transport.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>enable</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The ......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"myTopicSender"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.initial_naming_factory</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.broker_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"artemis"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.provider_url</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"TopicConnectionFactory"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.connection_factory_type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"topic"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.cache_level</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"producer"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.naming_security_principal</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.naming_security_credential</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.transactionality</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.transaction_jndi_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.cache_user_transaction</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.session_transaction</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.session_acknowledgement</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"AUTO_ACKNOWLEDGE"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.jms_spec_version</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"1.1"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.username</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.password</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.destination</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.destination_type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"queue"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.default_reply_destination</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.default_destination_type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"queue"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.message_selector</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.subscription_durable</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.durable_subscriber_client_id</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.durable_subscriber_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.pub_sub_local</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.receive_timeout</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"1s"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.concurrent_consumer</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>1</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.max_concurrent_consumer</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>1</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.idle_task_limit</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>10</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.max_message_per_task</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>-1</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.initial_reconnection_duration</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"10s"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.reconnect_progress_factor</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>2</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.max_reconnect_duration</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"1h"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.reconnect_interval</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"1h"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.max_jsm_connection</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>10</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.max_consumer_error_retrieve_before_delay</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>20</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.consume_error_delay</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"100ms"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.consume_error_progression</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"2.0"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.vender_class_loader</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[[transport.jms.sender]]

enable = true
name = "myTopicSender"
parameter.initial_naming_factory = "org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory"
parameter.broker_name = "artemis"
parameter.provider_url = "tcp://localhost:61616"
parameter.connection_factory_name = "TopicConnectionFactory"
parameter.connection_factory_type = "topic"
parameter.cache_level = "producer"

parameter.naming_security_principal = ""
parameter.naming_security_credential = ""
parameter.transactionality = ""
parameter.transaction_jndi_name = ""
parameter.cache_user_transaction = true
parameter.session_transaction = true
parameter.session_acknowledgement = "AUTO_ACKNOWLEDGE"
parameter.jms_spec_version = "1.1"
parameter.username = ""
parameter.password = ""
parameter.destination = ""
parameter.destination_type = "queue" # queue/topic
parameter.default_reply_destination = ""
parameter.default_destination_type = "queue"
parameter.message_selector = ""
parameter.subscription_durable = false
parameter.durable_subscriber_client_id = ""
parameter.durable_subscriber_name = ""
parameter.pub_sub_local = false
parameter.receive_timeout = "1s"
parameter.concurrent_consumer = 1
parameter.max_concurrent_consumer = 1
parameter.idle_task_limit = 10
parameter.max_message_per_task = -1
parameter.initial_reconnection_duration = "10s"
parameter.reconnect_progress_factor = 2
parameter.max_reconnect_duration = "1h"
parameter.reconnect_interval = "1h"
parameter.max_jsm_connection = 10
parameter.max_consumer_error_retrieve_before_delay = 20
parameter.consume_error_delay = "100ms"
parameter.consume_error_progression = "2.0"

parameter.vender_class_loader = false
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the JNDI connection factories</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.jndi.connection_factories]</code>
                    <p>
                    This...
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>QueueConnectionFactory</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>TopicConnectionFactory</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">"amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'"<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.jndi.connection_factories]
QueueConnectionFactory = "amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'"
TopicConnectionFactory = "amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the JNDI queues</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.jndi.queue]</code>
                    <p>
                    This...
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>JMSMS</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"JMSMS"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>StockQuotesQueue</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">"StockQuotesQueue"<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.jndi.queue]
JMSMS = "JMSMS"
StockQuotesQueue = "StockQuotesQueue"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the JNDI topics</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.jndi.topic]</code>
                    <p>
                    This...
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>MyTopic</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">"example.MyTopic"<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.jndi.topic]
MyTopic = "example.MyTopic"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the RabbitMQ listener</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[[transport.rabbitmq.listener]]</code>
                    <p>
                        Add...
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The ......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.hostname</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"localhost"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.port</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>5672</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.username</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"guest"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.password</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"guest"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.connection_factory</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.exchange_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"amq.direct"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"MyQueue"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_auto_ack</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.consumer_tag</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.channel_consumer_qos</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.durable</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_exclusive</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_auto_delete</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_routing_key</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_auto_declare</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.exchange_auto_declare</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.exchange_type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.exchange_durable</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.exchange_auto_delete</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.default_destination_type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.retry_interval</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"10s"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.retry_count</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>5</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.ssl_enable</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.ssl_version</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"SSL"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.keystore_file_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.file_name}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.keystore_type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.type}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.keystore_password</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.truststore_file_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.file_name}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.truststore_type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.type}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>
parameter.truststore_password</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[[transport.rabbitmq.listener]]

name = "rabbitMQListener"
parameter.hostname = "localhost"
parameter.port = 5672
parameter.username = "guest"
parameter.password = "guest"
parameter.connection_factory = ""
parameter.exchange_name = "amq.direct"
parameter.queue_name = "MyQueue"
parameter.queue_auto_ack = false
parameter.consumer_tag = ""
parameter.channel_consumer_qos = ""
parameter.durable = ""
parameter.queue_exclusive = ""
parameter.queue_auto_delete = ""
parameter.queue_routing_key = ""
parameter.queue_auto_declare = ""
parameter.exchange_auto_declare = ""
parameter.exchange_type = ""
parameter.exchange_durable = ""
parameter.exchange_auto_delete = ""
parameter.message_content_type = ""

parameter.retry_interval = "10s"
parameter.retry_count = 5

parameter.ssl_enable = true
parameter.ssl_version = "SSL"
parameter.keystore_file_name ="$ref{keystore.tls.file_name}"
parameter.keystore_type = "$ref{keystore.tls.type}"
parameter.keystore_password = "$ref{keystore.tls.password}"
parameter.truststore_file_name ="$ref{truststore.file_name}"
parameter.truststore_type = "$ref{truststore.type}"
parameter.truststore_password = "$ref{truststore.password}"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the RabbitMQ sender</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[[transport.rabbitmq.sender]]</code>
                    <p>
                        Add...
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"rabbitMQSender"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The ......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.hostname</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"localhost"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.port</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>5672</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.username</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"guest"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.password</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"guest"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.exchange_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"amq.direct"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.routing_key</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"MyQueue"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"MyQueue"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.reply_to_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code></code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_delivery_mode</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>1</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.exchange_type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"MyQueue"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.reply_to_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_delivery_mode</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>1</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.exchange_type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_durable</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_exclusive</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_auto_delete</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.exchange_durable</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.queue_auto_declare</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>parameter.exchange_auto_declare</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The......</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[[transport.rabbitmq.sender]]

name = "rabbitMQSender"
parameter.hostname = "localhost"
parameter.port = 5672
parameter.username = "guest"
parameter.password = "guest"
parameter.exchange_name = "amq.direct"
parameter.routing_key = "MyQueue"
parameter.reply_to_name = ""
parameter.queue_delivery_mode = 1 # 1/2
parameter.exchange_type = ""
parameter.queue_name = "MyQueue"
parameter.queue_durable = false
parameter.queue_exclusive = false
parameter.queue_auto_delete = false
parameter.exchange_durable = ""
parameter.queue_auto_declare = ""
parameter.exchange_auto_declare = ""
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the FIX transport</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.fix]</code>
                    <p>
                    Add this config heading to the ei.toml to group the parameters that configure the ESB server to communicate through the FIX transport. Read more about how the ESB uses FIX communication.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.enabled</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the FIX transport listener.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.enabled</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">false<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the FIX transport sender.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.vfs]
listener.enabled=false
sender.enabled=false
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the MQTT transport</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.mqtt]</code>
                    <p>
                    This....
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.enabled</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the MQTT transport listener.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.hostname</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">"$ref{server.hostname}"<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.connection_factory</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">"mqttConFactory"<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.server_port</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">1883<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.client_id</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">"client-id-1234"<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.topic_name</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">"esb.test"<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.subscription_qos</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">0<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.session_clean</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">false<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.enable_ssl</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">false<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.subscription_username</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">""<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.subscription_password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">""<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.temporary_store_directory</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">""<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.blocking_sender</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">false<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.connect_type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">"text/plain"<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>listener.parameter.message_retained</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">false<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.enable</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">false<span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.mqtt]

listener.enable = false
listener.parameter.hostname = "$ref{server.hostname}"
listener.parameter.connection_factory = "mqttConFactory"
listener.parameter.server_port = 1883
listener.parameter.client_id = "client-id-1234"
listener.parameter.topic_name = "esb.test"

# not reqired parameter list
listener.parameter.subscription_qos = 0
listener.parameter.session_clean = false
listener.parameter.enable_ssl = false
listener.parameter.subscription_username = ""
listener.parameter.subscription_password = ""
listener.parameter.temporary_store_directory = ""
listener.parameter.blocking_sender = false
listener.parameter.connect_type = "text/plain"
listener.parameter.message_retained = false

sender.enable = false
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the HL7 transport</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.hl7]</code>
                    <p>
                    Add this config heading to group the parameters that configure the ESB server communicate through the HL7 transport. Read more about HL7 communication in the ESB.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.enabled</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the HL7 transport listener.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.port</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>9292</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The port of the HL7 transport listener.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.enabled</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">false<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the HL7 transport sender.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.non_blocking</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">true<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The...</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.hl7]

listener.enable = false
listener.port = 9292

sender.enable = false
sender.non_blocking = true
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the SAP transport</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.sap]</code>
                    <p>
                        Add this config heading to the ei.toml file to group the parameters that configure the ESB to communicate with a SAP system. Read more about SAP integration of the ESB.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.enabled</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling SAP transport listener.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.idoc.class</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"org.wso2.carbon.transports.sap.SAPTransportListener"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.bapi.class</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"org.wso2.carbon.transports.sap.SAPTransportListener"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>sender.enabled</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">false<span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code>.</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the SAP transport sender.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>sender.idoc.class</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"org.wso2.carbon.transports.sap.SAPTransportSender"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>sender.bapi.class</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"org.wso2.carbon.transports.sap.SAPTransportSender"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.sap]
listener.enabled=false
sender.enabled=false
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the MSMQ transport</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.msmq]</code>
                    <p>
                        Add.....
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.enabled</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling MSMQ transport listener.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.hostname</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{server.hostname}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>sender.enable</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The....</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.msmq]

listener.enable = false
listener.hostname = "$ref{server.hostname}"
sender.enable = false
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the TCP transport</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.tcp]</code>
                    <p>
                        Add.....
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.enabled</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the TCP transport listener.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.port</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>8000</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.hostname</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{server.hostname}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.content_type</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>["application/xml"]</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.response_client</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>sender.enabled</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the TCP transport sender.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.tcp]
listener.enable = false
listener.port = 8000
listener.hostname = "$ref{server.hostname}"
listener.content_type = ["application/xml"]
listener.response_client = true
sender.enable = true
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the Websocket transport</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.ws]</code>
                    <p>
                        Add.....
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>sender.enable</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the websocket transport listener.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>sender.parameter.outflow_dispatch_sequence</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"outflowDispatchSeq"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>sender.parameter.outflow_dispatch_fault_sequence</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"outflowFaultSeq"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.ws]

sender.enable = false
sender.parameter.outflow_dispatch_sequence = "outflowDispatchSeq"
sender.parameter.outflow_dispatch_fault_sequence = "outflowFaultSeq"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the Websocket secured transport</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.wss]</code>
                    <p>
                        Add.....
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>sender.enable</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the websocket secured transport sender.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>sender.parameter.outflow_dispatch_sequence</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"outflowDispatchSeq"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>sender.parameter.outflow_dispatch_fault_sequence</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"outflowFaultSeq"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>sender.truststore_location</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.file_name}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>sender.truststore_password</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.wss]

sender.enable = false
sender.parameter.outflow_dispatch_sequence = "outflowDispatchSeq"
sender.parameter.outflow_dispatch_fault_sequence = "outflowFaultSeq"
sender.truststore_location = "$ref{truststore.file_name}"
sender.truststore_password = "$ref{truststore.password}"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the UDP transport</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[transport.udp]</code>
                    <p>
                        Add.....
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>listener.enabled</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the UDP transport listener.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>sender.enabled</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>true</code> or <code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The parameter for enabling the UDP transport sender.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[transport.udp]

listener.enable = false
sender.enable =false
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring a custom transport listener</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[[custom_transport.listener]]</code>
                    <p>
                        This config heading is used to group the parameters for a custom transport implementation that you want to use in your product.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>class</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">""</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The .....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>protocol</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"ISO8583"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>parameter.port</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>8081</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>parameter.hostname</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{server.node_ip}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>parameter.non-blocking</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>parameter.bind-address</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{server.hostname}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>parameter.WSDLEPRPrefix</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{server.hostname}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>keystore.file_name</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.file_name}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>keystore.type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.type}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>keystore.password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>keystore.key_password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.key_password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>truststore.file_name</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.file_name}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>truststore.type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.type}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>truststore.password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>ssl_profile.file_path</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"conf/sslprofiles/listenerprofiles.xml"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>ssl_profile.read_interval</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"30s"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[[custom_transport.listener]]

class=""
protocol = "ISO8583"
parameter.port = 8081
parameter.hostname = "$ref{server.node_ip}"
parameter.non-blocking = true
parameter.bind-address = "$ref{server.hostname}"
parameter.WSDLEPRPrefix = "$ref{server.hostname}"  # inferred

keystore.file_name = "$ref{keystore.tls.file_name}"
keystore.type = "$ref{keystore.tls.type}"
keystore.password = "$ref{keystore.tls.password}"
keystore.key_password = "$ref{keystore.tls.key_password}"
truststore.file_name = "$ref{truststore.file_name}"
truststore.type = "$ref{truststore.type}"
truststore.password = "$ref{truststore.password}"

ssl_profile.file_path= "conf/sslprofiles/listenerprofiles.xml"
ssl_profile.read_interval = "30s"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring a custom transport sender</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[[custom_transport.sender]]</code>
                    <p>
                        This config heading is used to group the parameters for a custom transport implementation that you want to use in your product.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>class</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value">""</span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The .....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>protocol</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"ISO8583"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>parameter.port</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-possible">
                                    <span class="param-possible-value"><code>8081</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>parameter.hostname</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{server.node_ip}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>parameter.non-blocking</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>parameter.non_proxy_host</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>""</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>.......</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>keystore.file_name</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.file_name}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>keystore.type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.type}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>keystore.password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>keystore.key_password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{keystore.tls.key_password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>truststore.file_name</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.file_name}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>truststore.type</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.type}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>truststore.password</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"$ref{truststore.password}"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>ssl_profile.file_path</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"conf/sslprofiles/senderprofiles.xml"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"> <code>ssl_profile.read_interval</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string"> string </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"30s"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>...</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[[custom_transport.sender]]

class=""
protocol = "ISO8583"
parameter.hostname = ""
parameter.port = ""
parameter.non_proxy_host = ""
parameter.non_blocking = true

keystore.file_name = "$ref{keystore.tls.file_name}"
keystore.type = "$ref{keystore.tls.type}"
keystore.password = "$ref{keystore.tls.password}"
keystore.key_password = "$ref{keystore.tls.key_password}"
truststore.file_name = "$ref{truststore.file_name}"
truststore.type = "$ref{truststore.type}"
truststore.password = "$ref{truststore.password}"

ssl_profile.file_path = "conf/sslprofiles/senderprofiles.xml"
ssl_profile.read_interval = "30s"
</code></pre>
        </div>
    </section>
</div>
<div class="mb-config-catalog">
    <section class="title">
        <div class="mb-config-options">    
            <h2>Configuring the mediation process</h2>
        </div>
        <div class="mb-config-example"></div>
    </section>
    <section>
        <div class="mb-config-options">
            <div class="mb-config">
                <div class="config-wrap">
                    <code>[mediation]</code>
                    <span class="badge-required">Required</span>
                    <p>
                        Add this config heading to the ei.toml file to group the parameters for tuning the mediation process (Synapse engine) of the ESB. These parameters are mainly used when mediators such as Iterate and Clone (which leverage on internal thread pools) are used.
                    </p>
                </div>
                <div class="params-wrap">
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.core_threads</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"20"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The initial number of synapse threads in the pool. This parameter is applicable only if the Iterate or the Clone mediator is used to handle a higher load. The number of threads specified for this parameter should be increased as required to balance an increased load.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.max_threads</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"100"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The maximum number of synapse threads in the pool. This parameter is applicable only if the Iterate or the Clone mediator is used to handle a higher load. The number of threads specified for this parameter should be increased as required to balance an increased load.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.threads_queue_length</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"10"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The length of the queue that is used to hold the runnable tasks to be executed by the pool. This parameter is applicable only if the Iterate or the Clone mediator is used to handle a higher load.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.global_timeout_interval_millis</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer"> integer </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"120000"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The maximum number of milliseconds within which a response for the request should be received. A response which arrives after the specified number of seconds cannot be correlated with the request. Hence, a warning all be logged and the request will be dropped. This parameter is also referred to as the time-out handler.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.enable_xpath_dom_failover</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean"> boolean </span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>If this parameter is set to true , it will be possible for ESB to switch to xpath 2.0. The default value for this parameter is false since xpath 2.0 evaluations can cause performance degradation.
                                <details class="warning classes" open="open">
                                    <summary>Warning</summary>
                                    <p>WSO2 EI uses the Saxon Home Edition in implementing XPATH 2.0 functionalities, and thus supports all the functions that are shipped with it. For more information on the supported functions, see Saxon Documentation.</p>
                                </details>
                                </p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.temp_data_chunk_size</code> </span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>3072</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The message size that can be processed by the ESB server.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.command_debugger_port</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>9005</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.event_debugger_port</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>9006</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.script_mediator_pool_size</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>15</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>When using externally referenced scripts, this parameter is used to specify the size of the script engine pool to be used per script mediator. The script engines from this pool are used for externally referenced script execution where updates to external scripts on an engine currently in use may otherwise not be thread safe. It is recommended to keep this value at a reasonable size since there will be a pool per externally referenced script.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.enable_xml_nil</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.disable_auto_primitive_regex</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string">string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"^-?(0|[1-9][0-9]*)(\\.[0-9]+)?([eE][+-]?[0-9]+)?$"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.disable_custom_replace_regex</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string">string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"@@@"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.enable_namespace_declaration</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.build_valid_nc_name</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.enable_auto_primitive</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.json_out_auto_array</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>synapse.preserve_namespace_on_xml_to_json</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Preserves the namespace declarations in the JSON output in XML to JSON transformations.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>flow.statistics.enable</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Set this property to true and enable statistics for the required ESB artifact, to record information such as the following.
                                <ul>
                                    <li>The time spent on each mediator.</li>
                                    <li>The time spent to process each message.</li>
                                    <li>The fault count of a single message flow.</li>
                                </ul>
                                </p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>flow.statistics.capture_all</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Set this property to true and set the mediation.flow.statistics.enable property also to true, to enable mediation statistics for all the artifacts in the ESB profile by default.
                                <details class="warning classes" open="open">
                                    <summary>Note</summary>
                                    <p>If you set this property to false, you need to set the mediation.flow.statistics.enable property to true and manually enable statistics for the required ESB artifact.</p>
                                </details>
                                </p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>statistics.enable_clean</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>true</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>If this parameter is set to true, all the existing statistics would be cleared before processing a request. This is recommended if you want to increase the processing speed.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>statistics.clean_interval</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type string">string</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>"1000ms"</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>flow.statistics.tracer.collect_payload</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Set this property to true and enable tracing for the required ESB artifact, to record the message payload before and after the mediation performed by individual mediators.</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>flow.statistics.tracer.collect_properties</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type boolean">boolean</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>false</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>Set this property to true and enable tracing for the required ESB artifact, to record the following information.
                                <ul>
                                    <li>Message context properties.</li>
                                    <li>Message transport-scope properties.</li>
                                </ul>
                                </p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>inbound.core_threads</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>20</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                    <div class="param">
                        <div class="param-name">
                          <span class="param-name-wrap"><code>inbound.max_threads</code></span>
                        </div>
                        <div class="param-info">
                            <div>
                                <p>
                                    <span class="param-type integer">integer</span>
                                </p>
                                <div class="param-default">
                                    <span class="param-default-value"><code>100</code></span>
                                </div>
                            </div>
                            <div class="param-description">
                                <p>The.....</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mb-config-example">
<pre><code class="toml">[mediation]
synapse.core_threads = 20
synapse.max_threads = 100
synapse.threads_queue_length = 10

synapse.global_timeout_interval = "120000ms"

synapse.enable_xpath_dom_failover=true
synapse.temp_data_chunk_size=3072

synapse.command_debugger_port=9005
synapse.event_debugger_port=9006

synapse.script_mediator_pool_size=15
synapse.enable_xml_nil=false
synapse.disable_auto_primitive_regex = "^-?(0|[1-9][0-9]*)(\\.[0-9]+)?([eE][+-]?[0-9]+)?$"
synapse.disable_custom_replace_regex = "@@@"
synapse.enable_namespace_declaration = false
synapse.build_valid_nc_name = false
synapse.enable_auto_primitive = false
synapse.json_out_auto_array = false
synapse.preserve_namespace_on_xml_to_json=false
flow.statistics.enable=false
flow.statistics.capture_all=false
statistics.enable_clean=true
statistics.clean_interval = "1000ms"
flow.statistics.tracer.collect_payloads=false
flow.statistics.tracer.collect_properties=false
inbound.core_threads = 20
inbound.max_threads = 100
</code></pre>
        </div>
    </section>
</div>
