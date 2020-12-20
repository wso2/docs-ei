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


## Database connection

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="8" type="checkbox" id="_tab_8">
                <label class="tab-selector" for="_tab_8"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[datasource]]
id = "WSO2_CARBON_DB"
url= "jdbc:h2:./repository/database/WSO2CARBON_DB;DB_CLOSE_ON_EXIT=FALSE;LOCK_TIMEOUT=60000"
username="username"
password="password"
driver="org.h2.Driver"
pool_options.maxActive=50
pool_options.maxWait = 60000
pool_options.testOnBorrow = true</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[datasource]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for connecting to a database from the Micro Integrator. Databases are only required if you are connecting the Micro Integrator to an RDBMS user store.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>id</code> </span>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The name of the database.</p>
                                    </div>
                                </div>
                            </div><div class="param">
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
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The connection URL for your database. Note that the URL depends on the type of database you use.</p>
                                    </div>
                                </div>
                            </div><div class="param">
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
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The user name for connecting to the database.</p>
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
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The password for connecting to the database.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>driver</code> </span>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The driver class of your database.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.maxActive</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>50</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The maximum number of active connections that can be allocated from this pool at the same time. If you set this value too low, the response times for some requests might slow down as they have to wait for connections to get free. A value too high might cause too much memory/resource utilization and the system may slow down or be unresponsive.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.maxWait</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>60000</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Maximum number of milliseconds that the pool waits (when there are no available connections) for a connection to be returned before throwing an exception.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.testOnBorrow</code> </span>
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
                                        <p>Used to indicate if objects will be validated before being borrowed from the pool. If the object fails to validate, it will be dropped from the pool, and we will attempt to borrow another one.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.maxIdle</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>8</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The maximum number of connections that can remain idle in the pool, without extra ones being released. Default value is 8. Put a negative value for unlimited. Idle connections are checked periodically (if enabled) and connections that have been idle for longer than minEvictableIdleTimeMillis will be released.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.minIdle</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>0</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The minimum number of connections that can remain idle in the pool, without extra ones being created. The connection pool can shrink below this number if validation queries fail. Default value is 0.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.validationInterval</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>30000</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This parameter controls how frequently a given validation query is executed (time in milliseconds). The default value is 30000 (30 seconds). That is, if a connection is due for validation, but has been validated previously within this interval, it will not be validated again.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.validationQuery</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>Null</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The SQL query used to validate connections from this pool before returning them to the caller. If specified, this query does not have to return any data, it just can&#39;t throw an SQLException. The default value is null. Example values are SELECT 1(mysql), select 1 from dual(oracle), SELECT 1(MS Sql Server).</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.MaxPermSize</code> </span>
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
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The memory size allocated for WSO2 Micro Integrator.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.removeAbandoned</code> </span>
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
                                        <p>If this property is set to &#39;true&#39;, a connection is considered abandoned and eligible for removal if it has been in use for longer than the removeAbandonedTimeout value explained below.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.removeAbandonedTimeout</code> </span>
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
                                        <p>The time in seconds that should pass before a connection that is in use can be removed. This is the time period after which the connection will be declared abandoned. This value should be set to the longest running query that the applications might have.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.logAbandoned</code> </span>
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
                                        <p>Set this property to &#39;true&#39; if you wish to log when the connection was abandoned. If this option is set to &#39;true&#39;, a stack trace is recorded during the dataSource.getConnection call and is printed when a connection is not returned.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.initialSize</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>0</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The initial number of connections created when the pool is started. Default value is 0.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.defaultTransactionIsolation</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>TRANSACTION_NONE</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;TRANSACTION_NONE&quot;, &quot;TRANSACTION_UNKNOWN&quot;, &quot;TRANSACTION_READ_COMMITTED&quot;, &quot;TRANSACTION_READ_UNCOMMITTED&quot;, &quot;TRANSACTION_REPEATABLE_READ&quot;, &quot;TRANSACTION_SERIALIZABLE&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The default TransactionIsolation state of connections created by this pool.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.validationQueryTimeout</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-1</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The timeout in seconds before a connection validation queries fail. This works by calling java.sql.Statement.setQueryTimeout(seconds) on the statement that executes the validationQuery . The pool itself doesn&#39;t timeout the query. It is still up to the JDBC driver to enforce query timeouts. A value less than or equal to zero will disable this feature. The default value is -1.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.timeBetweenEvictionRunsMillis</code> </span>
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
                                        <p>The number of milliseconds to sleep between runs of the idle connection validation/cleaner thread. This value should not be set under 1 second. It dictates how often we check for idle, abandoned connections, and how often we validate idle connections. The default value is 5000 (5 seconds).</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.numTestsPerEvictionRun</code> </span>
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
                                        <p>The number of objects to examine during each run of the idle object evictor thread.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.minEvictableIdleTimeMillis</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>60000</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The minimum amount of time an object may sit idle in the pool before it is eligible for eviction. The default value is 60000 (60 seconds).</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.defaultCatalog</code> </span>
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
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The default catalog of connections created by this pool.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.validatorClassName</code> </span>
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
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The name of a class that implements the org.apache.tomcat.jdbc.pool.Validator interface and provides a no-arg constructor (may be implicit). If specified, the class will be used to create a Validator instance, which is then used instead of any validation query to validate connections. The default value is null. An example value is com.mycompany.project.SimpleValidator.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.connectionProperties</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>Null</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The connection properties that will be sent to our JDBC driver when establishing new connections. Format of the string must be [propertyName=property;]* NOTE - The &#39;user&#39; and &#39;password&#39; properties will be passed explicitly, so they do not need to be included here. The default value is null.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.initSQL</code> </span>
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
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The ability to run a SQL statement exactly once, when the connection is created.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.jdbcInterceptors</code> </span>
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
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Flexible and pluggable interceptors to create any customizations around the pool, the query execution and the result set handling.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.abandonWhenPercentageFull</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>0</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Connections that have been abandoned (timed out) wont get closed and reported up unless the number of connections in use are above the percentage defined by abandonWhenPercentageFull. The value should be between 0-100. The default value is 0, which implies that connections are eligible for closure as soon as removeAbandonedTimeout has been reached.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.maxAge</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>0</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Time in milliseconds to keep this connection. When a connection is returned to the pool, the pool will check to see if the now - time-when-connected &gt; maxAge has been reached, and if so, it closes the connection rather than returning it to the pool. The default value is 0, which implies that connections will be left open and no age check will be done upon returning the connection to the pool.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>pool_options.suspectTimeout</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>0</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Timeout value in seconds. Default value is 0. Similar to to the removeAbandonedTimeout value but instead of treating the connection as abandoned, and potentially closing the connection, this simply logs the warning if logAbandoned is set to true. If this value is equal or less than 0, no suspect checking will be performed. Suspect checking only takes place if the timeout value is larger than 0 and the connection was not abandoned or if abandon check is disabled. If a connection is suspect a WARN message gets logged and a JMX notification gets sent once.</p>
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


