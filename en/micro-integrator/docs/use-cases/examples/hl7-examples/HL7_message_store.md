# Storing HL7 Messages

You can use the HL7 message store to automatically store HL7 messages, allowing you to audit and replay messages as needed. The HL7 store is a custom message store implementation on top of Open JPA. 

## Synapse configuration

In this configuration, when the HL7 proxy service runs, an HL7 service will start listening on the port defined in the `transport.hl7.Port` service parameter. When an HL7 message arrives, the proxy will send an ACK back to the client as specified in the `HL7_RESULT_MODE` property. The <b>Clone</b> mediator is used inside the proxy to replicate the message for the <b>Send</b> and <b>Store</b> sequences. This ensures that the message is sent to the specified endpoint and is also stored in the message store (`HL7StoreJPA`).

!!! Info
    Note the following:
    
    - The `HL7StoreJPA` message store is a custom message store implementation of `org.wso2.micro.integrator.business.messaging.hl7.store.jpa.JPAStore`. 
    - It takes OpenJPA properties as parameters. 
    - In this example, the `openjpa.ConnectionProperties` and `openjpa.ConnectionDriverName` properties are used to create an Apache DBCP pooled connection to a MySQL database. 
    - You need to create the database specified in the connection properties and provide the database authentication details matching your database. 
    - Add the JDBC drivers for your database to the `<MI_HOME>/lib` folder. 

<!--
You can view the messages in this message store using the <b>HL7 Console UI</b>. You can search for messages on the unique message UUID or HL7 specific Control ID. The search field supports the wildcard ‘%’ to allow LIKE queries. The table can also be filtered to search for content within messages. Selected messages can be edited and injected into a proxy service. Reinjecting a message to the same service will result in a new message being stored with a different message UUID.
-->

See the instructions on how to [build and run](#build-and-run) this example.

```xml tab='Proxy Service'
<proxy name="HL7Store" startOnLoad="true" trace="disable" transports=”hl7”>
   <description/>
   <target>
      <inSequence>
         <property name="HL7_RESULT_MODE" value="ACK" scope="axis2"/>
         <log level="full"/>
         <property name="messageType" value="application/edi-hl7" scope="axis2"/>
         <clone>
            <target sequence="StoreSequence"/>
            <target sequence="SendSequence"/>
         </clone>
      </inSequence>
   </target>
   <parameter name="transport.hl7.AutoAck">false</parameter>
   <parameter name="transport.hl7.Port">55557</parameter>
   <parameter name="transport.hl7.ValidateMessage">false</parameter>
</proxy>
```

```xml tab='Store Sequence'
<sequence name="StoreSequence">
   <property name="OUT_ONLY" value="true"/>
   <store messageStore="HL7StoreJPA"/>
</sequence>
```

```xml tab='Send Sequence'
<sequence name="SendSequence">
    <call>
        <endpoint>
            <address uri="hl7://localhost:9988">
                <suspendOnFailure>
                    <initialDuration>-1</initialDuration>
                    <progressionFactor>1</progressionFactor>
                </suspendOnFailure>
                <markForSuspension>
                    <retriesBeforeSuspension>0</retriesBeforeSuspension>
                </markForSuspension>
            </address>
        </endpoint>
    </call>
    <log level="full"/>
    <drop/>
</sequence>
```

```xml tab='Message Store'
<messageStore class="org.wso2.micro.integrator.business.messaging.hl7.store.jpa.JPAStore"
                 name="HL7StoreJPA">
   <parameter name="openjpa.ConnectionDriverName">org.apache.commons.dbcp.BasicDataSource</parameter>
   <parameter name="openjpa.ConnectionProperties">DriverClassName=com.mysql.jdbc.Driver, Url=jdbc:mysql://localhost/hl7storejpa,  MaxActive=100,  MaxWait=10000,  TestOnBorrow=true,  Username=root,  Password=root</parameter>
   <parameter name="openjpa.jdbc.DBDictionary">blobTypeName=LONGBLOB</parameter>
</messageStore>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an integration project](../../../../develop/create-integration-project) with an <b>ESB Configs</b> module and an <b>Composite Exporter</b>.
3. Create the [proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service), [sequences](../../../../develop/creating-artifacts/creating-reusable-sequences), and [custom message store](../../../../develop/creating-artifacts/creating-a-message-store) with the configurations given above.
4. [Enable the HL7 transport](../../../../setup/transport_configurations/configuring-transports/#configuring-the-hl7-transport) in your Micro Integrator.
5. [Deploy the artifacts](../../../../develop/deploy-artifacts) in your Micro Integrator.

To test this scenario, you need an HL7 client and HL7 back-end application set up and running. 

!!! Info
    To see a sample that illustrates how to create an HL7 client and back-end 
    application, see [WSO2 Hl7 Samples](https://github.com/wso2/carbon-mediation/tree/v4.7.61/components/business-adaptors/hl7/org.wso2.carbon.business.messaging.hl7.samples/src/main/java/org/wso2/carbon/business/messaging/hl7/samples).