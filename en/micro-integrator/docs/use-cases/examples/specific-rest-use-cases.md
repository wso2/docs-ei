# Configuring Specific Use Cases

This page describes how to configure APIs for the following use cases:

-   [Setting query parameters on outgoing
    messages](#ConfiguringSpecificUseCases-Settingqueryparametersonoutgoingmessages)
-   [Content
    negotiation](#ConfiguringSpecificUseCases-Contentnegotiation)
-   [Transforming the content
    type](#ConfiguringSpecificUseCases-Transformingthecontenttype)
    -   [Setting up the back
        end](#ConfiguringSpecificUseCases-Settingupthebackend)
    -   [Configuring the
        API](#ConfiguringSpecificUseCases-ConfiguringtheAPI)
    -   [Executing the
        sample](#ConfiguringSpecificUseCases-Executingthesample)
-   [Enabling REST to
    SOAP](#ConfiguringSpecificUseCases-EnablingRESTtoSOAP)
    -   [Setting up the back
        end](#ConfiguringSpecificUseCases-Settingupthebackend.1)
    -   [Configuring the
        API](#ConfiguringSpecificUseCases-ConfiguringtheAPI.1)
    -   [Executing the
        sample](#ConfiguringSpecificUseCases-Executingthesample.1)
    -   [Setting HTTP status
        codes](#ConfiguringSpecificUseCases-SettingHTTPstatuscodes)
-   [Enabling REST to
    JMS](#ConfiguringSpecificUseCases-EnablingRESTtoJMS)
    -   [Create a queue in the message
        broker](#ConfiguringSpecificUseCases-Createaqueueinthemessagebroker)
    -   [Configuring the
        API](#ConfiguringSpecificUseCases-ConfiguringtheAPI.2)
    -   [Executing the
        sample](#ConfiguringSpecificUseCases-Executingthesample.2)
-   [Exposing a back-end REST service using a different
    API](#ConfiguringSpecificUseCases-Exposingaback-endRESTserviceusingadifferentAPI)
    -   [Setting up the back
        end](#ConfiguringSpecificUseCases-Settingupthebackend.2)
    -   [Configuring the
        API](#ConfiguringSpecificUseCases-ConfiguringtheAPI.3)
    -   [Executing the
        sample](#ConfiguringSpecificUseCases-Executingthesample.3)
-   [Handling non-matching
    resources](#ConfiguringSpecificUseCases-Handlingnon-matchingresources)
    -   [Setting up the back
        end](#ConfiguringSpecificUseCases-Settingupthebackend.3)
    -   [Configuring the
        API](#ConfiguringSpecificUseCases-ConfiguringtheAPI.4)
    -   [Creating the
        sequence](#ConfiguringSpecificUseCases-Creatingthesequence)
    -   [Executing the
        sample](#ConfiguringSpecificUseCases-Executingthesample.4)
-   [Retrieving metadata associated with the state of the resource using
    the HEAD request
    method](#ConfiguringSpecificUseCases-RetrievingmetadataassociatedwiththestateoftheresourceusingtheHEADrequestmethod)
    -   [Setting up the
        backend](#ConfiguringSpecificUseCases-Settingupthebackend.4)
    -   [Configuring the
        API](#ConfiguringSpecificUseCases-ConfiguringtheAPI.5)
    -   [Executing the
        sample](#ConfiguringSpecificUseCases-Executingthesample.5)
-   [Sending form data](#ConfiguringSpecificUseCases-Sendingformdata)
    -   [Setting up the
        backend](#ConfiguringSpecificUseCases-Settingupthebackend.5)
    -   [Configuring the
        API](#ConfiguringSpecificUseCases-ConfiguringtheAPI.6)
    -   [Executing the
        sample](#ConfiguringSpecificUseCases-Executingthesample.6)
-   [Uncommon Scenarios for HTTP Methods in
    REST](#ConfiguringSpecificUseCases-UncommonScenariosforHTTPMethodsinREST)
    -   [Using POST with No
        Body](#ConfiguringSpecificUseCases-UsingPOSTwithNoBody)
    -   [Using POST with Query
        Parameters](#ConfiguringSpecificUseCases-UsingPOSTwithQueryParameters)
    -   [Using GET with a
        Body](#ConfiguringSpecificUseCases-UsingGETwithaBody)
    -   [Using DELETE with a
        Body](#ConfiguringSpecificUseCases-UsingDELETEwithaBody)
-   [Configuring non-HTTP
    endpoints](#ConfiguringSpecificUseCases-Configuringnon-HTTPendpoints)

### Setting query parameters on outgoing messages

REST clients use query parameters to provide inputs for the relevant
operation. These query parameters may be required to carry out the
back-end operations either in a REST service or a proxy service. Let’s
take a sample request that is sent to the
[SimpleStockQuoteService](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples)
, and see how these parameters can be set in the outgoing message.

``` plain
    curl -v -X GET "http://localhost:8280/stockquote/view/IBM?param1=value1&param2=value2"
```

In this request, there are two query parameters (customer name and ID)
that must be set in the outgoing message from the ESB profile. We can
configure the API to set those parameters as follows:

``` xml
    <api xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteAPI" context="/stockquote">
       <resource methods="GET" uri-template="/view/{symbol}">
          <inSequence>
             <payloadFactory media-type="xml">
                <format>
                   <m0:getQuote xmlns:m0="http://services.samples">
                      <m0:request>
                         <m0:symbol>$1</m0:symbol>
                         <m0:customerName>$2</m0:customerName>
                         <m0:customerId>$3</m0:customerId>
                      </m0:request>
                   </m0:getQuote>
                </format>
                <args>
                   <arg evaluator="xml" expression="get-property('uri.var.symbol')"/>
                   <arg evaluator="xml" expression="get-property('query.param.param1')"/>
                   <arg evaluator="xml" expression="get-property('query.param.param2')"/>
                </args>
             </payloadFactory>
             <send>
                <endpoint>
                   <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="soap11"/>
                   <property name="SOAPAction" value="getQuote" scope="transport"/>
                </endpoint>
             </send>
          </inSequence>
          <outSequence>
             <send/>
          </outSequence>
       </resource>
    </api>                  
```

  
The query parameter values can be accessed through the "get-property"
function by specifying the parameter number as highlighted in the above
request.

### Content negotiation

Content negotiation is a mechanism by which a client and a server
communicate with each other and decide on a content format to use for
data transfer. In our samples so far, we used XML (application/xml) as
the primary means of data transfer. However, we could use other content
formats such as plain text and JSON to achieve the same result.

Real-world client applications usually have preferred content types. For
example:

-   A web browser usually prefers HTML.

-   A Java-based desktop application usually prefers plain-old XML
    (POX).

-   A mobile application usually prefers JSON.

To maintain interoperability, the server-side applications should be
prepared to serve content using any of these formats. Using content
negotiation, the client can indicate its content type preferences to the
server, and the server can serve the requests using a content type
preferred by the client.

The HTTP specification provides the basic elements to build a powerful
content negotiation framework. The HTTP client can indicate its content
type preferences by sending the Accept header along with the requests.
First, we need to retrieve the value of the Accept header sent by the
client. We do this in the in-sequence using the `         $trp        `
XPath variable, as follows:


Now in the out-sequence we can run a few comparisons to see which
content type to use for sending the response. We use the
`         $ctx        ` XPath variable to specify the

``` html/xml
        <case regex=".*atom.*">
                <send/>
        </case>
        <case regex=".*text/html.*">
                 <send/>
        </case>
        <case regex=".*json.*">
                 <send/>
        </case>
        <case regex=".*application/xml.*">
                 <send/>
        </case>
        <default>
                 <send/>
        </default>
</switch>
```
    
    The above configuration results in the following priority order of
    content types.
    
    1.  Atom (Also used as default)
    
    2.  HTML
    
    3.  JSON
    
    4.  POX
    
    To try this out, invoke the following Curl commands and see how the
    response format changes according to the value of the Accept header.
    
        curl -v http://localhost:8280/orders
    
        curl -v -H "Accept: application/xml" http://localhost:8280/orders
    
        curl -v -H "Accept: application/json" http://localhost:8280/orders
    
        curl -v -H "Accept: text/html" http://localhost:8280/orders
    
    ### Transforming the content type
    
    This section describes how you can transform the content type of a
    message using an API. In this scenario, the API exposes a REST back-end
    service that accepts and returns XML and JSON messages for HTTP methods
    as follows:
    
    -   GET - response is in JSON format
    
    -   POST - accepts XML request and returns response in JSON format
    
    -   PUT - accepts JSON request and returns response in JSON format
    
    -   DELETE - empty request body should be sent
    
    Transformation in HTTP GET:
    
    ![](attachments/119130400/119130405.png)  
    Transformation in HTTP POST:
    
    ![](attachments/119130400/119130406.png)
    
    Transformation in HTTP PUT:
    
    ![](attachments/119130400/119130403.png)
    
    #### Setting up the back end
    
    To deploy the REST back-end, download the StarbucksService.war from
    <https://svn.wso2.org/repos/wso2/people/charitham/REST-API/> and follow
    the steps below to deploy it in the ESB profile:
    
    1.  Log in to WSO2 EI management consoles at
        <https://localhost:8243/carbon/> using admin as the user name and
        password.
    2.  Navigate to Applications -\> Add -\> Web Applications and upload the
        StarbucksService.war file you downloaded.
    
    #### Configuring the API
    
    Create an API using the following configuration:
    
``` html/xml
<api xmlns="http://ws.apache.org/ns/synapse" name="StarbucksService" context="/Starbucks_Service">
    <resource methods="POST" url-mapping="/orders/add">
        <inSequence>
            <property name="REST_URL_POSTFIX" scope="axis2" action="remove"/>
            <send>
                <endpoint>
                    <address uri="http://localhost:8280/StarbucksService/services/Starbucks_Outlet_Service/orders/"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence>
            <log level="full"/>
            <property name="messageType" value="application/xml" scope="axis2"/>
            <send/>
        </outSequence>
    </resource>
    <resource methods="PUT" url-mapping="/orders/edit">
        <inSequence>
            <log level="full"/>
            <property name="REST_URL_POSTFIX" scope="axis2" action="remove"/>
            <property name="messageType" value="application/json" scope="axis2"/>
            <property name="ContentType" value="application/json" scope="axis2"/>
            <send>
                <endpoint>
                    <address uri="http://localhost:8280/StarbucksService/services/Starbucks_Outlet_Service/orders/" format="rest"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence>
            <log level="full"/>
            <property name="messageType" value="application/xml" scope="axis2"/>
            <send/>
        </outSequence>
    </resource>
    <resource methods="DELETE GET" uri-template="/orders/{id}">
        <inSequence>
            <send>
                <endpoint>
                    <address uri="http://localhost:8280/StarbucksService/services/Starbucks_Outlet_Service/"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence>
            <log level="full"/>
            <property name="messageType" value="application/xml" scope="axis2"/>
            <send/>
        </outSequence>
    </resource>
</api>
```
    
    #### Executing the sample
    
    The context of the API is ‘/Starbucks\_Service’. For every HTTP method,
    a url-mapping or uri-template is defined, and the URL to call the
    methods differ with the defined mapping or template.
    
    Following is the CURL command to send a GET request to the API:
    
    `         curl -v -X GET                              http://localhost:8280/Starbucks_Service/orders/123                           `
    
    The response from the back end to the ESB profile will be:
    
    `         {"Order":{"additions":"Milk","drinkName":"Vanilla Flavored Coffee","locked":false,"orderId":123}}        `
    
    The ESB profile transforms this response to XML and send it back as:
    
``` html/xml
<Order>
    <additions>Milk</additions>
    <drinkName>Vanilla Flavored Coffee</drinkName>
    <locked>false</locked>
    <orderId>123</orderId>
</Order>
```
    
    Following is the cURL command to send an HTTP POST request to the API:
    
    `         curl -v -H "Content-Type: application/xml" -X POST -d @placeOrder.xml                              http://localhost:8280/Starbucks_Service/orders/add                           `
    
    where `         placeOrder.xml        ` has the following content on the
    order:
    
``` html/xml
<Order>
    <drinkName>Mocha Flavored Coffee</drinkName>
    <additions>Caramel</additions>
</Order>
```
    
    This XML request will be sent to the back end, which will send back a
    JSON response to the ESB profile:
    
    `         {"Order":{"additions":"Caramel","drinkName":"Mocha Flavored Coffee","locked":false,"orderId":"d088a289-1be3-453b-ab37-7609828d2197"}}        `
    
    The ESB profile converts this response to XML and sends it back to the
    client:
    
``` html/xml
<Order>
    <additions>Caramel</additions>
    <drinkName>Mocha Flavored Coffee</drinkName>
    <locked>false</locked>
    <orderId>d088a289-1be3-453b-ab37-7609828d2197</orderId>
</Order>
```
    
    Following is the cURL command for sending an HTTP PUT request:
    
    `         curl -v -H "Content-Type: application/xml" -X PUT -d @editOrder.xml                              http://localhost:8280/Starbucks_Service/orders/edit                           `
    
    where `         editOrder.xml        ` has the following syntax:
    
``` html/xml
<Order>
    <orderId>d088a289-1be3-453b-ab37-7609828d2197</orderId>
    <additions>Chocolate Chip Cookies</additions>
</Order>
```
    
    The ESB profile will convert this request to JSON and send it to the
    back end. The response will be in JSON format:
    
    `         {"Order":{"additions":"Chocolate Chip Cookies","drinkName":"Mocha Flavored Coffee","locked":false,"orderId":        `
    "d088a289-1be3-453b-ab37-7609828d2197"}}
    
    The ESB profile converts this response to XML and sends it back to the
    client:
    
``` html/xml
<Order>
    <additions>Chocolate Chip Cookies</additions>
    <drinkName>Mocha Flavored Coffee</drinkName>
    <locked>false</locked>
    <orderId>d088a289-1be3-453b-ab37-7609828d2197</orderId>
</Order>
```
    
    Following is the cURL command for sending an HTTP DELETE request:
    
    `         curl -v -X DELETE                              http://localhost:8280/Starbucks_Service/orders/d088a289-1be3-453b-ab37-7609828d2197                           `
    
    This request will be sent to the back end, and the order with the
    specified ID will be deleted.
    
    ### Enabling REST to SOAP
    
    In this scenario, you expose a SOAP service over REST using an API in
    the ESB profile.
    
    ![](attachments/119130400/119130402.png)
    
    #### Setting up the back end
    
    For the SOAP back-end service, we are using the StockQuote Service that
    is shipped with the ESB profile. Configure and start the back-end
    service as described in [Starting Sample Back-End
    Services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples)
    . We will use [cURL](http://curl.haxx.se/) as the REST client to invoke
    the API in the ESB profile.
    
    #### Configuring the API
    
    Following is a sample API configuration that we can used to implement
    this scenario.
    
``` html/xml
<api xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteAPI" context="/stockquote">
   <resource uri-template="/view/{symbol}" methods="GET">
      <inSequence>
        <payloadFactory>
          <format>
             <m0:getQuote xmlns:m0="http://services.samples">
                <m0:request>
                   <m0:symbol>$1</m0:symbol>
                </m0:request>
             </m0:getQuote>
           </format>
           <args>
            <arg expression="get-property('uri.var.symbol')"/>
           </args>
        </payloadFactory>
        <header name="Action" value="urn:getQuote"/>
        <send>
          <endpoint>
            <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="soap11"/>
          </endpoint>
        </send>
      </inSequence>
      <outSequence>
        <send/>
      </outSequence>
   </resource>
   <resource url-mapping="/order/*" methods="POST">
      <inSequence>
        <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
        <property name="OUT_ONLY" value="true"/>
        <header name="Action" value="urn:placeOrder"/>
        <send>
            <endpoint>
                <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="soap11"/>
            </endpoint>
         </send>
      </inSequence>      
   </resource>
</api>
```
    
    #### Executing the sample
    
    In this API configuration we have defined two resources. One is for the
    HTTP method GET and the other one is for POST.
    
    In the first resource, we have defined the uri-template as
    /view/{symbol} so that request will be dispatched to this resource when
    you invoke the API using the following URI:
    
    `                   http://127.0.0.1:8280/stockquote/view/IBM                 `
    
    Following is the cURL command to send this URI:
    
    `         curl -v                              http://127.0.0.1:8280/stockquote/view/IBM                           `
    
    The context of this API is `         stockquote        ` .
    
    The SOAP payload required for the SOAP back-end service is constructed
    using the payload factory mediator defined in the inSequence. The value
    for the \<m0:symbol\> element is extracted using the following
    expression:
    
    `         get-property('uri.var.symbol')        `
    
    Here ‘symbol’ refers to the variable we defined in the uri-template
    (/view/{symbol}). Therefore, for the above invocation, the
    'uri.var.symbol' property will resolve to the value ‘IBM’.
    
    After constructing the SOAP payload, the request will be sent to the
    SOAP back-end service from the \<send\> mediator, which has an address
    endpoint defined inline with the format="soap11" attribute in the
    address element. The response received from the back-end soap service
    will be sent to the client in plain old XML (POX) format.
    
    In the second resource, we have defined the URL mapping as "/order/\*".
    Since this has POST as the HTTP method, the client has to send a payload
    to invoke this. Following is a sample cURL command to invoke this:
    
    `         curl -v -d @placeorder.xml -H "Content-type: application/xml" http://127.0.0.1:8280/stockquote/order/        `
    
    Save the following sample place order request as placeorder.xml in your
    local file system and execute the command. This payload is used to
    invoke a SOAP service.
    
``` html/xml
<placeOrder xmlns="http://services.samples">
  <order>
     <price>50</price>
     <quantity>10</quantity>
     <symbol>IBM</symbol>
  </order>
</placeOrder>
```
    
    You will see something similar to following line printed in the sample
    axis2 server (back-end server) console.
    
    `         Tue Mar 19 09:30:33 IST 2013 samples.services.SimpleStockQuoteService  :: Accepted order #1 for : 10 stocks of IBM at $ 50.0        `
    
    This SOAP service invocation is an OUT\_ONLY invocation, so the ESB
    profile is not expecting any response back from the SOAP service. Since
    we have set the FORCE\_SC\_ACCEPTED property value to true, the ESB
    profile returns a 202 response back to the client.
    
    #### Setting HTTP status codes
    
    A REST service typically sends HTTP status codes with its response. When
    you configure an API that send messages to a SOAP back-end service, you
    can set the status code of the HTTP response within the ESB
    configuration. To achieve this, set the status code parameter within the
    out sequence of the API definition:  
    
    `         <definitions>        `
    
    `         <api xmlns="                   http://ws.apache.org/ns/synapse                  " name="StockQuoteAPI" context="/stockquote">        `
    
    `         ...        `
    
    `                   <outSequence>                 `
    
    **`          <property name="HTTP_SC" value="201" scope="axis2" />         `**
    
    **`          </outSequence>         `**
    
    `         </resource>        `
    
         </api>
    
        </definitions>
    
    Now you can try the REST call with the following command.  
    
        curl -v http://127.0.0.1:8280/stockquote/view/IBM
    
    The response message will contain the following response code (201) as
    configured above.  
    
        < HTTP/1.1 201 Created
    
        < Content-Type: text/xml; charset=UTF-8
    
        < Server: Synapse-HttpComponents-NIO
    
        < Date: Thu, 21 Mar 2013 09:03:00 GMT
    
        < Transfer-Encoding: chunked
    
    ### Enabling REST to JMS
    
    This section describes how an API can receive an HTTP message sent from
    a REST client and place it on a JMS queue.
    
      
    ![](https://docs.google.com/a/wso2.com/drawings/d/svKnhPUX3GnCiQ1N-YQlaRQ/image?w=635&h=283&rev=74&ac=1)  
    
    The API receives the HTTP message and sends it to a JMS queue that
    resides in a message broker.
    
    #### Create a queue in the message broker
    
    Create a queue by the name of SimpleStockQuoteService in the MB profile.
    
    #### Configuring the API
    
    Following is a sample API configuration in the ESB profile that we can
    used to implement this scenario:
    
``` html/xml
<api xmlns="http://ws.apache.org/ns/synapse" name="RestToJmsAPI" context="/jmsstockquote">
   <resource methods="POST" url-mapping="/order/*">
      <inSequence>
         <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
         <property name="OUT_ONLY" value="true"/>
         <send>
            <endpoint>
               <address uri="jms:/SimpleStockQuoteService?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.wso2.andes.jndi.PropertiesFileInitialContextFactory&amp;java.naming.provider.url=conf/jndi.properties&amp;transport.jms.DestinationType=queue"/>
            </endpoint>
         </send>
      </inSequence>
   </resource>
</api>
```
    
    In this API configuration, we have set the OUT\_ONLY property to true,
    as it is a one-way invocation. The message is sent to the JMS queue
    using the JMS Transport sender. (Note the prefix “jms” in the address
    endpoint URI.) Since we have set the FORCE\_SC\_ACCEPTED property value
    to true ,the ESB profile returns a 202 response back to the client.
    
    #### Executing the sample
    
    Following is a sample cURL command to invoke this API:
    
    `         curl -v -d @placeorder.xml -H "Content-type: application/xml" http://127.0.0.1:8280/jmsstockquote/order/        `
    
    Save the following sample place order request as "placeorder.xml" in
    your local file system and execute the above command.
    
``` html/xml
<placeOrder xmlns="http://services.samples">
  <order>
     <price>50</price>
     <quantity>10</quantity>
     <symbol>IBM</symbol>
  </order>
</placeOrder>
```
    
    If you go to the management console of the MB profile and inspect the
    SimpleStockQuoteService queue, you will see a message queued there.
    
    ### Exposing a back-end REST service using a different API
    
    In this scenario, we are exposing a back-end REST service using a REST
    API in the ESB profile. That means we are exposing a different API to
    the client for a REST back-end service.
    
    ![](attachments/119130400/119130401.png)
    
    #### Setting up the back end
    
    For the REST back-end service, we are going to use the [JAX-RS
    Basics](https://docs.wso2.com/display/EI650/JAX-RS+Basics) sample
    service available in the ESB profile. Configure and deploy the JAX-RS
    Basics sample by following the instructions for b uilding and running
    the sample on the [JAX-RS
    Basics](https://docs.wso2.com/display/EI650/JAX-RS+Basics) page.
    
    #### Configuring the API
    
    Following is a sample configuration we can use to configure this
    scenario:
    
``` html/xml
<api xmlns="http://ws.apache.org/ns/synapse" name="ClientServiceAPI" context="/clientservice">
   <resource methods="OPTIONS DELETE GET" uri-template="/clients/{id}">
      <inSequence>

         <property name="REST_URL_POSTFIX" expression="fn:concat('/customers/',get-property('uri.var.id'))" scope="axis2"/>
         <send>
            <endpoint>
               <address uri="http://localhost:9763/jaxrs_basic/services/customers/customerservice/"/>
            </endpoint>
         </send>
      </inSequence>
      <outSequence>
         <send/>
      </outSequence>
   </resource>
   <resource methods="POST PUT" uri-template="/clients">
      <inSequence>
         <property name="REST_URL_POSTFIX" value="/customers" scope="axis2"/>
         <switch source="$ctx:REST_METHOD">
            <case regex="POST">
               <payloadFactory>
                  <format>
                     <Customer xmlns="">
                        <name>$1</name>
                     </Customer>
                  </format>
                  <args>
                     <arg expression="//addClient/name"/>
                  </args>
               </payloadFactory>
            </case>
            <case regex="PUT">
               <payloadFactory>
                  <format>
                     <Customer xmlns="">
                        <id>$1</id>
                        <name>$2</name>
                     </Customer>
                  </format>
                  <args>
                     <arg expression="//updateClient/id"/>
                     <arg expression="//updateClient/name"/>
                  </args>
               </payloadFactory>
            </case>
         </switch>
         <send>
            <endpoint>
               <address uri="http://localhost:9763/jaxrs_basic/services/customers/customerservice/"/>
            </endpoint>
         </send>
      </inSequence>
      <outSequence>
         <send/>
      </outSequence>
   </resource>
</api>
```
    
    The service URL for the back-end REST service is:
    
    `         http://localhost:8280/jaxrs_basic/services/customers/customerservice/        `
    
    The service URL for the REST API (ClientServiceAPI) is:
    
    `         http://localhost:8280/clientservice/        `
    
    As you can see, we have exposed the back-end REST service in a different
    context using this API. In addition to altering the context, the API is
    accepting the payload in a different format.
    
    For the HTTP POST method, the back-end REST service accepts the payload
    in the following format:
    
``` html/xml
<Customer>
   <name>Jack</name>
</Customer>
```
    
    The ClientServiceAPI accepts the payload in the following format:
    
``` html/xml
<addClient>
   <name>Jack</name>
</addClient>
```
    
    For the HTTP PUT method, the back-end REST service accepts the payload
    in the following format:
    
``` html/xml
<Customer>
   <id>123</id>
   <name>John</name>
</Customer>
```
    
    The ClientServiceAPI accepts the payload in the following format:
    
``` html/xml
<updateClient>
   <id>123</id>
   <name>John</name>
</updateClient>
```
    
    Conversion of the payload format is done using the \<payloadFactory\>
    mediator.
    
    We have now exposed a different REST API for the REST back-end services
    using a REST API in the ESB profile. In the ClientServiceAPI, there are
    two resources. The first resource is for the GET, OPTIONS, and DELETE
    HTTP methods. URL postfix is needed for the REST back-end service and is
    calculated using the following expression:
    
    `         fn:concat('/customers/',get-property('uri.var.id'))        `
    
    The calculated URL postfix is appended to the back-end service URL, as
    we have set that value to the REST\_URL\_POSTFIX property. Therefore,
    the final request URL will be something similar to the following:
    
    `                              http://localhost:8280/jaxrs_basic/services/customers/customerservice/customers/123                           `
    
    #### Executing the sample
    
    Following is the cURL command to send a GET request to the API:
    
    `         curl -v                   http://localhost:8280/clientservice/clients/123                 `
    
    You will see the following line printed as the response:
    
    `         <?xml version="1.0" encoding="UTF-8" standalone="yes"?><Customer><id>123</id><name>John</name></Customer>        `
    
    The second resource is for the POST and PUT HTTP methods. The incoming
    REST request is filtered using the HTTP\_METHOD, and the appropriate
    payload required for the back-end REST service is constructed using the
    \<payloadFactory\> mediator.
    
    Following is the cURL command to send a POST request to the API:
    
    `         curl -v -d @addClient.xml -H "Content-type: text/xml"                   http://localhost:8280/clientservice/clients                 `
    
    Content of the addClient.xml file should be in the following format:
    
``` html/xml
<addClient>
   <name>WSO2</name>
</addClient>
```
    
    When you execute the above command in the application server console,
    the following line will be printed:
    
    `         ----invoking addCustomer, Customer name is: WSO2        `
    
    Following is the cURL command to send a PUT request to the API:
    
    `         curl -v -d @updateClient.xml -X PUT -H "Content-type: text/xml"                              http://localhost:8280/clientservice/clients                           `
    
    The content of the updateClient.xml file should be in the following
    format:
    
``` html/xml
<updateClient>
    <id>123</id>
    <name>EI</name>        
</updateClient>
```
    
    When you execute the above command in the application server console,
    the following line will be printed:
    
    `         ----invoking updateCustomer, Customer name is: EI        `
    
    ### Handling non-matching resources
    
    In this scenario, we are defining a sequence to be invoked if the the
    ESB profile is unable to find a matching resource definition for a
    specific API invocation. This sequence generates a response indicating
    an error when no matching resource definition is found.
    
    #### Setting up the back end
    
    For the back-end service, we are using the StockQuote Service that is
    shipped with the ESB profile. Configure and start the back-end service
    as described in [Starting Sample Back-End
    Services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples)
    . We will use [cURL](http://curl.haxx.se/) as the REST client to invoke
    the the ESB profile API.
    
    #### Configuring the API
    
    Create an API using the following configuration:
    
``` xml
<api xmlns="http://ws.apache.org/ns/synapse" name="jaxrs" context="/jaxrs">
   <resource methods="GET" uri-template="/customers/{id}">
      <inSequence>
         <send>
            <endpoint>
               <address uri="http://localhost:8280/jaxrs_basic/services/customers/customerservice"/>
            </endpoint>
         </send>
      </inSequence>
      <outSequence>
         <send/>
      </outSequence>
   </resource>
</api> 
```
    
    #### Creating the sequence
    
    Create a new sequence with the following configuration:
    
``` xml
 <sequence xmlns="http://ws.apache.org/ns/synapse" name="_resource_mismatch_handler_">
   <payloadFactory>
      <format>
         <tp:fault xmlns:tp="http://test.com">
            <tp:code>404</tp:code>
            <tp:type>Status report</tp:type>
            <tp:message>Not Found</tp:message>
            <tp:description>The requested resource (/$1) is not available.</tp:description>
         </tp:fault>
      </format>
      <args>
         <arg xmlns:ns="http://org.apache.synapse/xsd" expression="$axis2:REST_URL_POSTFIX"/>
      </args>
   </payloadFactory>
   <property name="RESPONSE" value="true" scope="default"/>
   <property name="NO_ENTITY_BODY" action="remove" scope="axis2"/>
   <property name="HTTP_SC" value="404" scope="axis2"/>
   <header name="To" action="remove"/>
   <send/>
   <drop/>
</sequence>
```
    
    #### Executing the sample
    
    Send an invalid request to the back end as follows:
    
``` xml
curl -X GET http://localhost:8280/jaxrs/customers-wrong/123
```
    
    You will get the following response:
    
``` xml
<tp:fault xmlns:tp="http://test.com">
<tp:code>404</tp:code>
<tp:type>Status report</tp:type>
<tp:message>Not Found</tp:message>
<tp:description>The requested resource (//customers-wrong/123) is not available.</tp:description>
</tp:fault>
```
    
    ### Retrieving metadata associated with the state of the resource using the HEAD request method
    
    In this scenario, we are using the http request method HEAD to retrieve
    the metadata associated with the state of a resource. This is useful in
    situations where you want to check the content length of the response,
    last modified date etc. When the HEAD request method is used, the ESB
    profile accepts the request and sends it back with only the headers and
    no body.
    
    #### Setting up the backend
    
    For the back-end service, we are using the StockQuote Service that is
    shipped with the ESB profile. [cURL](http://curl.haxx.se/) is used as
    the REST client to invoke the ESB profile API. Run [Sample
    800](https://docs.wso2.com/display/EI6xx/Sample+800%3A+Introduction+to+REST+API)
    as follows.
    
    For further information on how to run an ESB profile sample in WSO2 EI,
    see [Setting Up the ESB
    Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples)
    .
    
    1.  Execute one of the following commands to start the ESB profile with
        the configuration for sample 800.
        -   On Windows: `            wso2ei-samples.bat -sn 800           `
        -   On Linux/Solaris: ./
            `            wso2ei-samples.sh -sn 800           `
    2.  Execute one of the following commands to start the the Axis2 server.
        -   On Windows: `            axis2server.bat           `
        -   `            On Linux/Solaris: ./                         axis2server.sh            `
    3.  Run `          ant         ` from the
        `          <EI_HOME>/samples/axis2Server/src/SimpleStockQuoteService         `
        directory.
    
    #### Configuring the API
    
    Create an API using the following configuration:
    
``` xml
<api xmlns="http://ws.apache.org/ns/synapse" name="TestAPI" context="/test">
  <resource methods="POST GET DELETE OPTIONS HEAD">
    <inSequence>
      <property name="AM_REQUEST_METHOD" expression="get-property('axis2', 'HTTP_METHOD')"></property>
      <send>
        <endpoint>
          <http method="get" uri-template="http://localhost:8280/StarbucksService/services/Starbucks_Outlet_Service/orders/"></http>
        </endpoint>
      </send>
    </inSequence>
    <outSequence>
      <filter xpath="get-property('AM_REQUEST_METHOD')='HEAD'">
        <property name="NO_ENTITY_BODY" value="true" scope="axis2" type="BOOLEAN"></property>
      </filter>
      <send></send>c
    </outSequence>
  </resource>
</api>
```
    
    #### Executing the sample
    
    Send a request to the backend as follows:
    
``` xml
curl -v -X HEAD http://localhost:8280/StarbucksService/services/Starbucks_Outlet_Service/orders/
```
    
!!! info

To send the above request to the API, the starbucks service should be
deployed in WSO2 Application server.


  

You would get the following response:

``` xml
    < HTTP/1.1 302 Found
    < Set-Cookie: JSESSIONID=32C2FF28247F1826FB0F21B68099D3D1; Path=/; HttpOnly
    < Location: https://10.100.5.73:9444/carbon/
    < Content-Type: text/html;charset=UTF-8
    < Content-Length: 0
    < Date: Mon, 09 Jun 2014 05:26:54 GMT
    * Server WSO2 Carbon Server is not blacklisted
    < Server: WSO2 Carbon Server
```

### Sending form data

In this scenario, an API is used to send form data to a REST service
which accepts data of the `         x-www-form-urlencoded        `
content type.

#### Setting up the backend

Configure an endpoint as follows for the REST back end service to which
the form data should be sent. Note that the endpoint has to be an HTTP
endpoint. See [Adding an
Endpoint](https://docs.wso2.com/display/EI650/Working+with+Endpoints+via+Tooling)
for further instructions.

``` xml
    <endpoint xmlns="http://ws.apache.org/ns/synapse" name="FormDataReceiver">
       <http uri-template="http://www.eaipatterns.com/MessageEndpoint.html" method="post">
          <suspendOnFailure>
             <progressionFactor>1.0</progressionFactor>
          </suspendOnFailure>
          <markForSuspension>
             <retriesBeforeSuspension>0</retriesBeforeSuspension>
             <retryDelay>0</retryDelay>
          </markForSuspension>
       </http>
    </endpoint>
```

#### Configuring the API

Configure the API as follows:

``` xml
    <api xmlns="http://ws.apache.org/ns/synapse" name="FORM" context="/Service">
       <resource methods="POST">
          <inSequence>
             <log level="full"></log>
             <property name="name" value="Mark" scope="default" type="STRING"></property>
             <property name="company" value="wso2" scope="default" type="STRING"></property>
             <property name="country" value="US" scope="default" type="STRING"></property>
             <payloadFactory media-type="xml">
                <format>
                   <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
                      <soapenv:Body>
                         <root xmlns="">
                            <name>$1</name>
                            <company>$2</company>
                            <country>$3</country>
                         </root>
                      </soapenv:Body>
                   </soapenv:Envelope>
                </format>
                <args>
                   <arg evaluator="xml" expression="$ctx:name"></arg>
                   <arg evaluator="xml" expression="$ctx:company"></arg>
                   <arg evaluator="xml" expression="$ctx:country"></arg>
                </args>
             </payloadFactory>
             <log level="full"></log>
             <property name="messageType" value="application/x-www-form-urlencoded" scope="axis2" type="STRING"></property>
             <property name="DISABLE_CHUNKING" value="true" scope="axis2" type="STRING"></property>
             <call>
                <endpoint key="FormDataReceiver"></endpoint>
             </call>
             <respond></respond>
          </inSequence>
       </resource>
    </api>
```

In this configuration, `         name        ` ,
`         company        ` and `         country        ` are defined as
properties to be sent as form data using the [Property
mediator](https://docs.wso2.com/display/EI650/Property+Mediator) . These
properties are set as key value pairs by the [PayloadFactory
mediator](https://docs.wso2.com/display/EI650/PayloadFactory+Mediator)
and sent to the the ESB profile. Then the messageType property is set to
`         application/x-www-form-urlencoded        ` to enable the ESB
profile to identify these key value pairs as form data. The
DISABLE\_CHUNKING property is set to `         true        ` t remove
the chunking from the outgoing message. Then the [Call
mediator](https://docs.wso2.com/display/EI650/Call+Mediator) is used to
send the message to the endpoint defined for the REST backend service.

#### Executing the sample

Send a request to the backend as follows:

`         curl -v -X POST -d @request -H "Content-Type: text/xml"                   http://localhost:8280/Service                 `

### Uncommon Scenarios for HTTP Methods in REST

When [sending REST messages to an API](_Configuring_Specific_Use_Cases_)
, you typically use POST or PUT to send a message and GET to request a
message. However, there are some unusual scenarios you might want to
support, which are described in the following sections:

#### Using POST with No Body

Typically, POST is used to send a message that has data enclosed as a
payload inside an HTML body. However, you can also use POST without a
payload. WSO2 Enterprise Integrator (WSO2 EI) treats it as a normal
message and forwards it to the endpoint without any extra configuration.

The following diagram depicts a scenario where a REST client
communicates with a REST service using the ESB profile. Apache Tcpmon is
used solely for monitoring the communication between the ESB profile and
the back-end service and has no impact on the messages passed between
the ESB profile and back-end service. For this particular scenario, the
cURL client is used as the REST client, and the [basic JAX-RS
sample](https://docs.wso2.com/display/EI650/JAX-RS+Basics) is used as
the back-end REST service.

![](attachments/119130400/119130404.png)  

To implement this scenario:

1.  Configure and deploy the JAX-RS Basics sample by following the
    instructions for b uilding and running the sample on the [JAX-RS
    Basics](https://docs.wso2.com/display/EI650/JAX-RS+Basics) page.
2.  [Start the ESB
    profile](https://docs.wso2.com/display/EI650/Running+the+Product)
    and change the configuration as follows:

    ``` html/xml
            <definitions xmlns="http://ws.apache.org/ns/synapse">
               <sequence name="fault">
                  <log level="full">
                     <property name="MESSAGE" value="Executing default sequence"/>
                     <property name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
                     <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
                  </log>
                  <drop/>
               </sequence>
               <sequence name="main">
                  <log/>
                  <drop/>
               </sequence>
               <api name="testAPI" context="/customerservice">
                  <resource methods="POST" url-mapping="/customers">
                     <inSequence>
                        <send>
                           <endpoint>
                              <address uri="http://localhost:8282/jaxrs_basic/services/customers/customerservice"/>
                           </endpoint>
                        </send>
                     </inSequence>
                     <outSequence>
                        <send/>
                     </outSequence>
                  </resource>
               </api>
            </definitions>
    ```

    In this proxy configuration, testAPI intercepts messages that are
    sent to the relative URL
    `           /customerservice/customers          ` and sends them to
    the relevant endpoint by appending the url-mapping of the resource
    tag to the end of the endpoint URL.

3.  Start tcpmon and make it listen to port 8282 of your local machine.
    It is also important to set the target host name and the port as
    required. In this case, the target port needs to be set to 8280
    (i.e. the port where the backend service is running).  
    We will now test the connection by sending a POST message that
    includes a payload inside an HTML body.  

4.  Go to the terminal and issue the following command:  
    `           curl -v -H "Content-Type: application/xml" -d "<Customer><id>123</id><name>John</name></Customer>"                                    http://localhost:8280/customerservice/customers                                 `  

5.  The following reply message appears in the console:

    ``` html/xml
            <Customer>
               <id>132</id>
               <name>John</name>
            </Customer> 
    ```

6.  Now send the same POST message but without the enclosed data as
    follows:  
    `           curl -v -H "Content-Type: application/xml" -d ""                                    http://localhost:8280/customerservice/customer                                 `

        !!! note
    
        You would need to configure the backend service to handle such
        requests, if not the theESB profile will throw exceptions.
    

The tcpmon output shows the same REST request that was sent by the
client, demonstrating that the ESB profile handled the POST message
regardless of whether it included a payload.

#### Using POST with Query Parameters

Sending a POST message with query parameters is an unusual scenario, but
the ESB supports it with no additional configuration. The ESB
profile forwards the message like any other POST message and includes
the query parameters.

To test this scenario, use the same setup as above, but instead of
removing the data part from the request, add some query parameters to
the URL as follows:

`         curl -v -H "Content-Type: text/xml" -d "<Customer><id>123</id><name>John</name></Customer>" '                   http://localhost:8280/customerservice/customers?param1=value1&param2=value2                  '        `

When you execute this command, you can see the following output
in tcpmon:

``` html/xml
    POST /jaxrs_basic/services/customers/customerservice/customers?param1=value1&param2=value2 HTTP/1.1
    Content-Type: application/xml; charset=UTF-8
    Accept: */*
    Transfer-Encoding: chunked
    Host: 127.0.0.1:8282
    Connection: Keep-Alive
     
    32
    <Customer><id>123</id><name>John</name></Customer>
    0
```

As you can see, the query parameters are present in the REST request,
demonstrating that the ESB profile sent them along with the message. You
could write resource methods to support this type of a request. In this
example, the resource method accessed by this request simply ignores the
parameters.

#### Using GET with a Body

Typically, a GET request does not contain a body, and the ESB
profile does not support these types of requests. When it receives a GET
request that contains a body, it drops the message body as it sends the
message to the endpoint. You can test this scenario using the same setup
as above, but this time the client command should look like this:

`         curl -v -H "Content-Type: text/xml" -d "<Customer><id>123</id><name>John</name></Customer>" '                   http://localhost:8280/jaxrs_basic/services/customers/customerservice/customers/123                  ' -X GET        `

The additional parameter `         -X        ` replaces the original
POST method with the specified method, which in this case is GET. This
will cause the client to send a GET request with a message similar to a
POST request. If you view the output in tcpmon, you will see that there
is no message body in the request.

#### Using DELETE with a Body

Typically, DELETE is used to send a HTTP request that does not have data
enclosed as a payload. However, if you send a DELETE request with a
payload, and if the backend service accepts a DELETE request without a
payload, you can drop the payload when the message flows through WSO2
EI.

Follow the steps below to implement this scenario.

1.  [Start the ESB
    profile](https://docs.wso2.com/display/EI650/Running+the+Product)
    and create the API that is defined in the following configuration.
    For instructions, see [Creating an API](_Creating_an_API_) .

    **DropPayloadFromDelete API**

    ``` xml
            <api xmlns="http://ws.apache.org/ns/synapse" name="DropPayloadFromDelete" context="/droppayloadfromdelete">
               <resource methods="DELETE OPTIONS POST PUT GET" url-mapping="/*">
                  <inSequence>
                     <log level="full"/>
                     <property name="NO_ENTITY_BODY" value="true" scope="axis2" type="BOOLEAN"/>
                     <call>
                        <endpoint>
                           <address uri="http://localhost:8282/jaxrs_basic/services/customers/customerservice"/>
                        </endpoint>
                     </call>
                     <log level="full"/>
                     <respond/>
                  </inSequence>
               </resource>
            </api>
    ```

2.  Start TCPMon and make it listen to port 8282 of your local machine.
    It is also important to set the target host name and the port as
    required. In this case, the target port of the TCPMon needs to be
    set to 8280. This is the port where the backend service (i.e., WSO2
    EI) is running.
3.  Open a Terminal and execute the following command to test the
    connection by sending a DELETE message that includes a payload
    inside an HTML body:

    ``` actionscript3
            curl -X DELETE --header 'Content-type: application/json' --header 'Accept: application/json' --header 'Transfer-Encoding: chunked' --data '{ "Animal": { "id": "10" }}' 'http://localhost:8280/droppayloadfromdelete'
    ```

    The TCPMon output shows that the DELETE request sent from WSO2 EI to
    the backend service does not have the payload.

### Configuring non-HTTP endpoints

When using a non-HTTP endpoint, such as a JMS endpoint, in the API
definition, you must remove the REST\_URL\_POSTFIX property to avoid any
characters specified after the context (such as a trailing slash) in the
request from being appended to the JMS endpoint. For example:

``` html/xml
    <api xmlns="http://ws.apache.org/ns/synapse" name="EventDelayOrderAPI" context="/orderdelayAPI"> 
        <resource methods="POST" url-mapping="/"> 
           <inSequence> 
              <property name="REST_URL_POSTFIX" action="remove" scope="axis2"></property> 
              <send> 
                 <endpoint> 
                    <address uri=
    "jms:/DelayOrderTopic?transport.jms.ConnectionFactoryJNDIName=TopicConnectionFactory&
    java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&
    java.naming.provider.url=tcp://localhost:61616&transport.jms.DestinationType=topic">
                  </address> 
                 </endpoint> 
              </send> 
           </inSequence> 
        </resource> 
     </api>
```

Notice that we have specified the REST\_URL\_POSTFIX property with the
value set to "remove". When invoking this API, even if the request
contains a trailing slash after the context (e.g.,
`         POST                                          http://127.0.0.1:8287/orderdelayAPI                                                 /                 `
instead of
`         POST                                                       http://127.0.0.1:8287/orderdelayAPI                                                  `
), the endpoint will be called correctly.