## Management API Token Handler

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="9" type="checkbox" id="_tab_9">
                <label class="tab-selector" for="_tab_9"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[management_api_token_handler]
enable = true</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[management_api_token_handler]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for disabling JWT-based user authentication for the Micro Integrator's Management API. Read more about securing the Management API.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>enable</code> </span>
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
                                        <p>Set this paramter to &#39;false&#39; if you want to disable user authentication for the management API.</p>
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


## Management API Token Store

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="10" type="checkbox" id="_tab_10">
                <label class="tab-selector" for="_tab_10"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[management_api.handler.token_store_config]
max_size = "200"
clean_up_interval = "600"
remove_oldest_token_on_overflow = true</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[management_api.handler.token_store_config]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for changing the default JWT token store configurations of the Micro Integrator's Management API. Read more about securing the Management API.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>max_size</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>200</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Number of tokens stored in the in-memory token store. User can increase or decrease this value accordingly.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>clean_up_interval</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>600</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Token cleanup will be handled through a seperate thread and the frequency of the token clean up can be configured from this setting. This will clean all the expired and revoked security tokens. The thread will run only when there are tokens in the store. If it is empty, the cleanup thread will automatically stop. Interval is specified in seconds.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>remove_oldest_token_on_overflow</code> </span>
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
                                        <p>If set to &#39;true&#39;, this will remove the oldest accessed token when the token store is full. If it is set to &#39;false&#39;, the user should either wait until other tokens expire or increase the token store max size accordingly.</p>
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


