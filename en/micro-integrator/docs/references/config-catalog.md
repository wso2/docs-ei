# Product Configurations
This document describes all the product configuration parameters that are used in WSO2 Micro Integrator. 

## Instructions for use

To update the product configurations:

1. Open the `deployment.toml` file (stored in the `MI_HOME/conf` directory).
2. Select the required configuration headers and parameters from the list given below and apply them to the `deployment.toml` file.

See the example `deployment.toml` file given below.

```toml
# This is an example .toml file.

[server]
hostname="localhost"

[[database]]
pool_options.maxActiv=5
```



## Deployment

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="2" type="checkbox" id="_tab_2">
                <label class="tab-selector" for="_tab_2"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
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
proxy_context_path = ""
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[server]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the deployment parameters that are used for identifying a Micro Integrator server node. You need to update these values when you deploy <a href="../../setup/deployment/deploying_wso2_ei">WSO2 Micro Integrator</a>. The required and optional parameters for this configuration are listed below.
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
                                            <span class="param-default-value">Default: <code>&quot;localhost&quot;</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;localhost&quot;
,&quot;127.0.0.1&quot;,&quot;&lt;any-ip-address&gt;&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The hostname of the Micro Integrator instance.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>offset</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>0</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Port offset allows you to run multiple WSO2 products, multiple instances of a WSO2 product, or multiple WSO2 product clusters on the same server or virtual machine (VM). </br></br>Port offset defines the number by which all ports defined in the runtime such as the HTTP/S ports will be offset. For example, if the default HTTP port is 9443 and the port offset is 1, the effective HTTP port will be 9444. Therefore, for each additional WSO2 product instance, set the port offset to a unique value so that they can all run on the same server without any port conflicts.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>enable_mtom</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot; or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Use this paramater to enable MTOM (Message Transmission Optimization Mechanism) for the product server.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>enable_swa</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot; or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Use this paramater to enable SwA (SOAP with Attachments) for the product server. When SwA is enabled, the Micro Integrator will process the files attached to SOAP messages.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>hide_admin_service_wsdl</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p></p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>userAgent</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>WSO2 ${product.key} ${product.version}</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p></p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>serverDetails</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>WSO2 ${product.key} ${product.version}</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p></p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>synapse_config_file_path</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>repository/deployment/server/synapse-configs</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p></p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Primary keystore

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="3" type="checkbox" id="_tab_3">
                <label class="tab-selector" for="_tab_3"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[keystore.tls]
file_name = "wso2carbon.jks"
type = "JKS"
password = "wso2carbon"
alias = "wso2carbon"
key_password = "wso2carbon"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[keystore_tls]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the parameters that connect the Micro Integrator to the <a href="../../setup/security/configuring_keystores/#changing-the-default-primary-keystore">primary keystore</a>. This keystore is used for SSL handshaking (when the server communicates with another server) and for encrypting plain text information in configuration files. By default, this keystore is also used for encrypted data in internal datastores, unless you have configured a <a href="#internal-keystore">separate keystore</a> for internal data encryption.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>file_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon.jks</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the keystore file that is used for SSL communication and for encrypting/decrypting data in configuration files.</p>
                                    </div>
                                </div>
                            </div><div class="param">
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
                                            <span class="param-default-value">Default: <code>JKS</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot;,&quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file.</p>
                                    </div>
                                </div>
                            </div><div class="param">
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
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used for SSL communication and for encrypting/decrypting data in configuration files. The keystore password is used when accessing the keys in the keystore.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>alias</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The alias of the public key corresponding to the private key that is included in the keystore. The public key is used for encrypting data in the Micro Integrator server, which only the corresponding private key can decrypt. The public key is embedded in a digital certificate, and this certificate can be shared over the internet by storing it in a separate trust store file.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>key_password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
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
            </div>
        </div>
    </section>
</div>


## Internal keystore

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="4" type="checkbox" id="_tab_4">
                <label class="tab-selector" for="_tab_4"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[keystore.internal]
file_name = "$ref{keystore.tls.file_name}"
type = "$ref{keystore.tls.type}"
password = "$ref{keystore.tls.password}"
alias = "$ref{keystore.tls.alias}"
key_password = "$ref{keystore.tls.key_password}"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[keystore_internal]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the parameters that connect the Micro Integrator to the keystore used for encrypting/decrypting data in internal data stores. You may sometimes choose to configure a separate keystore for this purpose because the primary keystore needs to renew certificates frequently. However, for encrypting information in internal data stores, the keystore certificates should not be changed frequently because the data that is already encrypted will become unusable every time the certificate changes. Read more about <a href="../../setup/security/configuring_keystores/#separating-the-internal-keystore">configuring the internal keystore</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>file_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the keystore file that is used for data encryption/decryption in internal data stores. By default, the keystore file of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
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
                                            <span class="param-default-value">Default: <code>JKS</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot;,&quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file. By default, the keystore type of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
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
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used for data encryption/decryption in internal data stores. This keystore password is used when accessing the keys in the keystore. By default, the keystore password of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>alias</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The alias of the public key corresponding to the private key that is included in the keystore. The public key is used for encrypting data in the Micro Integrator server, which only the corresponding private key can decrypt. The public key is embedded in a digital certificate, and this certificate can be shared over the internet by storing it in a separate trust store file. By default, the alias of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>key_password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the private key that is included in the keystore. The private key is used to decrypt the data that has been encrypted using the keystore's public key. By default, the public key password of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Trust store

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="5" type="checkbox" id="_tab_5">
                <label class="tab-selector" for="_tab_5"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[truststore]
