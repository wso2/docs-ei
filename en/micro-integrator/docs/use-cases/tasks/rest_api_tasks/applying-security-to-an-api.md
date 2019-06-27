# Applying Security to an API

-   [Using a Basic Auth
    handler](#ApplyingSecuritytoanAPI-BasicAuthUsingaBasicAuthhandler)
    -   [Using a custom basic auth
        handler](#ApplyingSecuritytoanAPI-Usingacustombasicauthhandler)
-   [Using Kerberos to secure the REST
    API](#ApplyingSecuritytoanAPI-UsingKerberostosecuretheRESTAPI)
-   [Using an OAuth base security
    token](#ApplyingSecuritytoanAPI-UsinganOAuthbasesecuritytoken)
    -   [Creating the custom
        handler](#ApplyingSecuritytoanAPI-Creatingthecustomhandler)
    -   [Creating the API](#ApplyingSecuritytoanAPI-CreatingtheAPI)
    -   [Getting the OAuth
        token](#ApplyingSecuritytoanAPI-GettingtheOAuthtoken)
-   [Using a policy file to authenticate with a secured back-end
    service](#ApplyingSecuritytoanAPI-Usingapolicyfiletoauthenticatewithasecuredback-endservice)
-   [Transforming Basic Auth to
    WS-Security](#ApplyingSecuritytoanAPI-TransformingBasicAuthtoWS-Security)

### Using a Basic Auth handler

In most of the real-world use cases of REST, when a consumer attempts to
access a privileged resource, access will be denied unless the
consumer's credentials are provided in an Authorization header. By
default, the ESB profile of WSO2 EI and [WSO2 Micro
Integrator](https://docs.wso2.com/display/EI650/WSO2+Micro+Integrator)
validates the credentials of the consumer (that is provided in the
Authorization header) against the credentials of users that are
registered in the user store connected to the server. This validation is
done using the following basic auth handler that is built into the ESB.
When a REST API is created, this handler should be added to the API
configuration in order to enable basic auth.

-   [**ESB profile**](#92a3a94fa689457cbb9fe069d1cf67bc)
-   [**WSO2 Micro Integrator**](#767f16ef4c3144c8b110afe6fda93600)

**Using the basic auth handler of the ESB profile**

You need to use the following handler for the ESB profile.

``` html/xml
    <handlers>
        <handler class="org.wso2.carbon.integrator.core.handler.RESTBasicAuthHandler"/>
    </handlers>
```

See the REST API given below for an example of how the default basic
auth handler is used.

``` html/xml
    <api xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteAPI" context="/stockquote">
       <resource methods="GET" uri-template="/view/{symbol}">
          <inSequence>
             <payloadFactory media-type="xml">
                <format>
                   <m0:getQuote xmlns:m0="http://services.samples">
                      <m0:request>
                         <m0:symbol>$1</m0:symbol>
                      </m0:request>
                   </m0:getQuote>
                </format>
                <args>
                   <arg evaluator="xml" expression="get-property('uri.var.symbol')"/>
                </args>
             </payloadFactory>
             <header name="Action" scope="default" value="urn:getQuote"/>
             <send>
                <endpoint>
                   <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="soap11"/>
                </endpoint>
             </send>
          </inSequence>
          <outSequence>
             <send/>
          </outSequence>
          <faultSequence/>
       </resource>
       <handlers>
        <handler class="org.wso2.carbon.integrator.core.handler.RESTBasicAuthHandler"/>
       </handlers>
    </api>
```

Follow the steps given below to test the above REST API:

1.  Create the REST API given above using WSO2 Integration Studio, and
    deploy it in the ESB profile of WSO2 EI.
2.  The above REST API invokes the SimpleStockQuoteService, which is a
    sample back-end service that is shipped with the product. Follow the
    instructions given
    [here](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Deployingsampleback-endservices)
    to start this back-end service.
3.  First, invoke the service using the following service URL without
    providing any user credentials:
    <http://127.0.0.1:8280/stockquote/view/IBM>  
    Note that you will receive the following error: '401 Unauthorized'
4.  Now, invoke the service again by providing the credentials of a user
    that is registered in the ESB's user store. For example, use admin
    as the username and password. The request will be successfully
    passed to the back-end service and you will receive the following
    response:

    ``` java
            <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
                <soapenv:Body>
                    <ns:getQuoteResponse xmlns:ns="http://services.samples">
                        <ns:return xmlns:ax21="http://services.samples/xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ax21:GetQuoteResponse">
                            <ax21:change>-2.6989539095024164</ax21:change>
                            <ax21:earnings>12.851852793420885</ax21:earnings>
                            <ax21:high>-166.81703170012037</ax21:high>
                            <ax21:last>170.03627716039932</ax21:last>
                            <ax21:lastTradeTimestamp>Mon Jul 30 15:10:56 IST 2018</ax21:lastTradeTimestamp>
                            <ax21:low>178.02122263133768</ax21:low>
                            <ax21:marketCap>-7306984.135450081</ax21:marketCap>
                            <ax21:name>IBM Company</ax21:name>
                            <ax21:open>-165.86249647643422</ax21:open>
                            <ax21:peRatio>23.443106773044992</ax21:peRatio>
                            <ax21:percentageChange>1.5959734616866617</ax21:percentageChange>
                            <ax21:prevClose>-169.11019978052138</ax21:prevClose>
                            <ax21:symbol>IBM</ax21:symbol>
                            <ax21:volume>9897</ax21:volume>
                        </ns:return>
                    </ns:getQuoteResponse>
                </soapenv:Body>
            </soapenv:Envelope>
    ```

****Using the basic auth handler of WSO2 Micro Integrator****

You need to use the following handler for WSO2 Micro Integrator.

``` html/xml
    <handlers>
        <handler class="org.wso2.carbon.micro.integrator.security.handler.RESTBasicAuthHandler"/>
    </handlers>
```

See the REST API given below for an example of how the default basic
auth handler is used.

``` html/xml
    <api xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteAPI" context="/stockquote">
       <resource methods="GET" uri-template="/view/{symbol}">
          <inSequence>
             <payloadFactory media-type="xml">
                <format>
                   <m0:getQuote xmlns:m0="http://services.samples">
                      <m0:request>
                         <m0:symbol>$1</m0:symbol>
                      </m0:request>
                   </m0:getQuote>
                </format>
                <args>
                   <arg evaluator="xml" expression="get-property('uri.var.symbol')"/>
                </args>
             </payloadFactory>
             <header name="Action" scope="default" value="urn:getQuote"/>
             <send>
                <endpoint>
                   <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="soap11"/>
                </endpoint>
             </send>
          </inSequence>
          <outSequence>
             <send/>
          </outSequence>
          <faultSequence/>
       </resource>
       <handlers>
        <handler class="org.wso2.carbon.micro.integrator.security.handler.RESTBasicAuthHandler"/>
       </handlers>
    </api>
```

Follow the steps given below to test the above REST API:

1.  Create the REST API given above using WSO2 Integration Studio.  
    For more information on creating a REST API, see [Creating an
    API](_Creating_an_API_) . Once the API is created, you can copy the
    code given above to the source view of the API.
2.  Create the CAR file:  
    1.  Open the `               pom.xml              ` file of the
        C-App project in the Composite Application Project POM Editor.
    2.  Select the artifact that needs to be included into the CAR file.
    3.  Click and define the location you want to create the CAR file.  
3.  Add the [CAR file you created](#ApplyingSecuritytoanAPI-car-file) to
    the
    `             <MI_HOME>/repository/deployment/server/carbonapps            `
    directory to deploy it in WSO2 Micro Integrator.
4.  The above REST API invokes the
    `             SimpleStockQuoteService            ` , which is a
    sample back-end service that is shipped with the product.  
    Follow the instructions given
    [here](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Deployingsampleback-endservices)
    to start this back-end service.
5.  Configuring the user store:
    1.  Create your user store and host it because WSO2 Micro Integrator
        does not include a default user store.
    2.  Once hosted, configure the following properties in the
        `                <MI_HOME>/conf/user-mgt.xml               `
        file with your user store's details.

                !!! info
        
                For more information on configuring the user store, see
                [Configuring User
                Stores](https://docs.wso2.com/display/ADMIN44x/Configuring+User+Stores)
                .
        

        For this sample, we configure a read- only LDAP user store.

        <table>
        <thead>
        <tr class="header">
        <th>Property</th>
        <th>Description</th>
        </tr>
        </thead>
        <tbody>
        <tr class="odd">
        <td><code>                    ConnectionURL                   </code></td>
        <td><p>Connection URL to the user store server you hosted.</p>
        <p>If you are connecting over LDAPS (secured LDAP), you need to import the certificate of the user store to the <code>                     client-truststore.jks                    </code> of the WSO2 product. For information on how to add certificates to the truststore, and how keystores are configured and used in a system, see <a href="https://docs.wso2.com/display/ADMIN44x/Using+Asymmetric+Encryption">Using Asymmetric Encryption</a> .<br />
        <br />
        If LDAP connection pooling is used, see the following guide on how to <a href="https://docs.wso2.com/display/ADMIN44x/Performance+Tuning#PerformanceTuning-ldaps_pooling">enable connection pooling for LDAPS connections</a> .</p></td>
        </tr>
        <tr class="even">
        <td><code>                    ConnectionName                   </code></td>
        <td>The username used to connect to the user store and perform various operations. This user does not need to be an administrator in the user store or have an administrator role in the WSO2 product that you are using, but this user MUST have permissions to read the user list and users' attributes and to perform search operations on the user store. The value you specify is used as the DN (Distinguish Name) attribute of the user who has sufficient permissions to perform operations on users and roles in LDAP.</td>
        </tr>
        <tr class="odd">
        <td><code>                    ConnectionPassword                   </code></td>
        <td>Password for the ConnectionName user.</td>
        </tr>
        <tr class="even">
        <td><code>                    UserSearchBase                   </code></td>
        <td>Distinguish Name ( DN) of the context or object under which the user entries are stored in the user store. When the user store searches for users, it will start from this location of the directory.</td>
        </tr>
        <tr class="odd">
        <td><code>                    UserNameAttribute                   </code></td>
        <td><p>The attribute used for uniquely identifying a user entry. Users can be authenticated using their email address, UID, etc. The name of the attribute is considered as the username.</p></td>
        </tr>
        <tr class="even">
        <td><code>                    GroupSearchBase                   </code></td>
        <td>Distinguish Name ( DN) of the context or object under which the group entries are stored in the user store. When the user store searches for groups, it will start from this location of the directory.</td>
        </tr>
        </tbody>
        </table>

6.  Start WSO2 Micro Integrator:

    1.  Navigate to the `                <MI_HOME>/bin               `
        directory via the terminal.

    2.  Run the Micro Integrator:

        -   On Linux/Mac OS:
            `                  sh micro-integrator.sh                 `

        -   On Windows:
            `                  micro-integrator.bat                 `

7.  First, invoke the service using the following service URL without
    providing any user credentials:
    `              http://127.0.0.1:8280/stockquote/view/IBM             `

        !!! info
    
        You can invoke the service using Postman or curl.
    

    Note that you will receive the following error because the username
    and password are not passed and the service cannot be authenticated:
    `              '401 Unauthorized'             `

8.  Now, invoke the service again by providing the credentials of a user
    that is registered in the user store that is hosted.  
    The request is passed to the back-end service and you will receive a
    response similar to what is shown below:

    ``` java
        <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
            <soapenv:Body>
                <ns:getQuoteResponse xmlns:ns="http://services.samples">
                    <ns:return xmlns:ax21="http://services.samples/xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ax21:GetQuoteResponse">
                        <ax21:change>-2.6989539095024164</ax21:change>
                        <ax21:earnings>12.851852793420885</ax21:earnings>
                        <ax21:high>-166.81703170012037</ax21:high>
                        <ax21:last>170.03627716039932</ax21:last>
                        <ax21:lastTradeTimestamp>Mon Jul 30 15:10:56 IST 2018</ax21:lastTradeTimestamp>
                        <ax21:low>178.02122263133768</ax21:low>
                        <ax21:marketCap>-7306984.135450081</ax21:marketCap>
                        <ax21:name>IBM Company</ax21:name>
                        <ax21:open>-165.86249647643422</ax21:open>
                        <ax21:peRatio>23.443106773044992</ax21:peRatio>
                        <ax21:percentageChange>1.5959734616866617</ax21:percentageChange>
                        <ax21:prevClose>-169.11019978052138</ax21:prevClose>
                        <ax21:symbol>IBM</ax21:symbol>
                        <ax21:volume>9897</ax21:volume>
                    </ns:return>
                </ns:getQuoteResponse>
            </soapenv:Body>
        </soapenv:Envelope>
    ```

#### **Using a custom basic auth handler**

If required, you can implement a custom basic auth handler (instead of
the default handler explained above). The following example of a
primitive security handler serves as a template that can be used to
write your own security handler to secure an API in the ESB profile.

![](images/icons/grey_arrow_down.png){.expand-control-image} Click here
to expand...

The custom Basic Auth handler in this sample simply verifies whether the
request uses username: admin and password: admin. Following is the code
for this handler:

``` java
    package org.wso2.rest;
    import org.apache.commons.codec.binary.Base64;
    import org.apache.synapse.MessageContext;
    import org.apache.synapse.core.axis2.Axis2MessageContext;
    import org.apache.synapse.core.axis2.Axis2Sender;
    import org.apache.synapse.rest.Handler;
     
    import java.util.Map;
     
    public class BasicAuthHandler implements Handler {
        public void addProperty(String s, Object o) {
            //To change body of implemented methods use File | Settings | File Templates.
        }
    
        public Map getProperties() {
            return null;  //To change body of implemented methods use File | Settings | File Templates.
        }
    
        public boolean handleRequest(MessageContext messageContext) {
    
            org.apache.axis2.context.MessageContext axis2MessageContext
                    = ((Axis2MessageContext) messageContext).getAxis2MessageContext();
            Object headers = axis2MessageContext.getProperty(
                    org.apache.axis2.context.MessageContext.TRANSPORT_HEADERS);
    
            if (headers != null && headers instanceof Map) {
                Map headersMap = (Map) headers;
                if (headersMap.get("Authorization") == null) {
                    headersMap.clear();
                    axis2MessageContext.setProperty("HTTP_SC", "401");
                    headersMap.put("WWW-Authenticate", "Basic realm=\"WSO2 ESB\"");
                    axis2MessageContext.setProperty("NO_ENTITY_BODY", new Boolean("true"));
                    messageContext.setProperty("RESPONSE", "true");
                    messageContext.setTo(null);
                    Axis2Sender.sendBack(messageContext);
                    return false;
    
                } else {
                    String authHeader = (String) headersMap.get("Authorization");
                    if (processSecurity(credentials)) {
                        return true;
                    } else {
                        headersMap.clear();
                        axis2MessageContext.setProperty("HTTP_SC", "403");
                        axis2MessageContext.setProperty("NO_ENTITY_BODY", new Boolean("true"));
                        messageContext.setProperty("RESPONSE", "true");
                        messageContext.setTo(null);
                        Axis2Sender.sendBack(messageContext);
                        return false;
                    }
                }
            }
            return false;
        }
     
        public boolean handleResponse(MessageContext messageContext) {
            return true;
        }
    
        public boolean processSecurity(String credentials) {
            String decodedCredentials = new String(new Base64().decode(credentials.getBytes()));
            String usernName = decodedCredentials.split(":")[0];
            String password = decodedCredentials.split(":")[1];
            if ("admin".equals(username) && "admin".equals(password)) {
                return true;
            } else {
                return false;
            }
        }
    }
```

You can build the project (mvn clean install) for this handler by
accessing its source here:

<https://github.com/wso2/product-esb/tree/v5.0.0/modules/samples/integration-scenarios/starbucks_sample/BasicAuth-handler>

!!! note

When building the sample using the source ensure you update
`           pom.xml          ` with the online repository. To do this,
add the following section before `           <dependencies>          `
tag in `           pom.xml          ` :

``` java
    <repositories>
        <repository>
           <id>wso2-nexus</id>
           <name>WSO2 internal Repository</name>
           <url>http://maven.wso2.org/nexus/content/groups/wso2-public/</url>
           <releases>
              <enabled>true</enabled>
              <updatePolicy>daily</updatePolicy>
              <checksumPolicy>ignore</checksumPolicy>
           </releases>
         </repository>
         <repository>
           <id>wso2-maven2-repository</id>
           <name>WSO2 Maven2 Repository</name>
           <url>http://dist.wso2.org/maven2</url>
           <snapshots>
             <enabled>false</enabled>
           </snapshots>
           <releases>
             <enabled>true</enabled>
             <updatePolicy>never</updatePolicy>
             <checksumPolicy>fail</checksumPolicy>
           </releases>
        </repository>
    </repositories>
```
    

Alternatively, you can download the JAR file from the following
location, copy it to the `           <EI_HOME>/lib          ` directory,
and restart WSO2 EI:

<https://github.com/wso2/product-esb/blob/v5.0.0/modules/samples/integration-scenarios/starbucks_sample/bin/WSO2-REST-BasicAuth-Handler-1.0-SNAPSHOT.jar>

Add the handler to the REST API:

``` html/xml
    <api xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteAPI" context="/stockquote">
       <resource methods="GET" uri-template="/view/{symbol}">
          <inSequence>
             <payloadFactory media-type="xml">
                <format>
                   <m0:getQuote xmlns:m0="http://services.samples">
                      <m0:request>
                         <m0:symbol>$1</m0:symbol>
                      </m0:request>
                   </m0:getQuote>
                </format>
                <args>
                   <arg evaluator="xml" expression="get-property('uri.var.symbol')"/>
                </args>
             </payloadFactory>
             <header name="Action" scope="default" value="urn:getQuote"/>
             <send>
                <endpoint>
                   <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="soap11"/>
                </endpoint>
             </send>
          </inSequence>
          <outSequence>
             <send/>
          </outSequence>
          <faultSequence/>
       </resource>
       <handlers>
         <handler class="org.wso2.rest.BasicAuthHandler"/>
        </handlers>
    </api>                                    
```

You can now send a request to the secured API.

### Using Kerberos to secure the REST API

First, you need to set up the Kerberos server and create the credentials
for the ESB server (which hold the REST API). For example, let's set up
Active Directory as the kerberos server (KDC):

1.  [Download Apache Directory
    Studio](https://directory.apache.org/studio/users-guide/apache_directory_studio/download_install.html)
    and install the product.

2.  In Active Directory(AD), create a new user as shown below.

    <table style="width:100%;">
    <colgroup>
    <col style="width: 16%" />
    <col style="width: 83%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Username</td>
    <td>esbservice</td>
    </tr>
    </tbody>
    </table>

3.  Be sure to select **Password does not expire** , and deselect **User
    must change password** .
4.  Open a terminal and execute the following command to set the Service
    Principal Name (spn) for the ESB server:

    ``` java
            setspn -s HTTP/myserver esbservice
    ```

    You will now have the following:

    <table>
    <colgroup>
    <col style="width: 22%" />
    <col style="width: 77%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Username</td>
    <td>esbservice</td>
    </tr>
    <tr class="even">
    <td>Service Principal Name</td>
    <td>HTTP/myserver</td>
    </tr>
    </tbody>
    </table>

5.  Open a terminal and execute the command shown below to g enerate the
    keytab file for the above service. The keytab file contains the
    password for the service.

    ``` java
    ```
    
    Follow the steps given below to apply the Kerberos handler to the ESB
    server:
    
    1.  Download the
        `                     WSO2-REST-KerberosAuth-Handler-1.0.0-SNAPSHOT.jar                   `
        file and copy it to the `          <EI_HOME>/lib         `
        directory.
    2.  Create the following REST API and add the Kerberos handler to secure
        it with Kerberos authentication. Be sure to replace the handler
        configurations as described below.
    
    ``` java
    <api xmlns="http://ws.apache.org/ns/synapse"
        name="HealthCheckAPI"
        context="/HealthCheck">
      <resource methods="GET" url-mapping="/status" faultSequence="fault">
         <inSequence>
            <payloadFactory media-type="json">
               <format>{"Status":"OK"}</format>
               <args/>
            </payloadFactory>
            <log>
               <property name="JSON-Payload" expression="json-eval($.)"/>
            </log>
            <property name="NO_ENTITY_BODY" scope="axis2" action="remove"/>
            <property name="messageType"
                      value="application/json"
                      scope="axis2"
                      type="STRING"/>
            <respond/>
         </inSequence>
      </resource>
      <handlers>
         <handler class="org.wso2.rest.handler.KerberosAuthHandler">
            <property name="keyTabFilePath" value="<PATH_TO_KEYTAB_FILE>"/>
         </handler>
      </handlers>
    </api>
    ```
    
        Kerberos handler parameters:
    
        <table>
        <colgroup>
        <col style="width: 11%" />
        <col style="width: 88%" />
        </colgroup>
        <thead>
        <tr class="header">
        <th>Property</th>
        <th>Description</th>
        </tr>
        </thead>
        <tbody>
        <tr class="odd">
        <td>keyTabFilePath</td>
        <td>The file path to the KeyTab file that was generated for the ESB server. Note that this file contains the password corresponding to the server principal name.</td>
        </tr>
        <tr class="even">
        <td>serverPrincipal</td>
        <td>This is the server principal name (spn) that was generated for the ESB server.</td>
        </tr>
        </tbody>
        </table>
    
    You have now configured a REST API which is secured with the Kerberos
    authentication.  
    
    ### Using an OAuth base security token
    
    You can generate an OAuth base security token using WSO2 Identity
    Server, and then use that token when invoking your API to connect to a
    REST endpoint. This approach involves the following tasks:
    
    1.  Create a custom handler that will validate the token
    2.  Create an API that points to the REST endpoint and includes the
        custom handler
    3.  Create an OAuth application in Identity Server and get the access
        token
    4.  Invoke the API with the access token
    
    #### Creating the custom handler
    
    The custom handler must extend AbstractHandler and implement
    ManagedLifecycle as shown in the following example. You can download the
    Maven project for this example at
    <https://github.com/wso2-docs/ESB/tree/master/ESB-Artifacts/OAuthHandler_Sample>
    
``` java
package org.wso2.handler;

import org.apache.axis2.AxisFault;
import org.apache.axis2.client.Options;
import org.apache.axis2.client.ServiceClient;
import org.apache.axis2.context.ConfigurationContext;
import org.apache.axis2.context.ConfigurationContextFactory;
import org.apache.axis2.transport.http.HTTPConstants;
import org.apache.axis2.transport.http.HttpTransportProperties;
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.apache.http.HttpHeaders;
import org.apache.synapse.core.axis2.Axis2MessageContext;
import org.wso2.carbon.identity.oauth2.stub.OAuth2TokenValidationServiceStub;
import org.wso2.carbon.identity.oauth2.stub.dto.OAuth2TokenValidationRequestDTO;
import org.apache.synapse.ManagedLifecycle;
import org.apache.synapse.MessageContext;
import org.apache.synapse.core.SynapseEnvironment;
import org.apache.synapse.rest.AbstractHandler;
import org.wso2.carbon.identity.oauth2.stub.dto.OAuth2TokenValidationRequestDTO_OAuth2AccessToken;

import java.util.Map;

public class SimpleOauthHandler extends AbstractHandler implements ManagedLifecycle {

    private static final String CONSUMER_KEY_HEADER = "Bearer";
    private static final String OAUTH_HEADER_SPLITTER = ",";
    private static final String CONSUMER_KEY_SEGMENT_DELIMITER = " ";
    private static final String OAUTH_TOKEN_VALIDATOR_SERVICE = "oauth2TokenValidationService";
    private static final String IDP_LOGIN_USERNAME = "identityServerUserName";
    private static final String IDP_LOGIN_PASSWORD = "identityServerPw";
    private ConfigurationContext configContext;
    private static final Log log = LogFactory.getLog(SimpleOauthHandler.class);

    @Override
    public boolean handleRequest(MessageContext msgCtx) {
        if (this.getConfigContext() == null) {
            log.error("Configuration Context is null");
            return false;
        }
        try{
            //Read parameters from axis2.xml
            String identityServerUrl =
                    msgCtx.getConfiguration().getAxisConfiguration().getParameter(
                            OAUTH_TOKEN_VALIDATOR_SERVICE).getValue().toString();
            String username =
                    msgCtx.getConfiguration().getAxisConfiguration().getParameter(
                            IDP_LOGIN_USERNAME).getValue().toString();
            String password =
                    msgCtx.getConfiguration().getAxisConfiguration().getParameter(
                            IDP_LOGIN_PASSWORD).getValue().toString();
            OAuth2TokenValidationServiceStub stub =
                    new OAuth2TokenValidationServiceStub(this.getConfigContext(), identityServerUrl);
            ServiceClient client = stub._getServiceClient();
            Options options = client.getOptions();
            HttpTransportProperties.Authenticator authenticator = new HttpTransportProperties.Authenticator();
            authenticator.setUsername(username);
            authenticator.setPassword(password);
            authenticator.setPreemptiveAuthentication(true);
            options.setProperty(HTTPConstants.AUTHENTICATE, authenticator);
            client.setOptions(options);
            OAuth2TokenValidationRequestDTO dto = this.createOAuthValidatorDTO(msgCtx);
            return stub.validate(dto).getValid();
        }catch(Exception e){
            log.error("Error occurred while processing the message", e);
            return false;
        }
    }
    private OAuth2TokenValidationRequestDTO createOAuthValidatorDTO(MessageContext msgCtx) {
        OAuth2TokenValidationRequestDTO dto = new OAuth2TokenValidationRequestDTO();
        Map headers = (Map) ((Axis2MessageContext) msgCtx).getAxis2MessageContext().
                getProperty(org.apache.axis2.context.MessageContext.TRANSPORT_HEADERS);
        String apiKey = null;
        if (headers != null) {
            apiKey = extractCustomerKeyFromAuthHeader(headers);
        }
        OAuth2TokenValidationRequestDTO_OAuth2AccessToken token =
                new OAuth2TokenValidationRequestDTO_OAuth2AccessToken();
        token.setTokenType("bearer");
        token.setIdentifier(apiKey);
        dto.setAccessToken(token);
        return dto;
    }
    private String extractCustomerKeyFromAuthHeader(Map headersMap) {
        //From 1.0.7 version of this component onwards remove the OAuth authorization header from
        // the message is configurable. So we dont need to remove headers at this point.
        String authHeader = (String) headersMap.get(HttpHeaders.AUTHORIZATION);
        if (authHeader == null) {
            return null;
        }
        if (authHeader.startsWith("OAuth ") || authHeader.startsWith("oauth ")) {
            authHeader = authHeader.substring(authHeader.indexOf("o"));
        }
        String[] headers = authHeader.split(OAUTH_HEADER_SPLITTER);
        if (headers != null) {
            for (String header : headers) {
                String[] elements = header.split(CONSUMER_KEY_SEGMENT_DELIMITER);
                if (elements != null && elements.length > 1) {
                    boolean isConsumerKeyHeaderAvailable = false;
                    for (String element : elements) {
                        if (!"".equals(element.trim())) {
                            if (CONSUMER_KEY_HEADER.equals(element.trim())) {
                                isConsumerKeyHeaderAvailable = true;
                            } else if (isConsumerKeyHeaderAvailable) {
                                return removeLeadingAndTrailing(element.trim());
                            }
                        }
                    }
                }
            }
        }
        return null;
    }
    private String removeLeadingAndTrailing(String base) {
        String result = base;
        if (base.startsWith("\"") || base.endsWith("\"")) {
            result = base.replace("\"", "");
        }
        return result.trim();
    }
    @Override
    public boolean handleResponse(MessageContext messageContext) {
        return true;
    }
    @Override
    public void init(SynapseEnvironment synapseEnvironment) {
        try {
            this.configContext =
                    ConfigurationContextFactory.createConfigurationContextFromFileSystem(null, null);
        } catch (AxisFault axisFault) {
            log.error("Error occurred while initializing Configuration Context", axisFault);
        }
    }
    @Override
    public void destroy() {
        this.configContext = null;
    }
    private ConfigurationContext getConfigContext() {
        return configContext;
    }
}
```
    
    #### Creating the API
    
    You will now create an API named TestGoogle that connects to the
    following endpoint: https://www.google.lk/search?q=wso2
    
    1.  In WSO2 EI Management Console, go to Manage -\> Service Bus and
        click Source View .
    2.  Insert the following XML configuration into the source view before
        the closing `           </definitions>          ` tag to create the
        TestGoogle API:
    
    ``` html/xml
     <api xmlns="http://ws.apache.org/ns/synapse"
         name="TestGoogle"
         context="/search">
       <resource methods="GET">
          <inSequence>
             <log level="full">
               <property name="STATUS" value="***** REQUEST HITS IN SEQUENCE *****"/>
             </log>
             <send>
                <endpoint>
                   <http method="get" uri-template="https://www.google.lk/search?q=wso2"/>
                </endpoint>
             </send>
          </inSequence>
       </resource>
       <handlers>
           <handler class="org.wso2.handler.SimpleOauthHandler"/>
       </handlers>
    </api>
    ```
        
            Notice that the `           <handlers>          ` tag contains the
            reference to the custom handler class.
        
        3.  Copy the custom `          handler.jar         ` to the
            `          <EI_HOME>/lib         ` directory.
        4.  Open `           <EI_HOME>/repository/axis2/axis2.xml          ` and
            add the following parameters:
        
    ``` html/xml
    <!-- OAuth2 Token Validation Service -->
    <parameter name="oauth2TokenValidationService">https://localhost:9444/services/OAuth2TokenValidationService</parameter>
    <!-- Server credentials -->
    <parameter name="identityServerUserName">admin</parameter>
    <parameter name="identityServerPw">admin</parameter>
    ```
        
        5.  Restart WSO2 EI.
        
        #### Getting the OAuth token
        
        You will now use Identity Server to create an OAuth application and
        generate the security token.
        
        1.  Start WSO2 Identity Server and log into the management console.
        2.  On the **Main** tab, click **Add** under **Service Providers** , and
            then [add a service
            provider](https://docs.wso2.com/display/IS570/Adding+and+Configuring+a+Service+Provider)
            [.](http://docs.wso2.com/identity-server/Adding%20a%20Service%20Provider)
        3.  Note the access token URL and embed it in a cURL request to get the
            token. For example, use the following command and replace and with
            the actual values:  
              
            curl -v -k -X POST --user : -H "Content-Type:
            application/x-www-form-urlencoded;charset=UTF-8" -d
            'grant\_type=password&username=admin&password=admin'
            https://localhost:9444/oauth2/token
        
        Identity Server returns the access token, which you can now use when
        invoking the API. For example:
        
        `                   curl -v -X GET -H "Authorization: Bearer ca1799fc84986bd87c120ba499838a7"                     http://localhost:8280/search                           `
        
        ### Using a policy file to authenticate with a secured back-end service
        
        You can connect a REST client to a secured back-end service (such as a
        SOAP service) through an API that reads from a policy file.
        
        First, you configure the ESB profile to expose the API to the REST
        client. For example:
        
``` html/xml
<definitions xmlns="http://ws.apache.org/ns/synapse">
   <localEntry key="sec_policy"
               src="file:repository/samples/resources/policy/policy_3.xml"/>
  <api name="StockQuoteAPI" context="/stockquote">
      <resource methods="GET" uri-template="/view/{symbol}">e
         <inSequence trace="enable">
            <header name="Action" value="urn:getQuote"/>
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
            <send>
               <endpoint name="rest">
                  <address uri="http://localhost:9000/services/SecureStockQuoteService"
                           format="soap11">
                     <enableAddressing/>
                     <enableSec policy="sec_policy"/>
                  </address>
               </endpoint>
            </send>
            </inSequence>
         <outSequence trace="enable">
            <header xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"
                    name="wsse:Security"
                    action="remove"/>
            <send/>
         </outSequence>
      </resource>
      <resource methods="POST">
         <inSequence>
            <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
            <property name="OUT_ONLY" value="true"/>
            <send>
               <endpoint>
                  <address uri="http://localhost:9000/services/SimpleStockQuoteService"
                           format="soap11"/>
               </endpoint>
            </send>
         </inSequence>
      </resource>
   </api>
</definitions>
```
    
    The policy file stores the security parameters that are going to
    authenticated by the back-end service. You specify the policy file with
    the localEntry property, which in this example we've named sec\_policy:
    
``` html/xml
<localEntry key="sec_policy"
               src="file:repository/samples/resources/policy/policy_3.xml"/> 
```
    
    You use then reference the policy file by its localEntry name when
    enabling the security policy for the end point:
    
``` html/xml
<address uri="http://localhost:9000/services/SecureStockQuoteService"
    format="soap11">
    <enableAddressing/>
    <enableSec policy="sec_policy"/>
</address>
```
    
    In the outSequence property, the security header must be removed before
    sending the response back to the REST client.
    
``` html/xml
<outSequence trace="enable">
            <header xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"
                    name="wsse:Security"
                    action="remove"/>
```
    
    To test this API configuration, you must run the
    SecureStockQuoteService, which is bundled in the samples folder, as the
    back-end server. Start this sample as described in [Setting Up the ESB
    Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples)
    . This sample uses Apache Rampart as the back-end security
    implementation. Therefore, you need to download and install the
    unlimited strength policy files for your JDK before using Apache
    Rampart.
    
    **To download and install the unlimited strength policy files:**
    
    1.  Go to
        <http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html>
        , and download the unlimited strength JCE policy files for your JDK
        version.
    2.  Uncompress and extract the downloaded ZIP file. This creates a
        directory named JCE that contains the
        `          local_policy.jar         ` and
        `          US_export_policy.jar         ` files.
    3.  In your Java installation directory, go to the
        `          jre/lib/security         ` directory, make a copy of the
        existing `          local_policy.jar         ` and
        `          US_export_policy.jar         ` files, and then replace
        the original policy files with the policy files you extracted in the
        previous step.
    
    Now that you have set up the API and the secured back-end SOAP service,
    you are ready to test this configuration with the following curl
    command.
    
    `         curl -v                   http://127.0.0.1:8280/stockquote/view/IBM                 `
    
    Observe that the REST client is getting the correct response (the
    `         wsse:Security        ` header is removed from the decrypted
    message) while going through the secured back-end service and the API
    implemented in the WSO2 EI. You can use a TCP monitoring tool such as
    [tcpmon](https://code.google.com/p/tcpmon/downloads/list) to monitor the
    message sent from the the ESB profile and the response message received
    by the ESB profile. For a tutorial on using tcpmon, see:
    <http://technonstop.com/tcpmon-tutorial>
    
    ### Transforming Basic Auth to WS-Security
    
    REST clients typically use Basic Auth over HTTP to authenticate the user
    name and password, but if the back-end service uses WS-Security, you can
    configure the API to transform the authentication from Basic Auth to
    WS-Security.
    
    ![](attachments/119130394/119130399.png){width="700"}
    
    To achieve this transformation, you configure the ESB profile to expose
    the API to the REST client as shown in the [previous
    example](#ApplyingSecuritytoanAPI-policyConf) , but you add the HTTPS
    protocol and [Basic Auth handler](#ApplyingSecuritytoanAPI-BasicAutho)
    to the configuration as shown below:
    
``` java
<definitions xmlns="http://ws.apache.org/ns/synapse">
  <localEntry key="sec_policy"
src="file:repository/samples/resources/policy/policy_3.xml"/>
  <api name="StockQuoteAPI" context="/stockquote">
    <resource methods="GET" uri-template="/view/{symbol}" protocol="https" >
     ...
    </resource>
    <handlers>
      <handler class="org.wso2.rest.BasicAuthHandler"/>
    </handlers>
  </api>
</definitions>
```
    
    This configuration allows two-fold security: the client authenticates
    with the API using Basic Auth over HTTPS, and then the API sends the
    request to the back-end service using the security policy. You can test
    this configuration using the following command:
    
``` bash
 curl -v -k -H "Authorization: Basic YWRtaW46YWRtaW4=" https://localhost:8243/stockquote/view/IBM  
```
