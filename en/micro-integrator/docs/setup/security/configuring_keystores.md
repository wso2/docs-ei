# Configuring Keystores for WSO2 EI
WSO2 Carbon-based products are shipped with a default keystore named wso2carbon.jks, which is stored in the  <EI_HOME>/repository/resources/security directory. This is the primary keystore of the product, and it is used, by default, for all of the following requirements.

* **Encrypting/decrypting passwords** and other confidential information, which are maintained in various configuration files as well as internal data stores.

    > Note that it is recommended to separate the [keystore for encrypting information in internal data stores](#separating-the-internal-keystore).

* **Signing messages** when the WSO2 product communicates with external parties (such as SAML, OIDC id_token signing).

## Changing the default primary keystore

Follow the steps given below to change the default primary keystore that is shipped with the product.

1. [Create a new keystore](../../setup/security/creating_keystores.md) and copy it to the <EI_HOME>/repository/security/ directory.
  > Note that CA-signed certificates are recommended for this keystore because it is used for communicating with external parties.

2. Open the ei.toml file, add the following config section and update the parameters of our internal keystore.
    ```Java
    [keystore.tls]
    file_name="wso2carbon.jks"
    type="JKS"
    password="wso2carbon"
    alias="wso2carbon"
    key_password="wso2carbon"
    ```
## Separating the internal keystore
By default, the primary keystore is used for internal **data encryption** (encrypting data in internal data stores and configuration files) as well as for **signing messages** that are communicated with external parties.

> **Why separate the internal keystore?**

> It is sometimes a common requirement to have separate keystores for communicating messages with external parties (such as SAML, OIDC id_token signing) and for encrypting information in internal data stores. This is because, for the first scenario of signing messages, the keystore certificates need to be frequently renewed. However, for encrypting information in internal data stores, the keystore certificates should not be changed frequently because the data that is already encrypted will become unusable every time the certificate changes.

Follow the steps given below to separate the keystore that is used for encrypting data in internal data stores.

1. [Create a new keystore](../../setup/security/creating_keystores.md) and copy it to the <EI_HOME>/repository/security/ directory.
  > Note that you do not require CA-signed certificates for this keystore because it will not be used for communicating with external parties.

2. Open the ei.toml file, add the following config section and update the parameters of our internal keystore.
    ```Java
    [keystore.internal]
    file_name="wso2carbon.jks"
    type="JKS"
    password="wso2carbon"
    alias="wso2carbon"
    key_password="wso2carbon"
    ```

## Optional: Changing the default truststore
The truststore is used for storing the digital certificates that are used for SSL communication over the network. If required, you can change the default truststore (which is the truststore.jks file stored in the <EI_HOME>/repository/security/ directory) as follows:

1. [Create a new keystore](../../setup/security/creating_keystores.md) and copy it to the <EI_HOME>/repository/security/ directory.
  > Note that you do not require CA-signed certificates for this keystore because it will not be used for communicating with external parties.

2. Open the ei.toml file, add the following config section and update the parameters of our internal keystore.
    ```Java
    [truststore]
    file_name="wso2carbon.jks"
    type="JKS"
    password="wso2carbon"
    alias="symmetric.key.value"
    algorithm=""
    ```
3. Import all the required certificates.
