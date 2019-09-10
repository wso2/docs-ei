# Sample 270: Transport switching from HTTP to MSMQ and MSMQ to HTTP

!!! warning

Note that WSO2 EI is shipped with the following changes to what is
mentioned in this documentation :

-   `           <PRODUCT_HOME>/          `
    `           repository/samples/          ` directory that includes
    all Integration profile samples is changed to
    `           <EI_HOME>/          `
    `           samples/service-bus/          ` .
    `                     `
-   `           <PRODUCT_HOME>/          `
    `           repository/samples/resources/          ` directory that
    includes all artifacts related to the Integration profile samples is
    changed to `           <EI_HOME>/          `
    `           samples/service-bus/resources/          ` .

TEST  

**Objective 1** : Demonstrate the capability of working MSMQ transport
messages.

**Objective 2:** Introduction to switching transports with proxy
services

**Prerequisites:**

!!! info

Make sure that the given MSMQ sample is ONLY working on windows
environment, since it invokes Microsoft C++ API for MSMQ via JNI
invocation.

TEST  

-   Start the Axis2 server and deploy the SimpleStockQuoteService (Refer
    steps above).

-   Download the
    `                       axis2-transport-msmq-2.0.0-wso2v2.jar                     `
    file and add it to the `           <EI_HOME>/dropins          `
    directory. This file provides the JNI invocation required by MSMQ
    bridging.

-   Please make sure MQ is installed and running. For more information
    please refer <http://msdn.microsoft.com/en-us/library/aa967729.aspx>
    .

-   Make sure that you have installed Visual C++ 2008 (VC9), it works
    with Microsoft Visual Studio 2008 Express.

For a default MSMQ v4.0 installation, you may place following in the
Axis2 transport sender/ listener configuration at
`         repository/conf/axis2/axis2.xml        ` as,

``` html/xml
<transportSender name="msmq"class="org.apache.axis2.transport.msmq.MSMQSender"/>
<transportReceiver name="msmq" class="org.apache.axis2.transport.msmq.MSMQListener">    
    <parameter name="msmq.receiver.host" locked="false">localhost</parameter>
</transportReceiver>
```

  

Synapse Configuration for MSMQ,

``` html/xml
<proxy xmlns="http://ws.apache.org/ns/synapse" name="msmqTest" transports="msmq" startOnLoad="true">
    <target>
        <inSequence>
            <property name="OUT_ONLY" value="true"/>
            <send>
                <endpoint>
                    <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence>
            <send/>
        </outSequence>
        <parameter name="transport.msmq.ContentType">application/xml</parameter>
    </target>
</proxy>
<proxy xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteProxy" transports="http" startOnLoad="true" trace="disable">
     <description/>
      <target>
        <endpoint>
          <address uri="msmq:DIRECT=OS:localhost\private$\msmqTest"/>
        </endpoint>
        <inSequence>
           <property name="FORCE_SC_ACCEPTED"  value="true"  scope="axis2"  type="STRING"/>
           <property name="OUT_ONLY" value="true" scope="default" type="STRING"/>
        </inSequence>
       <outSequence>
          <log level="custom">
              <property name="MESSAGE" value="OUT SEQENCE CALLED"/>
          </log>
           <send/>
         </outSequence>
      </target>
      <publishWSDL uri="http://localhost:9000/services/SimpleStockQuoteService?wsdl"/>
 </proxy>
```

Invoke the sample as follows,

``` java
ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy -Dmode=placeorder -Dsymbol=MSFT
```

  

The sample Axis2 server console will print a message indicating that it
has accepted the order as follows,

``` java
Accepted order for : 18406 stocks of MSFT at $ 83.58806051152119
```

Above samples works as follows,

-   Sending Place stockquote request to the ESB proxy.

<!-- -->

-   Proxy sends the incoming message to the MSMQ server.

!!! info

msmq:DIRECT=OS:localhost\\private$\\msmqTest

TEST  

-   Another proxy known as ' msmqTest' listening to the MSMQ queue,
    invoke the message from MSMQ and send to the Aix2 back-end server.

  
  
