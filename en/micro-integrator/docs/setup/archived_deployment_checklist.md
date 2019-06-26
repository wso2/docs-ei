
# Production Deployment Checklist

The requirements for deploying WSO2 products can change based on the deployment scenario and pattern. The recommendations in this topic are for general production use, assuming moderate load conditions. For situations where a high volume of traffic is expected and if there are large deployments, these guidelines may not be sufficient. See Troubleshooting in Production Environments for information on how to obtain and analyze information to solve production issues.

## Common checklist

<table>
  <tr>
    <th>Guideline</th>
    <th>Details</th>
  </tr>
  <tr>
    <td>Hostname</td>
    <td>By default, WSO2 products identify the hostname of the current machine through the Java API. However, this value sometimes yields erroneous results on some environments. Therefore, users are recommended to configure the hostname by setting the HostName parameter in the ei.toml file. </br>
    To configure hostnames for WSDLs and endpoints, users are recommended to add the following parameter in the ei.toml file.</td>
  </tr>
  <tr>
    <td>User Stores</td>
    <td>WSO2 products offer three choices to store user details:
      <ul>
        <li>Using a database</li>
        <li>Using an LDAP server</li>
        <li>Using an Active Directory service</li>
      </ul>
      The default is to use the embedded H2 database, with the user store schema. Like in the case of the registry database, you can switch to a database like Oracle, MySQL or MSSQL. You can point to an existing LDAP or an Active Directory to make use of existing user bases and grant access privileges for WSO2 products based on those user stores.
    </td>
  </tr>
  <tr>
    <td>Tuning WSO2 products</td>
    <td>Most of the performance tuning recommendations are common to all WSO2 products. However, each WSO2 product may have additional guidelines for optimizing the performance of product-specific features.
    </td>
  </tr>
  <tr>
    <td>Firewalls</td>
    <td>The following ports must be accessed when operating within a firewall.
    <ul>
      <li>9443 - Used by the management console and services that use the servlet transport.</li>
      <li>9763 - Used by the services that use servlet transport.</li>
      <li>9999 - Used for JMX monitoring.</li>
      <li>8280 - Default HTTP port used by ESB for proxy services.</li>
      <li>8243 - Default HTTPS port used by ESB for proxy services.</li>
    </ul>
    </td>
  </tr>
  <tr>
    <td>Proxy Servers</td>
    <td>If the product is hosted behind a proxy such as ApacheHTTPD, users can configure products to use the proxy server by providing the following system properties at the start-up.
    </td>
  </tr>
  <tr>
    <td>High availability</td>
    <td>For high availability, WSO2 products must run on a cluster. This enables the WSO2 products to still work in the case of failover. Databases used for the repository, user management, and business activity monitoring can also be configured in a cluster or can use replication management provided by the RDBMS.
    </td>
  </tr>
  <tr>
    <td>Data backup and archiving</td>
    <td>For data backup and for archiving of data, use the functionality provided by the RDBMS.
    </td>
  </tr>
</table>

## Security checklist

Given below are the common security guidelines for deploying a WSO2 product in a production environment.

Also, see the production deployment guidelines and any other product-specific guidelines that might come in the respective product's documentation.

### Product-level security