## Management API Token

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="11" type="checkbox" id="_tab_11">
                <label class="tab-selector" for="_tab_11"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[management_api.handler.token_config]
expiry = "3600"
size = "2048"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[management_api.handler.token_config]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for changing configurations of the JWT token that is used by the Micro Integrator's Management API. Read more about securing the Management API.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>expiry</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>3600</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This configures the expiry time of the token (specified in seconds).</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>size</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>2048</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specifies the key size of the token.</p>
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


## Management API - Default User Store

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="12" type="checkbox" id="_tab_12">
                <label class="tab-selector" for="_tab_12"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[management_api.handler.file_user_store]
enable = true</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[management_api.handler.file_user_store]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for disabling the default file-based user store of the Micro Integrator's Management API. Read more about securing the Management API.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>enable</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> integer </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot; or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Set this paramter to &#39;false&#39; if you want to disable the default file-based user store. This allows you to use an external user store for user authentication in the Management API.</p>
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


## Management API - Users

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="13" type="checkbox" id="_tab_13">
                <label class="tab-selector" for="_tab_13"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[management_api.handler.user_store.users]]
user.name = "admin"
user.password = "admin"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[management_api.handler.user_store.users]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for defining the user name and password for the Management API. Reuse this header when you want to add more users. The user credentials are stored in the default file-based user store of the Management API. Read more about securing the Management API.
                            </p>
                        </div>
                        <div class="params-wrap">
                            <div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>user.name</code> </span>
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
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Enter a user name. Note that this will overwrite the default &#39;admin&#39; user that is stored in the user store.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>user.password</code> </span>
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
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Enter a password for the user specified by &#39;user.name&#39;. Note that this will overwrite the default &#39;admin&#39; password that is stored in the user store.</p>
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


## Management API - CORS

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="14" type="checkbox" id="_tab_14">
                <label class="tab-selector" for="_tab_14"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[management_api.cors]
enabled = true
allowed_origins = "*"
allowed_headers = "Authorization"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[management_api.cors]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring CORs for the Management API of the Micro Integrator. Read more about securing the Management API.
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
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>true</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Set this paramter to &#39;false&#39; if you want to disable CORs for the Management API.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>allowed_origins</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>*</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot; or &quot;false&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specify the allowed origins. By default &#39;*&#39; indicates that all origins are allowed.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>allowed_headers</code> </span>
                                </div>
                                <div class="param-info">
                                    <div>
                                        <p>
                                            <span class="param-type string"> string </span>
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>Authorization</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>Specify the allowed authorization headers.</p>
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


## Message Builders (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="15" type="checkbox" id="_tab_15">
                <label class="tab-selector" for="_tab_15"><i class="icon fa fa-code"></i></label>
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
application_binary = "org.apache.axis2.format.BinaryBuilder"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[message_builders]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the message builder implementation that is used to build messages that are received by the Micro Integrator in the default non-blocking mode. If you are using the Micro Integrator in blocking mode, see the message builder configurations for blocking mode.
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the &#39;application_xml&#39; content type. If required, you can change the default builder class.</p>
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
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>org.apache.synapse.commons.builders.XFormURLEncodedBuilder</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the &#39;form_urlencoded&#39; content type. If required, you can change the default builder class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the &#39;multipart_form_data&#39; content type. If required, you can change the default builder class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the &#39;text_plain&#39; content type. If required, you can change the default builder class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the &#39;application_json&#39; content type. If required, you can change the default builder class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the &#39;json_badgerfish&#39; content type. If required, you can change the default builder class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the &#39;text_javascript&#39; content type. If required, you can change the default builder class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the &#39;octet_stream&#39; content type. If required, you can change the default builder class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message builder implementation that builds messages with the &#39;application_binary&#39; content type. If required, you can change the default builder class.</p>
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


