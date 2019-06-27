# Securing Carbon Applications

When you work with WSO2 products, you may host multiple applications
(Web applications and Jaggery applications) and expose them to external
users. Before you expose the applications to the external network, be sure to
configure the security settings for all your applications.

## Restricting access to Carbon applications

When hosting your products in production, it's imperative that you
restrict the access to the management console from the external network.
Additionally, you may also need to restrict access to other
applications. Accessing Carbon applications in your server (including
the management console) can be restricted to selected IP addresses. You
can use the Tomcat servlet filter (
`         org.apache.catalina.filters.RemoteAddrFilter        ` ) for
the purpose of restricting access.

**Note** that you can either restrict access to the management console
exclusively, or you can restrict access to all web applications in your
server (which includes the management console) at the same time.  

### For jaggery Apps only

If you want only selected IP addresses to be able to access the jaggery
apps like publisher, store, admin portal which resides in WSO2 API
Manager, go
to  \<PRODUCT\_HOME\>/repository/deployment/server/jaggeryapps/\<jaggeryapp-name\>/jaggery.conf file
and add the configuration as follows.

``` java
    {
       "name":"Remote Address Filter",
       "class":"org.apache.catalina.filters.RemoteAddrFilter",
       "params":[
           {"name" : "allow", "value" : "localhost|127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|your IP added here"}
        ]
    }
    { "name":"Remote Address Filter", "url":"*" }
```

The value of the "allow" key that we defined in this file will apply to
the publisher/store/admin portal as the the particular jagger.conf file
you add this configuration and, thereby, all outside requests to the
publisher/store/admin portal will be blocked.

### For all web applications

To control access to all web applications deployed in your server
(including the management console), add the IP addresses to the
`         <PRODUCT_HOME>/repository/conf/context.        ` xml file as
follows:  

``` java
    <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="<IP-address-01>|<IP-address-02>|<IP-address-03>"/>
```

The `         RemoteAddrValve        ` Tomcat valve defined in this file
will apply to each web application hosted on the Carbon server.
Therefore, all outside requests to any web application will be blocked.

### For web application servlets

You can also restrict access to particular servlets in a web application
by adding a Remote Address Filter to the `         web.xml        ` file
(stored in the `         <PRODUCT_HOME>/repository/conf/tomcat/        `
directory), and by mapping that filter to the servlet URL. In the Remote
Address Filter that you add, you can specify the IP addresses that
should be allowed to access the servlet.

  
The following example `         web.xml        ` file illustrates how
access to the `         login.jsp        ` of the management console (
`         /carbon/admin/login.jsp        ` ) is granted only to one IP
address:  

``` java
    <filter>
        <filter-name>Remote Address Filter</filter-name>
        <filter-class>org.apache.catalina.filters.RemoteAddrFilter</filter-class>
            <init-param>
                <param-name>allow</param-name>
                <param-value>127.0.01</param-value>
            </init-param>
    </filter>
    
    <filter-mapping>
        <filter-name>Remote Address Filter</filter-name>
        <url-pattern>/carbon/admin/login.jsp</url-pattern>
    </filter-mapping>
```

## Enabling HTTP Strict Transport Security (HSTS) Headers

Enable "HTTP Strict Transport Security headers" (HSTS) for the
applications deployed in your server, to confirm that the relevant
headers are present in the HTTP response. HSTS is not enabled for
applications in WSO2 products by default.

> Note that HSTS should not be enabled in **development environments**
because transport security validations can interrupt the development
processes by validating signatures of self-signed certificates.

### For web applications

Similar to the management console, check whether the
`         HttpHeaderSecurityFilter        ` (stored in the
`         <PRODUCT_HOME>        `
`         /repository/deployment/server/webapps/        ` directory) is
available in the web.xml file of that particular web application. If the
filter is available, enable HSTS as shown below.

``` java
    <filter>
        <filter-name>HttpHeaderSecurityFilter</filter-name>        
        <filter-class>org.apache.catalina.filters.HttpHeaderSecurityFilter</filter-class> 
    </filter>
    <filter-mapping>     
        <filter-name>HttpHeaderSecurityFilter</filter-name>     
        <url-pattern>*</url-pattern> 
    </filter-mapping>
```
### For Jaggery applications

For Jaggery applications, the
`         HttpHeaderSecurityFilter        ` element should be configured
in the `         jaggery.conf        ` file (stored in the
`         <PRODUCT_HOME>        `
`         /repository/deployment/server/jaggeryapps/        `
directory). This filter configuration is applicable to the /dashboard
jaggery applications in this location. To enable H STS for a Jaggery
application, change the default filter configuration as shown below.

The default filter configuration:

``` java
     "params" : [{"name" : "hstsEnabled", "value" : "false"}]
```

The filter configuration after enabling HSTS:

``` java
    "params" : [{"name" : "hstsMaxAgeSeconds", "value" : "15768000"}]
```

> **NOTE:** Returning HTTP security headers could also be achieved by
configuring those headers from the Proxy/LB configuration.


## Preventing browser caching

If there are dynamic pages in your application, which also include
sensitive information, you need to prevent caching. This can be done by
making sure that the applications return the following HTTP security
headers in HTTP responses.

Expires:0  
Pragma:no-cache  
Cache-Control:no-store, no-cache, must-revalidate

The following topics explain how you can configure these security
headers for different types of applications used in WSO2 products.

### For web applications

If your web application (stored in the
`          <PRODUCT_HOME>/repository/deployment/server/webapps/         `
directory) serves dynamic pages/content, then make sure that either
`          URLBasedCachePreventionFilter         ` or
`          ContentTypeBasedCachePreventionFilter         ` is available
in the `          web.xml         ` file of the particular application.

Note that the applications that are included in the
`          /         ` webapps directory by default in a WSO2 product do
not serve sensitive content that requires cache prevention. However, if
you are adding any new applications, you need to be mindful of this
requirement.

### For Jaggery applications

For Jaggery-based applications (stored in the
`         <PRODUCT_HOME>/repository/deployment/server/jaggeryapps/        `
directory), either `         URLBasedCachePreventionFilter        ` or
`         ContentTypeBasedCachePreventionFilter        ` should be
available in the `         jaggery.conf        ` file as shown below.

``` java
    "filters":
    [{"name": "ContentTypeBasedCachePreventionFilter","class": "org.wso2.carbon.ui.filters.cache.ContentTypeBasedCachePreventionFilter","params": 
     [{"name":"patterns","value":"text/html\",application/json\",plain/text"},{"name" : "filterAction","value":"enforce"},  {"name":"httpHeaders","value": "Cache-Control: no-store, no-cache, must-revalidate, private"}]        
    }],
```