file_name="wso2truststore.jks"
type="JKS"
password="wso2carbon"
alias="symmetric.key.value"
algorithm=""
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[truststore]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the parameters that connect the Micro Integrator to the keystore file (trust store) that is used to store the digital certificates that the server trusts for SSL communication. Read more about <a href="../../setup/security/configuring_keystores/#optional-changing-the-default-truststore">configuring the truststore</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>file_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2truststore.jks</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the keystore file that is used for storing the trusted digital certificates. The product is shipped with a default trust store (wso2truststore.jks), which contains the self-signed digital certificate of the default keystore.</p>
                                    </div>
                                </div>
                            </div><div class="param">
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
                                            <span class="param-default-value">Default: <code>JKS</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot;,&quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file that is used as the trust store.</p>
                                    </div>
                                </div>
                            </div><div class="param">
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
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used as the trust store.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>alias</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>symmetric.key.value</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The alias is the password of the digital certificate (which holds the public key) that is included in the trustore.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Message builders (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="6" type="checkbox" id="_tab_6">
                <label class="tab-selector" for="_tab_6"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[message_builders]
application_xml = "org.apache.axis2.builder.ApplicationXMLBuilder"
form_urlencoded = "org.apache.synapse.commons.builders.XFormURLEncodedBuilder"
multipart_form_data = "org.apache.axis2.builder.MultipartFormDataBuilder"
text_plain = "org.apache.axis2.format.PlainTextBuilder"
application_json = "org.wso2.micro.integrator.core.json.JsonStreamBuilder"
json_badgerfish = "org.apache.axis2.json.JSONBadgerfishOMBuilder"
text_javascript = "org.apache.axis2.json.JSONBuilder"
octet_stream =  "org.wso2.carbon.relay.BinaryRelayBuilder"
application_binary = "org.apache.axis2.format.BinaryBuilder"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[message_builders]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>message builder</a> implementation that is used to build messages that are received by the Micro Integrator in the default <b>non-blocking</b> mode. If you are using the Micro Integrator in <b>blocking</b> mode, see the <a href='#message-builders-blocking-mode'>message builder configurations for blocking mode</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>application_xml</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.builder.ApplicationXMLBuilder</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the 'application_xml' content type. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default builder class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>form_urlencoded</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>admin</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>org.apache.synapse.commons.builders.XFormURLEncodedBuilder</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the 'form_urlencoded' content type. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default builder class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>multipart_form_data</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.builder.MultipartFormDataBuilder</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the 'multipart_form_data' content type. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default builder class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>text_plain</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.format.PlainTextBuilder</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the 'text_plain' content type. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default builder class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>application_json</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.wso2.micro.integrator.core.json.JsonStreamBuilder</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the 'application_json' content type. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default builder class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>json_badgerfish</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.json.JSONBadgerfishOMBuilder</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the 'json_badgerfish' content type. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default builder class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>text_javascript</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.json.JSONBuilder</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the 'text_javascript' content type. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default builder class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>octet_stream</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.wso2.carbon.relay.BinaryRelayBuilder</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the 'octet_stream' content type. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default builder class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>application_binary</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.format.BinaryBuilder</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the 'application_binary' content type. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default builder class</a>.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Message builders (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="7" type="checkbox" id="_tab_7">
                <label class="tab-selector" for="_tab_7"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[message_builders.blocking]
application_xml = "org.apache.axis2.builder.ApplicationXMLBuilder"
form_urlencoded = "org.apache.synapse.commons.builders.XFormURLEncodedBuilder"
multipart_form_data = "org.apache.axis2.builder.MultipartFormDataBuilder"
text_plain = "org.apache.axis2.format.PlainTextBuilder"
application_json = "org.wso2.micro.integrator.core.json.JsonStreamBuilder"
json_badgerfish = "org.apache.axis2.json.JSONBadgerfishOMBuilder"
text_javascript = "org.apache.axis2.json.JSONBuilder"
octet_stream =  "org.wso2.carbon.relay.BinaryRelayBuilder"
application_binary = "org.apache.axis2.format.BinaryBuilder"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[message_builders.blocking]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>message builder</a> implementation that is used to build messages that are received by the Micro Integrator in <b>blocking</b> mode. You can use the <a href='#message-builders-non-blocking-mode'>same list of parameters</a> that are available for message builders in non-blocking mode.
                            </p>
                        </div>
                        <div class="params-wrap">
                            
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Message formatters (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="8" type="checkbox" id="_tab_8">
                <label class="tab-selector" for="_tab_8"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[message_formatters]
form_urlencoded =  "org.apache.synapse.commons.formatters.XFormURLEncodedFormatter"
multipart_form_data =  "org.apache.axis2.transport.http.MultipartFormDataFormatter"
application_xml = "org.apache.axis2.transport.http.ApplicationXMLFormatter"
text_xml = "org.apache.axis2.transport.http.SOAPMessageFormatter"
soap_xml = "org.apache.axis2.transport.http.SOAPMessageFormatter"
text_plain = "org.apache.axis2.format.PlainTextFormatter"
application_json =  "org.wso2.micro.integrator.core.json.JsonStreamFormatter"
json_badgerfish = "org.apache.axis2.json.JSONBadgerfishMessageFormatter"
text_javascript = "org.apache.axis2.json.JSONMessageFormatter"
octet_stream = "org.wso2.carbon.relay.ExpandingMessageFormatter"
application_binary =  "org.apache.axis2.format.BinaryFormatter"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[message_formatters]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>message formatting</a> implementation that is used for formatting messages that are sent out of the Micro Integrator in <b>non-blocking</b> mode. If you are using the Micro Integrator in <b>blocking</b> mode, see the <a href='#message-formatter-blocking-mode'>message formatter configurations for blocking mode</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>application_xml</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.transport.http.ApplicationXMLFormatter</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the 'application_xml' content type before they are sent out of the Micro Integrator. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default formating class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>form_urlencoded</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.synapse.commons.formatters.XFormURLEncodedFormatter</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the 'form_urlencoded' content type before they are sent out of the Micro Integrator. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default formating class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>multipart_form_data</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.transport.http.MultipartFormDataFormatter</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the 'multipart_form_data' content type before they are sent out of the Micro Integrator. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default formating class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>text_plain</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.format.PlainTextFormatter</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the 'text_plain' content type before they are sent out of the Micro Integrator. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default formating class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>application_json</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.wso2.micro.integrator.core.json.JsonStreamFormatter</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the 'application_json' content type before they are sent out of the Micro Integrator. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default formating class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>json_badgerfish</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.json.JSONBadgerfishMessageFormatter</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the 'json_badgerfish' content type before they are sent out of the Micro Integrator. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default formating class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>text_javascript</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.json.JSONMessageFormatter</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the 'text_javascript' content type before they are sent out of the Micro Integrator. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default formating class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>octet_stream</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.wso2.carbon.relay.ExpandingMessageFormatter</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the 'octet_stream' content type before they are sent out of the Micro Integrator. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default formating class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>application_binary</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.format.BinaryFormatter</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the 'application_binary' content type before they are sent out of the Micro Integrator. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default formating class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>text_xml</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.transport.http.SOAPMessageFormatter</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the 'text_xml' content type before they are sent out of the Micro Integrator. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default formating class</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>soap_xml</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.apache.axis2.transport.http.SOAPMessageFormatter</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the 'soap_xml' content type before they are sent out of the Micro Integrator. If required, you can <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>change the default formating class</a>.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Message formatter (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="9" type="checkbox" id="_tab_9">
                <label class="tab-selector" for="_tab_9"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[message_formatters.blocking]
form_urlencoded =  "org.apache.synapse.commons.formatters.XFormURLEncodedFormatter"
multipart_form_data =  "org.apache.axis2.transport.http.MultipartFormDataFormatter"
application_xml = "org.apache.axis2.transport.http.ApplicationXMLFormatter"
text_xml = "org.apache.axis2.transport.http.SOAPMessageFormatter"
soap_xml = "org.apache.axis2.transport.http.SOAPMessageFormatter"
text_plain = "org.apache.axis2.format.PlainTextFormatter"
application_json =  "org.wso2.micro.integrator.core.json.JsonStreamFormatter"
json_badgerfish = "org.apache.axis2.json.JSONBadgerfishMessageFormatter"
text_javascript = "org.apache.axis2.json.JSONMessageFormatter"
octet_stream = "org.wso2.carbon.relay.ExpandingMessageFormatter"
application_binary =  "org.apache.axis2.format.BinaryFormatter"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[message_formatters.blocking]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>message formatter</a> implementations that are used to format messages that are sent out from the Micro Integrator in <b>blocking</b> mode. You can use the <a href='#message-formatters-non-blocking-mode'>same list of parameters</a> that are available for message formatters in non-blocking mode.
                            </p>
                        </div>
                        <div class="params-wrap">
                            
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Custom message builder (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="10" type="checkbox" id="_tab_10">
                <label class="tab-selector" for="_tab_10"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[custom_message_builders]]
content_type = "application/json/badgerfish"
class = "org.apache.axis2.json.JSONBadgerfishOMBuilder"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[custom_message_builders]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the custom message builder implementation class and the selected content types to which the builder should apply <b>in non-blocking mode</b>. See the instructions on configuring <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>custom message builders and formatters</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>content_type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The content types to which the custom message builder implementation should apply. You can specify the list of content types as follows: <code>application/json/badgerfish</code>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>class</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The custom message builder implementation that should apply to the given content types.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Custom message builder (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="11" type="checkbox" id="_tab_11">
                <label class="tab-selector" for="_tab_11"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[custom_message_builders.blocking]]
content_type = "application/json/badgerfish"
class = "org.apache.axis2.json.JSONBadgerfishOMBuilder"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[custom_message_builders.blocking]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the custom message builder implementation class and the selected content types to which the builder should apply <b>in blocking mode</b>. See the instructions on configuring <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>custom message builders and formatters</a>. You can use the <a href='#custom-message-builder-non-blocking-mode'>same list of parameters</a> that are available for custom message builders in non-blocking mode.
                            </p>
                        </div>
                        <div class="params-wrap">
                            
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Custom message formatter (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="12" type="checkbox" id="_tab_12">
                <label class="tab-selector" for="_tab_12"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[custom_message_formatters]]
content_type = "application/json/badgerfish"
class = "org.apache.axis2.json.JSONBadgerfishMessageFormatter"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[custom_message_formatters]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the custom message formatter implementation class and the selected content types to which the formatter should apply <b>in non-blocking mode</b>. See the instructions on configuring <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>custom message builders and formatters</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>content_type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The content types to which the custom message formatter implementation should apply. You can specify the list of content types as follows: <code>application/json/badgerfish</code>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>class</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The custom message formatter implementation that should apply to the given content types.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Custom message formatter (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="13" type="checkbox" id="_tab_13">
                <label class="tab-selector" for="_tab_13"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[custom_message_formatters.blocking]]
content_type = "application/json/badgerfish"
class = "org.apache.axis2.json.JSONBadgerfishMessageFormatter"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[custom_message_formatters.blocking]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the custom message formatter implementation class and the selected content types to which the formatter should apply <b>in blocking mode</b>. See the instructions on configuring <a href='../../setup/message_builders_formatters/message-builders-and-formatters'>custom message builders and formatters</a>. You can use the <a href='#custom-message-formatter-non-blocking-mode'>same list of parameters</a> that are available for custom message formatters in non-blocking mode.
                            </p>
                        </div>
                        <div class="params-wrap">
                            
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Server request processor

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="14" type="checkbox" id="_tab_14">
                <label class="tab-selector" for="_tab_14"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[server.get_request_processor]]
item = "swagger.yaml"
class = "org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerYamlProcessor"

[[server.get_request_processor]]
item = "swagger.json"
class = "org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerJsonProcessor"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[server.get_request_processor]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the parameters that specify how special HTTP GET requests (such as '?wsdl', '?policy', etc.) are processed. This is an array-type header, which you can reuse depending on the number of processors you want to enable.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>item</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>&#39;swagger.yaml&#39; and &#39;swagger.json&#39;</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The item repesents the first parameter in the query string (e.g. ?wsdl), which needs special processing. The following two items are enabled by defualt:</br>- <code>swagger.yaml</code></br>- <code>swagger.json</code></p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>class</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>&#39;org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerYamlProcessor&#39; and &#39;org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerYamlProcessor&#39;</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>This is the class that implements the <code>org.wso2.carbon.transport.HttpGetRequestProcessor</code> processor. By default, the following two classes are used for handling the two default request items:</br></br>- For <b>swagger.yaml</b>: <code>org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerYamlProcessor</code></br>- For <b>swagger.json</b>: <code>org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerYamlProcessor</code></p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## HTTP/S transport (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="15" type="checkbox" id="_tab_15">
                <label class="tab-selector" for="_tab_15"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
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
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.http]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the parameters that are used for <a href='../../setup/performance_tuning/http_transport_tuning'>tuning the default HTTP/S passthrough transport</a> of the Micro Integrator in non-blocking mode.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>socket_timeout</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>120000</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>This is the maximum period of inactivity between two consecutive data packets, specified in milliseconds.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>core_worker_pool_size</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>400</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The Micro Integrator uses a thread pool executor to create threads and to handle incoming requests. This parameter controls the number of core threads used by the executor pool. If you increase this parameter value, the number of requests received that can be processed by the integrator increases, hence, the throughput also increases. The nature of the integration scenario and the number of concurrent requests received by the integrator are the main factors that helps to determine this parameter.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>max_worker_pool_size</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>400</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>This is the maximum number of threads in the worker thread pool. Specifying a maximum limit avoids performance degradation that can occur due to context switching. If the specified value is reached, you will see the error 'SYSTEM ALERT - HttpServerWorker threads were in BLOCKED state during last minute'. This can occur due to an extraordinarily high number of requests sent at a time when all the threads in the pool are busy, and the maximum number of threads is already reached.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>worker_pool_queue_length</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-1</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>This defines the length of the queue that is used to hold runnable tasks to be executed by the worker pool. The thread pool starts queuing jobs when all the existing threads are busy, and the pool has reached the maximum number of threads. The value for this parameter should be -1 to use an unbound queue. If a bound queue is used and the queue gets filled to its capacity, any further attempts to submit jobs fail causing some messages to be dropped by Synapse.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>io_buffer_size</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>16384</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>This is the value of the memory buffer allocated when reading data into the memory from the underlying socket/file channels. You should leave this property set to the default value.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>max_http_connection_per_host_port</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>32767</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>This defines the maximum number of connections allowed per host port.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>preserve_http_user_agent</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,&quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>If this parameter is set to true, the user-agent HTTP header of messages passing through the integrator is preserved and printed in the outgoing message.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>preserve_http_headers</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>Content-Type</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>This parameter allows you to specify the header field/s of messages passing through the EI that need to be preserved and printed in the outgoing message such as <code>Location</code>, <code>CommonsHTTPTransportSenderKeep-Alive</code>, <code>Date</code>, <code>Server</code>, <code>User-Agent</code>, and <code>Host</code>. For example, <code>http.headers.preserve = Location, Date, Server</code>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>disable_connection_keepalive</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,&quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>If this parameter is set to true, the HTTP connections with the back end service are closed soon after the request is served. It is recommended to set this property to false so that the integrator does not have to create a new connection every time it sends a request to a back-end service. However, you may need to close connections after they are used if the back-end service does not provide sufficient support for keep-alive connections.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.port</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>8280</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The port on which this transport receiver should listen for incoming messages.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.wsdl_epr_prefix</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>$ref{server.hostname}</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>A URL prefix which will be added to all service EPRs and EPRs in WSDLs etc.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.secured_port</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>8243</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The secured port on which this transport receiver should listen for incoming messages.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.keystore.file_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the keystore file that is used for securing the HTTP passthrough connection. By default, the keystore file of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.keystore.type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>JKS</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot;,&quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file. By default, the keystore type of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.keystore.password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used for securing the HTTP passthrough connection. This keystore password is used when accessing the keys in the keystore. By default, the keystore password of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.keystore.key_password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the private key that is used for securing the HTTP passthrough connection. This keystore password is used when accessing the keys in the keystore. By default, the keystore password of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.truststore.file_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2truststore.jks</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the keystore file that is used for storing the trusted digital certificates. By default, the product's <a href='#trust-store'>trust store</a> is configured for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.truststore.type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>JKS</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot;,&quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file that is used as the trust store. By default, the product's <a href='#trust-store'>trust store</a> is configured for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.truststore.password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used as the trust store. By default, the product's <a href='#trust-store'>trust store</a> is configured for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.warn_on_http_500</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>If the outgoing messages should be sent through an HTTP proxy server, use this parameter to specify the target proxy.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.proxy_host</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>${deployement.node_ip}</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>If the outgoing messages should be sent through an HTTP proxy server, use this parameter to specify the target proxy.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.proxy_port</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>3128</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The port through which the target proxy accepts HTTP traffic.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.non_proxy_hosts</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>[&quot;$ref{server.hostname}&quot;]</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The list of hosts to which the HTTP traffic should be sent directly without going through the proxy. When trying to add multiple hostnames along with an asterisk in order to define a set of sub-domains for non-proxy hosts, you need to add a period before the asterisk when configuring proxy server.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.hostname_verifier</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>AllowAll</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The list of hosts to which the HTTP traffic should be sent directly without going through the proxy. When trying to add multiple hostnames along with an asterisk in order to define a set of sub-domains for non-proxy hosts, you need to add a period before the asterisk when configuring proxy server.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.keystore.file_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the keystore file that is used for securing the HTTP passthrough connection. By default, the keystore file of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.keystore.type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>JKS</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot;,&quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file. By default, the keystore type of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.keystore.password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used for securing the HTTP passthrough connection. This keystore password is used when accessing the keys in the keystore. By default, the keystore password of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.keystore.key_password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the private key that is used for securing the HTTP passthrough connection. This keystore password is used when accessing the keys in the keystore. By default, the keystore password of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.truststore.file_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2truststore.jks</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the keystore file that is used for storing the trusted digital certificates. By default, the product's <a href='#trust-store'>trust store</a> is configured for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.truststore.type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>JKS</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot;,&quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file that is used as the trust store. By default, the product's <a href='#trust-store'>trust store</a> is configured for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.truststore.password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used as the trust store. By default, the product's <a href='#trust-store'>trust store</a> is configured for this purpose.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## HTTP transport (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="16" type="checkbox" id="_tab_16">
                <label class="tab-selector" for="_tab_16"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.blocking.http]

listener.enable = true
listener.port = 8200
listener.hostname = ""
listener.origin_server = ""
listener.request_timeout = ""
listener.request_tcp_no_delay = ""
listener.request_core_thread_pool_size = ""
listener.request_max_thread_pool_size = ""
listener.thread_keepalive_time = ""
listener.thread_keepalive_time_unit = ""

sender.enable = true
sender.enable_client_caching = true
sender.transfer_encoding = "chunked"
sender.default_connections_per_host = 200
sender.omit_soap12_action = true
sender.so_timeout = 60000
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.blocking.http]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the parameters that are used for configuring the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-httphttps-transport'>default HTTP/S passthrough transport in blocking mode</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.enable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,&quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This parameter is used for enabling the HTTP passthrough transport listener in blocking mode.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.port</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>8200</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The port on which this transport receiver should listen for incoming messages.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.hostname</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>N/A</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p></p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.origin_server</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>N/A</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p></p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.request_timeout</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>N/A</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p></p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.request_tcp_no_delay</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>N/A</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p></p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.request_core_thread_pool_size</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>N/A</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p></p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.request_max_thread_pool_size</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>N/A</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p></p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.thread_keepalive_time</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>N/A</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p></p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.thread_keepalive_time_unit</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>N/A</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p></p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,&quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This parameter is used for enabling the HTTP passthrough transport sender in blocking mode.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.enable_client_caching</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,&quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This parameter is used to specify whether the HTTP client should save cache entries and the cached responses in the JVM memory or not.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.transfer_encoding</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>chunked</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;chunked&quot;,&quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This parameter enables you to specify whether the data sent should be chunked. It can be used instead of the Content-Length header if you want to upload data without having to know the amount of data to be uploaded in advance.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.default_connections_per_host</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot; or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The maximum number of connections that will be created per host server by the client. If the backend server is slow, the connections in use at a given time will take a long time to be released and added back to the connection pool. As a result, connections may not be available for some requests. In such situations, it is recommended to increase the value for this parameter.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.omit_soap12_action</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot; or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>If following is set to 'true', optional action part of the Content-Type will not be added to the SOAP 1.2 messages.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.so_timeout</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot; or &quot;false&quot;</code></span>
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
            </div>
        </div>
    </section>
