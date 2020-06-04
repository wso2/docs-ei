# Encrypting Synapse Secrets

When you define your integration artifacts, some data in the synapse configurations need to be encrypted for better security.

## Encrypted secret

In a VM deployment, static secrets are defined in the server configuration file (`deployment.toml`) and encrypted using the [Cipher Tool](../../references/security/customizing-secure-vault.md).

For example, shown below is the encrypted secret for the `property_expression` alias in the `deployment.toml` file:

```toml
[secret]
property_expression= "<encrypted_secret>"
```

If you don't already have a secret encrypted for an alias, follow the instructions on [encrypting secrets](../../../setup/security/encrypting_plain_text).

## Using encrypted secrets in synapse configurations

You can then refer an encrypted secret in your synapse configurations by using the **vault lookup** function (`{wso2:vault-lookup('alias')`). Use the vault lookup function in place of the plain-text secret in the synapse configuration as shown below. Replace `alias` with a value that represents the configuration secret.

```xml
<log level="custom">
  <property expression="wso2:vault-lookup('property_expression')" name="secret"/>
</log>
```

This configuration will lookup the encrypted secret for the `property_expression` alias in the `deployment.toml` file.
