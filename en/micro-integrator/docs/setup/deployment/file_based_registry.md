# Configuring the File-Based Registry

WSO2 Micro Integrator is shipped with a file-system-based registry. Thus, by default, the `MI_HOME/registry/` directory will act as the registry to store registry artifacts etc. This main registry directory will consist of the following sub registry directories.

* **Local**
* **Config**
* **Governance**

The artifacts can be stored in any of the above directories. The directory structure is maintained to have backward compatibility with previous EI versions.

If you want to change the default locations of the registry directories, uncomment and change the following configuration in the `MI_HOME/repository/deployment/server/synapse-configs/default/registry.xml` file.

```xml
<registry xmlns="http://ws.apache.org/ns/synapse" provider="org.wso2.micro.integrator.registry.MicroIntegratorRegistry">
    <parameter name="cachableDuration">15000</parameter>
    <!--
        Uncomment below parameters (ConfigRegRoot, GovRegRoot, LocalRegRoot) to configure registry root paths
        Default : <MI_HOME>/registry/{governance | config | local}
        Example : <parameter name="GovRegRoot">file:///Users/JohnDoe/registry/governance</parameter>
    -->
    <!--
    <parameter name="ConfigRegRoot">{Root directory path for configuration Registry}</parameter>
    <parameter name="GovRegRoot">{Root directory path for governance Registry}</parameter>
    <parameter name="LocalRegRoot">{Root directory path for local Registry}</parameter>
    -->
</registry>
```