</div>


## HTTP proxy profile

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="17" type="checkbox" id="_tab_17">
                <label class="tab-selector" for="_tab_17"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[transport.http.proxy_profile]]
target_hosts = ["example.com", ".*.sample.com"]
proxy_host = "localhost"
proxy_port = "3128"
proxy_username = "squidUser"
proxy_password = "password"
bypass_hosts = ["xxx.sample.com"]
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[transport.http.proxy_profile]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring <a href='../../setup/configuring_proxy_servers/#configuring-proxy-profiles-in-wso2-micro-integrator'>HTTP proxy profiles</a> when you use multiple proxy servers to route messages to different endpoints.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>target_hosts</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>*&quot;,&quot;example.com&quot;,&quot;&lt;any-ip-address&gt;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>A host name or a comma-separated list of host names for a target endpoint. Host names can be specified as regular expressions that match a pattern. When asterisks (*) is specified as the target hostname, it will match all the hosts in the profile.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>proxy_host</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>localhost</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The host name of the proxy server.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>proxy_port</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The port number of the proxy server.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>proxy_username</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The user name for the authenticating the proxy server.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>proxy_password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password for authenticating the proxy server.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>bypass_hosts</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>A host name or a comma-separated list of host names that should <b>not</b> be sent via the proxy server.</br> For example, if you want all requests sent to <code>*.sample.com</code> to be sent via a proxy server, while you need to directly send requests to <code>hello.sample.com</code> (without going through the proxy server), you can add <code>hello.sample.com</code> as a bypass host name.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## HTTP secured proxy profile

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="18" type="checkbox" id="_tab_18">
                <label class="tab-selector" for="_tab_18"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[transport.http.secured_proxy_profile]]
