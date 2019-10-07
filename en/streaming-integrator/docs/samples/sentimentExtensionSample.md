
## Purpose:
This application demonstrates calculating the sentiment value for a given string as per Affin word list.

## Prerequisites:
1) Save this sample.
2) If there is no syntax error, the following message is shown on the console.
	```
	* Siddhi App sentimentExtensionSample successfully deployed.
	```

## Executing the Sample:
1) Start the Siddhi application by clicking on 'Run'.
2) If the Siddhi application starts successfully, the following messages are shown on the console.
	```
	* sentimentExtensionSample.siddhi - Started Successfully!
	```

## Testing the Sample:
Send events through one or more of the following methods.

##### Send events to `userWallPostStream`, via event simulator.
1. Open the event simulator by clicking on the second icon or pressing Ctrl+Shift+I.
2. In the Single Simulation tab of the panel, specify the values as follows:
	* Siddhi App Name: sentimentExtensionSample
	* Stream Name: userWallPostStream
3. In the userId and text fields, enter the following and then click Send to send the event.
	```
	userId: "Mohan"
	text: "David is a good person. David is a bad person"
	```
4. Send another event.
	```
	userId: "Nuwan",
	text: "David is a good person."
	```

##### Send events to the simulator http endpoint through the curl commands.
1. Open a new terminal and issue the following commands one after the other:
	```bash
	curl -X POST \
		http://localhost:9390/simulation/single \
		-u admin:admin \
		-H 'content-type: text/plain' \
		-d '{
			"siddhiAppName": "sentimentExtensionSample",
			"streamName": "userWallPostStream",
			"timestamp": null,
			"data": [
				"Mohan",
				"David is a good person. David is a bad person"
			]
		}'
	```

	```bash
	curl -X POST \
		http://localhost:9390/simulation/single \
		-u admin:admin \
		-H 'content-type: text/plain' \
		-d '{
			"siddhiAppName": "sentimentExtensionSample",
			"streamName": "userWallPostStream",
			"timestamp": null,
			"data": [
				"Nuwan",
				"David is a good person."
			]
		}'
	```

2. If there is no error, the following messages are shown on the terminal.
	```json
	{"status":"OK","message":"Single Event simulation started successfully"}
	```

##### Publish events with Postman.
1. Install 'Postman' application from Chrome web store.
2. Launch the application.
3. Make 'POST' request to the 'http://localhost:9390/simulation/single' endpoint. Set the Content-Type to 'text/plain'.
4. Set the following request body in text and send the first event.
	```json
	{
		"streamName": "userWallPostStream",
		"siddhiAppName": "sentimentExtensionSample",
		"data":  [
			"Mohan",
			"David is a good person. David is a bad person"
		]
	}
	```
5. If there are no errors, the following messages are shown on the console:
	```
	* "status": "OK",
	* "message": "Single Event simulation started successfully"
	```
6. Send another event by setting the request body in text.
	```json
	{
		"streamName": "userWallPostStream",
		"siddhiAppName": "sentimentExtensionSample",
		"data":  [
			"Nuwan",
			"David is a good person."
		]
	}
	```

## Viewing the Results:
See the output on the terminal.
```
INFO {io.siddhi.core.stream.output.sink.LogSink} - sentimentExtensionSample : outputStream : Event{timestamp=1513619669217, data=[Mohan , David is a good person. David is a bad person, 0], isExpired=false}
INFO {io.siddhi.core.stream.output.sink.LogSink} - sentimentExtensionSample : outputStream : Event{timestamp=1513619687568, data=[Nuwan , David is a good person., 3], isExpired=false}
```

```sql
@APP:name('sentimentExtensionSample')
@App:Description('Demonstrates calculating the sentiment value for a given string as per Affin word list..')

-- Please refer to https://docs.wso2.com/display/SP400/Quick+Start+Guide on getting started with SP editor.

define stream userWallPostStream (userId string, text string);

@sink(type='log')
define stream outputStream(userId string, text string, rate int) ;

from userWallPostStream
select userId, text, sentiment:getRate(text) as rate
insert into outputStream;
```