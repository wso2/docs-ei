# Using Inbound Endpoints

See the examples given below.

## Example 1: Specifying inbound endpoint parameters as registry values

Other than specifying parameter values inline, you can also
specify parameter values as registry entries. The advantage of
specifying a parameter value as a registry entry is that the same
inbound endpoint configuration can be used in different environments
simply by changing the registry entry value.

```
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="file" sequence="request" onError="fault" protocol="file" suspend="false">
       <parameters>
          ...............
          <parameter name="transport.vfs.FileURI" key="conf:/repository/ei/ei-configurations/test"/>
          ...............
       </parameters>
    </inboundEndpoint>
```

## Example 2: Using secure vault aliases in your inbound endpoint configuration

To use secure vault support in your inbound configuration, you can add
`         {wso2:vault-lookup('xx')}        ` as inbound parameters,
where `         xx        ` is the alias.

You can encrypt and store the password using the alias
`         my.password        ` , and retrieve this password  by adding
the following parameter in your inbound endpoint configuration:

`         <parameter name="transport.jms.Password">{wso2:vault-lookup('my.password')}</parameter>        `