<table>
  <tr>
    <th>Guideline</th>
    <th>Details</th>
  </tr>
  <tr>
    <td>Apply security updates</td>
    <td>Apply all the security patches relevant to your product version. If your WSO2 product version is supported by WSO2 Update Manager (WUM), you need to use WUM to get the latest fixes.
    <ul>
      <li>If your WSO2 product is listed as a WUM-supported product here, follow the instructions in Getting Started with WUM.</li>
      <li>If you are using an older WSO2 product version, which is not WUM-supported, you need to download the security patches relevant to your product from the WSO2 Security Patch Release page and apply them to your system manually. The instructions are given in WSO2 Patch Application Process.</li>
    </ul>
    </td>
  </tr>
  <tr>
    <td>User Stores</td>
    <td>WSO2 products offer three choices to store user details:
      <ul>
        <li>Using a database</li>
        <li>Using an LDAP server</li>
        <li>Using an Active Directory service</li>
      </ul>
      The default is to use the embedded H2 database, with the user store schema. Like in the case of the registry database, you can switch to a database like Oracle, MySQL or MSSQL. You can point to an existing LDAP or an Active Directory to make use of existing user bases and grant access privileges for WSO2 products based on those user stores. </br>
      Note the following:
      <ul>
        <li>WSO2 releases security patch notifications monthly via the Support Portal and the above mentionedWSO2 Security Patch Releases page. However, for highly critical issues, patches are issued immediately to customers.</li>
        <li>The WSO2 Security Patch Release page has all the security patches for the latest product versions. WSO2 does not issue patches publicly for older product versions. Community users are encouraged to use the latest product version to receive all the security fixes.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Change default keystores</td>
    <td>Change the default key stores and create new keys for all the cryptographic operations. WSO2 products by default come with a self-signed SSL key. Since these keys are public, it is recommended to configure your own keys for security purposes. Consider the following guidelines when creating the keystores:
    <ul>
      <li>Select a key size of at least 2048 bits.</li>
      <li>Use an SHA256 certificate.</li>
      <li>Make sure that WSO2 default certificates do not exist in any of the keystores in your production environment. For example, be sure to delete the default public certificate in the default trust store that is shipped with the product.</li>
    </ul>
    See the recommendations for using keystores in WSO2 products for information.
    See Creating New Keystores for information on how to create and configure your own keys.
    </td>
  </tr>
  <tr>
    <td>Encrypt passwords in configuration files</td>
    <td>WSO2 products use a tool called Secure Vault to encrypt the plain-text passwords in configuration files. </br>See Securing Passwords in Configuration Files for instructions.
    </td>
  </tr>
  <tr>
    <td>Change default ports</td>
    <td>All the default ports used by WSO2 products are listed in Default Ports of WSO2 Products. For example, the default HTTPS port is 9443 and the HTTP port is 9763. Also, Axis2 services are exposed over the following ports: 8243 and 8280. </br>To change a default port, update the <Offset> element in the carbon.xml file as explained in Changing the Default Ports.
    </td>
  </tr>
  <tr>
    <td>Enable read-only access to external user stores (LDAPs etc.)</td>
    <td>If your WSO2 product is connecting to an external user store, such as Microsoft Active Directory, for the purpose of reading and retrieving user information, be sure to enable read-only access to that user store. </br> For example, see Configuring a Read-Only LDAP User Store under Configuring User Stores for instructions.
    </td>
  </tr>
  <tr>
    <td>Always communicate (with external user stores) over TLS</td>
    <td>All connections from your WSO2 product to external databases, userstores (LDAP), or other services, should be over TLS, to ensure adequate network-level protection. Therefore, be sure to use external systems (user stores, databases) that are TLS-enabled.
    </td>
  </tr>
  <tr>
    <td>Configure strong HTTP(S) security</td>
    <td>To have strong transport-level security, use TLS 1.2 and disable SSL, TLS 1.0 and 1.1. The TLS protocol and strong ciphers are configured for an HTTP connector in the catalina-server.xml file (using the sslEnabledProtocols and ciphers attributes). See the following links for instructions:
    <ul>
      <li>Configuring Transport-Level Security</li>
      <li>Supported Cipher Suites</li>
    </ul>
    Note the following:
    <ul>
      <li>When deciding on the TLS protocol and the ciphers, consider the compatibility with existing client applications. Imposing maximum security might cause functional problems with client applications.</li>
      <li>Apply ciphers with 256 bits key length if you have applied the Unlimited strength policy. Note that Unlimited strength policy is recommended.</li>
      <li>
      DES/3DES are deprecated and should not be used.
      MD5 should not be used, due to known collision attacks.
      RC4 should not be used, due to crypto-analytical attacks.
      DSS is limited to a small 1024 bit key size.
      Cipher-suites that do not provide Perfect Forward Secrecy/ Forward Secrecy (PFS/FS).
      GCM based ciphers are recommended over CBC ciphers.
      </li>
    </ul>
    </td>
  </tr>
  <tr>
    <td>Remove weak ciphers for PassThrough transport</td>
    <td>
    Applicable only to products that use the PassThrough transport, such as WSO2 API Manager and WSO2 Enterprise Integrator (ESB profile). </br>
    Remove any weak ciphers from the PassThrough transport and ensure that the server does not accept connections using those weak ciphers. The PassThrough transport is configured using the axis2.xml file (stored in the <PRODUCT_HOME>/repository/conf/axis2/ directory. You need to add the PreferredCiphers parameter under the "Transport Ins (Listeners)" section along with the list of relevant cipher suites. </br>
    See Configuring the PassThrough Transport for instructions.
    </td>
  </tr>
</table>