## Message Builders (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="16" type="checkbox" id="_tab_16">
                <label class="tab-selector" for="_tab_16"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[blocking.message_builders]
application_xml = "org.apache.axis2.builder.ApplicationXMLBuilder"
form_urlencoded = "org.apache.synapse.commons.builders.XFormURLEncodedBuilder"
multipart_form_data = "org.apache.axis2.builder.MultipartFormDataBuilder"
text_plain = "org.apache.axis2.format.PlainTextBuilder"
application_json = "org.wso2.micro.integrator.core.json.JsonStreamBuilder"
json_badgerfish = "org.apache.axis2.json.JSONBadgerfishOMBuilder"
text_javascript = "org.apache.axis2.json.JSONBuilder"
octet_stream =  "org.wso2.carbon.relay.BinaryRelayBuilder"
application_binary = "org.apache.axis2.format.BinaryBuilder"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[blocking.message_builders]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the message builder implementation that is used to build messages that are received by the Micro Integrator in blocking mode. You can use the same list of parameters that are available for message builders in non-blocking mode.
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


## Message Formatters (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="17" type="checkbox" id="_tab_17">
                <label class="tab-selector" for="_tab_17"><i class="icon fa fa-code"></i></label>
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
application_binary =  "org.apache.axis2.format.BinaryFormatter"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[message_formatters]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the message formatting implementation that is used for formatting messages that are sent out of the Micro Integrator in non-blocking mode. If you are using the Micro Integrator in blocking mode, see the message formatter configurations for blocking mode.
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the &#39;application_xml&#39; content type before they are sent out of the Micro Integrator. If required, you can change the default formating class.</p>
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
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>org.apache.synapse.commons.formatters.XFormURLEncodedFormatter</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the &#39;form_urlencoded&#39; content type before they are sent out of the Micro Integrator. If required, you can change the default formating class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the &#39;multipart_form_data&#39; content type before they are sent out of the Micro Integrator. If required, you can change the default formating class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the &#39;text_plain&#39; content type before they are sent out of the Micro Integrator. If required, you can change the default formating class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the &#39;application_json&#39; content type before they are sent out of the Micro Integrator. If required, you can change the default formating class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the &#39;json_badgerfish&#39; content type before they are sent out of the Micro Integrator. If required, you can change the default formating class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the &#39;text_javascript&#39; content type before they are sent out of the Micro Integrator. If required, you can change the default formating class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message formatting implementation that formats messages with the &#39;octet_stream&#39; content type before they are sent out of the Micro Integrator. If required, you can change the default formating class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the &#39;application_binary&#39; content type before they are sent out of the Micro Integrator. If required, you can change the default formating class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the &#39;text_xml&#39; content type before they are sent out of the Micro Integrator. If required, you can change the default formating class.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The message formating implementation that formats messages with the &#39;soap_xml&#39; content type before they are sent out of the Micro Integrator. If required, you can change the default formating class.</p>
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


## Message Formatters (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="18" type="checkbox" id="_tab_18">
                <label class="tab-selector" for="_tab_18"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[blocking.message_formatters]
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
application_binary =  "org.apache.axis2.format.BinaryFormatter"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[blocking.message_formatters]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the message formatter implementations that are used to format messages that are sent out from the Micro Integrator in blocking mode. You can use the same list of parameters that are available for message formatters in non-blocking mode.
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


## Custom Message Builders (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="19" type="checkbox" id="_tab_19">
                <label class="tab-selector" for="_tab_19"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[custom_message_builders]]
content_type = "application/json/badgerfish"
class = "org.apache.axis2.json.JSONBadgerfishOMBuilder"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[custom_message_builders]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the custom message builder implementation class and the selected content types to which the builder should apply in non-blocking mode. See the instructions on configuring custom message builders and formatters.
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The content types to which the custom message builder implementation should apply. You can specify the list of content types as follows: application/json/badgerfish.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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


