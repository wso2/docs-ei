# LDAP Connector Configuration

To use the LDAP connector, add the `<ldap.init>` element in your configuration before carrying out any other LDAP operations. 

??? note "ldap.init"
    The ldap.init operation initializes the connector to interact with an LDAP.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>providerUrl</td>
            <td>The URL of the LDAP server.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>securityPrincipal</td>
            <td>The Distinguished Name (DN) of the admin of the LDAP Server.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>securityCredentials</td>
            <td>The password of the LDAP admin.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>secureConnection</td>
            <td>The boolean value for the secure connection.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>disableSSLCertificateChecking</td>
            <td>The boolean value to check whether the certificate is enabled or not.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**
    ```xml
    <ldap.init>
        <providerUrl>{$ctx:providerUrl}</providerUrl>
        <securityPrincipal>{$ctx:securityPrincipal}</securityPrincipal>
        <securityCredentials>{$ctx:securityCredentials}</securityCredentials>
        <secureConnection>{$ctx:secureConnection}</secureConnection>
        <disableSSLCertificateChecking>{$ctx:disableSSLCertificateChecking}</disableSSLCertificateChecking>
    </ldap.init>
    ```


You can follow the steps below to import your LDAP certificate into wso2ei client’s keystore as follows:

1. To encrypt the connections, you need to configure a certificate authority (https://www.digitalocean.com/community/tutorials/how-to-encrypt-openldap-connections-using-starttls) 
and use it to sign the keys for the LDAP server.
2. Use the following command to import the certificate into the EI client keystore. 
   ```bash
   keytool -importcert -file <certificate file> -keystore <EI>/repository/resources/security/client-truststore.jks -alias "LDAP"
   ```
3. Restart the server and deploy the LDAP configuration.

#### Ensuring secure data
Secure Vault is supported for encrypting passwords. See, 
[Working with Passwords](https://docs.wso2.com/display/ADMIN44x/Encrypting+Passwords+with+Cipher+Tool) on integrating 
and using Secure Vault.

#### Re-using LDAP configurations

You can save the LDAP configuration as a [local entry](../../../develop/creating-artifacts/registry/creating-local-registry-entries.md) 
and then easily reference it with the configKey attribute in your operations. For example, if you saved the above 
**<ldap.init>** entry as a local entry named MyLDAPConfig, you could reference it from an operation like addEntry as follows:
    ```xml
    <ldap.addEntry configKey="MyLDAPConfig"/>
    ```