target_hosts = ["example.com", ".*.sample.com"]
proxy_host = "localhost"
proxy_port = "3128"
proxy_username = "squidUser"
proxy_password = "password"
bypass_hosts = ["xxx.sample.com"]
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[transport.http.secured_proxy_profile]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring <a href='../../setup/configuring_proxy_servers/#configuring-proxy-profiles-in-wso2-micro-integrator'>secured HTTP proxy profiles</a> when you use multiple (secured) proxy servers to route messages to different endpoints.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>target_hosts</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>*&quot;,&quot;example.com&quot;,&quot;&lt;any-ip-address&gt;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>A host name or a comma-separated list of host names for a target endpoint. Host names can be specified as regular expressions that match a pattern. When asterisks (*) is specified as the target hostname, it will match all the hosts in the profile.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>proxy_host</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>localhost</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The host name of the proxy server.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>proxy_port</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The port number of the proxy server.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>proxy_username</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The user name for the authenticating the proxy server.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>proxy_password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password for authenticating the proxy server.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>bypass_hosts</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>A host name or a comma-separated list of host names that should <b>not</b> be sent via the proxy server.</br> For example, if you want all requests sent to <code>*.sample.com</code> to be sent via a proxy server, while you need to directly send requests to <code>hello.sample.com</code> (without going through the proxy server), you can add <code>hello.sample.com</code> as a bypass host name.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## VFS transport

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="19" type="checkbox" id="_tab_19">
                <label class="tab-selector" for="_tab_19"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.vfs]

listener.enable = true
listener.keystore.file_name = "$ref{keystore.tls.file_name}" 
listener.keystore.type = "$ref{keystore.tls.type}"
listener.keystore.password = "$ref{keystore.tls.password}"
listener.keystore.key_password = "$ref{keystore.tls.key_password}"
listener.keystore.alias = "$ref{keystore.tls.alias}"

listener.parameter.customParameter = ""

sender.enable = true
sender.parameter.customParameter = ""
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.vfs]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring how the Micro Integrator communicates through the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-vfs-transport'>VFS transport</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling the VFS transport listener.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.keystore.file_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the keystore file that is used for securing a VFS connection. By default, the keystore file of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.keystore.type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>JKS</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot;,&quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file. By default, the keystore type of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.keystore.password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used for securing a VFS connection. This keystore password is used when accessing the keys in the keystore. By default, the keystore password of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.keystore.alias</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The alias of the public key corresponding to the private key that is included in the keystore. The public key is used for encrypting data in the Micro Integrator server, which only the corresponding private key can decrypt. The public key is embedded in a digital certificate, and this certificate can be shared over the internet by storing it in a separate trust store file. By default, the alias of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.keystore.key_password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>wso2carbon</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the private key that is included in the keystore. The private key is used to decrypt the data that has been encrypted using the keystore's public key. By default, the public key password of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,or &quot;false&quot;</code></span>
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
            </div>
        </div>
    </section>
</div>


## MAIL transport listener (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="20" type="checkbox" id="_tab_20">
                <label class="tab-selector" for="_tab_20"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.mail.listener]
enable = true   
name = "mailto"
parameter.customParameter = ""
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.mail.listener]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-mailto-transport'>MailTo transport</a> listener implementation of the Micro Integrator in non-blocking mode. Note that the list of parameters given below can be used for the non-blocking transport listener as well as the <a href='#mail-transport-listener-blocking-mode'>blocking transport listener</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling the MAIL transport listener in the Micro Integrator.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the transport receiver.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## MAIL transport listener (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="21" type="checkbox" id="_tab_21">
                <label class="tab-selector" for="_tab_21"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.blocking.mail.listener]
