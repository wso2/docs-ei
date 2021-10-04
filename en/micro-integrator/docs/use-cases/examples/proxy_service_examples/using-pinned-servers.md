# Using Pinned Servers

You can use the `pinnedServers` attribute to specify the list of Synapse servers where this proxy service should be deployed. If there is no pinned server list, the proxy service is started in all server instances. The `pinnedServers ` attribute takes the Synapse server names separated by commas or spaces. 

The Synapse server name is specified in the system property `SynapseServerName`. If this is not set then it defaults to localhost. You can give a name to a Synapse server instance by specifying the property `-DSynapseServerName=` when you execute the startup script `micro-integrator.bat` or `micro-integrator.sh`, or by editing `wrapper.conf` to do this where Synapse is started as a service.