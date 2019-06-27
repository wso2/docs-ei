# Working with Keystores

!!! warning

This section is currently a work in progress!


A keystore is a repository that stores the cryptographic keys and
certificates that are used for various security purposes, such as
encrypting sensitive information and for establishing trust between your
server and outside parties that connect to your server. The usage of
keys and certificates contained in a keystore are explained below.

**Key pairs** : According to public-key cryptography, a key pair
(private key and the corresponding public key) is used for encrypting
sensitive information and for authenticating the identity of parties
that communicate with your server. For example, information that is
encrypted in your server using the public key can only be decrypted
using the corresponding private key. Therefore, if any party wants to
decrypt this encrypted data, they should have the corresponding private
key, which is usually kept as a secret (not publicly shared).

**Digital certificate** : When there is a key pair, it is also necessary
to have a digital certificate to verify the identity of the keys.
Typically, the public key of a key pair is embedded in this digital
certificate, which also contains additional information such as the
owner, validity, etc. of the keys. For example, if an external party
wants to verify the integrity of data or validate the identity of the
signer (by validating the digital signature), it is necessary for them
to have this digital certificate.

**Trusted certificates** : To establish trust, the digital certificate
containing the public key should be signed by a trusted certifying
authority (CA). You can generate self-signed certificates for the public
key (thereby creating your own certifying authority), or you can get the
certificates signed by an external CA. Both types of trusted
certificates can be effectively used depending on the sensitivity of the
information that is protected by the keys. When the certificate is
signed by a reputed CA, all the parties who trust this CA also trust the
certificates signed by them.

!!! info

Identity and Trust

The key pair and the CA-signed certificates in a keystore establishes
two security functions in your server: The key pair with the digital
certificate is an indication of identity and the CA-signed certificate
provides trust to the identity. Since the public key is used to encrypt
information, the keystore containing the corresponding private key
should always be protected, as it can decrypt the sensitive information.
Furthermore, the privacy of the private key is important as it
represents its own identity and protects the integrity of data. However,
the CA-signed digital certificates should be accessible to outside
parties that require to decrypt and use the information.

To facilitate this requirement, the certificates must be copied to a
separate keystore (called a Truststore), which can then be shared with
outside parties. Therefore, in a typical setup, you will have one
keystore for identity (containing the private key) that is protected,
and a separate keystore for trust (containing CA certificates) that is
shared with outside parties.


## Setting up keystores in WSO2 SP

WSO2 SP uses keystores mainly for the following purposes:

-   Authenticating the communication over Secure Sockets Layer
    (SSL)/Transport Layer Security (TLS) protocols
-   Communication over Secure Sockets Layer (SSL) when invoking Rest
    APIs.
-   Protecting sensitive information via Cipher Tool.  

## Default keystore settings in WSO2 products

WSO2 SP is shipped with two default keystore files stored in the
`         <SP_HOME>/resources/security/        ` directory.

-   `          wso2carbon.jks         ` : This keystore contains a key
    pair and is used by default in your SP servers for all of the
    purposes explained above.
-   `          client-truststore.jks         ` : This is the default
    trust store, which contains the trusted certificates of the keystore
    used in SSL communication.

By default, the following files provide paths to these keystores:

-   `           <SP_HOME>/wso2/<PROFILE>/bin/carbon.s          ` h
    file  
      
    This script is run when you start an SP server. It contains the
    following parameters, and makes references to the two files
    mentioned above by default.

    | Parameter                                         | Default Value                                                                            | Description                                                                                        |
    |---------------------------------------------------|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
    | `               keyStore              `           | `               "$CARBON_HOME/resources/security/wso2carbon.jks" \              `        | This specifies the path to the keystore to be used when running the SP server on a secure network. |
    | `               keyStorePassword              `   | `               "wso2carbon" \              `                                            | The password to access the keystore                                                                |
    | `               trustStore              `         | `               "$CARBON_HOME/resources/security/client-truststore.jks" \              ` | This specifies the path to the trust store to be used when running the server on a secure network. |
    | `               trustStorePassword              ` | `               "wso2carbon" \              `                                            | The password to access the trust store.                                                            |

-   `          <SP_HOME>/conf/<PROFILE>/deployment.yaml         ` file
    refers to the above keystore and trust store by default for the
    following configurations:
    -   Listener configurations  
        This specifies the key store to be used when WSO2 SP is
        receiving events via a secure network and the password to access
        the key store.
    -   Databridge configurations  
        This specifies the key store to be used when WSO2 SP is
        publishing events via databrige using a secure network, and
        the password to access the key store.
    -   Secure vault configurations  
        This specifies the key store to be used when you are configuring
        a secure vault to protect sensitive information.

!!! note

It is recommended to replace this default keystore with a new keystore
that has self-signed or CA signed certificates when the products are
deployed in production environments. This is because
`         wso2carbon.jks        ` is available with open source WSO2
products, which means anyone can have access to the private key of the
default keystore.


  

## Managing keystores

  

  

  
