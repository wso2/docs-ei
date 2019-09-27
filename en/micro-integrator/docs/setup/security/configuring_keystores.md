# Configuring Keystores for the Micro Integrator

Follow the instructions given below to configure [keystores for the Micro Integrator](../../references/using_keystores.md).

## The default keystore configuration
WSO2 Micro Integrator is shipped with a default keystore (wso2carbon.jks) and default trust store client-truststore.jks, which are stored in the MI_HOME/repository/resources/security/ directory. 

The **default keystore** is used for the following requirements:

* **Encrypting/decrypting passwords** and other confidential information, which are maintained in various configuration files as well as internal data stores.

    > Note that it is recommended to separate the [keystore for encrypting information in internal data stores](#separating-the-internal-keystore).

* **Signing messages** when the WSO2 product communicates with external parties (such as SAML, OIDC id_token signing).

The **default trust store** contains the certificates of reputed CAs that can validate the identity of third party systems that communicate with the Micro Integrator. This trust store also contains the self-signed certificate of the default `wso2carbon.jks` keystore.


## Changing the default primary keystore

If you want to change the [default primary keystore](#the-default-keystore-configuration) that is shipped with the product, follow the steps given below.

1. [Create a new keystore](../../setup/security/creating_keystores.md) and copy it to the MI_HOME/repository/security/ directory.
  
    !!! Note
        CA-signed certificates are recommended for this keystore because it is used for communicating with external parties.

2. Open the deployment.toml file, add the following config section, and update the parameter values for the newly-created keystore.
    ```toml
    [keystore.tls]
    file_name="wso2carbon.jks"
    type="JKS"
    password="wso2carbon"
    alias="wso2carbon"
    key_password="wso2carbon"
    ```
    Find more details about [keystore parameters](../../../references/ei_config_catalog/#configuring-the-tls-keystore).

3. [Import the required CA-signed certificates](../../setup/security/importing_ssl_certificate.md) to the key store.

## Separating the internal keystore
By default, the [primary keystore](#the-default-keystore-configuration) is used for internal **data encryption** (encrypting data in internal data stores and configuration files) as well as for **signing messages** that are communicated with external parties.

!!! Info
    **Why separate the internal keystore?**
    
    It is sometimes a common requirement to have separate keystores for communicating messages with external parties (such as SAML, OIDC id_token signing) and for encrypting information in **internal data stores**. This is because, for the first scenario of signing messages, the keystore certificates need to be frequently renewed. However, for encrypting information in internal data stores, the keystore certificates should not be changed frequently because the data that is already encrypted will become unusable every time the certificate changes.

Follow the steps given below to separate the keystore that is used for encrypting data in internal data stores.

1. [Create a new keystore](../../setup/security/creating_keystores.md) and copy it to the MI_HOME/repository/security/ directory.
  
    !!! Note
        You do not require CA-signed certificates for this keystore because it will **not** be used for communicating with external parties.

2. Open the deployment.toml file, and update the parameter values for the newly-created internal keystore.
    ```toml
    [keystore.internal]
    file_name="wso2carbon.jks"
    type="JKS"
    password="wso2carbon"
    alias="wso2carbon"
    key_password="wso2carbon"
    ```
    Find more details about [internal keystore parameters](../../../references/ei_config_catalog/#configuring-the-internal-keystore).

## Optional: Changing the default truststore
If you want to change the [default trust store](#the-default-keystore-configuration) that is shipped with the product, follow the steps given below.

1. [Create a new keystore](../../setup/security/creating_keystores.md) and copy it to the MI_HOME/repository/security/ directory.

    !!! Note 
        You do not require CA-signed certificates for this keystore.

2. Open the deployment.toml file, add the following config section and update the values for the newly-created trust store.
    ```toml
    [truststore]
    file_name="wso2carbon.jks"
    type="JKS"
    password="wso2carbon"
    alias="symmetric.key.value"
    algorithm=""
    ```
3. [Import the required certificates](../../setup/security/importing_ssl_certificate.md#importing-ssl-certificates-to-a-truststore) to the trust store.
