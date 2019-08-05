# Performing Real-time ETL with Files

## Introduction

The Streaming Integrator (SI) allows you to perform real-time ETL with data which are stored in files. 

This tutorial takes you through the different modes and options you could use, in order to perform real-time ETL with files, using the SI. 

## Outline:

- [Preparing the server](#Preparing-the-server)
- [Extracting data from a file](#Extracting-data-from-a-file)
    - [Tailing a text file line by line](#Tailing-a-text-file-line-by-line)
    - [Tailing a text file using a regular expression](#Tailing-a-text-file-using-a-regular-expression)
    - [Reading a text file and moving it after processing](#Reading-a-text-file-and-moving-it-after-processing)
    - [Reading a binary file and moving it after processing](#reading-a-binary-file-and-moving-it-after-processing)
    - [Reading a file line by line and delete it after processing](#Reading-a-file-line-by-line-and-delete-it-after-processing)
    - [Reading a file using a regular expression and deleting it after processing](#reading-a-file-using-a-regular-expression-and-deleting-it-after-processing)

- [Extracting data from a folder](#Extracting-data-from-a-folder)
    - [Processing all files in the folder](#Processing-all-files-in-the-folder)

- [Loading data into a file](#Loading-data-into-a-file)
    - [Appending or over-writing events to a file](#Appending-or-over-writing-events-to-a-file)
    
## Preparing the server 
Navigate to the `<SI_HOME>/bin` directory and issue the following command: `sh server.sh`

You will see following log on the SI console when the server is started successfully.
```
INFO {org.wso2.carbon.kernel.internal.CarbonStartupHandler} - WSO2 Streaming Integrator started in 4.240 sec
``` 
    
## Extracting data from a file

In this section of the tutorial, you are exploring the different ways in which you could extract data, from a specific file.  

### Tailing a text file line by line

In this scenario, you are tailing a text file, line by line, in order to extract data from it. Each line is extracted as an event which goes through a simple transformation thereafter. Let's write a simple Siddhi application to do this.

1. Download `productions.csv` file from [here](todo) and save it in a location of your choice. 

2. Open a text file and copy-paste following Siddhi application to it.
    ```
    @App:name('TailFileLineByLine')
    
    @App:description('Tails a file line by line and does a simple transformation.')
    
    @source(type='file', mode='LINE',
        file.uri='file:/Users/foo/productions.csv',
        tailing='true',
        @map(type='csv'))
    define stream SweetProductionStream (name string, amount double);
    
    @sink(type = 'log')
    define stream LogStream (name string, amount double);
    
    from SweetProductionStream
    select str:upper(name) as name, amount
    insert into LogStream;
    ```
    Change the `file.uri` parameter above, to the file path to which you downloaded `productions.csv` file in step 1. 
    
3. Save this file as `TailFileLineByLine.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.   

    !!!info
        This Siddhi application tails the file `productions.csv` line by line. Each line is converted into an event in `SweetProductionStream`. After that, a simple transformation is done on the sweet productions. That is the `name` attribute from the event is converted into upper case. Finally, the output is logged on the SI console.
        
    Upon successful deployment, following log appears on the SI console:
    ```
    INFO {org.wso2.carbon.streaming.integrator.core.internal.StreamProcessorService} - Siddhi App TailFileLineByLine deployed successfully
    ```
4. Now the Siddhi application starts to process the `productions.csv` file. The file has below two entries:
    ```
    Almond cookie,100.0
    Baked alaska,20.0
    ```  
    As a result, the following log appears in the SI console:
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - ReceiveEventsFromFile : LogStream : Event{timestamp=1564490830652, data=[ALMOND COOKIE, 100.0], isExpired=false}
    INFO {io.siddhi.core.stream.output.sink.LogSink} - ReceiveEventsFromFile : LogStream : Event{timestamp=1564490830657, data=[BAKED ALASKA, 20.0], isExpired=false}
    ```     
            
4. Now append the following line to `productions.csv` file and save the file. 
    ```
    Cup cake,300.0
    ```
5. The following log appears in the SI console:
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - ReceiveEventsFromFile : LogStream : Event{timestamp=1564490869579, data=[CUP CAKE, 300.0], isExpired=false}
    ```
    
### Tailing a text file using a regular expression

In this scenario, you are using a regular expression to extract data from the file. After data has being extracted, a simple transformation if performed on them. Finally, the transformed event is logged on the SI console. Let's write a simple Siddhi application to do this.

1. Download `noisy_data.txt` file from [here](todo) and save it in a location of your choice. 

2. Open a text file and copy-paste following Siddhi application to it.
    ```
    @App:name('TailFileRegex')
    
    @App:description('Tails a file using a regex and does a simple transformation.')
    
    @source(type='file', mode='REGEX',
        file.uri='file:/Users/foo/noisy_data.txt',
        begin.regex='\<', end.regex='\>',
        tailing='true',
        @map(type='text', fail.on.missing.attribute = 'false', regex.A='(\w+)\s([-0-9]+)',regex.B='volume\s([-0-9]+)', @attributes(symbol = 'A[1]',price = 'A[2]',volume = 'B')))
    define stream StockStream (symbol string, price float, volume long);
    
    @sink(type = 'log')
    define stream LogStream (symbol string, price float, volume long);
    
    from StockStream[NOT(symbol is null)]
    select str:upper(symbol) as symbol, price, volume  
    insert into LogStream;
    ```
    Change the `file.uri` parameter above, to the file path to which you downloaded `noisy_data.txt` file in step 1. 
 
3. Save this file as `TailFileRegex.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

    !!!info
        This Siddhi application tails the file `noisy_data.txt` to find matches according to the regular expressions given: `begin.regex` and `end.regex`. Each match is converted into an event in `StockStream`. After that, a simple transformation is done on the `StockStream`. That is the `symbol` attribute from the event is converted into upper case. Finally, the output is logged on the SI console.
        
    Upon successful deployment, following log appears on the SI console:
    ```
    INFO {org.wso2.carbon.streaming.integrator.core.internal.StreamProcessorService} - Siddhi App TailFileRegex deployed successfully
    ```
    
4. Now the Siddhi application starts to process the `noisy_data.txt` file. 
    
    As a result, the following log appears in the SI console:
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - TailFileRegex : LogStream : Event{timestamp=1564583307974, data=[WSO2, 75.0, 100], isExpired=false}
    INFO {io.siddhi.core.stream.output.sink.LogSink} - TailFileRegex : LogStream : Event{timestamp=1564583307975, data=[ORCL, 95.0, 200], isExpired=false}
    ```
            
5. Now append the following text to `noisy_data.txt` file and save the file. 
    ```
    IBM <ibm 88 volume 150> 1 New Orchard Rd  Armonk, NY 10504 
    Phone Number: (914) 499-1900
    Fax Number: (914) 765-6021
    ```
6. The following log appears in the SI console:
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - TailFileRegex : LogStream : Event{timestamp=1564588585214, data=[IBM, 88.0, 150], isExpired=false}
    ```

### Reading a text file and moving it after processing

In the previous scenarios, you tailed a file and each file generated multiple events. In this scenario, you will read the complete file to build a single event. 

1. Download `portfolio.txt` file from [here](todo) and save it in a location of your choice.
 
2. Open a text file and copy-paste following Siddhi application to it.
    ```
    @App:name('TextFullFileProcessing')
        
    @App:description('Reads a text file and moves it after processing.')
    
    @source(type='file', mode='TEXT.FULL',
        file.uri='file:/Users/foo/portfolio.txt',
        action.after.process='MOVE', move.after.process='file:/Users/foo/move.after.process', 
        @map(type='json', enclosing.element="$.portfolio", @attributes(symbol = "stock.company.symbol", price = "stock.price", volume = "stock.volume")))
    define stream StockStream (symbol string, price float, volume long);
     
    @sink(type = 'log')
    define stream LogStream (symbol string, price float, volume long);
     
    from StockStream
    select str:upper(symbol) as symbol, price, volume   
    insert into LogStream;
    ```
 
    Change the `file.uri` parameter above, to the file path to which you downloaded `portfolio.txt` file in step 1. 
 
3. Save this file as `TextFullFileProcessing.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

    !!!info
        This Siddhi application reads the file `portfolio.txt` fully to create a `StockStream` event. After that, a simple transformation is done on the `StockStream`. That is the `symbol` attribute from the event is converted into upper case. Finally, the output is logged on the SI console.
        
    Upon successful deployment, following log appears on the SI console:
    ```
    INFO {org.wso2.carbon.streaming.integrator.core.internal.StreamProcessorService} - Siddhi App TextFullFileProcessing deployed successfully
    ```
 
4. Now the Siddhi application starts to process the `portfolio.txt` file. 
    
    As a result, the following log appears in the SI console:
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - TextFullFileProcessing :  LogStream : Event{timestamp=1564660443519, data=[WSO2, 55.6, 100], isExpired=false} 
    ```
 
!!!info
    In this scenario, you moved the file after processing. To delete a file after processing, remove the parameters: `action.after.process` and `move.after.process` from the Siddhi application. Refer other configuration options in [Siddhi File Source documentation](https://siddhi-io.github.io/siddhi-io-file/api/latest/#file-source). 

### Reading a binary file and moving it after processing

In the previous scenarios, you processed text files, in order to extract data. In this scenario, you are reading a binary file. The content of the file generates a single event.

1. Download `google.bin` file from [here](todo) and save it in a location of your choice.

2. Open a text file and copy-paste following Siddhi application to it.
    ```
    @App:name('BinaryFullFileProcessing')
        
    @App:description('Reads a binary file and moves it after processing.')
    
    @source(type='file', mode='TEXT.FULL',
        file.uri='file:/Users/foo/google.bin',
        action.after.process='MOVE', move.after.process='file:/Users/foo/move.after.process', 
        @map(type='json', enclosing.element="$.portfolio", @attributes(symbol = "stock.company.symbol", price = "stock.price", volume = "stock.volume")))
    define stream StockStream (symbol string, price float, volume long);
     
    @sink(type = 'log')
    define stream LogStream (symbol string, price float, volume long);
     
    from StockStream
    select str:upper(symbol) as symbol, price, volume   
    insert into LogStream;
    ```
 
    Change the `file.uri` parameter above, to the file path to which you downloaded `google.bin` file in step 1.

3. Save this file as `BinaryFullFileProcessing.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

    !!!info
        This Siddhi application reads the file `google.bin` fully to create a `StockStream` event. After that, a simple transformation is done on the `StockStream`. That is the `symbol` attribute from the event is converted into upper case. Finally, the output is logged on the SI console.
        
    Upon successful deployment, following log appears on the SI console:
    ```
    INFO {org.wso2.carbon.streaming.integrator.core.internal.StreamProcessorService} - Siddhi App BinaryFullFileProcessing deployed successfully
    ```
4. Now the Siddhi application starts to process the `google.bin` file. 
    
    As a result, the following log appears in the SI console:
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - BinaryFullFileProcessing :  LogStream : Event{timestamp=1564660553623, data=[WSO2, 55.6, 100], isExpired=false} 
    ```

### Reading a file line by line and delete it after processing    

In this scenario, you are reading a text file completely, and deleting it after  processing. In other words, the file is not tailed. You read the file line by line where each line generates an event. 

1. Download `productions.csv` file from [here](todo) and save it in a location of your choice. 

2. Open a text file and copy-paste following Siddhi application to it.
    ```
    @App:name('ReadFileLineByLine')
    
    @App:description('Reads a file line by line and does a simple transformation.')
    
    @source(type='file', mode='LINE',
        file.uri='file:/Users/foo/productions.csv',
        tailing='false',
        @map(type='csv'))
    define stream SweetProductionStream (name string, amount double);
    
    @sink(type = 'log')
    define stream LogStream (name string, amount double);
    
    from SweetProductionStream
    select str:upper(name) as name, amount
    insert into LogStream;
    ```
    Change the `file.uri` parameter above, to the file path to which you downloaded `productions.csv` file in step 1. 
    
3. Save this file as `ReadFileLineByLine.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.   

    !!!info
        This Siddhi application tails the file `productions.csv` line by line. Each line is converted into an event in `SweetProductionStream`. After that, a simple transformation is done on the sweet productions. That is the `name` attribute from the event is converted into upper case. Finally, the output is logged on the SI console.
        
    Upon successful deployment, following log appears on the SI console:
    ```
    INFO {org.wso2.carbon.streaming.integrator.core.internal.StreamProcessorService} - Siddhi App ReadFileLineByLine deployed successfully
    ```
4. Now the Siddhi application starts to process the `productions.csv` file. The file has below two entries:
    ```
    Almond cookie,100.0
    Baked alaska,20.0
    ```  
    As a result, the following log appears in the SI console:
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - ReadFileLineByLine : LogStream : Event{timestamp=1564490867341, data=[ALMOND COOKIE, 100.0], isExpired=false}
    INFO {io.siddhi.core.stream.output.sink.LogSink} - ReadFileLineByLine : LogStream : Event{timestamp=1564490867341, data=[BAKED ALASKA, 20.0], isExpired=false}
    ``` 
5. Notice that `productions.csv` file is not present in the `file.uri` location. 

6. Next you will create a new `productions.csv` file on the `file.uri` location which includes the latest set of productions. Download `productions.csv` file from [here](todo) and save it in `file.uri` location.  

7. Now the Siddhi application starts to process the new set of productions in the `productions.csv` file. The file has below two entries:
    ```
    Cup cake,300.0
    Doughnut,500.0
    ```  
    As a result, the following log appears in the SI console:
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - ReadFileLineByLine : LogStream : Event{timestamp=1564902130543, data=[CUP CAKE, 300.0], isExpired=false}
    INFO {io.siddhi.core.stream.output.sink.LogSink} - ReadFileLineByLine : LogStream : Event{timestamp=1564902130543, data=[DOUGHNUT, 500.0], isExpired=false}
    ```

### Reading a file using a regular expression and deleting it after processing

In this scenario, you are using a regular expression to extract data from the content of the file. Here, you do not tail the file, rather read the full content of the file and generate a single event. After this is done, the file is deleted. To generate an event stream, you can keep re-creating the file with new data. 

1. Download `noisy_data.txt` file from [here](todo) and save it in a location of your choice. 

2. Open a text file and copy-paste following Siddhi application to it.
    ```
    @App:name('ReadFileRegex')
    
    @App:description('Reads a file using a regex and does a simple transformation.')
    
    @source(type='file', mode='REGEX',
        file.uri='file:/Users/foo/noisy_data.txt',
        begin.regex='\<', end.regex='\>',
        tailing='false',
        @map(type='text', fail.on.missing.attribute = 'false', regex.A='(\w+)\s([-0-9]+)',regex.B='volume\s([-0-9]+)', @attributes(symbol = 'A[1]',price = 'A[2]',volume = 'B')))
    define stream StockStream (symbol string, price float, volume long);
    
    @sink(type = 'log')
    define stream LogStream (symbol string, price float, volume long);
    
    from StockStream[NOT(symbol is null)]
    select str:upper(symbol) as symbol, price, volume  
    insert into LogStream;
    ```
    Change the `file.uri` parameter above, to the file path to which you downloaded `noisy_data.txt` file in step 1. 
 
3. Save this file as `ReadFileRegex.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

    !!!info
        This Siddhi application tails the file `noisy_data.txt` to find matches according to the regular expressions given: `begin.regex` and `end.regex`. Each match is converted into an event in `StockStream`. After that, a simple transformation is done on the `StockStream`. That is the `symbol` attribute from the event is converted into upper case. Finally, the output is logged on the SI console.
        
    Upon successful deployment, following log appears on the SI console:
    ```
    INFO {org.wso2.carbon.streaming.integrator.core.internal.StreamProcessorService} - Siddhi App ReadFileRegex deployed successfully
    ```
    
4. Now the Siddhi application starts to process the `noisy_data.txt` file. 
    
    As a result, the following log appears in the SI console:
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - ReadFileRegex : LogStream : Event{timestamp=1564906475623, data=[WSO2, 75.0, 100], isExpired=false}
    ```

5. Notice that `noisy_data.txt` file is not present in the `file.uri` location. 

6. Next you will create a new `noisy_data.txt` file on the `file.uri` location which includes the latest set of productions. Download `noisy_data.txt` file from [here](todo) and save it in `file.uri` location.  

7. Now the Siddhi application starts to process the new content in the `noisy_data.txt` file. The file has below content:
    ```
    Oracle Corporation <orcl 95 volume 200> 500 Oracle Parkway.
    Redwood Shores CA, 94065.
    Corporate Phone: 650.506.7000.
    HQ-Security: 650.506.5555
    ```  
    As a result, the following log appears in the SI console:
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - ReadFileRegex : LogStream : Event{timestamp=1564906713176, data=[ORCL, 95.0, 200], isExpired=false}
    ```

## Extracting data from a folder 

### Processing all files in the folder 

In this scenario, you extract data from a specific folder. All of the files are processed sequentially, where each file generates a single event.

1. Download `productions.zip` file from [here](todo) and extract it. Now you have `productions` folder. Place it in a location of your choice.

2. Open a text file and copy-paste following Siddhi application to it.
    ```
    @App:name('ProcessFolder')
    
    @App:description('Process all files in the folder and delete files after processing.')
            
    @source(type='file', mode='text.full',
        dir.uri='file:/Users/foo/stocks',  
        @map(type='json', enclosing.element="$.portfolio", @attributes(symbol = "stock.company.symbol", price = "stock.price", volume = "stock.volume")))
    define stream StockStream (symbol string, price float, volume long);
    
    @sink(type = 'log')
    define stream LogStream (symbol string, price float, volume long);
    
    from StockStream
    select str:upper(symbol) as symbol, price, volume    
    insert into LogStream;
    ```
    Change the `dir.uri` parameter above so that it points to the `productions` folder you created in step 1. 
 
3. Save this file as `ProcessFolder.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

    !!!info
        This Siddhi application processes each file in folder `productions`. Each file generates an event into `StockStream`. After that, a simple transformation is done on the `StockStream`. That is the `symbol` attribute from the event is converted into upper case. Finally, the output is logged on the SI console.
        
    Upon successful deployment, following log appears on the SI console:
    ```
    INFO {org.wso2.carbon.streaming.integrator.core.internal.StreamProcessorService} - Siddhi App ProcessFolder deployed successfully
    ```

4. Now the Siddhi application starts to process each file in the `productions` folder. 
    
    As a result, the following logs appear in the SI console:
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - ProcessFolder : LogStream : Event{timestamp=1564932255417, data=[WSO2, 75.0, 100], isExpired=false}
    INFO {io.siddhi.core.stream.output.sink.LogSink} - ProcessFolder : LogStream : Event{timestamp=1564932255417, data=[ORCL, 95.0, 200], isExpired=false}
    INFO {io.siddhi.core.stream.output.sink.LogSink} - ProcessFolder : LogStream : Event{timestamp=1564932255417, data=[IBM, 88.0, 150], isExpired=false}
    ```

!!!info
    In this scenario, you deleted each file in the folder after processing. You can choose to move the files instead of deleting them. To do this, set parameter `action.after.process` to `MOVE` and specify the directory to which the files should be moved using parameter `move.after.process`. Refer [Siddhi File Source documentation](https://siddhi-io.github.io/siddhi-io-file/api/latest/#file-source) to read more about these parameters. 

## Loading data into a file

In this section of the tutorial, you are exploring the different ways in which you could load data into a file.  

### Appending or over-writing events to a file 

In this scenario, you will append a stream of events to the end of a file.

1. Open a text file and copy-paste following Siddhi application to it.
    ```
    @App:name('AppendToFile')
    
    @App:description('Append incoming events in to a file.')
    
    define stream SweetProductionStream (name string, amount double);
    
    @sink(type='file', @map(type='json'), file.uri='/Users/foo/low_productions.txt')
    define stream LowProductionStream (name string, amount double);
    
    -- Query to filter productions which have amount < 500.0
    @info(name='query1') 
    from SweetProductionStream[amount < 500.0]
    select *
    insert into LowProductionStream;
    ```

    Create an empty file and specify the location of the file under parameter `file.uri`. If this file does not exist, it will be created at the runtime.

2. Save this file as `AppendToFile.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

    !!!info
        This Siddhi application filters incoming `SweetProductionStream` events, selects the productions which have an `amount` smaller than `500.0` and insert the results into `LowProductionStream`. Finally, all `LowProductionStream` events will be appended to the file specified under parameter `file.uri` in the Siddhi application. 
        
    Upon successful deployment, following log appears on the SI console:
    ```
    INFO {org.wso2.carbon.streaming.integrator.core.internal.StreamProcessorService} - Siddhi App AppendToFile deployed successfully
    ```
3. Now let's insert a few events into `SweetProductionStream` by executing following `CURL` commands. On the command line, run following commands:
    ```
    curl -X POST -d '{"streamName": "SweetProductionStream", "siddhiAppName": "AppendToFile","data": ["Almond cookie", 100.0]}' http://localhost:9390/simulation/single -H 'content-type: text/plain' -H 'Authorization: Basic YWRtaW46YWRtaW4='
    ```
    ```
    curl -X POST -d '{"streamName": "SweetProductionStream", "siddhiAppName": "AppendToFile","data": ["Baked alaska", 20.0]}' http://localhost:9390/simulation/single -H 'content-type: text/plain' -H 'Authorization: Basic YWRtaW46YWRtaW4='
    ```
    ```
    curl -X POST -d '{"streamName": "SweetProductionStream", "siddhiAppName": "AppendToFile","data": ["Cup cake", 300.0]}' http://localhost:9390/simulation/single -H 'content-type: text/plain' -H 'Authorization: Basic YWRtaW46YWRtaW4='
    ```

4. Now open the file which you specified under `file.uri` parameter. Observe that the file has following content:
``` 
{"event":{"name":"Almond cookie","amount":100.0}}
{"event":{"name":"Baked alaska","amount":20.0}}
{"event":{"name":"Cup cake","amount":300.0}}
``` 

!!!info
    Instead of appending each event to the end of the file, you can configure your Siddhi application to over-write the file. To do this, use configuration `append='false'` in the Siddhi application. Refer sample `file` sink configuration below:
    ```
    @sink(type='file', append='false',  @map(type='json'), file.uri='/Users/foo/low_productions.txt')
       define stream LowProductionAlertStream (name string, amount double);
    ``` 
    Refer other configuration options in [Siddhi File Sink documentation](https://siddhi-io.github.io/siddhi-io-file/api/latest/#file-sink).   