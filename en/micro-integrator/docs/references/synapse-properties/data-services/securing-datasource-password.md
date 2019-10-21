# Securing Datasource Password

You can secure the datasource connection password by using an encrypted password values instead of a plain-text password. 

<!--
!!! Note
    You need to manually secure the password if you are creating the
datasource inline when creating the data service. However, If you create
the datasource via the **Configure** → **Datasources** → **Add
Datasource** option in the Management Console, it stores the datasource
in the Registry, and thereby, t he password you enter will be encrypted
automatically .
-->

1.  First, [encrypt your data service password](../../../../develop/creating-artifacts/encrypting-synapse-passwords) using secure vault.
2.  Open the data service file from WSO2 Integation Studio and add the password alias instead of the plain text password. The namespace and alias will be added to the .dbs file
    ```xml
    <data name="nested_queries" transports="http https local" xmlns:svns="http://org.wso2.securevault/configuration">
        <config id="Datasource">
            <property name="driverClassName">com.mysql.jdbc.Driver</property>
            <property name="url">jdbc:mysql://localhost:3306/Company</property>
            <property name="org.wso2.ws.dataservice.user">root</property>
            <property name="org.wso2.ws.dataservice.password">svns:secretAlias="DSS.Samples.DB.Pwd"</property>
        </config>
    </data>
    ```
