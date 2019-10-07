# Using Pinned Servers
## Example use case

You can use the `         pinnedServers        ` attribute to specify
the list of Synapse servers where this proxy service should be
deployed. If there is no pinned server list, the proxy service is
started in all server instances. The `         pinnedServers        `
attribute takes the Synapse server names separated by commas or spaces.
The Synapse server name is specified in the system property
`         SynapseServerName        ` or through the
`         axis2.xml        ` parameter
`         SynapseConfig.ServerName        ` . If neither of
these are set, the server hostname is used, or it defaults to
`         localhost        ` . You can give a name to a Synapse server
instance by specifying the property
`         -DSynapseServerName=<ServerName>        ` when you execute the
startup script `         integrator.bat        ` or
`         integrator.sh        ` , or by editing
`         wrapper.conf        ` to do this where Synapse is started as a
service.

## Synapse configuration

## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project.
3. Create a proxy service artifact with the above configuration.
4. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

........

Invoking the service: