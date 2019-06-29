# Deploying WSO2 Enterprise Integrator
The following sections provide information and instructions on how to cluster the ESB profile of WSO2 Enterprise Integrator (WSO2 EI) with a third-party load balancer.

## The deployment pattern

This deployment scenario uses a two-node ESB cluster. That is, two ESB nodes are configured to serve requests with high availability and scalability. As depicted by the following diagram, the product nodes in the cluster are fronted by an external third-party load balancer, which routes requests to the two nodes on a round-robin basis.

![Micro Integrator Deployment Pattern](../../assets/img/deployment_ei.png)

## Installing WSO2 Enterprise Integrator

Follow the instructions on [downloading and installing WSO2 EI](../setup/installation/install_in_vm.md) on a single machine.

## Setting up the load balancer

Follow the instructions on [setting up a load balancer](../../setup/setting_up_lb.md) for a two-node deployment of WSO2 EI.

## Setting up the primary database

By default, the embedded H2 database is configured as the product's primary database. Select your preferred database type from the following list and follow the given instructions:

* [Setting up MySQL](../../setup/databases/setting-up-MySQL.md)
* [Setting up MSSQL](../../setup/databases/setting-up-MSSQL.md)
* [Setting up Oracle](../../setup/databases/setting-up-Oracle.md)
* [Setting up Oracle RAC](../../setup/databases/setting-up-Oracle-RAC.md)
* [Setting up IBM Informix](../../setup/databases/setting-up-IBM-Informix.md)
* [Setting up IBM DB2](../../setup/databases/setting-up-IBM-DB2.md)
* [Setting up Maria DB](../../setup/databases/setting-up-MariaDB.md)
* [Setting up embedded Derby](../../setup/databases/setting-up-Embedded-Derby.md)
* [Setting up remote Derby](../../setup/databases/setting-up-Remote-Derby.md)
* [Setting up PostgreSQL](../../setup/databases/setting-up-PostgreSQL.md)
* [Setting up remote H2](../../setup/databases/setting-up-Remote-H2.md)

## Updating keystores

1. [Create a new SSL certificate](../../setup/security/importing_ssl_certificate.md) and import it to the primary keyStore and trustStore (which are located in the /repository/security/ directory). Your primary keystore can now be configured for SSL communication.
2. [Create a new keystore](../../setup/security/creating_keystores.md) to use as the internal keystore (for the purpose of data encryption/decryption in internal data stores).

## Configuring the WSO2 EI nodes

Follow the steps given below to configure the two nodes in the WSO2 EI deployment.

1. Open the esb.toml file of both the nodes. This file is located in the <EI_HOME>/conf/ directory.
2. To specify the default settings that should be applicable to the nodes, update the following toml parameters with the required values.

    ```java
    // The config section that groups the parameters that identify the server.
    [server]

    // The hostname of the server.
    hostname = "localhost"

    // If you are running both product nodes (of your cluster) on the same VM, set a port offset for on the servers.
    offset = 0
    ```
   Find more [parameters](../../../references/ei_config_catalog/#configuring-the-default-deployment-settings) for deployment settings.

3. To define the clustering configurations that specify how the two nodes communicate with one another, add the following to the esb.toml file and update the values.
    ``` java
    // The config section that groups the parameters that define cluster coordination.
    [clustering]

    // Specify the port on which the current server node is running.
    local_member_port = 4000

    // Specify the hostname of the current server node.
    local_member_host = "10.100.5.86"

    // Specify the IP address:port of each of the nodes in the cluster as shown below. Be sure to use the same port number and hostname you specified above.
    members = ["10.100.5.86:4000","10.100.5.86:4001"]
    ```
    Find more [parameters](../../../references/ei_config_catalog/#configuring-the-cluster-settings) to define clustering.

4. To enable the two nodes to access the shared database, go to the section on [setting up the primary database](../../setup/deployment/deploying_wso2_ei.md#setting-up-the-primary-database), select your database type, and verify the parameters specific to your database.

 5. To manage the users and roles in the system, update the `[user_store]` section in the esb.toml file.
    
    * If you are using primary database of your product as the user store, use the following:
        ``` java
        // The config section that groups the parameters that define the user store (which is the shared DB in this example) connected to the server.
        [user_store]

        // Specify "database" as the database type to infer the connection details of your shared DB.
        type = "database"
        ```
      Find more [parameters](../../../references/ei_config_catalog/#connecting-to-the-primary-data-store) to configure the primary database.
    * If you have set up a separate database, LDAP, or Active Directory user store, see the section under [changing the default user store](../../setup/deployment/deploying_wso2_ei.md#changing-the-default-user-store).

6. To change the credentials of the admin user of each node in the cluster, update the following parameters in the esb.toml file. In the below example, a plain text password is used. If required, you can [encrypt the password](../../setup/security/encrypting_plain_text.md).
    ``` java
    // The config section that groups the parameters defining the system administrator.
    [super_admin]

    // Specify the username of connecting to the server.
    username = "admin"

    // Specify the password for connecting to the server.
    password = "admin"

    // Set this property to 'true'. This ensures that the admin account is created in the user store.
    create_admin_account = true
    ```
    
    Find more [parameters](../../../references/ei_config_catalog/#configuring-the-system-administrator) to configure the system administrator.

7. If you have [separated the internal keystore](../../setup/deployment/deploying_wso2_ei.md#updating-keystores) of your product, update the `[keystore.internal]` section in the esb.toml file.
   
    See [Configuring Keystores](../../setup/security/configuring_keystores.md) for instructions.
    
## Optional configurations

See the topics given below.

### Changing the default user store
By default, the primary database that you configure will be the user store (that stores all user information) for the server. This will be a JDBC user store.

If required, you can configure another database, or an LDAP/Active Directory as your user store and connect it to the product server. Find out more about:

* [Setting up a JDBC user store](../../setup/user_stores/setting_up_jdbc_userstore.md)
* [Setting up a read-only LDAP](../../setup/user_stores/setting_up_ro_ldap.md)
* [Setting up a read-write LDAP](../../setup/user_stores/setting_up_rw_ldap.md)
* [Setting up a read-write Active Directory](../../setup/user_stores/setting_up_rw_ad.md)

### Configuring Deployment Synchronization

Configure deployment synchronization so that all the integration artifacts you develop can be shared between all the nodes in the cluster. You need to use the deployment synchronization mechanism that is suitable for your environment. See [Configuring Deployment Synchronization](../../setup/deployment_synchronization.md) for instructions.

### Configuring Analytics

If you have already done the configurations explained above, you have the option of applying the following configurations for your deployment.

If you wish to view reports, statistics, and graphs related to the message mediation that happens through the ESB, you need to configure the Analytics profile of WSO2 EI. Follow configuring WSO2 EI Analytics in a production setup.

## Verify your deployment

Ensure that you have taken into account the respective security hardening factors (e.g., changing and encrypting the default passwords, configuring JVM security etc.) before deploying WSO2 EI. For more information, see the [Production Deployment Checklist](../../setup/deployment/deployment_checklist.md).

## Starting the ESB servers

Start the server using the following standard start-up script.

* On **Linux/MacOS** : `cd <EI_HOME>/bin/ sh intergrator.sh`
* ON **Windows** : `cd <EI_HOME>\bin\wso2server.bat`
