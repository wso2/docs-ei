# Enabling Logs for APIs

The advantage of having per-API log files is that it is very easy to
analyze/monitor what went wrong in a particular REST API defined in the
ESB profile by looking at the log files. The API log is an additional
log file, which will contain a copy of the logs to a particular REST
API.

Below are the configuration details to configure the logs of a REST API
called `         TestAPI        ` using `         log4j        `
properties.

Open `         <EI_HOME>/conf/log4j.properties        ` file using your
favorite text editor to configure `         log4j        ` to log the
API specific logs to a file. You can configure the logger for either
[INFO level logs](#EnablingLogsforAPIs-infoLevel) or [DEBUG
level logs](#EnablingLogsforAPIs-debugLevel) as follows:

##### INFO level

Add the following section to the end of the file to configure the logger
for log messages where the Log Category is **INFO.**

``` java
    log4j.category.API_LOGGER=INFO, API_APPENDER
    log4j.additivity.API_LOGGER=false
    log4j.appender.API_APPENDER=org.apache.log4j.RollingFileAppender
    log4j.appender.API_APPENDER.File=${carbon.home}/repository/logs/${instance.log}/wso2-ei-api${instance.log}.log
    log4j.appender.API_APPENDER.MaxFileSize=1000KB
    log4j.appender.API_APPENDER.MaxBackupIndex=10
    log4j.appender.API_APPENDER.layout=org.apache.log4j.PatternLayout
    log4j.appender.API_APPENDER.layout.ConversionPattern=%d{ISO8601} [%X{ip}-%X{host}] [%t] %5p %c{1} %m%n</pre>
```

!!! tip

The in-sequence of the REST API will need to contain a [Log
mediator](https://docs.wso2.com/display/EI650/Log+Mediator) with the Log
Category defined as **INFO** to be able to view logs in the log file.


##### DEBUG level

Add the following section to the end of the file to configure the logger
for log messages where the Log Category is **DEBUG.**

``` java
log4j.additivity.API_LOGGER.TestAPI=false
```
    
!!! info

The above configuration creates a log file names
`         TestAPI.log        ` in the folder
`         <EI_HOME>/repository/logs        ` .

!!! tip

The in-sequence of the REST API will need to contain a [Log
mediator](https://docs.wso2.com/display/EI650/Log+Mediator) with the Log
Category defined as **DEBUG** to be able to view logs in the log file.