enable = true
name = "mailto"
parameter.customParameter = "value"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.blocking.mail.listener]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that are used to configure the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-mailto-transport'>MailTo transport</a> listener in blocking mode. You can use the <a href='#mail-transport-listener-non-blocking-mode'>same list of parameters</a> that are available for the non-blocking mail sender.
                            </p>
                        </div>
                        <div class="params-wrap">
                            
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## MAIL transport sender (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="22" type="checkbox" id="_tab_22">
                <label class="tab-selector" for="_tab_22"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
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
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[transport.mail.sender]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that are used to configure the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-mailto-transport'>MailTo transport</a> sender implementation of the Micro Integrator in non-blocking mode. Note that the list of parameters given below can be used for the non-blocking transport sender as well as the <a href='#mail-transport-sender-blocking-mode'>blocking transport sender</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>mailto</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling the MAIL transport sender in the Micro Integrator.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.hostname</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>smtp.gmail.com</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The mail server that serves outgoing mails from the Micro Integrator.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.port</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>587</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The port of the mail server.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.enable_tls</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This parameter specifies whether TLS is enabled for the MailTo transport.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.username</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>demo_user</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The user name of the email account (mail sender). Note that in some email service providers, the user name is the same as the email address specified for 'parameter.from'.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>mailpassword</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the email account (mail sender).</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.from</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>demo_user@wso2.com</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The email address from which mails will be sent.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## MAIL transport sender (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="23" type="checkbox" id="_tab_23">
                <label class="tab-selector" for="_tab_23"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[transport.blocking.mail.sender]]

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
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[transport.blocking.mail.sender]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that are used to configure the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-mailto-transport'>MailTo transport</a> sender in blocking mode. You can use the <a href='#mail-transport-sender-non-blocking-mode'>same list of parameters</a> that are available for the non-blocking mail sender.
                            </p>
                        </div>
                        <div class="params-wrap">
                            
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## JMS transport listener (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="24" type="checkbox" id="_tab_24">
                <label class="tab-selector" for="_tab_24"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
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
parameter.receive_timeout = "1000"
parameter.concurrent_consumer = 1
parameter.max_concurrent_consumer = 1
parameter.idle_task_limit = 10
parameter.max_message_per_task = -1
parameter.initial_reconnection_duration = "10000"
parameter.reconnect_progress_factor = 2
parameter.max_reconnect_duration = "3600000"
parameter.reconnect_interval = "3600000"
parameter.max_jsm_connection = 10
parameter.max_consumer_error_retrieve_before_delay = 20
parameter.consume_error_delay = "100"         
parameter.consume_error_progression = "2.0"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[transport.jms.listener]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that are used to configure the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-jms-transport'>JMS transport</a> listener implementation of the Micro Integrator in non-blocking mode. Note that the list of parameters given below can be used for the non-blocking transport listener as well as the <a href='#jms-transport-listener-blocking-mode'>blocking transport listener</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The user-defined name of the JMS listener.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.initial_naming_factory</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>JNDI initial context factory class. The class must implement the <code>java.naming.spi.InitialContextFactory</code> interface.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.provider_url</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>URL of the JNDI provider.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.connection_factory_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JNDI name of the connection factory.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.cache_level</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>consumer</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>consumer</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The cache level that should apply when JMS objects startup. When the Micro Integrator produces JMS messages, you need to specify this cache level in the deployment.toml file. If the Micro Integrator works as JMS listener, you need to specify the JMS cache level in the proxy service. See the list of <a href='../../references/synapse-properties/transport-parameters/jms-transport-parameters'/>service-level JMS parameters</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.naming_security_principal</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JNDI Username.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.naming_security_credential</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JNDI password.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.transactionality</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Preferred mode of transactionality. <b>Note</b> that JMS transactions only works with either the <a href='../../references/mediators/callout-Mediator'>Callout mediator</a> or the <a href='../../references/mediators/call-Mediator'>Call mediator</a> in blocking mode.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.transaction_jndi_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>JNDI name to be used to require user transaction.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.cache_user_transaction</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether or not caching should be enabled for user transactions.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.session_transaction</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether or not the JMS session should be transacted.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.session_acknowledgement</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>AUTO_ACKNOWLEDGE</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>JMS session acknowledgment mode.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.jms_spec_version</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>1.1</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>JMS API version.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.username</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JMS connection username.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JMS connection password.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.destination</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JNDI name of the destination.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.destination_type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&#39;queue&#39; or &#39;topic&#39;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the destination.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.default_reply_destination</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JNDI name of the default reply destination.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.default_destination_type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>queue</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;queue&quot;, &quot;topic&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the reply destination.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.message_selector</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message selector implementation.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.subscription_durable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether or not the connection factory is subscription durable.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.durable_subscriber_client_id</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The ClientId parameter when using durable subscriptions.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.durable_subscriber_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the durable subscriber.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.pub_sub_local</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether or not the messages should should be published by the same connection in which the messages were received.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.receive_timeout</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>1000</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Time to wait for a JMS message during polling. Set this parameter value to a negative integer to wait indefinitely. Set to zero to prevent waiting.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.concurrent_consumer</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>1</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The number of concurrent threads to be started to consume messages when polling.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.max_concurrent_consumer</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>1</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The maximum number of concurrent threads to use during polling.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.idle_task_limit</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>10</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The number of idle runs per thread before it dies out.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.max_message_per_task</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-1</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The maximum number of successful message receipts per thread.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.initial_reconnection_duration</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>10000</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The initial reconnection attempts duration in milliseconds.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.reconnect_progress_factor</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>2</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The factor by which the reconnection duration will be increased.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.max_reconnect_duration</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>3600000</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The maximum reconnection duration in milliseconds.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.reconnect_interval</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>3600000</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The reconnection interval in milliseconds.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.max_jsm_connection</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>10</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The maximum cached JMS connections in the producer level.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.max_consumer_error_retrieve_before_delay</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>20</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The number of retries on consume errors before sleep delay becomes effective.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.consume_error_delay</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>100</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The sleep delay when a consume error is encountered (in milliseconds).</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.consume_error_progression</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>2.0</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The factor by which the consume error retry sleep will be increased.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## JMS transport listener (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="25" type="checkbox" id="_tab_25">
                <label class="tab-selector" for="_tab_25"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[transport.blocking.jms.listener]]

name = "myTopicListener"
parameter.initial_naming_factory = "org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory"
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
parameter.receive_timeout = "1000"
parameter.concurrent_consumer = 1
parameter.max_concurrent_consumer = 1
parameter.idle_task_limit = 10
parameter.max_message_per_task = -1
parameter.initial_reconnection_duration = "10000"
parameter.reconnect_progress_factor = 2
parameter.max_reconnect_duration = "3600000"
parameter.reconnect_interval = "3600000"
parameter.max_jsm_connection = 10
parameter.max_consumer_error_retrieve_before_delay = 20
parameter.consume_error_delay = "100"        
parameter.consume_error_progression = "2.0"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[transport.blocking.jms.listener]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that are used to configure the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-jms-transport'>JMS transport</a> listener in blocking mode. You can use the <a href='#jms-transport-listener-non-blocking-mode'>same list of parameters</a> that are available for the non-blocking JMS listener.
                            </p>
                        </div>
                        <div class="params-wrap">
                            
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## JMS transport sender (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="26" type="checkbox" id="_tab_26">
                <label class="tab-selector" for="_tab_26"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[transport.jms.sender]]

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
parameter.destination_type = "queue"
parameter.default_reply_destination = ""
parameter.default_destination_type = "queue"
parameter.message_selector = ""
parameter.subscription_durable = false
parameter.durable_subscriber_client_id = ""
parameter.durable_subscriber_name = ""
parameter.pub_sub_local = false
parameter.receive_timeout = "1000"
parameter.concurrent_consumer = 1
parameter.max_concurrent_consumer = 1
parameter.idle_task_limit = 10
parameter.max_message_per_task = -1
parameter.initial_reconnection_duration = "10000"
parameter.reconnect_progress_factor = 2
parameter.max_reconnect_duration = "3600000"
parameter.reconnect_interval = "3600000"
parameter.max_jsm_connection = 10
parameter.max_consumer_error_retrieve_before_delay = 20
parameter.consume_error_delay = "100"
parameter.consume_error_progression = "2.0"

parameter.vender_class_loader = false
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[transport.jms.sender]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that are used to configure the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-jms-transport'>JMS transport</a> sender implementation of the Micro Integrator in non-blocking mode.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The user-defined name of the JMS sender.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.initial_naming_factory</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>JNDI initial context factory class. The class must implement the <code>java.naming.spi.InitialContextFactory</code> interface.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.broker_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the JMS broker.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.provider_url</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>URL of the JNDI provider.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.connection_factory_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JNDI name of the connection factory.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.cache_level</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>producer</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>producer</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The cache level that should apply when JMS objects startup. When the Micro Integrator produces JMS messages, you need to specify this cache level in the deployment.toml file. If the Micro Integrator works as JMS listener, you need to specify the JMS cache level in the proxy service. See the list of <a href='../../references/synapse-properties/transport-parameters/jms-transport-parameters'/>service-level JMS parameters</a>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.naming_security_principal</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JNDI Username.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.naming_security_credential</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JNDI password.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.transactionality</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Preferred mode of transactionality. <b>Note</b> that JMS transactions only works with either the <a href='../../references/mediators/callout-Mediator'>Callout mediator</a> or the <a href='../../references/mediators/call-Mediator'>Call mediator</a> in blocking mode.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.transaction_jndi_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>JNDI name to be used to require user transaction.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.cache_user_transaction</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether or not the JMS session should be transacted.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.session_transaction</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether or not the JMS session should be transacted.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.session_acknowledgement</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>AUTO_ACKNOWLEDGE</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>JMS session acknowledgment mode.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.jms_spec_version</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>1.1</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>JMS API version.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.username</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JMS connection username.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JMS connection password.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.destination</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JNDI name of the destination.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.destination_type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&#39;queue&#39; or &#39;topic&#39;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the destination.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.default_reply_destination</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The JNDI name of the default reply destination.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.default_destination_type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>queue</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;queue&quot;, &quot;topic&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the reply destination.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.message_selector</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message selector implementation.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.subscription_durable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether or not the connection factory is subscription durable.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.durable_subscriber_client_id</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The ClientId parameter when using durable subscriptions.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.durable_subscriber_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the durable subscriber.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.pub_sub_local</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether or not the messages should should be published by the same connection in which the messages were received.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.receive_timeout</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>1000</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Time to wait for a JMS message during polling. Set this parameter value to a negative integer to wait indefinitely. Set to zero to prevent waiting.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.concurrent_consumer</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>1</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The number of concurrent threads to be started to consume messages when polling.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.max_concurrent_consumer</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>1</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The maximum number of concurrent threads to use during polling.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.idle_task_limit</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>10</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The number of idle runs per thread before it dies out.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.max_message_per_task</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-1</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The maximum number of successful message receipts per thread.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.initial_reconnection_duration</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>10000</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The initial reconnection attempts duration in milliseconds.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.reconnect_progress_factor</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>2</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The factor by which the reconnection duration will be increased.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.max_reconnect_duration</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>3600000</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The maximum reconnection duration in milliseconds.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.reconnect_interval</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>3600000</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The reconnection interval in milliseconds.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.max_jsm_connection</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>10</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The maximum cached JMS connections in the producer level.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.max_consumer_error_retrieve_before_delay</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>20</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The number of retries on consume errors before sleep delay becomes effective.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.consume_error_delay</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>100</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The sleep delay when a consume error is encountered (in milliseconds).</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.consume_error_progression</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>2.0</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The factor by which the consume error retry sleep will be increased.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## JMS transport sender (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="27" type="checkbox" id="_tab_27">
                <label class="tab-selector" for="_tab_27"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[transport.blocking.jms.sender]]

