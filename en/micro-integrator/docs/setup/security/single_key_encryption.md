# Using Symmetric Encryption

WSO2 Carbon-based products use [asymmetric
encryption](../security/configuring_keystores.md) by default as explained in
the previous section. From Carbon 4.4.3 onwards, you have the option of
switching to symmetric encryption in your WSO2 product. Using symmetric
encryption means that a single key will be shared for encryption and
decryption of information.

To enable symmetric encryption, open the esb.toml file and add the following configurations:

``` java
//The config sectin...
[single_key_encryption]
    
// This property is used to enable single key encryption
IsEnabled  = "false"
    
// This property specifies the symmetric key algorithm.
Algorithm = ""
    
// This property is used to specify the secret alias if secure vault has been used to encrypt the secret key.
securevaultalias = ""
    
// This property is used to enable single key encryption
symmetric.key = ""
```

If Secure Vault has been used for encrypting the symmetric key, this
    value will be replaced by the secret alias as shown below. For
    detailed instructions on how the secret key can be encrypted using
    Secure Vault, see [Encrypting Plain Text](../security/encrypting_plain_text.md).

``` java
symmetric.key=secretAlias:symmetric.key.value
```
