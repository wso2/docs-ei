# Securing REST APIs

## Using a Basic Auth handler

In most of the real-world use cases of REST, when a consumer attempts to access a privileged resource, access will be denied unless the consumer's credentials are provided in an Authorization header. By default, the WSO2 Micro Integrator validates the credentials of the consumer (that is provided in the Authorization header) against the credentials of users that are registered in the user store connected to the server. 

## Synapse configuration

This validation is done using the following basic auth handler. When a REST API is created, this handler should be added to the API configuration in order to enable basic auth.

```xml
<handlers>
    <handler class="org.wso2.carbon.micro.integrator.security.handler.RESTBasicAuthHandler"/>
</handlers>
```

See the REST API given below for an example of how the default basic auth handler is used.

```xml
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

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. [Create the rest api](../../../../develop/creating-artifacts/creating-an-api) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Configuring the user store:

1.  Create your user store and host it because WSO2 Micro Integrator does not include a default user store.
2.  Once hosted, configure the following properties in the `MI_HOME/conf/deployment.toml` file with your user store's details. For this sample, we configure a read- only LDAP user store.

    ```toml
    [user_store]
    #usr-mgt.xml
    class = "org.wso2.micro.integrator.security.user.core.ldap.ReadOnlyLDAPUserStoreManager" # inferred
    connection_url = "ldap://localhost:10389"   #inferred
    connection_name = "uid=admin,ou=system"   #inferred
    connection_password = "admin"   #inferred
    anonymous_bind = false   #inferred
    user_search_base = "ou=system"   #inferred
    user_name_attribute = "uid"   #inferred
    user_name_search_filter = "(&amp;(objectClass=person)(uid=?))"   #inferred
    user_name_list_filter = "(objectClass=person)"   #inferred
    display_name_attribute = ''     # empty by default
    read_groups = true   #inferred
    group_search_base = "ou=system"   #inferred
    group_name_attribute = "cn"   #inferred
    group_name_search_filter = "(&amp;(objectClass=groupOfNames)(cn=?))"   #inferred
    group_name_list_filter = "(objectClass=groupOfNames)"   #inferred
    membership_attribute = "member"   #inferred
    back_links_enabled = false   #inferred
    username_java_regex = "[a-zA-Z0-9._\\-|//]{3,30}$"   #inferred
    rolename_java_regex = "[a-zA-Z0-9._\\-|//]{3,30}$"   #inferred
    password_java_regex = "^[\\S]{5,30}$"   #inferred
    scim_enabled = false   #inferred
    password_hash_method = "PLAIN_TEXT"   #inferred
    multi_attribute_separator = ","   #inferred
    max_user_name_list_length = 100   #inferred
    max_role_name_list_length = 100   #inferred
    user_roles_cache_enabled = true   #inferred
    connection_pooling_enabled = true   #inferred
    ldap_connection_timeout = 5000   #inferred          timeout in milliseconds
    read_timeout = '' # empty by default
    retry_attempts = '' # empty by default
    replace_escape_characters_at_user_login = true   #inferred
    ```

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
            If you are connecting over LDAPS (secured LDAP), you need to import the certificate of the user store to the <code>                     client-truststore.jks                    </code> of the WSO2 product. For information on how to add certificates to the truststore, and how keystores are configured and used in a system, see <a href="../../setup/security/creating_keystores">using keystores</a> .<br />
            </td>
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

Testing the API:

1.  First, invoke the service using the following service URL without providing any user credentials: `http://127.0.0.1:8290/stockquote/view/IBM`

    !!! Info
        You can invoke the service using Postman or Curl.
        
    ```bash
    curl -v http://127.0.0.1:8290/stockquote/view/IBM
    ```
    
    Note that you will receive the following error because the username and password are not passed and the service cannot be authenticated: `'401 Unauthorized'`

2.  Now, invoke the service again by providing the credentials of a user that is registered in the user store that is hosted.

    ```bash
    curl -v http://127.0.0.1:8290/stockquote/view/IBM -H "Authorization: Basic YWRtaW46YWRtaW4="
    ```

    The request is passed to the back-end service and you will receive a response similar to what is shown below:

    ```xml
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