## Custom Message Builders (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="20" type="checkbox" id="_tab_20">
                <label class="tab-selector" for="_tab_20"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[blocking.custom_message_builders]]
content_type = "application/json/badgerfish"
class = "org.apache.axis2.json.JSONBadgerfishOMBuilder"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[blocking.custom_message_builders]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the custom message builder implementation class and the selected content types to which the builder should apply in blocking mode. See the instructions on configuring custom message builders and formatters. You can use the same list of parameters that are available for custom message builders in non-blocking mode.
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


## Custom Message Formatters (non-blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="21" type="checkbox" id="_tab_21">
                <label class="tab-selector" for="_tab_21"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[custom_message_formatters]]
content_type = "application/json/badgerfish"
class = "org.apache.axis2.json.JSONBadgerfishMessageFormatter"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[custom_message_formatters]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the custom message formatter implementation class and the selected content types to which the formatter should apply in non-blocking mode. See the instructions on configuring custom message builders and formatters.
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The content types to which the custom message formatter implementation should apply. You can specify the list of content types as follows: application/json/badgerfish.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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


## Custom Message Formatters (blocking mode)

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="22" type="checkbox" id="_tab_22">
                <label class="tab-selector" for="_tab_22"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[blocking.custom_message_formatters]]
content_type = "application/json/badgerfish"
class = "org.apache.axis2.json.JSONBadgerfishMessageFormatter"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[[blocking.custom_message_formatters]]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the custom message formatter implementation class and the selected content types to which the formatter should apply in blocking mode. See the instructions on configuring custom message builders and formatters. You can use the same list of parameters that are available for custom message formatters in non-blocking mode.
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


## Server Request Processor

<div class="mb-config-catalog">
    <section>
        <div class="mb-config-options">
            <div class="superfences-tabs">
            
            <input name="23" type="checkbox" id="_tab_23">
                <label class="tab-selector" for="_tab_23"><i class="icon fa fa-code"></i></label>
                <div class="superfences-content">
                    <div class="mb-config-example">
<pre><code class="toml">[[server.get_request_processor]]
item = "swagger.yaml"
class = "org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerYamlProcessor"