name = "myTopicSender"
parameter.initial_naming_factory = "org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory"
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
parameter.destination_type = "queue"
parameter.default_reply_destination = ""
parameter.default_destination_type = "queue"
parameter.message_selector = ""
parameter.subscription_durable = false
parameter.durable_subscriber_client_id = ""
parameter.durable_subscriber_name = ""
parameter.pub_sub_local = false
parameter.receive_timeout = "1000"
parameter.concurrent_consumer = 1
parameter.max_concurrent_consumer = 1
parameter.idle_task_limit = 10
parameter.max_message_per_task = -1
parameter.initial_reconnection_duration = "10000"
parameter.reconnect_progress_factor = 2
parameter.max_reconnect_duration = "3600000"
parameter.reconnect_interval = "3600000"
parameter.max_jsm_connection = 10
parameter.max_consumer_error_retrieve_before_delay = 20
parameter.consume_error_delay = "100"
parameter.consume_error_progression = "2.0"
parameter.vender_class_loader = false
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[transport.blocking.jms.sender]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that are used to configure the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-jms-transport'>JMS transport</a> sender in blocking mode. You can use the <a href='#jms-transport-sender-non-blocking-mode'>same list of parameters</a> that are available for the non-blocking JMS sender.
                            </p>
                        </div>
                        <div class="params-wrap">
                            
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## JNDI connection factories

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="28" type="checkbox" id="_tab_28">
                <label class="tab-selector" for="_tab_28"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.jndi.connection_factories]
QueueConnectionFactory = "amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'"
TopicConnectionFactory = "amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.jndi.connection_factories]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters used for specifying the JNDI connection factory classes.
                            </p>
                        </div>
                        <div class="params-wrap">
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
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>amqp://admin:admin@clientID/carbon?brokerlist=&#39;tcp://localhost:5675&#39;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The connection factory URL for connecting to a JMS queue.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>QueueConnectionFactory</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>amqp://admin:admin@clientID/carbon?brokerlist=&#39;tcp://localhost:5675&#39;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The connection factory URL for connecting to a JMS topic.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## JNDI queues

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="29" type="checkbox" id="_tab_29">
                <label class="tab-selector" for="_tab_29"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.jndi.queue]
JMSMS = "JMSMS"
StockQuotesQueue = "StockQuotesQueue"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.jndi.queue]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is used to specify the list of queues that are defined your JMS broker. The JNDI name of the queue, and the actual queue name should be specifed as a key-value pair as follows: <code>jndi_name</code> = <code>queue_name</code>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>&lt;jndi_queue_name&gt;</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&lt;queue_name&gt;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The jndi queue name and the actual queue name as a key-value pair.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## JNDI topics

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="30" type="checkbox" id="_tab_30">
                <label class="tab-selector" for="_tab_30"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.jndi.topic]
MyTopic = "example.MyTopic"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.jndi.topic]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is used to specify the list of topics that are defined your JMS broker. The JNDI name of the topic, and the actual topic name should be specifed as a key-value pair as follows: <code>jndi_name</code> = <code>topic_name</code>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>&lt;jndi_topic_name&gt;</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&lt;topic_name&gt;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The jndi queue name and the actual topic name as a key-value pair.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## RabbitMQ listener

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="31" type="checkbox" id="_tab_31">
                <label class="tab-selector" for="_tab_31"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
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
parameter.connection_pool_size = 25

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
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[transport.rabbitmq.listener]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required if you are configuring WSO2 Micro Integrator to receive messages from a RabbitMQ client. Read more about <a href='../../setup/brokers/configure-with-rabbitMQ'>connecting to RabbitMQ</a> from the Micro Integrator.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the broker.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.hostname</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>localhost</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The IP address of the server node.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.port</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>5672</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The port on which the RabbitMQ broker can be accessed.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.username</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>guest</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The user name for connecting to RabbitMQ broker.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>guest</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password for connecting to the RabbitMQ broker.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.connection_factory</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>org.apache.axis2.transport.rabbitmq.RabbitMQListener</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the connection factory.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.exchange_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>amq.direct</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Name of the RabbitMQ exchange to which the queue is bound. Use this parameter instead of rabbitmq.queue.routing.key, if you need to use the default exchange and publish to a queue.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.queue_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>MyQueue</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The queue name to send or consume messages. If you do not specify this parameter, you need to specify the rabbitmq.queue.routing.key parameter.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.queue_auto_ack</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Defines how the message processor sends the acknowledgement when consuming messages recived from the RabbitMQ message store. If you set this to true, the message processor automatically sends the acknowledgement to the messages store as soon as it receives messages from it. This is called an auto acknowledgement.</br> If you set it to false, the message processor waits until it receives the response from the backend to send the acknowledgement to the mssage store. This is called a client acknowledgement.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.consumer_tag</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The client­ generated consumer tag to establish context.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.channel_consumer_qos</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The consumer qos value. You need to specify this parameter only if the rabbitmq.queue.auto.ack parameter is set to false.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.durable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether the queue should remain declared even if the broker restarts.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.queue_exclusive</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether the queue should be exclusive or should be consumable by other connections.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.queue_auto_delete</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether to keep the queue even if it is not being consumed anymore.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.queue_routing_key</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The routing key of the queue.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.queue_auto_declare</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether to create queues if they are not present. However, you should set this parameter only if queues are not declared prior on the broker. Setting this parameter in the publish URL to false improves RabbitMQ transport performance.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.exchange_auto_declare</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether to create exchanges if they are not present. However, you should set this parameter only if exchanges are not declared prior on the broker. Setting this parameter in the publish URL to false improves RabbitMQ transport performance.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.exchange_type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the exchange.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.exchange_durable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether the exchange should remain declared even if the broker restarts.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.exchange_auto_delete</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether to keep the exchange even if it is not bound to any queue anymore.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.default_destination_type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>text/xml</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The content type of the consumer.</br> Note that if the content type is specified in the message, this parameter does not override the specified content type.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.retry_interval</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>30000</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>In the case of network failure or broker shut down, the Micro Integrator will attempt to reconnect a number of times (as sepcified by the <b>parameter.retry_count</b> parameter) with an interval (specified by this parameter) between the retry attempts.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.retry_count</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>3</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>In the case of network failure or broker shut down, the Micro Integrator will attempt to reconnect as many times as sepcified by this parameter with an interval (specified by the <b>parameter.retry_interval</b> parameter) between the retry attempts.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.ssl_enable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether or not SSL is enabled for RabbitMQ connection. If you set this to 'true', be sure to update the keystore and trust store parameters given below.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.ssl_version</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>SSL</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The SSL versions.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.keystore_file_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the keystore file that is used for securing a RabbitMQ connection. By default, the keystore file of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.keystore_type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>JKS</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot;,&quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file. By default, the keystore type of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.keystore_password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used for securing a RabbitMQ connection. This keystore password is used when accessing the keys in the keystore. By default, the keystore password of the <a href="#primary-keystore">primary keystore</a> is enabled for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.truststore_file_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2truststore.jks</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the keystore file that is used for storing the trusted digital certificates. By default, the product's <a href='#trust-store'>trust store</a> is configured for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.truststore_type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>JKS</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot;,&quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file that is used as the trust store. By default, the product's <a href='#trust-store'>trust store</a> is configured for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.truststore_password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>wso2carbon</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used as the trust store. By default, the product's <a href='#trust-store'>trust store</a> is configured for this purpose.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## RabbitMQ sender

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="32" type="checkbox" id="_tab_32">
                <label class="tab-selector" for="_tab_32"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
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
parameter.connection_pool_size = 10
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[transport.rabbitmq.sender]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required if you are configuring WSO2 Micro Integrator to send messages to a RabbitMQ client. Read more about <a href='../../setup/brokers/configure-with-rabbitMQ'>connecting to RabbitMQ</a> from the Micro Integrator.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>rabbitMQSender</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the broker.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.hostname</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>localhost</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The IP address of the server node.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.port</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>5672</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The port on which the RabbitMQ broker can be accessed.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.username</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>guest</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The user name for connecting to RabbitMQ broker.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>guest</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The password for connecting to the RabbitMQ broker.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.exchange_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>amq.direct</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Name of the RabbitMQ exchange to which the queue is bound. Use this parameter instead of rabbitmq.queue.routing.key, if you need to use the default exchange and publish to a queue.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.routing_key</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>MyQueue</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The routing key of the queue.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.queue_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>MyQueue</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The queue name to send or consume messages. If you do not specify this parameter, you need to specify the rabbitmq.queue.routing.key parameter.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.reply_to_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the call back­ queue. Specify this parameter if you expect a response.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.queue_delivery_mode</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>1</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>persistent</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The delivery mode of the queue. Possible values are <b>Non­-persistent</b> and <b>Persistent</b>.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.exchange_type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the exchange.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.queue_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>MyQueue</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The queue name to send or consume messages. If you do not specify this parameter, you need to specify the rabbitmq.queue.routing.key parameter.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.queue_durable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Whether the queue should remain declared even if the broker restarts. The default value is false.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.queue_exclusive</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Whether the queue should be exclusive or should be consumable by other connections. The default value is false.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.queue_auto_delete</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether to keep the queue even if it is not being consumed anymore.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.exchange_durable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies whether the exchange should remain declared even if the broker restarts.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.queue_auto_declare</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Whether to keep the queue even if it is not being consumed anymore. The default value is false.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>parameter.exchange_auto_declare</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Whether to create queues if they are not present. However, you should set this parameter only if queues are not declared prior on the broker. Setting this parameter in the publish URL to false improves RabbitMQ transport performance.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## FIX transport

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="33" type="checkbox" id="_tab_33">
                <label class="tab-selector" for="_tab_33"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.fix]

