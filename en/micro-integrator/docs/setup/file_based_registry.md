# Configuring the File-Based Registry

WSO2 Micro Integrator is shipped with a file-system-based registry. Thus, by default, the MI_HOME/registry/ directory will act as the registry to store registry artifacts etc. This main registry directory will consist of the following sub registry directories.

* **Local**: To store local artifacts of the product server that are not shared with the other products in the deployment.
* **Config**: To store all product-specific artifacts that are shared between similar product instances.
* **Governance**: To store all artifacts that re relevant to the governance of the product.
If you want to change the default locations of the registry directories, uncomment and change the following configuration in the MI_HOME/repository/deployment/server/synapse-config/default/directoryregistry.xml file.

```
<registry xmlns="http://ws.apache.org/ns/synapse" provider="org.wso2.carbon.mediation.registry.MicroIntegratorRegistry">
    <parameter name="cachableDuration">15000</parameter>
    <!--
        Uncomment below parameters (ConfigRegRoot, GovRegRoot, LocalRegRoot) to configure registry root paths
        Default : <EI_HOME>/wso2/micro-integrator/registry/{governance | config | local}
    -->
    <!--
    <parameter name="ConfigRegRoot">{Root directory path for configuration Registry}</parameter>
    <parameter name="GovRegRoot">{Root directory path for governance Registry}</parameter>
    <parameter name="LocalRegRoot">{Root directory path for local Registry}</parameter>
    -->
</registry>
```