[[server.get_request_processor]]
item = "swagger.json"
class = "org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerJsonProcessor"</code></pre>
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
                                            <span class="param-default-value">Default: <code>&quot;swagger.yaml&quot; and &quot;swagger.json&quot;</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The item repesents the first parameter in the query string (e.g. ?wsdl), which needs special processing.</p>
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
                                            <span class="param-default-value">Default: <code>&quot;org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerYamlProcessor&quot; and &quot;org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerYamlProcessor&quot;</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This is the class that implements the org.wso2.carbon.transport.HttpGetRequestProcessor processor. By default, the following two classes are used for handling the two default request items: org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerYamlProcessor (for swagger.yaml) and org.wso2.micro.integrator.transport.handlers.requestprocessors.swagger.format.SwaggerYamlProcessor (for swagger.json).</p>
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
            
            <input name="24" type="checkbox" id="_tab_24">
                <label class="tab-selector" for="_tab_24"><i class="icon fa fa-code"></i></label>
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
sender.ssl_profile.read_interval = "30s"</code></pre>
                    </div>
                </div>
                <div class="doc-wrapper">
                    <div class="mb-config">
                        <div class="config-wrap">
                            <code>[transport.http]</code>
                            <span class="badge-required">Required</span>
                            <p>
                                This configuration header is required for configuring the parameters that are used for tuning the default HTTP/S passthrough transport of the Micro Integrator in non-blocking mode.
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
                                            <span class="param-default-value">Default: <code>180000</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This is the maximum number of threads in the worker thread pool. Specifying a maximum limit avoids performance degradation that can occur due to context switching. If the specified value is reached, you will see the error &#39;SYSTEM ALERT - HttpServerWorker threads were in BLOCKED state during last minute&#39;. This can occur due to an extraordinarily high number of requests sent at a time when all the threads in the pool are busy, and the maximum number of threads is already reached.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                            <span class="param-default-value">Default: <code>&quot;true&quot; or &quot;false&quot;</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>This parameter allows you to specify the header field/s of messages passing through the EI that need to be preserved and printed in the outgoing message such as Location, CommonsHTTPTransportSenderKeep-Alive, Date, Server, User-Agent, and Host. For example, http.headers.preserve = Location, Date, Server.</p>
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
                                            <span class="param-possible-values">Possible Values: <code>&quot;true&quot; or &quot;false&quot;</code></span>
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
                                            <span class="param-default-value">Default: <code>8290</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                            <span class="param-default-value">Default: <code>8253</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                            <span class="param-default-value">Default: <code>MI_HOME/repository/resources/security/wso2carbon.jks</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The path to the keystore file that is used for securing the HTTP passthrough connection. By default, the keystore file of the primary keystore is enabled for this purpose.</p>
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
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot; or &quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file. By default, the keystore type of the primary keystore is enabled for this purpose.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used for securing the HTTP passthrough connection. This keystore password is used when accessing the keys in the keystore. By default, the keystore password of the primary keystore is enabled for this purpose.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the private key that is used for securing the HTTP passthrough connection. This keystore password is used when accessing the keys in the keystore. By default, the keystore password of the primary keystore is enabled for this purpose.</p>
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
                                            <span class="param-default-value">Default: <code>MI_HOME/repository/resources/security/wso2truststore.jks</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The path to the keystore file that is used for storing the trusted digital certificates. By default, the product&#39;s trust store is configured for this purpose.</p>
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
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot; or &quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file that is used as the trust store. By default, the product&#39;s trust store is configured for this purpose.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used as the trust store. By default, the product&#39;s trust store is configured for this purpose.</p>
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
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                            
                                        </p>
                                        <div class="param-default">
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The port through which the target proxy (specified by the &#39;sender.proxy_port&#39; parameter) accepts HTTP traffic.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.secured_proxy_host</code> </span>
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
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>If the outgoing messages should be sent through an HTTPS proxy server, use this parameter to specify the target proxy.</p>
                                    </div>
                                </div>
                            </div><div class="param">
                                <div class="param-name">
                                  <span class="param-name-wrap"> <code>sender.secured_proxy_port</code> </span>
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
                                        <p>The port through which the target proxy (specified by the &#39;sender.secured_proxy_port&#39; parameter) accepts HTTPS traffic.</p>
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
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                            <span class="param-default-value">Default: <code>-</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
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
                                            <span class="param-default-value">Default: <code>MI_HOME/repository/resources/security/wso2carbon.jks</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The path to the keystore file that is used for securing the HTTP passthrough connection. By default, the keystore file of the primary keystore is enabled for this purpose.</p>
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
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot; or &quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file. By default, the keystore type of the primary keystore is enabled for this purpose.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used for securing the HTTP passthrough connection. This keystore password is used when accessing the keys in the keystore. By default, the keystore password of the primary keystore is enabled for this purpose.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the private key that is used for securing the HTTP passthrough connection. This keystore password is used when accessing the keys in the keystore. By default, the keystore password of the primary keystore is enabled for this purpose.</p>
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
                                            <span class="param-default-value">Default: <code>MI_HOME/repository/resources/security/wso2truststore.jks</code></span>
                                        </div>
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The path to the keystore file that is used for storing the trusted digital certificates. By default, the product&#39;s trust store is configured for this purpose.</p>
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
                                            <span class="param-possible-values">Possible Values: <code>&quot;JKS&quot; or &quot;PKCS12&quot;</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The type of the keystore file that is used as the trust store. By default, the product&#39;s trust store is configured for this purpose.</p>
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
                                        <div class="param-possible">
                                            <span class="param-possible-values">Possible Values: <code>-</code></span>
                                        </div>
                                    </div>
                                    <div class="param-description">
                                        <p>The password of the keystore file that is used as the trust store. By default, the product&#39;s trust store is configured for this purpose.</p>
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