listener.enable = false
listener.parameter.customParameter = ""
sender.enable = false
sender.parameter.customParameter = ""
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.fix]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that configure the Micro Integrator to communicate through the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-fix-transport'>FIX transport</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling the FIX transport listener.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
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
            </div>
        </div>
    </section>
</div>


## MQTT transport

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="34" type="checkbox" id="_tab_34">
                <label class="tab-selector" for="_tab_34"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.mqtt]

listener.enable = false
listener.hostname = "$ref{server.hostname}"
listener.connection_factory = "mqttConFactory"
listener.server_port = 1883
listener.client_id = "client-id-1234"
listener.topic_name = "esb.test"

# not reqired parameter list
listener.subscription_qos = 0
listener.session_clean = false
listener.enable_ssl = false
listener.subscription_username = ""
listener.subscription_password = ""
listener.temporary_store_directory = ""
listener.blocking_sender = false
listener.connect_type = "text/plain"
listener.message_retained = false

listener.parameter.customParameter = ""

sender.enable = false
sender.parameter.customParameter = ""
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.mqtt]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that configure the Micro Integrator to communicate through the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-mqtt-transport'>MQTT transport</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling the MQTT transport listener.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.hostname</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>$ref{server.hostname}</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the host. By default, the hostname of the Micro Integrator server is used.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.connection_factory</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The connection factory URL for connecting to a JMS topic.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.server_port</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&#39;1883&#39;, or &#39;1885&#39;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The port ID.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.client_id</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The client ID.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.topic_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the topic.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.parameter.customParameter</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Replace 'customParameter' with a required parameter name.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.enable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling the MQTT transport sender.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.parameter.customParameter</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Replace 'customParameter' with a required parameter name.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## SAP transport

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="35" type="checkbox" id="_tab_35">
                <label class="tab-selector" for="_tab_35"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.sap]

listener.enable = true
listener.idoc.class = "org.wso2.carbon.transports.sap.SAPTransportListener"
listener.idoc.parameter.customParameter = ""
listener.bapi.class = "org.wso2.carbon.transports.sap.SAPTransportListener"
listener.bapi.parameter.customParameter = ""
sender.enable = true
sender.idoc.class = "org.wso2.carbon.transports.sap.SAPTransportSender"
sender.idoc.parameter.customParameter = ""
sender.bapi.class = "org.wso2.carbon.transports.sap.SAPTransportSender"
sender.bapi.parameter.customParameter = ""
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.sap]</code>
                            
                            <p>
                                This configuration header groups the parameters that configure the Micro Integrator to <a href='../../use-cases/tutorials/sap-integration'>communicate with SAP</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling SAP transport listener.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.idoc.class</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.wso2.carbon.transports.sap.SAPTransportListener</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The class that implements the SAP transport listener for the Sap IDoc libary.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.bapi.class</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.wso2.carbon.transports.sap.SAPTransportListener</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The class that implements the SAP transport listener for the SAP BAPI library.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling the SAP transport sender.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.idoc.class</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.wso2.carbon.transports.sap.SAPTransportSender</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The class that implements the SAP transport sender for the Sap IDoc library.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.bapi.class</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.wso2.carbon.transports.sap.SAPTransportSender</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The class that implements the SAP transport listener for the Sap BAPI library.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## MSMQ transport

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="36" type="checkbox" id="_tab_36">
                <label class="tab-selector" for="_tab_36"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.msmq]

listener.enable = false
listener.hostname = "$ref{server.hostname}"
listener.parameter.customParameter = ""

sender.enable = false
sender.parameter.customParameter = ""
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.msmq]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that configure the Micro Integrator to communicate through the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-msmq-transport'>MSMQ transport</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling MSMQ transport listener.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.enable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling MSMQ transport sender.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.hostname</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>$ref{server.hostname}</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The hostname.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## TCP transport (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="37" type="checkbox" id="_tab_37">
                <label class="tab-selector" for="_tab_37"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.tcp]

listener.enable = false
listener.port = 8000
listener.hostname = "$ref{server.hostname}"
listener.content_type = ["application/xml"]
listener.response_client = true
listener.parameter.customParameter = ""

sender.enable = true
sender.parameter.customParameter = ""
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.tcp]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that configure the Micro Integrator to communicate through the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-tcp-transport'>TCP transport</a>. Note that the list of parameters given below can be used for the non-blocking transport as well as the <a href='#tcp-transport-blocking-mode'>blocking transport</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling the TCP transport listener.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.port</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>8000</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>A positive integer less than 65535.</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The port on which the TCP server should listen for incoming messages.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.hostname</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>$ref{server.hostname}</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The host name of the server to be displayed in WSDLs, etc.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.content_type</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&#39;application/xml&#39;, &#39;application/json&#39;, or &#39;text/html&#39;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The content type of the input message.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.response_client</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Whether or not the client needs to get the response.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
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
            </div>
        </div>
    </section>
</div>


## TCP transport (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="38" type="checkbox" id="_tab_38">
                <label class="tab-selector" for="_tab_38"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.blocking.tcp]

listener.enable = false
listener.port = 8000
listener.hostname = "$ref{server.hostname}"
listener.content_type = ["application/xml"]
listener.response_client = true
listener.parameter.customParameter = ""

