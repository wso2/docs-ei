---
title: Importing CSV Data to Mongo DB
commitHash: a76e9f433c9db062427fd53e641e158cc9223de9
note: This is an auto-generated file do not edit this, You can edit content in "ballerina-integrator" repo
---

## About 
Ballerina is an open-source programming language that empowers developers to integrate their system easily with the support of connectors. In this guide, we are mainly focusing on importing CSV file having contacts into MongoDB using FTP connector.

`wso2/mongodb` module allows you to perform CRUD operations on Mongo DB.<br/> 
The `wso2/ftp` module provides an FTP client and an FTP server listener implementation to facilitate an FTP connection 
to a remote location. You can find other integration modules from the [wso2-ballerina](https://github.com/wso2-ballerina) Github repository. 

## What you'll build

This application listens to a remote FTP location and when a CSV file is added to that FTP location, it will fetch the CSV file, read its contents and insert the content into Mongo DB. Then a 
success message is logged if the operation is successful.

![inserting csv data to mongo db](../../../../../../assets/img/mongo_insert.png)

## Prerequisites
Link to download Ballerina integrator.

## Implementation
The Ballerina project is created for the integration use case explained above. Please follow the steps given below. You can learn about the Ballerina project and module by following the [documentation on creating a project and using modules](../../../../develop/using-modules/).

1. Create a project.
```bash
$ ballerina new insert_data
```
Navigate to the insert_data directory.

2. Add a module.
```bash
$ ballerina add import_csv
```

The project structure should look like below.
```shell
├── insert_data
    ├── ballerina.conf    
    ├── Ballerina.toml
    └── src
        └── import_csv
            ├── main.bal
            ├── Module.md
            ├── resources
            └── tests
                └── resources
```

3. Set up remote FTP server and obtain the following credentials.
   - FTP Host
   - FTP Port
   - FTP Username
   - FTP Password
   - Path in the FTP server to which the CSV files are added

    Add the `insert_data/src/import_csv/resources/contacts.csv` file to the FTP path you mentioned above.

4. Add project configuration file by creating `ballerina.conf` file under the root path of the project structure. <br/>
This file should have following Mongo DB and FTP configurations.

```  
MONGO_HOST="<MongoDB_Host>"
MONGO_DB_NAME="<MongoDB_Name>"
MONGO_USERNAME="<MongoDB_Username>"
MONGO_PASSWORD="<MongoDB_Password>"
FTP_HOST="<FTP_Host>"
FTP_PORT=<FTP_PORT>
FTP_USERNAME="<FTP_Username>"
FTP_PASSWORD="<FTP_Password>"
FTP_PATH="<FTP_Location>""
```  

3. Write your integration.
You can open the project with VS Code. The implementation will be written in the `main.bal` file.

  **main.bal**
    ```ballerina
    import ballerina/config;
    import ballerina/io;
    import ballerina/log;
    import wso2/ftp;
    import wso2/mongodb;
    
    mongodb:ClientEndpointConfig mongoConfig = {
        host: config:getAsString("MONGO_HOST"),
        dbName: config:getAsString("MONGO_DB_NAME"),
        username: config:getAsString("MONGO_USERNAME"),
        password: config:getAsString("MONGO_PASSWORD"),
        options: {sslEnabled: false, serverSelectionTimeout: 500}
    };
    
    listener ftp:Listener ftpListener = new ({
        protocol: ftp:FTP,
        host: config:getAsString("FTP_HOST"),
        port: config:getAsInt("FTP_PORT"),
        pollingInterval: 30000,
    
        secureSocket: {
            basicAuth: {
                username: config:getAsString("FTP_USERNAME"),
                password: config:getAsString("FTP_PASSWORD")
            }
        },
        path: config:getAsString("FTP_PATH")
    });
    
    ftp:ClientEndpointConfig ftpConfig = {
        protocol: ftp:FTP,
        host: config:getAsString("FTP_HOST"),
        port: config:getAsInt("FTP_PORT"),
        secureSocket: {
            basicAuth: {
                username: config:getAsString("FTP_USERNAME"),
                password: config:getAsString("FTP_PASSWORD")
            }
        }
    };
    ftp:Client ftp = new (ftpConfig);
    mongodb:Client mongoClient = check new (mongoConfig);
    
    service ftpServerConnector on ftpListener {
        resource function onFileChange(ftp:WatchEvent fileEvent) returns error? {
            foreach ftp:FileInfo v1 in m.addedFiles {
                log:printInfo("Added file path  " + v1.path + " to FTP location");
    
                var y = insertToMongo(v1.path);
            }
        }
    }
    
    function readFile(string sourcePath) returns @untainted json[] | error {
        io:ReadableByteChannel getResult = check ftp->get(sourcePath);
    
        io:ReadableCharacterChannel readableCharChannel = new io:ReadableCharacterChannel(getResult, "UTF-8");
        io:ReadableCSVChannel csvChannel = new io:ReadableCSVChannel(readableCharChannel);
        json[] j2 = [];
        int i = 0;
        while (csvChannel.hasNext()) {
            var records = check csvChannel.getNext();
            json j1 = {x: records};
            j2[i] = j1;
    
            i = i + 1;
        }
        var result = csvChannel.close();
        return j2;
    }
    
    function insertToMongo(string path) returns error? {
        json[] | error data = readFile(path);
    
        if (data is json) {
            foreach json doc in data {
                var insertResult = mongoClient->insert("projects", doc);
            }
        } else {
            log:printError("Error occured in reading the file");
        }
    
        handleInsert(data);
    }
    
    function handleInsert(json | error returned) {
        if (returned is json) {
            log:printInfo("Successfully inserted data to mongo db");
        } else {
            log:printError(returned.reason());
        }
    }
    ```

 Here `ftpServerConnector` service is running on `remoteServer`, which listens to the configured FTP server location.
 When a CSV file is added to the FTP server, the file content will be retrieved and inserted into Mongo DB.


## Run the integration
First, let’s build the module. While being in the insert_data directory, execute the following command.

```bash
$ ballerina build import_csv
```

The build command would create an executable .jar file. Now run the .jar file created in the above step to execute the .jar.

```bash
$ java -jar target/bin/import_csv.jar
```

You will see the following log after successfully importing the contacts.csv file to Mongo DB.
```
2019-09-27 12:40:40,882 INFO  [wso2/import_csv] - Added file path  /home/ftp-user/in/mongo/contacts.csv to FTP location 
2019-09-27 12:40:40,953 INFO  [wso2/import_csv] - Successfully inserted data to mongo db 
```
