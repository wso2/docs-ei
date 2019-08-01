# Performing Real-time ETL with Files

## Introduction

The Streaming Integrator (SI) allows you to perform real-time ETL with data which are stored in files. 

This tutorial takes you through the different modes and options you could use, in order to perform real-time ETL with files, using the SI. 

## Outline:

- Preparing the server
- Extracting data from a file
    - Tailing a text file line by line
    - Tailing a text file using a regular expression
    - Reading a text file and moving it after processing
    - Reading a binary file and moving it after processing
    - Reading a file line by line and moving it after processing
    - Reading a file using a regular expression and moving it after processing

- Extracting data from a folder
    - Processing all files in the folder

- Loading data into a file
    - Appending event to the end of the file
    - Overwriting a file content with event data
    
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

## Extracting data from a folder

In this section of the tutorial, you are exploring the different ways in which you could extract data, from a specific folder.  

## Loading data into a file

In this section of the tutorial, you are exploring the different ways in which you could load data into a file.  

   