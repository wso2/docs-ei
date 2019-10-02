# Using the Address Endpoint
## Example use case

This sample demonstrates how you can convert a POX message to a SOAP request.

## Synapse configuration

The XML configuration for this sample is as follows:

```xml 
<?xml version="1.0" encoding="UTF-8"?>
<proxy name="SimpleStockQuoteProxy" startOnLoad="true" transports="http https" xmlns="http://ws.apache.org/ns/synapse">
    <target>
        <inSequence>
			<!-- filtering of messages with XPath and regex matches -->
			<filter source="get-property('To')" regex=".*/StockQuote.*">
				<header name="Action" value="urn:getQuote" />
				<send>
					<endpoint>
						<address
							uri="http://localhost:9000/services/SimpleStockQuoteService"
							format="soap11" />
					</endpoint>
				</send>
			</filter>
		</inSequence>
		<outSequence>
			<send />
		</outSequence>
    </target>
</proxy>
```

## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the following artifacts.
4. Deploy the artifacts in your Micro Integrator.

Configure the ActiveMQ broker.

Set up the back-end service.

Invoking the service.

The request sent by the client is as follows:

```bash
POST /services/SimpleStockQuoteProxy/StockQuote HTTP/1.1
Content-Type: application/xml; charset=UTF-8;action="urn:getQuote";
SOAPAction: urn:getQuote
User-Agent: Axis2
Host: 127.0.0.1
Transfer-Encoding: chunked

<m0:getQuote xmlns:m0="http://services.samples" xmlns:xsd="http://services.samples/xsd">
   <m0:request>
      <m0:symbol>IBM</m0:symbol>
   </m0:request>
</m0:getQuote>
```

It is a HTTP REST request, which will be transformed into a SOAP request and forwarded to the stock quote service.