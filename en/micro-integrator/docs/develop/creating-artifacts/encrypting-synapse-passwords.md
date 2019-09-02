# Encrypting Synapse Passwords

All WSO2 products are shipped with a [**Secure Vault** implementation](../../references/security/customizing-secure-vault.md)
that allows you to store encrypted passwords that are mapped to aliases.
This approach allows you to use the aliases instead of the actual
passwords in your configurations for better security. For example, some
configurations require the admin username and password. If the admin
user's password is "admin", you could use `         UserManager.AdminUser.Password        ` as the password alias.
You will then map that alias to the actual "admin" password using Secure
Vault. The WSO2 product will then look up this alias in Secure Vault during runtime, decrypt and use its password.

## Encrypting passwords for synapse configurations

Follow the steps given below:

1.  Open a command terminal and navigate to the `MI_HOME/bin/` directory.
2.  Execute the following command to initialize secure vault:

	<table>
		<tr>
			<th>OS</th>
			<th>Command</th>
		</tr>
		<tr>
			<td><b>Linux/Mac OS</b></td>
			<td>sh securevault.sh</td>
		</tr>
		<tr>
			<td><b>Windows</b></td>
			<td>securevault.bat </td>
		</tr>
	</table>

3.  You can then enter the secret alias (vault key) for the password that you want to encrypt. For example, enter 'PasswordAlias'.
4.  In the next step, enter the password of the keystore that is used for secure vault in the product. If the default product keystore is used, the password is 'wso2carbon'.
5.  Then, specify the plain text password that should be encrypted.

The encrypted password is stored in the `secure-vault.properties` file as shown below. This file is stored in the `<MI_HOME>/registry/config/repository/components/secure-vault/` directory.

```
#Secure Vault keys for micro integrator
#Thu Sep 27 14:38:33 IST 2018
ProductAlias=hMH3b4S2AhG6l0699uhUVD9O38G04NRygA6TW46/OFMkdNO/0Ucj1ql/x9gCRKrR2TVLFYaM7Sx7E14dJ4IoOaIX9zql9ZxG9bF6ktG2rrktRGoB39BuaLIJ/wPYLoNT26bKr7QXj+NR16eQWlckn1f40Ru2zvE/2wG2smuQL7g67Ptw4DL800IaNYWW8vnhHfaeK+E5CgOKQnTDnwuDDodjiXsJh+2mu2l0KdgDPdxcSjb8uPVC1OubRymygqOJpzKg6Md1R42fGgKGBG9CP9pRj7hW95dVy9h23tHx22ejCrSoxIiEoQjAIIu2wVCBI7fY2HUKBUQOHhb+kenawA\=\=
```

## Using encrypted passwords in synapse configurations

To use the alias of an encrypted password in a synapse configuration,
you need to add the `         {wso2:vault-lookup('alias')}        `
custom path expression when you define the synapse configuration. For
example, instead of hard coding the admin user's password as
`         <Password>admin</Password>        ` , you can encrypt and
store the password using the `         AdminUser.Password        ` alias
as follows:
`         <Password>{wso2:vault-lookup('AdminUser.Password')}</Password>.        `

This password in the synapse configuration can now be retrieved by
using the `         {wso2:vault-lookup('alias')}        ` custom path
expression to logically reference the password mapping.

## Updating the password validation

The default expression used for password validation is `^[\\S]{5,30}$`. This allows the password to have 5 to 30 characters.

If you want to change the expression that is used to validate the password, you need to add the `org.wso2.SecureVaultPasswordRegEx` system property to the `<MI_HOME>/conf/carbon.properties` file. Example:

``` java
org.wso2.SecureVaultPasswordRegEx=^[\\S]{5,60}$
```