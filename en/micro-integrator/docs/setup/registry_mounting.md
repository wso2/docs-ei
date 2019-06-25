
# Optional: Mounting a Registry

Add the following configurations to the
`         <EI_HOME>/conf/registry.xml        ` file of each ESB node to
configure the shared registry database and mounting details. This
ensures that the shared registry for governance and configurations
(i.e., the `         REGISTRY_DB        ` database) mount on both ESB
nodes.

## Before you begin

Note the following when adding these configurations:

-   The existing `          dbConfig         ` called
    `          wso2registry         ` **must not be removed** .
-   The datasource you specify in the
    `          <dbConfig name="sharedregistry">         ` tag must match
    the JNDI Config name you specify in the
    `          <EI_HOME>/conf/datasources/master-datasources.xml         `
    file.
-   The registry mount path denotes the type of registry. For example, ”
    `           /_system/config          ` ” refers to configuration
    Registry, and " `           /_system/governance          ` " refers
    to the governance registry.

-   The \<dbconfig\> entry enables you to identify the datasource you
    configured in the
    `           <EI_HOME>/conf/datasources/master-datasources.xml          `
    file. The unique name "s `           haredregistry          ` "
    refers to that datasource entry.

-   The `          <remoteInstance>         ` section refers to an
    external registry mount. Specify the read-only/read-write nature of
    this instance, caching configurations and the registry root location
    in this section.
-   Also, specify the cache ID in the
    `           <remoteInstance>          ` section. This enables
    caching to function properly in the clustered environment.

    > Cache ID is the same as the JDBC connection URL of the registry
    database. This value is the Cache ID of the remote instance. It
    should be in the format of
    `           $database_username@$database_url          ` , where
    `           $database_username          ` is the username of the
    remote instance database and `           $database_url          ` is
    the remote instance database URL. This cacheID denotes the enabled
    cache. In this case, the database it should connect to is
    `           REGISTRY_DB          ` , which is the database shared
    across all the nodes. You can find that in the mounting
    configurations of the same datasource that is being used.


-   Define a unique name in the `           <id>          ` tag for each
    remote instance. This is then referred to from mount configurations.
    In the above example, the unique ID for the remote instance is "
    `           instanceId          ` ".

-   Specify the actual mount path and target mount path in each of the
    mounting configurations. The target path can be any meaningful name.
    In this instance, it is " `           /_system/eiconfig          `
    ".

## Update the configurations

Open the esb.toml file and addd the following configuration section.

```java
param1=""
param2=""
```