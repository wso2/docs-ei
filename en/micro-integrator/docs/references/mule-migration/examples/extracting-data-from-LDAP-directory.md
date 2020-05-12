---
title: Importing data from an LDAP Directory
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example illustrates how to connect to an LDAP directory using WSO2 Micro Integrator and retrieve a list of users 
which is stored in that directory. We are using the [LDAP connector](https://store.wso2.com/store/assets/esbconnector/details/4ecf8dde-60f3-4e91-ba22-5f49a4e302f4) to achieve this.

### LDAP

The Lightweight Directory Access Protocol (LDAP) is a public standard that facilitates distributed directories (such as 
network user privilege information) over the Internet Protocol (IP). The many available LDAP servers include free-use 
open source projects.

### Assumption

This document describes the details of the example within the context of **WSO2 Integration Studio**, WSO2 EI’s graphical user interface (GUI). This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case

The example application connects to an LDAP directory and retrieves a list of LDAP users.

<img width="60%" src="../../../assets/img/migration-examples/extracting-data-from-LDAP-directory-use-case.png">

### Set Up and Run the Example 

1. Set up LDAP on your machine
   * For Windows: Install OpenLDAP from [http://www.openldap.org](http://www.openldap.org/).
   * For MacOS: OpenLDAP comes bundled with MacOS, so you do not need to download an LDAP server.
   
2. Install Apache Directory Studio from [http://directory.apache.org/studio/downloads.html](http://directory.apache.org/studio/downloads.html).
 
3. Navigate to `etc\openldap\slapd.conf` in your OpenLDAP installation directory and set `rootpw` in the **BDB database definitions** section to `root`. You might also need to encrypt the password by using the following command before adding it to the `slapd.conf` file:
	```	
	slappasswd -h {SHA} -s <password>
    ```
   
4. Start the LDAP server
   * For Windows: Enter `libexec\StartLDAP.cmd` at a command line.
   * For MacOS: Enter `sudo /usr/libexec/slapd -d 255`.
    
5. Start Apache Directory Studio and create a new connection by selecting **File > New ... > LDAP Connection**. Use these settings:
	* name: local LDAP
	* hostname: localhost
	* port: 389

6. Click **Check network parameter**. If the test is not successful, your LDAP server is not running. If the test is successful, click **Next**.
	
7. Set **Bind dn or user** to `cn=Manager,dc=my-domain,dc=com` and **Bind password** to `root`. Click **Check Authentication** to verify the connection. Click **Finish**.

8. Click **File > Import... > LDIF into LDAP** and then click **Next**. 

9. Set the path to `ldap.ldif`, which is located in the `resources` directory of this project (`integration-studio-examples/migration/mule/extracting-data-from-LDAP-directory/resources/ldap.ldif`). Set **Import into** to `local LDAP`. Click **Finish** to finish the import process. If you click **ROOT DSE** in the panel **LDAP browser**, you should see the imported data structure.

10. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

11. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

12. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

13. Browse and select the file path to the downloaded sample of this Github project (`integration-studio-examples/migration/mule/extracting-data-from-LDAP-directory`) and click **Finish**.

14. Lets add the ldap connector into the workspace. Right click on **ExtractingDataFromLdapDirectory** and select **Add or Remove Connector**. In the **Add or Remove Connectors** window, select **Add connector** option and click **Next>**. Type `ldap` on the search bar of the WSO2 Connector Store. Click on download symbol and ldap connector will be downloaded form the store. Now click **Finish**.

15. Open the **ExtractingDataFromLdapDirectory.xml** under **extracting-data-from-LDAP-directory/ExtractingDataFromLdapDirectory/src/main/synapse-config/api** directory and specify following values.

	* URL: ldap://localhost:389/dc=my-domain,dc=com
	* Principal DN: cn=Manager,dc=my-domain,dc=com
	* Password: root<br>
    <img width="60%" src="../../../assets/img/migration-examples/extracting-data-from-LDAP-directory.png">

16. Run the sample by right click on the **ExtractingDataFromLdapDirectoryCompositeApplication** under the main **extracting-data-from-LDAP-directory** project and selecting **Export Project Artifacts and Run**.

17. Open HTTP Client in Integration Studio. Follow [HTTP Client Guidelines](../../../docs/common/adding-http-client-to-integration-studio.md) to open HTTP Client if the window is not visible in the interface.

18. Make a GET request to `http://localhost:8290/extract`.

You should see three user records in response:
```
[
    {
        "dn": "cn=admin,ou=people",
        "sn": "admin",
        "cn": "admin",
        "objectclass": [
            "top",
            "person"
        ],
        "userpassword": "mmcadmin"
    },
    {
        "dn": "cn=testuser1,ou=people",
        "sn": "testuser1",
        "cn": "testuser1",
        "objectclass": [
            "top",
            "person"
        ],
        "userpassword": "testuser1123"
    },
    {
        "dn": "cn=mmc,ou=people",
        "sn": "mmc",
        "cn": "mmc",
        "objectclass": [
            "top",
            "person"
        ],
        "userpassword": "mmc123"
    }
]
```
### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/extracting-data-from-LDAP-directory.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>
