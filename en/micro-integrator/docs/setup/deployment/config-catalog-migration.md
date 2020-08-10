# Migrating Product Configurations

The Micro Integrator of WSO2 Enterprise Integrator 7.0 introduces TOML-based product configurations. 
All the server-level configurations defined in config files has to be moved to the deployment.toml file (stored in the MI_HOME/conf directory).

## carbon.xml

-	hostname

    ```xml tab='xml configuration'
	<HostName>www.wso2.org</HostName>
	```

	```toml tab='TOML configuration'
	[server]
    hostname = "www.wso2.org"
	```
Find more [parameters](../../../references/config-catalog/#deployment).

-	port offset

    ```xml tab='xml configuration'
	<Ports><Offset>1</Offset></Ports>
	```

	```toml tab='TOML configuration'
        [server] 
        offset  = 0
	```
Find more [parameters](../../../references/config-catalog/#deployment).

-	primary keystore

    ```xml tab='xml configuration'
	<Security>
            <KeyStore>            
                <Location>${carbon.home}/repository/resources/security/wso2carbon.jks</Location>
                <Type>JKS</Type>
                <Password>wso2carbon</Password>
                <KeyAlias>wso2carbon</KeyAlias>
                <KeyPassword>wso2carbon</KeyPassword>
            </KeyStore>
        </Security>
	```

	```toml tab='TOML configuration'
	[keystore.primary] 
    file_name = "wso2carbon.jks"
    type = "JKS"
    password = "wso2carbon"
    alias = "wso2carbon"
    key_password = "wso2carbon"
	```
Find more [parameters](../../../references/config-catalog/#primary-keystore).

-	internal keystore

    ```xml tab='xml configuration'
	<InternalKeyStore>
             <Location>${carbon.home}/repository/resources/security/wso2carbon.jks</Location>
                <Type>JKS</Type>
                <Password>wso2carbon</Password>
                <KeyAlias>wso2carbon</KeyAlias>
               <KeyPassword>wso2carbon</KeyPassword>
        </InternalKeyStore>
	```

	```toml tab='TOML configuration'
	[keystore.internal] 
    file_name = "wso2carbon.jks"
    type = "JKS"
    password = "wso2carbon"
    alias = "wso2carbon"
    key_password = "wso2carbon" 
	```
Find more [parameters](../../../references/config-catalog/#internal-keystore).

-	truststore

    ```xml tab='xml configuration'
	<TrustStore>
                <Location>${carbon.home}/repository/resources/security/client-truststore.jks</Location>
                <Type>JKS</Type>
                <Password>wso2carbon</Password>
            </TrustStore>
	```

	```toml tab='TOML configuration'
	[truststore] 
    file_name = "client-truststore.jks"  
    type = "JKS"                        
    password = "wso2carbon"            
    alias = "symmetric.key.value"      
    algorithm = "AES"
	```
Find more [parameters](../../../references/config-catalog/#trust-store).

## user-mgt.xml

-	admin user config

    ```xml tab='xml configuration'
	<Realm>
            <Configuration>
                <AdminRole>admin</AdminRole>
                <AdminUser>                
                    <UserName>admin</UserName>                
                    <Password>admin</Password>
                </AdminUser>
            </Configuration>
    </Realm>
	```

	```toml tab='TOML configuration'
	[super_admin]
    username = "admin"              # inferred 
    password = "admin"              # inferred 
    admin_role = "admin"            # inferred
	```
	
-	user datasource

    ```xml tab='xml configuration'
	<Realm>
            <Configuration>
                 <Property name="isCascadeDeleteEnabled">true</Property>
                 <Property name="dataSource">jdbc/WSO2CarbonDB</Property>
            </Configuration>
    </Realm>

	```

	```toml tab='TOML configuration'
    [realm_manager] 
    data_source = "WSO2CarbonDB"       
    properties.isCascadeDeleteEnabled = true   
	```

-	LDAP userstore

    ```xml tab='xml configuration'
	<UserStoreManager class="org.wso2.carbon.user.core.ldap.ReadOnlyLDAPUserStoreManager">
                <Property name="TenantManager">org.wso2.carbon.user.core.tenant.CommonHybridLDAPTenantManager</Property>
                <Property name="ConnectionURL">ldap://localhost:10389</Property>
                <Property name="ConnectionName">uid=admin,ou=system</Property>
    </UserStoreManager>
	```

	```toml tab='TOML configuration'
	[internal_apis.file_user_store]
    enable = false
    
	[user_store]
    type = "read_only_ldap" # inferred default read_only_ldap # OR  read_write_ldap
    class = "org.wso2.micro.integrator.security.user.core.ldap.ReadOnlyLDAPUserStoreManager" # inferred
    connection_url = "ldap://localhost:10389"   #inferred
    connection_name = "uid=admin,ou=system"   #inferred
	```
Find more [parameters](../../../references/config-catalog/#ldap-user-store).

-	JDBC userstore

    ```xml tab='xml configuration'
	 <UserStoreManager class="org.wso2.carbon.user.core.jdbc.JDBCUserStoreManager">
                <Property name="TenantManager">org.wso2.carbon.user.core.tenant.JDBCTenantManager</Property>
     </UserStoreManager>
	```

	```toml tab='TOML configuration'
	[internal_apis.file_user_store]
    enable = false
    
    [user_store]
    class = "org.wso2.micro.integrator.security.user.core.jdbc.JDBCUserStoreManager"
    type = "database"
	```

## master-datasource.xml

-	WSO2_CARBON_DB

    ```xml tab='xml configuration'
	<datasource>
        <name>WSO2_CARBON_DB</name>
        <description>The datasource used for registry and user manager</description>
        <jndiConfig>
            <name>jdbc/WSO2CarbonDB</name>
        </jndiConfig>
        <definition type="RDBMS">
            <configuration>
                <url>jdbc:h2:./repository/database/WSO2CARBON_DB;DB_CLOSE_ON_EXIT=FALSE;LOCK_TIMEOUT=60000</url>
                <username>wso2carbon</username>
                <password>wso2carbon</password>
                <driverClassName>org.h2.Driver</driverClassName>
                <maxActive>50</maxActive>
                <maxWait>60000</maxWait>
                <testOnBorrow>true</testOnBorrow>
            </configuration>
        </definition>
    </datasource>
	```

	```toml tab='TOML configuration'
	[[datasource]]
    id = "WSO2_CARBON_DB"
    url= "jdbc:h2:./repository/database/WSO2CARBON_DB;DB_CLOSE_ON_EXIT=FALSE;LOCK_TIMEOUT=60000"
    username="username"
    password="password"
    driver="org.h2.Driver"
    pool_options.maxActive=50
    pool_options.maxWait = 60000 # wait in milliseconds
    pool_options.testOnBorrow = true
	```
Find more [parameters](../../../references/config-catalog/#database-connection).

## axis2.xml

-	hot deployment

    ```xml tab='xml configuration'
	<parameter name="hotdeployment" locked="false">true</parameter>
	```

	```toml tab='TOML configuration'
	[server]
    hot_deployment = true 
	```
-	enable MTOM

    ```xml tab='xml configuration'
	<parameter name="enableMTOM" locked="false">false</parameter>
	```

	```toml tab='TOML configuration'
	[server]
    enable_mtom = false
	```
Find more [parameters](../../../references/config-catalog/#deployment).

-	enable SWA

    ```xml tab='xml configuration'
	<parameter name="enableSwA" locked="false">false</parameter>
	```

	```toml tab='TOML configuration'
	[server]
    enable_swa = false
	```
Find more [parameters](../../../references/config-catalog/#deployment).

-	message formatters

    ```xml tab='xml configuration'
	<messageFormatters>
            <messageFormatter contentType="application/x-www-form-urlencoded"
                              class="org.apache.synapse.commons.formatters.XFormURLEncodedFormatter"/>
            <messageFormatter contentType="multipart/form-data"
                              class="org.apache.axis2.transport.http.MultipartFormDataFormatter"/>
            <messageFormatter contentType="application/xml"
                              class="org.apache.axis2.transport.http.ApplicationXMLFormatter"/>
            <messageFormatter contentType="text/xml"
                             class="org.apache.axis2.transport.http.SOAPMessageFormatter"/>
            <messageFormatter contentType="application/soap+xml"
                             class="org.apache.axis2.transport.http.SOAPMessageFormatter"/>
            <messageFormatter contentType="text/plain"
                             class="org.apache.axis2.format.PlainTextFormatter"/>
            <messageFormatter contentType="application/octet-stream"
                              class="org.wso2.carbon.relay.ExpandingMessageFormatter"/>
            <messageFormatter contentType="application/json"
                              class="org.wso2.carbon.integrator.core.json.JsonStreamFormatter"/>
    </messageFormatters>                         
	```

	```toml tab='TOML configuration'
	[message_formatters]
    form_urlencoded =  "org.apache.synapse.commons.formatters.XFormURLEncodedFormatter"
    multipart_form_data =  "org.apache.axis2.transport.http.MultipartFormDataFormatter"
    application_xml = "org.apache.axis2.transport.http.ApplicationXMLFormatter"
    text_xml = "org.apache.axis2.transport.http.SOAPMessageFormatter"
    soap_xml = "org.apache.axis2.transport.http.SOAPMessageFormatter"
    text_plain = "org.apache.axis2.format.PlainTextFormatter"
    application_json =  "org.wso2.micro.integrator.core.json.JsonStreamFormatter"
    octet_stream = "org.wso2.carbon.relay.ExpandingMessageFormatter"
    ```
Find more [parameters](../../../references/config-catalog/#message-formatters-non-blocking-mode).

-	message builders

    ```xml tab='xml configuration'
	<messageBuilders>
            <messageBuilder contentType="application/xml"
                            class="org.apache.axis2.builder.ApplicationXMLBuilder"/>
            <messageBuilder contentType="application/x-www-form-urlencoded"
                            class="org.apache.synapse.commons.builders.XFormURLEncodedBuilder"/>
            <messageBuilder contentType="multipart/form-data"
                            class="org.apache.axis2.builder.MultipartFormDataBuilder"/>
            <messageBuilder contentType="text/plain"
                            class="org.apache.axis2.format.PlainTextBuilder"/>
            <messageBuilder contentType="application/octet-stream"
                            class="org.wso2.carbon.relay.BinaryRelayBuilder"/>
            <messageBuilder contentType="application/json"
                            class="org.wso2.carbon.integrator.core.json.JsonStreamBuilder"/>
    </messageBuilders>                                               
	```

	```toml tab='TOML configuration'
	 [message_builders]
     application_xml = "org.apache.axis2.builder.ApplicationXMLBuilder"
     form_urlencoded = "org.apache.synapse.commons.builders.XFormURLEncodedBuilder"
     multipart_form_data = "org.apache.axis2.builder.MultipartFormDataBuilder"
     text_plain = "org.apache.axis2.format.PlainTextBuilder"
     application_json = "org.wso2.micro.integrator.core.json.JsonStreamBuilder"
     octet_stream =  "org.wso2.carbon.relay.BinaryRelayBuilder"
	```
Find more [parameters](../../../references/config-catalog/#message-builders-non-blocking-mode).

-	HTTP transport receiver

    ```xml tab='xml configuration'
	<transportReceiver name="http" class="org.apache.synapse.transport.passthru.PassThroughHttpListener">
            <parameter name="port" locked="false">8280</parameter>
            <parameter name="non-blocking" locked="false">true</parameter>
            <parameter name="bind-address" locked="false">hostname or IP address</parameter>
            <parameter name="WSDLEPRPrefix" locked="false">https://apachehost:port/somepath</parameter>      
    </transportReceiver>                                             
	```

	```toml tab='TOML configuration'
	 [transport.http] 
     listener.enable = true                     
     listener.port = 8280    
     listener.wsdl_epr_prefix ="https://apachehost:port/somepath" 
     listener.bind_address = "hostname or IP address"
	```
Find more [parameters](../../../references/config-catalog/#https-transport-non-blocking-mode).

-	HTTPS transport receiver

    ```xml tab='xml configuration'
	    <transportReceiver name="https" class="org.apache.synapse.transport.passthru.PassThroughHttpSSLListener">
            <parameter name="port" locked="false">8243</parameter>
            <parameter name="non-blocking" locked="false">true</parameter>
            <parameter name="HttpsProtocols">TLSv1,TLSv1.1,TLSv1.2</parameter>
            <parameter name="bind-address" locked="false">hostname or IP address</parameter>
            <parameter name="WSDLEPRPrefix" locked="false">https://apachehost:port/somepath</parameter>
            <parameter name="keystore" locked="false">
                <KeyStore>
                    <Location>repository/resources/security/wso2carbon.jks</Location>
                    <Type>JKS</Type>
                    <Password>wso2carbon</Password>
                    <KeyPassword>wso2carbon</KeyPassword>
                </KeyStore>
            </parameter>
            <parameter name="truststore" locked="false">
                <TrustStore>
                    <Location>repository/resources/security/client-truststore.jks</Location>
                    <Type>JKS</Type>
                    <Password>wso2carbon</Password>
                </TrustStore>
            </parameter>
        </transportReceiver>                                         
	```

	```toml tab='TOML configuration'
	 [transport.http]
     listener.secured_enable = true              
     listener.secured_port = 8243        
     listener.secured_wsdl_epr_prefix = "https://apachehost:port/somepath"  
     listener.secured_bind_address = "hostname or IP address" 
     listener.secured_protocols = "TLSv1,TLSv1.1,TLSv1.2"   
     listener.keystore.location ="repository/resources/security/wso2carbon.jks"
     listener.keystore.type = "JKS" listener.keystore.password = "wso2carbon"
     listener.keystore.key_password = "wso2carbon"
     listener.truststore.location = "repository/resources/security/client-truststore.jks" 
     listener.truststore.type = "JKS" listener.truststore.password = "wso2carbon"
	```
Find more [parameters](../../../references/config-catalog/#https-transport-non-blocking-mode).

-	VFS transport receiver

    ```xml tab='xml configuration'
	<transportReceiver name="vfs" class="org.apache.synapse.transport.vfs.VFSTransportListener"/>                                            
	```

	```toml tab='TOML configuration'
	 [transport.vfs] ]
     listener.enable = true
	```
Find more [parameters](../../../references/config-catalog/#vfs-transport).

-	mailto transport receiver

    ```xml tab='xml configuration'
	<transportReceiver name="mailto" class="org.apache.axis2.transport.mail.MailTransportListener"/>	```
    ```
    
	```toml tab='TOML configuration'
	 [transport.mail.listener]
	 enable = true
     name = "mailto"
	```
Find more [parameters](../../../references/config-catalog/#mail-transport-listener-non-blocking-mode).

-	jms transport receiver

    ```xml tab='xml configuration'
	<transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
            <parameter name="myTopicConnectionFactory" locked="false">
            	<parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
            	<parameter name="java.naming.provider.url" locked="false">tcp://localhost:61616</parameter>
            	<parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">TopicConnectionFactory</parameter>
    		    <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
            </parameter>
    </transportReceiver>
    ```
       
	```toml tab='TOML configuration'
	 [transport.jms] 
	 listener_enable = true 
	 
     [[transport.jms.listener]] 
     name = "myTopicListener"
     parameter.initial_naming_factory = "org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory" 
     parameter.provider_url = "tcp://localhost:61616" 
     parameter.connection_factory_name = "TopicConnectionFactory" 
     parameter.connection_factory_type = "topic" # [queue, topic] 
     parameter.cache_level = "consumer"
	```
Find more [parameters](../../../references/config-catalog/#jms-transport-listener-blocking-mode).

-	FIX transport receiver

    ```xml tab='xml configuration'
	<transportReceiver name="fix" class="org.apache.synapse.transport.fix.FIXTransportListener"/>
    ```
       
	```toml tab='TOML configuration'
	 [transport.fix] 
     listener.enable = true
	```
Find more [parameters](../../../references/config-catalog/#fix-transport).

-	RabbitMQ transport receiver

    ```xml tab='xml configuration'
	<transportReceiver name="rabbitmq" class="org.apache.axis2.transport.rabbitmq.RabbitMQListener">
            <parameter name="AMQPConnectionFactory" locked="false">
                <parameter name="rabbitmq.server.host.name" locked="false">localhost</parameter>
                <parameter name="rabbitmq.server.port" locked="false">5672</parameter>
                <parameter name="rabbitmq.server.user.name" locked="false">guest</parameter>
                <parameter name="rabbitmq.server.password" locked="false">guest</parameter>
            </parameter>
    </transportReceiver>
    ```
       
	```toml tab='TOML configuration'
	 [transport.rabbitmq] 
	 listener_enable = true
     
     [[transport.rabbitmq.listener]] 
     name = "AMQPConnectionFactory" 
     parameter.hostname = "localhost" 
     parameter.port = 5672 
     parameter.username = "guest" 
     parameter.password = "guest"
	```
Find more [parameters](../../../references/config-catalog/#rabbitmq-listener).

-	HTTP transport sender

    ```xml tab='xml configuration'
	<transportSender name="http" class="org.apache.synapse.transport.passthru.PassThroughHttpSender">
            <parameter name="non-blocking" locked="false">true</parameter>
     </transportSender>
    ```
       
	```toml tab='TOML configuration'
	 [transport.http] 
     sender.enable = true 
	```
Find more [parameters](../../../references/config-catalog/#https-transport-non-blocking-mode).

-	HTTPS transport sender

    ```xml tab='xml configuration'
	<transportSender name="https" class="org.apache.synapse.transport.passthru.PassThroughHttpSSLSender">
            <parameter name="non-blocking" locked="false">true</parameter>
            <parameter name="keystore" locked="false">
                <KeyStore>
                    <Location>repository/resources/security/wso2carbon.jks</Location>
                    <Type>JKS</Type>
                    <Password>wso2carbon</Password>
                    <KeyPassword>wso2carbon</KeyPassword>
                </KeyStore>
            </parameter>
            <parameter name="truststore" locked="false">
                <TrustStore>
                    <Location>repository/resources/security/client-truststore.jks</Location>
                    <Type>JKS</Type>
                    <Password>wso2carbon</Password>
                </TrustStore>
            </parameter>
    </transportSender>
    ```
       
	```toml tab='TOML configuration'
	 [transport.http] 
     sender.secured_enable = true
     sender.keystore.location ="repository/resources/security/wso2carbon.jks"
     sender.keystore.type = "JKS" 
     sender.keystore.password = "wso2carbon" 
     sender.keystore.key_password = "wso2carbon"
     sender.truststore.location = "repository/resources/security/client-truststore.jks" 
     sender.truststore.type = "JKS" 
     sender.truststore.password = "wso2carbon"
	```
Find more [parameters](../../../references/config-catalog/#https-transport-non-blocking-mode).

-	VFS transport sender

    ```xml tab='xml configuration'
	<transportSender name="vfs" class="org.apache.synapse.transport.vfs.VFSTransportSender"/>
    ```
       
	```toml tab='TOML configuration'
	 [transport.vfs]
     sender.enable = true
	```
Find more [parameters](../../../references/config-catalog/#vfs-transport).

-	VFS transport sender

    ```xml tab='xml configuration'
	<transportSender name="mailto" class="org.apache.axis2.transport.mail.MailTransportSender">
            <parameter name="mail.smtp.host">smtp.gmail.com</parameter>
            <parameter name="mail.smtp.port">587</parameter>
            <parameter name="mail.smtp.starttls.enable">true</parameter>
            <parameter name="mail.smtp.auth">true</parameter>
            <parameter name="mail.smtp.user">synapse.demo.0</parameter>
            <parameter name="mail.smtp.password">mailpassword</parameter>
            <parameter name="mail.smtp.from">synapse.demo.0@gmail.com</parameter>
    </transportSender>
    ```
       
	```toml tab='TOML configuration'
	 [[transport.mail.sender]]
     name = "mailto" 
     parameter.hostname = "smtp.gmail.com" 
     parameter.port = "587" 
     parameter.enable_tls = true 
     parameter.auth = true 
     parameter.username = "synapse.demo.0" 
     parameter.password = "mailpassword" 
     parameter.from = "synapse.demo.0@gmail.com"
	```
Find more [parameters](../../../references/config-catalog/#mail-transport-sender-non-blocking-mode).

-	FIX transport sender

    ```xml tab='xml configuration'
	<transportSender name="fix" class="org.apache.synapse.transport.fix.FIXTransportSender"/>
    ```
       
	```toml tab='TOML configuration'
	 [transport.fix]
     sender.enable = true
	```
Find more [parameters](../../../references/config-catalog/#fix-transport).

-	RabbitMQ transport sender

    ```xml tab='xml configuration'
	<transportSender name="rabbitmq" class="org.apache.axis2.transport.rabbitmq.RabbitMQSender"/>
	```
       
	```toml tab='TOML configuration'
	 [transport.rabbitmq]
     sender_enable = true
	```
Find more [parameters](../../../references/config-catalog/#rabbitmq-sender).

-	RabbitMQ transport sender

    ```xml tab='xml configuration'
    <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender"/>	
    ```
       
	```toml tab='TOML configuration'
	 [transport.jms]
     sender_enable = true
	```
Find more [parameters](../../../references/config-catalog/#jms-transport-sender-non-blocking-mode).

-	Clustering

    ```xml tab='xml configuration'
    <clustering class="org.wso2.carbon.core.clustering.hazelcast.HazelcastClusteringAgent"
                    enable="true">
    <parameter name="clusteringPattern">nonWorkerManager</parameter>
    </clustering>
    ```
       
	```toml tab='TOML configuration'
	 [[datasource]]
     id = "WSO2_COORDINATION_DB"
     url= "jdbc:mysql://localhost:3306/clusterdb"
     username="root"
     password="root"
     driver="com.mysql.jdbc.Driver"
	```
Find more [parameters](../../../setup/deployment/deploying_wso2_ei).

## synapse.properties

-	synapse thread pool properties

    ```xml tab='xml configuration'
    synapse.threads.core = 20
    synapse.threads.max = 100
    ```
       
	```toml tab='TOML configuration'
	 [mediation]
     synapse.core_threads = 20
     synapse.max_threads = 100
	```
Find more [parameters](../../../references/config-catalog/#message-mediation).
