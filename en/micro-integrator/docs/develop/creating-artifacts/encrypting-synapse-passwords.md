# Encrypting Synapse Secrets

When you define your integration artifacts, some data in the synapse configurations need to be encrypted for better security.

In a VM deployment, you can encrypt the sensitive data in the synapse configurations by using the [**Secure Vault** implementation](../../references/security/customizing-secure-vault.md) and then use **vault lookup** in your synapse configurations to refer the alias of the encrypted data.

## Encrypting secrets using Secure Vault

Follow the steps given below:

1.  Open a command terminal and navigate to the `MI_HOME/bin/` directory.
2.  If you are using the Cipher tool for the first time in your environment, you must enable the Cipher tool by
executing the `-Dconfigure` command with the Cipher tool script.
		Execute the following command to initialize the
cipher tool:

		```bash tab='On Linux'
		bash ciphertool.sh -Dconfigure
		```

		```bash tab='On MacOS'
		./ciphertool.sh -Dconfigure
		```

		```bash tab='On Windows'
		ciphertool.bat -Dconfigure
		```

    !!! Note
        If you are using **Linux**, make sure to run the script using **bash**.

3.  Execute the following command to initialize secure vault:
		!!! Warning
		    If you are using **Windows**, be sure to update the `securevault.bat` file with the following configuration before  encrypting passwords.
			```java
			set CARBON_CLASSPATH=.\conf
			    FOR %%c in ("%CARBON_HOME%\wso2\components\plugins\org.wso2.micro.integrator.security*.jar") DO (
			    set CARBON_CLASSPATH=!CARBON_CLASSPATH!;".\wso2\components\plugins\%%~nc%%~xc")
			```
4.  You can then enter the secret alias (vault key) for the password that you want to encrypt. For example, enter
'PasswordAlias'.
5.  In the next step, enter the password of the keystore that is used for secure vault in the product. If the default product keystore is used, the password is 'wso2carbon'.
6.  Then, specify the plain text password that should be encrypted.

The encrypted password is stored in the `secure-vault.properties` file as shown below. This file is stored in the `<MI_HOME>/registry/config/repository/components/secure-vault/` directory.

```bash
#Secure Vault keys for micro integrator
#Thu Sep 27 14:38:33 IST 2018
ProductAlias=hMH3b4S2AhG6l0699uhUVD9O38G04NRygA6TW46/OFMkdNO/0Ucj1ql/x9gCRKrR2TVLFYaM7Sx7E14dJ4IoOaIX9zql9ZxG9bF6ktG2rrktRGoB39BuaLIJ/wPYLoNT26bKr7QXj+NR16eQWlckn1f40Ru2zvE/2wG2smuQL7g67Ptw4DL800IaNYWW8vnhHfaeK+E5CgOKQnTDnwuDDodjiXsJh+2mu2l0KdgDPdxcSjb8uPVC1OubRymygqOJpzKg6Md1R42fGgKGBG9CP9pRj7hW95dVy9h23tHx22ejCrSoxIiEoQjAIIu2wVCBI7fY2HUKBUQOHhb+kenawA\=\=
```         

## Using encrypted secrets in Synapse configurations

To use the alias of an encrypted secret in a synapse configuration, add the `{wso2:vault-lookup('alias')}`
custom path expression when you define the synapse configuration.

For example, instead of hard coding a password as
`<Password>admin</Password>`, you can encrypt and
store the password using the `AdminUser.Password` alias
as follows:
`<Password>{wso2:vault-lookup('AdminUser.Password')}</Password>`

This password in the synapse configuration can now be retrieved by
using the `{wso2:vault-lookup('alias')}` custom path
expression to logically reference the password mapping.

Similarly, if you have [server secrets encrypted](../../../setup/security/encrypting_plain_text) in the `deployment.toml` file, you can refer them from the synapse configuration by using the alias.

```xml
<log level="custom">
  <property expression="wso2:vault-lookup('alias')" name="secret"/>
</log>
```
