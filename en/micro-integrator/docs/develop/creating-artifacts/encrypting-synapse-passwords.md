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
2.  If you are using the Cipher tool for the first time in your environment, you must enable the Cipher tool by 
executing the `-Dconfigure` command with the Cipher tool script. Execute the following command to initialize the 
cipher tool:

	<table>
		<tr>
			<th>OS</th>
			<th>Command</th>
		</tr>
		<tr>
        	<td><b>Linux</b></td>
        	<td>bash ciphertool.sh -Dconfigure</td>
        </tr>
		<tr>
			<td><b>Mac OS</b></td>
			<td>./ciphertool.sh -Dconfigure</td>
		</tr>
		<tr>
			<td><b>Windows</b></td>
			<td>ciphertool.bat -Dconfigure</td>
		</tr>
	</table>
	
    !!! Note 
        If you are using **Linux**, make sure to run the script using **bash** as explained above.

3.  Execute the following command to initialize secure vault:

	!!! Warning
	    If you are using **Windows**, be sure to update the `securevault.bat` file with the following configuration before  encrypting passwords.
		```java
		set CARBON_CLASSPATH=.\conf
		    FOR %%c in ("%CARBON_HOME%\wso2\components\plugins\org.wso2.micro.integrator.security*.jar") DO (
		    set CARBON_CLASSPATH=!CARBON_CLASSPATH!;".\wso2\components\plugins\%%~nc%%~xc")
		```

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
	
4.  You can then enter the secret alias (vault key) for the password that you want to encrypt. For example, enter 
'PasswordAlias'.
5.  In the next step, enter the password of the keystore that is used for secure vault in the product. If the default
 product keystore is used, the password is 'wso2carbon'.
6.  Then, specify the plain text password that should be encrypted.

The encrypted password is stored in the `secure-vault.properties` file as shown below. This file is stored in the `<MI_HOME>/registry/config/repository/components/secure-vault/` directory.

```bash
#Secure Vault keys for micro integrator
#Thu Sep 27 14:38:33 IST 2018
ProductAlias=hMH3b4S2AhG6l0699uhUVD9O38G04NRygA6TW46/OFMkdNO/0Ucj1ql/x9gCRKrR2TVLFYaM7Sx7E14dJ4IoOaIX9zql9ZxG9bF6ktG2rrktRGoB39BuaLIJ/wPYLoNT26bKr7QXj+NR16eQWlckn1f40Ru2zvE/2wG2smuQL7g67Ptw4DL800IaNYWW8vnhHfaeK+E5CgOKQnTDnwuDDodjiXsJh+2mu2l0KdgDPdxcSjb8uPVC1OubRymygqOJpzKg6Md1R42fGgKGBG9CP9pRj7hW95dVy9h23tHx22ejCrSoxIiEoQjAIIu2wVCBI7fY2HUKBUQOHhb+kenawA\=\=
```

!!! Warning
    If you're running securevault on a fresh Micro Integrator distribution, you may have to manually create the `secure-vault.properties` file inside the `<MI_HOME>/registry/config/repository/components/secure-vault/` directory to avoid the error mentioned in [MI-1060](https://github.com/wso2/micro-integrator/issues/1060)
         

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
