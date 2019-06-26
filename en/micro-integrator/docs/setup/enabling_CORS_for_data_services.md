# Enabling CORS for Data Services

You can enable Cross Origin Resource Sharing for data services deployed
in the ESB of WSO2 EI. As explained below, you have the option of
enabling CORS for selected data services or for all the data services.

To enable CORS:

1.  Download the following JARs: "
    [cors-filter-2.4.jar](https://docs.wso2.com/search.maven.org/remotecontent?filepath=com/thetransactioncompany/cors-filter/2.4/cors-filter-2.4.jar)
    " and "
    [java-property-utils-1.9.1.jar](https://docs.wso2.com/search.maven.org/remotecontent?filepath=com/thetransactioncompany/java-property-utils/1.9.1/java-property-utils-1.9.1.jar)
    ".
2.  Copy the JARs to the `          <EI_HOME>/lib/         ` directory.
3.  Add the following configurations to the
    `           <EI_HOME>/conf/tomcat/web.xml          ` file.

    ``` java
        <filter>  
            <filter-name>CORS</filter-name>
            <filter-class>com.thetransactioncompany.cors.CORSFilter</filter-class>  
        </filter>
        <filter-mapping>
            <filter-name>CORS</filter-name>
            <url-pattern>/*</url-pattern>
        </filter-mapping>
    ```

4.  Edit the `           <filter-mapping>          ` section in the
    above configuration to specify whether CORS should be enabled for
    all data services or only selected data services.

    -   To enable CORS for a selected data service, add the service's
        url as the url pattern:

        ``` java
                <filter-mapping>
                    <filter-name>CORS</filter-name>
                    <url-pattern>/services/example/*</url-pattern>
                </filter-mapping>
        ```

    -   To enable CORS for multiple data service, you add the urls in a
        comma separated list:

        ``` java
                    <filter-mapping>
                        <filter-name>CORS</filter-name>
                        <url-pattern>/services/sampleservice1/*,/services/sampleservice2/*</url-pattern>
                    </filter-mapping>
        ```

        Alternatively, you can add two separate filter mappings:

        ``` java
                    <filter-mapping>
                        <filter-name>CORS</filter-name>
                        <url-pattern>/services/sampleservice1/*</url-pattern>
                    </filter-mapping>
                    <filter-mapping>
                        <filter-name>CORS</filter-name>
                        <url-pattern>/services/sampleservice2/*</url-pattern>
                    </filter-mapping>
        ```

5.  Restart the server.
