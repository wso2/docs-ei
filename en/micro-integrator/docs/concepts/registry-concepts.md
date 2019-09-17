# Registry

WSO2 Micro Integrator uses a registry to store various configurations and artifacts such as [sequences](../../concepts/message-processing-units/#mediation-sequences) and [endpoints](message-exit-points.md). A registry is simply a content store and a metadata repository. Various SOA artifacts such as services, WSDLs, and configuration files can be stored in a registry and referred to by a key, which is a path similar to a UNIX file path. The WSO2 Micro Integrator uses two types of registries.

## File-based Registry

WSO2 Micro Integrator is shipped with a file-system-based registry. Thus, by default, the `MI_HOME/registry/` directory will act as the registry to store registry artifacts etc. This main registry directory will consist of the following sub registry directories.

* **Local**: To store local artifacts of the product server that are not shared with the other products in the deployment.
* **Config**: To store all product-specific artifacts that are shared between similar product instances.
* **Governance**: To store all artifacts that re relevant to the governance of the product.

## Local Registry

The **local registry** acts as a memory registry where you can store static content as a key-value pair, where the value could be a static entry such as a static text specified as **inline text** or static XML specified as an **inline XML** fragment, or a URL (using the `src` attribute). 

``` java tab='Inline text'
<localEntry key="version">0.1</localEntry>
```

``` java tab='Inline XML'
<localEntry key="validate_schema">
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" ...
    </xs:schema>
</localEntry>
```

``` java tab='Source URL'
<localEntry key="xslt-key-req" src="file:repository/samples/resources/transform/transform.xslt"/>
```

This is useful for the type of static content often found in XSLT files, WSDL files, URLs, etc. Local entries can be referenced from mediators in the Micro Integrator mediation flows and resolved at runtime. These entries are top-level entries and are globally visible within the entire system. Values of these entries can be retrieved via the extension XPath function `synapse:get-property(prop-name)`, and the keys of these entries could be specified wherever a registry key is expected within the configuration. A local entry shadows any entry with the same name from a remote Registry.