sender.enable = false
sender.parameter.customParameter = ""
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.blocking.tcp]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that are used to configure the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-tcp-transport'>TCP transport</a> in blocking mode. You can use the <a href='#tcp-transport-non-blocking-mode'>same list of parameters</a> that are available for the non-blocking TCP transport.
                            </p>
                        </div>
                        <div class="params-wrap">
                            
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Websocket transport

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="39" type="checkbox" id="_tab_39">
                <label class="tab-selector" for="_tab_39"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.ws]

sender.enable = false
sender.outflow_dispatch_sequence = "outflowDispatchSeq"
sender.outflow_dispatch_fault_sequence = "outflowFaultSeq"      
sender.parameter.customParameter = ""
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.ws]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that configure the Micro Integrator to communicate through the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-websocket-transport'>Websocket transport</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.enable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling the websocket transport listener.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.outflow_dispatch_sequence</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>outflowDispatchSeq</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The sequence for the back-end to client mediation.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.outflow_dispatch_fault_sequence</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>outflowFaultSeq</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The fault sequence for the back-end to client mediation path.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.parameter.customParameter</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Replave 'customParameter' with required parameter name.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Secure Websocket transport

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="40" type="checkbox" id="_tab_40">
                <label class="tab-selector" for="_tab_40"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.wss]

sender.enable = false
sender.outflow_dispatch_sequence = "outflowDispatchSeq"
sender.outflow_dispatch_fault_sequence = "outflowFaultSeq"
sender.parameter.customParameter = ""

sender.truststore_location = "$ref{truststore.file_name}"
sender.truststore_password = "$ref{truststore.password}"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.wss]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that configure the Micro Integrator to communicate through the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-websocket-transport'>secured Websocket transport</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.enable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling the websocket secured transport sender.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.outflow_dispatch_sequence</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>outflowDispatchSeq</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The sequence for the back-end to client mediation.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.outflow_dispatch_fault_sequence</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>outflowFaultSeq</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The fault sequence for the back-end to client mediation path.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.truststore_location</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>$ref{truststore.file_name}</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The file path to the truststore that stores the trusted digital certificates for websocket use cases. By default, the product's <a href='#trust-store'>trust store</a> is configured for this purpose.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.truststore_password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>$ref{truststore.password}</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>..</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The....</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.parameter.customParameter</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Replave 'customParameter' with required parameter name.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## UDP transport (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="41" type="checkbox" id="_tab_41">
                <label class="tab-selector" for="_tab_41"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.udp]

listener.enable = false
listener.parameter.customParameter = ""

sender.enable =false               
sender.parameter.customParameter = ""
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.udp]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that configure the Micro Integrator to communicate through the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-udp-transport'>UDP transport</a>. Note that the list of parameters given below can be used for the non-blocking transport as well as the <a href='#udp-transport-blocking-mode'>blocking transport</a>.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>listener.enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The parameter for enabling the UDP transport listener.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;, or &quot;false&quot;</code></span>
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
            </div>
        </div>
    </section>
</div>


## UDP transport (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="42" type="checkbox" id="_tab_42">
                <label class="tab-selector" for="_tab_42"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[transport.blocking.udp]

listener.enable = false
listener.parameter.customParameter = ""

sender.enable = false        
sender.parameter.customParameter = ""
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.blocking.tcp]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters that are used to configure the <a href='../../setup/transport_configurations/configuring-transports/#configuring-the-udp-transport'>UDP transport</a> in blocking mode. You can use the <a href='#udp-transport-non-blocking-mode'>same list of parameters</a> that are available for the non-blocking UDP transport.
                            </p>
                        </div>
                        <div class="params-wrap">
                            
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>


## Message Mediation

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="43" type="checkbox" id="_tab_43">
                <label class="tab-selector" for="_tab_43"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
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
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[mediation]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header groups the parameters used for tuning the mediation process (Synapse engine) of the Micro Integrator. These parameters are mainly used when mediators such as <a href='../../references/mediators/iterate-Mediator'>Iterate</a> and <a href='../../references/mediators/clone-Mediator'>Clone</a> (which uses the internal thread pools) are used.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>synapse.core_threads</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>20</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The initial number of synapse threads in the pool. This parameter is applicable only if the <a href='../../references/mediators/iterate-Mediator'>Iterate</a> and <a href='../../references/mediators/clone-Mediator'>Clone</a> mediators are used to handle a higher load. These mediators use a thread pool to create new threads when processing messages and sending messages in parallal. You can configure the size of the thread pool by this parameter. The number of threads specified via this parameter should be increased as required to balance an increased load. Increasing the value specified for this parameter results in higher performance of the <b>Iterate</b> and <b>Clone</b> mediators.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>synapse.max_threads</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>100</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The maximum number of synapse threads in the pool. This parameter is applicable only if the <a href='../../references/mediators/iterate-Mediator'>Iterate</a> and <a href='../../references/mediators/clone-Mediator'>Clone</a> mediators are used to handle a higher load. The number of threads specified for this parameter should be increased as required to balance an increased load.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>synapse.threads_queue_length</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>10</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The length of the queue that is used to hold the runnable tasks that are to be executed by the pool. This parameter is applicable only if the <a href='../../references/mediators/iterate-Mediator'>Iterate</a> and <a href='../../references/mediators/clone-Mediator'>Clone</a> mediators are used to handle a higher load.</br> You can specify a finite value as the queue length by giving any positive number. If this parameter is set to (-1) it means that the task queue length is infinite. If the queue length is finite, there can be situations where requests are rejected when the task queue is full and all the cores are occupied. If the queue length is infinite, and if some thread locking happens, the server can go out of memory. Therefore, you need to decide on an optimal value based on the actual load.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>synapse.threads.keepalive</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code></code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The keep-alive time in milliseconds for idle threads. Once this time has elapsed for an idle thread, the thread will be destroyed. This parameter is applicable only if the <a href='../../references/mediators/iterate-Mediator'>Iterate</a> and <a href='../../references/mediators/clone-Mediator'>Clone</a> mediators are used to handle a high load.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>synapse.global_timeout_interval</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>120000</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The maximum number of milliseconds within which a response for the request should be received. A response that arrives after the specified number of seconds cannot be correlated with the request. Hence, a warning will be logged and the request will be dropped. This parameter is also referred to as the time-out handler.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>synapse.enable_xpath_dom_failover</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,&quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>If this parameter is set to true , the Micro Integrator can switch to xpath 2.0. This parameter is 'false' by default since xpath 2.0 evaluations can cause performance degradation. The Micro Integrator uses the <b>Saxon Home Edition</b> when implementing XPATH 2.0 functionalities, and thus supports all the functions that are shipped with it. For more information on the supported functions, see the Saxon Documentation.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>synapse.temp_data_chunk_size</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>3072</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>The message size that can be processed by the Micro Integrator.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>synapse.script_mediator_pool_size</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>15</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>When using externally referenced scripts, this parameter specifies the size of the script engine pool that should be used per script mediator. The script engines from this pool are used for externally referenced script execution where updates to external scripts on an engine currently in use may otherwise not be thread safe. It is recommended to keep this value at a reasonable size since there will be a pool per externally referenced script.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>synapse.preserve_namespace_on_xml_to_json</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,&quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Preserves the namespace declarations in the JSON output during XML to JSON message transformations.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>flow.statistics.enable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,&quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Set this property to true and enable statistics for the required integration artifact to record information such as the following: <ul><li>The time spent on each mediator.</li><li>The time spent on processing each message.</li><li>The fault count of a single message flow.</li></ul></p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>flow.statistics.capture_all</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,&quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Set this property to 'true' and set the <code>mediation.flow.statistics.enable</code> property also to 'true'. This will enable mediation statistics for all the integration artifacts by default. If you set this property to 'false', you need to set the <code>mediation.flow.statistics.enable</code> property to 'true' and manually enable statistics for the required integration artifact.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>statistics.enable_clean</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,&quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>If this parameter is set to true, all the existing statistics would be cleared before processing a request. This is recommended if you want to increase the processing speed.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>flow.statistics.tracer.collect_payload</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,&quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Set this property to true and enable tracing for the required integration artifact to record the message payload before and after the message mediation performed by individual mediators.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>flow.statistics.tracer.collect_properties</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> boolean </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>false</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot;,&quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Set this property to true and enable tracing for the required integration artifact to record the following information: <ul><li>Message context properties.</li><li>Message transport-scope properties.</li></ul></p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>

