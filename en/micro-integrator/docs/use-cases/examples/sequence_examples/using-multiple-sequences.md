# Using Multiple Sequences
## Example use case

This sample demonstrates how a complex sequence can be separated into a set of simpler sequences. In this sample, you will send a simple request to a back-end service (StockQuoteService microservice) and receive a response. If you look at the sample's XML configuration, you will see how this mediation is performed by two separate sequence definitions instead of one main sequence.

## Synapse configuration

Create the following proxy service:

```xml
<proxy name="SequenceBreakdownSampleProxy" startOnLoad="true" transports="http https">
      <description/>
      <target inSequence="StockQuoteSeq"/>
</proxy>
```

Create the following mediation sequences:

```xml tab='Sequence 1'
<sequence name="StockQuoteSeq" onError="StockQuoteErrorSeq">
    <log level="custom">
        <property name="Sequence" value="StockQuoteSeq"/>
        <property name="Description" value="Request recieved"/>
    </log>
    <sequence key="CallStockQuoteSeq"/>
    <sequence key="TransformAndRespondSeq"/>
</sequence>
```

```xml tab='Sequence 2'
<sequence name="StockQuoteErrorSeq">
    <log level="custom">
        <property name="Description" value="Error occurred in StockQuoteErrorSeq"/>
        <property name="Status" value="ERROR"/>
    </log>
</sequence>
```

```xml tab='Sequence 3'
<sequence name="TransformAndRespondSeq" onError="TransformAndRespondErrorSeq">
    <log level="custom">
        <property name="Sequence" value="TransformAndRespondSeq"/>
        <property name="Description" value="Response is ready to be transformed"/>
    </log>
    <payloadFactory media-type="xml">
        <format>
            <Information xmlns="">
                <Name>$1</Name>
                <Last>$2</Last>
                <High>$3</High>
                <Low>$4</Low>
            </Information>
        </format>
        <args>
            <arg evaluator="json" expression="$.name"/>
            <arg evaluator="json" expression="$.last"/>
            <arg evaluator="json" expression="$.low"/>
            <arg evaluator="json" expression="$.high"/>
        </args>
    </payloadFactory>
    <log level="custom">
        <property name="Sequence" value="TransformAndRespondSeq"/>
        <property name="Description" value="Responding back to the client with the transformed response"/>
    </log>
    <respond/>
</sequence>
```

```xml tab='Sequence 4'
<sequence name="TransformAndRespondErrorSeq">
    <log level="custom">
        <property name="Description" value="Error occurred in TransformAndRespondSeq"/>
        <property name="Status" value="ERROR"/>
    </log>
</sequence>
```

```xml tab='Sequence 5'
<sequence name="CallStockQuoteErrorSeq">
      <log level="custom">
          <property name="Description" value="Error occurred in CallStockQuoteSeq"/>
          <property name="Status" value="ERROR"/>
      </log>
</sequence>
```

```xml tab='Sequence 6'
<sequence name="CallStockQuoteSeq" onError="CallStockQuoteErrorSeq">
    <switch source="//symbol" xmlns:ns="http://org.apache.synapse/xsd">
        <case regex="IBM">
            <log level="custom">
                <property name="Sequence" value="CallStockQuoteSeq"/>
                <property name="Description" value="Calling IBM endpoint"/>
            </log>
            <call blocking="true">
                <endpoint>
                    <http method="GET" uri-template="http://localhost:9090/stockquote/IBM"/>
                </endpoint>
            </call>
            <log level="custom">
                <property name="Sequence" value="CallStockQuoteSeq"/>
                <property name="Description" value="Response received from IBM endpoint"/>
            </log>
        </case>
        <case regex="GOOG">
            <log level="custom">
                <property name="Sequence" value="CallStockQuoteSeq"/>
                <property name="Description" value="Calling GOOG endpoint"/>
            </log>
            <call blocking="true">
                <endpoint>
                    <http method="GET" uri-template="http://localhost:9090/stockquote/GOOG"/>
                </endpoint>
            </call>
            <log level="custom">
                <property name="Sequence" value="CallStockQuoteSeq"/>
                <property name="Description" value="Response received from GOOG endpoint"/>
            </log>
        </case>
        <case regex="AMZN">
            <log level="custom">
                <property name="Sequence" value="CallStockQuoteSeq"/>
                <property name="Description" value="Calling AMZN endpoint"/>
            </log>
            <call blocking="true">
                <endpoint>
                    <http method="GET" uri-template="http://localhost:9090/stockquote/AMZN"/>
                </endpoint>
            </call>
            <log level="custom">
                <property name="Sequence" value="CallStockQuoteSeq"/>
                <property name="Description" value="Response received from AMZN endpoint"/>
            </log>
        </case>
        <default>
            <log level="custom">
                <property name="Sequence" value="CallStockQuoteSeq"/>
                <property name="Description" value="Invalid Symbol"/>
                <property name="Status" value="ERROR"/>
            </log>
            <drop/>
        </default>
    </switch>
</sequence>
```

## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the mediation artifacts with the above configuration.
4. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

........

Send a request to invoke the service: