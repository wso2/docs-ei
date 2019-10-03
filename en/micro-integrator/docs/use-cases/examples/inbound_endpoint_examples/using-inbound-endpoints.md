# Specifying inbound endpoint parameters as registry values

Other than specifying parameter values inline, you can also
specify parameter values as registry entries. The advantage of
specifying a parameter value as a registry entry is that the same
inbound endpoint configuration can be used in different environments
simply by changing the registry entry value.

```
    <?xml version="1.0" encoding="UTF-8"?>
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="file" sequence="request" onError="fault" protocol="file" suspend="false">
       <parameters>
          ...............
          <parameter name="transport.vfs.FileURI" key="conf:/repository/ei/ei-configurations/test"/>
          ...............
       </parameters>
    </inboundEndpoint>
```
 
```
<?xml version="1.0" encoding="UTF-8"?>
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" 
                 name="file1" sequence="request" 
                 onError="fault" 
                 protocol="file" 
                 suspend="false">
   <parameters>
      <parameter name="interval">1000</parameter>
      <parameter name="sequential">true</parameter> 
      <parameter name="coordination">true</parameter> 
      <parameter name="transport.vfs.ActionAfterProcess">MOVE</parameter>
      <parameter name="transport.vfs.MoveAfterProcess" key="gov:/custom/out.txt"/>
      <parameter name="transport.vfs.FileURI" key="gov:/custom/in.txt"/>
      <parameter name="transport.vfs.MoveAfterFailure" key="gov:/custom/failed.txt"/>
      <parameter name="transport.vfs.FileNamePattern">.*.txt</parameter>
      <parameter name="transport.vfs.ContentType">text/plain</parameter>
      <parameter name="transport.vfs.ActionAfterFailure">MOVE</parameter>
   </parameters>
</inboundEndpoint>
```
