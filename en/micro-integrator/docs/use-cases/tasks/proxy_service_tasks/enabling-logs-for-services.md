# Enabling Logs for Services

The advantage of having per-service log files is that it is very easy to
analyze/monitor what went wrong in this particular service (proxy
service, data service etc.) by looking at the service log. Enabling this
feature will not terminate the `         wso2-ei.log        ` file. This
file will contain the complete log with every log statement, including
the service logs that you have configured to be logged into a different
log file. In other words, the service log is an additional log file,
which will contain a copy of the logs to that particular service.

Follow the instructions below to configure the logs of a particular
service to be logged into a given log file.  The following steps explain
how to enable logs for a sample proxy service deployed in the ESB
profile of WSO2 EI.

1.  See [Sample
    150](https://docs.wso2.com/display/EI600/Sample+150%3A+Introduction+to+Proxy+Services)
    in [Proxy Service
    Samples](https://docs.wso2.com/display/EI650/Proxy+Service+Samples)
    . It has a proxy service named `          StockQuoteProxy         `
    .
2.  Configure `          log4j         ` to log the service
    specific logs to a file called
    `          stock-quote-proxy-service.log         ` in the logs
    directory of the EI installation directory.
    1.  Open up the `             log4j.properties            `
        file found in the `             conf            ` directory of
        the WSO2 EI installation directory using your favorite text
        editor and add the following section to the end of the file
        starting in a new line.

        ``` java
                log4j.category.SERVICE_LOGGER.StockQuoteProxy=DEBUG, SQ_PROXY_APPENDER
                log4j.additivity.SERVICE_LOGGER.StockQuoteProxy=false
                log4j.appender.SQ_PROXY_APPENDER=org.apache.log4j.DailyRollingFileAppender
                log4j.appender.SQ_PROXY_APPENDER.File=logs/stock-quote-proxy-service.log
                log4j.appender.SQ_PROXY_APPENDER.datePattern='.'yyyy-MM-dd-HH-mm
                log4j.appender.SQ_PROXY_APPENDER.layout=org.apache.log4j.PatternLayout
                log4j.appender.SQ_PROXY_APPENDER.layout.ConversionPattern=%d{ISO8601} \[%X{ip}-%X{host}\] \[%t\] %5p %c{1} %m%n
        ```

    2.  Save the file.

3.  Try it out. By default, the configuration does not do any logging at
    runtime, so configure the Proxy Service in-sequence to contain a log
    mediator to log the message at "Full" log level.
4.  Execute the sample client after starting the server with sample 150:
    and the sample axis2 server with the
    `           SimpleStockQuote          ` service deployed on it as
    per stated in the sample documentation.

    ``` java
            <EI_HOME>/samples/bin/wso2ei-samples.sh -sn 150
    ```

5.  Inspect the logs directory of the ESB profile's installation
    directory to see the
    `          stock-quote-proxy-service.log file         ` .

Further, to demonstrate the log file rotation; this particular logger
was configured to rotate the file in each minute whenever there is a log
going into the service log. Therefore, if you execute the sample client
once again after 1 minute, you will be able to see the service log file
rotation as well.
