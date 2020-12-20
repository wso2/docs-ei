# Product Configurations

The Micro Integrator of WSO2 Enterprise Integrator 7.0 introduces TOML-based product configurations. All the server-level configurations of your Micro Integrator instance can be applied using a single configuration file, which is the `deployment.toml` file (stored in the `MI_HOME/conf` directory).

The complete list of configuration parameters that you can use in the `deployment.toml` file are listed below along with descriptions. You can also see the documentation on product [installation and setup](../../setup/install_and_setup_overview) for details on applying product configurations to your Micro Integrator deployment.

## Instructions for use

To update the product configurations:

1. Open the `deployment.toml` file (stored in the `MI_HOME/conf` directory).
2. Select the required configuration headers and parameters from the list given below and apply them to the `deployment.toml` file.

The **default** `deployment.toml` file of the Micro Integrator is as follows:

```toml
[server]
hostname = "localhost"

[keystore.tls]  # IMPORTANT! Be sure to change this heading to [keystore.primary] when you use the product.
file_name = "wso2carbon.jks"
password = "wso2carbon"
alias = "wso2carbon"
key_password = "wso2carbon"

[truststore]
file_name = "client-truststore.jks"
password = "wso2carbon"
alias = "symmetric.key.value"
algorithm = "AES"
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
node_ip="127.0.0.1"
enable_mtom=false
enable_swa=false
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[server]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the deployment parameters that are used for identifying a Micro Integrator server node. You need to update these values when you deploy WSO2 Micro Integrator. The required and optional parameters for this configuration are listed below.
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
                                            <span class="param-possible-values">Possible Values: <code>&quot;127.0.0.1&quot;,&quot;localhost&quot;,&quot;&lt;any-ip-address&gt;&quot;</code></span>
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
                                        <p>Port offset allows you to run multiple WSO2 products, multiple instances of a WSO2 product, or multiple WSO2 product clusters on the same server or virtual machine (VM). Port offset defines the number by which all ports defined in the runtime such as the HTTP/S ports will be offset. For example, if the default HTTP port is 9443 and the port offset is 1, the effective HTTP port will be 9444. Therefore, for each additional WSO2 product instance, set the port offset to a unique value so that they can all run on the same server without any port conflicts.</p>
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
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>&quot;true&quot; or &quot;false&quot;</code></span>
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
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>&quot;true&quot; or &quot;false&quot;</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Use this paramater to enable SwA (SOAP with Attachments) for the product server. When SwA is enabled, the Micro Integrator will process the files attached to SOAP messages.</p>
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
                                            <span class="badge-required">Required</span>
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
                                            <span class="badge-required">Required</span>
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
                                            <span class="badge-required">Required</span>
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
                                            <span class="badge-required">Required</span>
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
<pre><code class="toml">[keystore.primary]
file_name = "wso2carbon.jks"
type = "JKS"
password = "wso2carbon"
alias = "wso2carbon"
key_password = "wso2carbon"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[keystore.primary]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the parameters that connect the Micro Integrator to the primary keystore. This keystore is used for SSL handshaking (when the server communicates with another server) and for encrypting plain text information in configuration files. By default, this keystore is also used for encrypted data in internal datastores, unless you have configured a separate keystore for internal data encryption.
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot;, &quot;PKCS12&quot;</code></span>
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
                                        <p>The password of the private key that is included in the keystore. The private key is used to decrypt the data that has been encrypted using the keystore&#39;s public key.</p>
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
<pre><code class="toml">[keystore.primary]
file_name = "wso2carbon.jks"
type = "JKS"
password = "wso2carbon"
alias = "wso2carbon"
key_password = "wso2carbon"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[keystore.internal]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the parameters that connect the Micro Integrator to the keystore used for encrypting/decrypting data in internal data stores. You may sometimes choose to configure a separate keystore for this purpose because the primary keystore needs to renew certificates frequently. However, for encrypting information in internal data stores, the keystore certificates should not be changed frequently because the data that is already encrypted will become unusable every time the certificate changes. Read more about configuring the internal keystore.
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the keystore file that is used for data encryption/decryption in internal data stores. By default, the keystore file of the primary keystore is enabled for this purpose.</p>
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
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot;, &quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file. By default, the keystore type of the primary keystore is enabled for this purpose.</p>
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
                                        <p>The password of the keystore file that is used for data encryption/decryption in internal data stores. This keystore password is used when accessing the keys in the keystore. By default, the keystore password of the primary keystore is enabled for this purpose.</p>
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
                                        <p>The alias of the public key corresponding to the private key that is included in the keystore. The public key is used for encrypting data in the Micro Integrator server, which only the corresponding private key can decrypt. The public key is embedded in a digital certificate, and this certificate can be shared over the internet by storing it in a separate trust store file. By default, the alias of the primary keystore is enabled for this purpose.</p>
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
                                        <p>The password of the private key that is included in the keystore. The private key is used to decrypt the data that has been encrypted using the keystore&#39;s public key. By default, the public key password of the primary keystore is enabled for this purpose.</p>
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


## System Parameters

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="5" type="checkbox" id="_tab_5">
                <label class="tab-selector" for="_tab_5"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[system.parameter]
org.wso2.SecureVaultPasswordRegEx = "any_valid_regex"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[system.parameter]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring system parameters for the server.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>org.wso2.SecureVaultPasswordRegEx</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>^[\S]{5,30}$</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>regex value</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>A regex pattern that specifies the password length and character composition for passwords in a synapse configuration. See encrypting synapse passwords for instructions.</p>
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


## Truststore

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="6" type="checkbox" id="_tab_6">
                <label class="tab-selector" for="_tab_6"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[truststore]
file_name="wso2truststore.jks"
type="JKS"
password="wso2carbon"
alias="symmetric.key.value"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[truststore]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the parameters that connect the Micro Integrator to the keystore file (trust store) that is used to store the digital certificates that the server trusts for SSL communication. Read more about configuring the truststore.
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot;, &quot;PKCS12&quot;</code></span>
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


## LDAP user store

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="7" type="checkbox" id="_tab_7">
                <label class="tab-selector" for="_tab_7"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[user_store]
type = "read_only_ldap"
class = "org.wso2.micro.integrator.security.user.core.ldap.ReadOnlyLDAPUserStoreManager"
connection_url = "ldap://localhost:10389"
connection_name = "uid=admin,ou=system"
connection_password = "admin"
anonymous_bind = false
user_search_base = "ou=Users,dc=wso2,dc=org"
user_name_attribute = "uid"
user_name_search_filter = "(&(objectClass=person)(uid=?))"
user_name_list_filter = "(objectClass=person)"
read_groups = true
group_search_base = "ou=Groups,dc=wso2,dc=org"
group_name_attribute = "cn"
group_name_search_filter = "(&(objectClass=groupOfNames)(cn=?))"
group_name_list_filter = "(objectClass=groupOfNames)"
membership_attribute = "member"
back_links_enabled = false
username_java_regex = "[a-zA-Z0-9._\\-|//]{3,30}$"
rolename_java_regex = "[a-zA-Z0-9._\\-|//]{3,30}$"
password_java_regex = "^[\\S]{5,30}$"
scim_enabled = false
password_hash_method = "PLAIN_TEXT"
multi_attribute_separator = ","
max_user_name_list_length = 100
max_role_name_list_length = 100
user_roles_cache_enabled = true
connection_pooling_enabled = true
ldap_connection_timeout = 5000
read_timeout = ''
retry_attempts = ''
connection_retry_delay = "120000"
</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[user_store]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for conencting the Micro Integrator to an LDAP user store.
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
                                            <span class="param-default-value">Default: <code>&quot;read_only_ldap&quot;, &quot;read_write_ldap&quot;</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This parameter specifies the type of user store. When you set this parameter, all of the remaining parameters (listed below) are inferred with default values. You can override the defaults by giving specific values to these parameters.</p>
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
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>org.wso2.micro.integrator.security.user.core.ldap.ReadOnlyLDAPUserStoreManager</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The implementation class that enables the read-only LDAP user store. If the type parameter is not used, you need to specify a value for this parameter.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>connection_url</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>ldap://localhost:10389</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The URL for connecting to the LDAP. Override the default URL for your setup. If you are connecting over ldaps (secured LDAP), you need to import the certificate of the user store to the truststore (wso2truststore.jks by default). See the instructions on how to add certificates to the truststore.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>connection_name</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>uid=admin,ou=system</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The username used to connect to the user store and perform various operations. This user does not need to be an administrator in the user store. However, the user requires permission to read the user list and user attributes, and to perform search operations on the user store. The value you specify is used as the DN (Distinguish Name) attribute of the user who has sufficient permissions to perform operations on users and roles in LDAP.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>connection_password</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            <span class="badge-required">Required</span>
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>admin</code></span>
                                        </div>
                                        
                                    </div>
                                    <div class="param-description">
                                        <p>Password for the connection user name.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>user_search_base</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>ou=system</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The DN of the context or object under which the user entries are stored in the user store. When the user store searches for users, it will start from this location of the directory.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>user_name_attribute</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>uid</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The attribute used for uniquely identifying a user entry. Users can be authenticated using their email address, UID, etc. The name of the attribute is considered as the username. Note that the email address is considered as a special case in WSO2 products. Read more about using the email address as user name.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>user_name_search_filter</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>(&amp;amp;(objectClass=person)(uid=?))</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Filtering criteria used to search for a particular user entry.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>user_name_list_filter</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>(objectClass=person)</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Filtering criteria for searching user entries in the user store. This query or filter is used when doing search operations on users with different search attributes. According to the default configuration, the search operation only provides the objects created from the person object class.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>read_groups</code> </span>
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
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot; or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This indicates whether groups should be read from the user store. If this is set to &#39;false&#39;, none of the groups in the user store can be read, and the following group configurations are NOT mandatory: &#39;group_search_base&#39;, &#39;group_name_list_filter&#39;, or &#39;group_name_attribute&#39;.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>group_search_base</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>ou=system</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The DN of the context or object under which the group entries are stored in the user store. When the user store searches for groups, it will start from this location of the directory.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>group_name_attribute</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>cn</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The attribute used for uniquely identifying a group entry. This attribute is to be treated as the group name.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>group_name_search_filter</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>(&amp;amp;(objectClass=groupOfNames)(cn=?))</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The filtering criteria used to search for a particular group entry.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>group_name_list_filter</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>(objectClass=groupOfNames)</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The filtering criteria for searching group entries in the user store. This query or filter is used when doing search operations on groups with different search attributes.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>membership_attribute</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>member</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Defines the attribute that contains the distinguished names (DN) of user objects that are in a group.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>back_links_enabled</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>member</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Defines whether the backlink support is enabled.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>username_java_regex</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>[a-zA-Z0-9._\-|//]{3,30}$</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The regular expression used by the back-end components for username validation. By default, a length of 3 to 30 allowed for strings with non-empty characters. You can provide ranges of alphabets, numbers, and also ranges of ASCII values in the RegEx properties.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>rolename_java_regex</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>[a-zA-Z0-9._\-|//]{3,30}$</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The regular expression used by the back-end components for role name validation. By default, a length of 3 to 30 allowed for strings with non-empty characters. You can provide ranges of alphabets, numbers, and also ranges of ASCII values in the RegEx properties.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>password_java_regex</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>^[\S]{5,30}$</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The regular expression used by the back-end components for password validation. By default, a length of 3 to 30 allowed for strings with non-empty characters. You can provide ranges of alphabets, numbers, and also ranges of ASCII values in the RegEx properties.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>scim_enabled</code> </span>
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
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot; or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The regular expression used by the back-end components for password validation. By default, a length of 3 to 30 allowed for strings with non-empty characters. You can provide ranges of alphabets, numbers, and also ranges of ASCII values in the RegEx properties.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>password_hash_method</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>PLAIN_TEXT</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;SHA&quot;, &quot;MD5&quot;, &quot;PLAIN_TEXT&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies the password hashing algorithm used for hashing the password before storing in the user store. You can use the SHA digest method (SHA-1, SHA-256), the MD 5 digest method, or plain text passwords.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>multi_attribute_separator</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>,</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This parameter is used to define a character to separate multiple attributes. This ensures that it will not appear as part of a claim value. Normally &#39;,&#39; is used to separate multiple attributes, but you can define &#39;,,,&#39;, &#39;...&#39;, or a similar character sequence.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>max_user_name_list_length</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>100</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Controls the number of users listed in the user store. This is useful when you have a large number of users and you don&#39;t want to list them all. Setting this property to 0 displays all users. In some user stores, there are policies to limit the number of records that can be returned from the query. Setting the value to 0 will list the maximum results returned by the user store. To increase that value, you need to set it at the user store level. Active directory has the &#39;MaxPageSize&#39; property with the default value set to 1000.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>max_role_name_list_length</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>100</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Controls the number of roles listed in the user store. This is useful when you have a large number of roles and you don&#39;t want to list them all. Setting this property to 0 displays all roles. In some user stores, there are policies to limit the number of records that can be returned from the query. Setting the value to 0 will list the maximum results returned by the user store. To increase that value, you need to set it at the user store level. Active directory has the &#39;MaxPageSize&#39; property with the default value set to 1000.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>user_roles_cache_enabled</code> </span>
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
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot; or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This parameter indicates whether the list of roles for a user should be cached. Set this to &#39;false&#39; if the user roles are changed by external means and the changes should be instantly reflected in the product instance.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>connection_pooling_enabled</code> </span>
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
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot; or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Define whether LDAP connection pooling is enabled. The connection performance will improve when this parameter is enabled.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>ldap_connection_timeout</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>5000</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This is the connection timeout period (in milliseconds) when the initial connection is created.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>read_timeout</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The value for this parameter is the read timeout in milliseconds for LDAP operations. If the LDAP provider cannot get an LDAP response within that period, it aborts the read attempt. The integer should be greater than zero. An integer less than or equal to zero means no read timeout is specified, which is equivalent to waiting for the response infinitely until it is received.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>retry_attempts</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Retry the authentication request if a timeout happened.</p>
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

