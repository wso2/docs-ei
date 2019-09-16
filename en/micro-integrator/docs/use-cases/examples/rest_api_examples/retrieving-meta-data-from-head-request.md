# Retrieving metadata associated with the state of the resource using the HEAD request method
    
In this scenario, we are using the http request method HEAD to retrieve the metadata associated with the state of a resource. This is useful in situations where you want to check the content length of the response, last modified date etc. When the HEAD request method is used, the ESB profile accepts the request and sends it back with only the headers and no body.
    
### Setting up the backend
    
For the back-end service, we are using the StockQuote Service that is shipped with the ESB profile. [cURL](http://curl.haxx.se/) is used as the REST client to invoke the ESB profile API. Run [Sample 800](https://docs.wso2.com/display/EI6xx/Sample+800%3A+Introduction+to+REST+API) as follows.
    
For further information on how to run an ESB profile sample in WSO2 EI, see [Setting Up the ESB Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples).
    
1.  Execute one of the following commands to start the ESB profile with the configuration for sample 800.
    -   On Windows: `            wso2ei-samples.bat -sn 800           `
    -   On Linux/Solaris: `          ./wso2ei-samples.sh -sn 800           `
2.  Execute one of the following commands to start the the Axis2 server.
    -   On Windows: `            axis2server.bat           `
    -   On Linux/Solaris: `./                         axis2server.sh            `
3.  Run `          ant         ` from the `          <EI_HOME>/samples/axis2Server/src/SimpleStockQuoteService         ` directory.
    
### Configuring the API
    
Create an API using the following configuration:
    
```
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

### Executing the sample
    
Send a request to the backend as follows:
    
```
curl -v -X HEAD http://localhost:8280/StarbucksService/services/Starbucks_Outlet_Service/orders/
```
    
!!! Info
    To send the above request to the API, the starbucks service should be deployed in WSO2 Application server.

You would get the following response:

``` 
< HTTP/1.1 302 Found
< Set-Cookie: JSESSIONID=32C2FF28247F1826FB0F21B68099D3D1; Path=/; HttpOnly
< Location: https://10.100.5.73:9444/carbon/
< Content-Type: text/html;charset=UTF-8
< Content-Length: 0
< Date: Mon, 09 Jun 2014 05:26:54 GMT
* Server WSO2 Carbon Server is not blacklisted
< Server: WSO2 Carbon Server
```