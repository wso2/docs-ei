# Enabling Logs for APIs

The advantage of having per-API log files is that it is very easy to analyze/monitor what went wrong in a particular REST API defined in WSO2 Micro Integrator by looking at the log files. The API log is an additional log file, which will contain a copy of the logs to a particular REST API.

Below are the configuration details to configure the logs of a REST API called `TestAPI` using `log4j`
properties.

Open `MI_HOME/conf/log4j2.properties` file using your favorite text editor to configure `         log4j        ` to log the
API specific logs to a file. You can configure the logger for either [INFO level logs](#info-level) or [DEBUG level logs](#debug-level) as follows:

## INFO level

1. Add the following section to the `log4j2.properties` file to configure the Appender for log messages where the **Log Category** is **INFO**.

	```bash
	# API_APPENDER is set to be a DailyRollingFileAppender using a PatternLayout.
	appender.API_APPENDER.type = RollingFile
	appender.API_APPENDER.name = API_APPENDER
	appender.API_APPENDER.fileName = ${sys:carbon.home}/repository/logs/wso2-ei-api.log
	appender.API_APPENDER.filePattern = ${sys:carbon.home}/repository/logs/wso2-ei-api-%d{MM-dd-yyyy}.log
	appender.API_APPENDER.layout.type = PatternLayout
	appender.API_APPENDER.layout.pattern = TID: [%d] %5p {%c} [%logger] - %m%ex%n
	appender.API_APPENDER.policies.type = Policies
	appender.API_APPENDER.policies.time.type = TimeBasedTriggeringPolicy
	appender.API_APPENDER.policies.time.interval = 1
	appender.API_APPENDER.policies.time.modulate = true
	appender.API_APPENDER.policies.size.type = SizeBasedTriggeringPolicy
	appender.API_APPENDER.policies.size.size=10MB
	appender.API_APPENDER.strategy.type = DefaultRolloverStrategy
	appender.API_APPENDER.strategy.max = 20
	appender.API_APPENDER.filter.threshold.type = ThresholdFilter
	appender.API_APPENDER.filter.threshold.level = INFO
	```

2. Add `API_APPENDER` as an appender
        
	```xml
	appenders = CARBON_CONSOLE, CARBON_LOGFILE, AUDIT_LOGFILE, API_APPENDER, 
	```

3. Add a logger to filter out `TestAPI` related logs
	```xml
	logger.API_LOG.name=API_LOGGER.TestAPI
	logger.API_LOG.level=INFO
	logger.API_LOG.appenderRef.API_APPENDER.ref = API_APPENDER
	logger.API_LOG.additivity=false
	```
4.  Add `API_LOG` as a logger
	```xml
	loggers = AUDIT_LOG, API_LOG, SERVICE_LOGGER,
	```  

!!! Tip
    The in-sequence of the REST API will need to contain a **Log mediator** with the **Log Category** defined as **INFO** to be able to view logs in the log file.

## DEBUG level

Change the threshold level to configure the logger for log messages where the **Log Category** is **DEBUG**.

```xml
appender.API_APPENDER.filter.threshold.level = DEBUG
```
    
!!! Tip
    -	The above configuration creates a log file names `TestAPI.log` in the folder `MI_HOME/repository/logs` .
    -	The in-sequence of the REST API will need to contain a **Log mediator** with the **Log Category** defined as **DEBUG** to be able to view logs in the log file.
