# Using Single Key Encryption

WSO2 Micro Integrator uses [asymmetric
encryption](../security/configuring_keystores.md) by default. If required, you can switch **single key encryption** (symmetric
encryption), which means that a single key will be shared for encryption and
decryption of information.

## Enable single key encryption

To enable symmetric encryption, open the ei.toml file and add the following configurations:

``` java
[encryption]
type= "symmetric"
key = "value"
```

## Encrypt the symmetric key

For better security, the symmetric key in the ei.toml file should be encrypted.
See [Encrypting Passwords](../security/encrypting_plain_text.md) to replace this key with an encrypted value.
