# Local Transport

Apache Axis2's local transport implementation is used to make fast, in-VM (Virtual Machine) service calls and transfer data within proxy services. The transport does not have a receiver implementation.

!!! Info
    -   WS-Security cannot be used with the local transport. Since the local is mainly used to make calls within the same VM, WS-Security is generally not required in scenarios where it is used.
    -   If you want to make calls across tenants, you should use a non local transport even if they run from the same VM.
    -   If you need to use local transport with callout mediator, you do not need to perform configuration mentioned in this section as callout mediator requires blocking local transport which is configured by default in WSO2 Enterprise Integrator distribution.