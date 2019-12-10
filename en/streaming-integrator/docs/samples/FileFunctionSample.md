
## Purpose:
This application demonstrates how to use `siddhi-execution-file` for manipulating files. 
In this demo, the following functionalities will be shown.
* Creating a file
* Archiving a folder with the created file to a given directory
* Listing the files in an archived file
* Unarchiving a file into a directory
* Listing the files in a given directory


## Prerequisites:
1. Save this sample.
2. Download 
<a href="../../../../../../assets/zip/siddhi-file-function-artefacts.zip">
   <img src="../../../../../../assets/img/download-zip.png" width="200" alt="File function artefacts">
</a> 
and extract the contents to `{WSO2SIHome}/samples/artifacts/FileFunctions/` folder.
3. The decompressed path of the downloaded file should be as `{WSO2SIHome}/samples/artifacts/FileFunctions/archive`

## Note:
`{WSO2SIHome}` in the curl commands should be replaced with the absolute path of your WSO2SI home directory.

## Executing the Sample and Viewing Results:
1. Start the Siddhi application by clicking on 'Run'.
2. If the Siddhi application starts successfully, the following messages would be shown on the console.
    ```
    FileFunctionsApp.siddhi - Started Successfully!
    ```
3. Publish an event with the file path of `created.txt` needed to be created. `createFileQuery` will be used to create the file. In our use case, the file will be created in the `archive` directory.
        
        curl -X POST -d "{ \"event\": { \"uri\": \"{WSO2SIHome}/samples/artifacts/FileFunctions/archive/created.txt\" } }" http://localhost:8006/CreateFileStream --header "Content-Type:application/json"
4. Publish an event to archive `{WSO2SIHome}/samples/artifacts/FileFunctions/archive` folder into `{WSO2SIHome}/samples/artifacts/FileFunctions/destination` directory. `fileArchiveQuery` will be used to archive the folder.
        
        curl -X POST -d "{ \"event\": { \"source\": \"{WSO2SIHome}/samples/artifacts/FileFunctions/archive/\",\"destination\": \"{WSO2SIHome}/samples/artifacts/FileFunctions/destination/\" } }" http://localhost:8006/SourceAndDestinationStream --header "Content-Type:application/json"
        
        sample console output:
            FileFunctionsApp: File archiving success: , StreamEvent{ timestamp=1575998289186, beforeWindowData=null, onAfterWindowData=null, outputData=[/Users/ramindu/wso2/git/mi/streaming-integrator-tooling/modules/distribution/target/wso2si-tooling-1.0.1-SNAPSHOT/samples/artifacts/FileFunctions/archive, /Users/ramindu/wso2/git/mi/streaming-integrator-tooling/modules/distribution/target/wso2si-tooling-1.0.1-SNAPSHOT/samples/artifacts/FileFunctions/destination/], type=CURRENT, next=null}
5. When the archive process successfully finished, `listFilesInArchiveQuery` will be used to get the list of files in the `{WSO2SIHome}/samples/artifacts/FileFunctions/destination/archive.zip` file
        
        sample console output: 
            FileFunctionsApp: File list in the archived file: , StreamEvent{ timestamp=1575998289186, beforeWindowData=null, onAfterWindowData=null, outputData=[/Users/ramindu/wso2/git/mi/streaming-integrator-tooling/modules/distribution/target/wso2si-tooling-1.0.1-SNAPSHOT/samples/artifacts/FileFunctions/destination/, [/subFolder/test3.txt, /.DS_Store, /test2.txt, /test.txt, /created.txt]], type=CURRENT, next=null}
6. Publish an event to unarchive `{WSO2SIHome}/samples/artifacts/FileFunctions/destination/archive.zip` to `{WSO2SIHome}/samples/artifacts/FileFunctions/destination`. `fileUnarchiveQuery` will be used to unarchive the file
        
        curl -X POST -d "{ \"event\": { \"source\": \"{WSO2SIHome}/samples/artifacts/FileFunctions/destination/archive.zip\",\"destination\": \"{WSO2SIHome}/samples/artifacts/FileFunctions/destination/\" } }" http://localhost:8006/UnArchiveFileStream --header "Content-Type:application/json"
        
        sample console output:
            FileFunctionsApp: File unarchived successfully: , StreamEvent{ timestamp=1576000909621, beforeWindowData=null, onAfterWindowData=null, outputData=[/Users/ramindu/wso2/git/mi/streaming-integrator-tooling/modules/distribution/target/wso2si-tooling-1.0.1-SNAPSHOT/samples/artifacts/FileFunctions/destination/archive.zip, /Users/ramindu/wso2/git/mi/streaming-integrator-tooling/modules/distribution/target/wso2si-tooling-1.0.1-SNAPSHOT/samples/artifacts/FileFunctions/destination/], type=CURRENT, next=null}
7. When the unarchive process successfully finished, `listFilesInDirectoryQuery` will be used to get the list of files in the `{WSO2SIHome}/samples/artifacts/FileFunctions/destination/archive` directory
        
        sample console output:
            FileFunctionsApp: File list in the unarchived directory: , StreamEvent{ timestamp=1576000909621, beforeWindowData=null, onAfterWindowData=null, outputData=[/Users/ramindu/wso2/git/mi/streaming-integrator-tooling/modules/distribution/target/wso2si-tooling-1.0.1-SNAPSHOT/samples/artifacts/FileFunctions/destination/, [/Users/ramindu/wso2/git/mi/streaming-integrator-tooling/modules/distribution/target/wso2si-tooling-1.0.1-SNAPSHOT/samples/artifacts/FileFunctions/destination/archive/subFolder/test3.txt, /Users/ramindu/wso2/git/mi/streaming-integrator-tooling/modules/distribution/target/wso2si-tooling-1.0.1-SNAPSHOT/samples/artifacts/FileFunctions/destination/archive/.DS_Store, /Users/ramindu/wso2/git/mi/streaming-integrator-tooling/modules/distribution/target/wso2si-tooling-1.0.1-SNAPSHOT/samples/artifacts/FileFunctions/destination/archive/test2.txt, /Users/ramindu/wso2/git/mi/streaming-integrator-tooling/modules/distribution/target/wso2si-tooling-1.0.1-SNAPSHOT/samples/artifacts/FileFunctions/destination/archive/test.txt, /Users/ramindu/wso2/git/mi/streaming-integrator-tooling/modules/distribution/target/wso2si-tooling-1.0.1-SNAPSHOT/samples/artifacts/FileFunctions/destination/archive/created.txt]], type=CURRENT, next=null}

## Viewing the Results:
At the end of the process `{WSO2SIHome}/samples/artifacts/FileFunctions/destination` directory should contain `archive.zip` file and `archive` directory with the unarchived files.


```sql
@App:name('FileFunctionsApp')

@Source(type = 'http',
        receiver.url='http://localhost:8006/SourceAndDestinationStream',
        basic.auth.enabled='false',
        @map(type='json'))
define stream SourceAndDestinationStream (source string, destination string);

@Source(type = 'http',
        receiver.url='http://localhost:8006/UnArchiveFileStream',
        basic.auth.enabled='false',
        @map(type='json'))
define stream UnArchiveFileStream (source string, destination string);

@Source(type = 'http',
        receiver.url='http://localhost:8006/CreateFileStream',
        basic.auth.enabled='false',
        @map(type='json'))
define stream CreateFileStream (uri string);

@info(name='fileCreateQuery')
from CreateFileStream#file:create(uri, false) 
select *
insert into CreatedFileStream;

@info(name='fileArchiveQuery')
from SourceAndDestinationStream#file:archive(source, destination)
select *
insert into ArchivedStream;

from ArchivedStream#log('File archiving success: ')
select * 
insert into SearchFilesInArchive;

@info(name='listFilesInArchiveQuery')
from SearchFilesInArchive#file:searchInArchive(str:concat(destination, '/archive.zip'))
select destination, fileNameList
insert into ArchivedFileNameListStream;

from ArchivedFileNameListStream#log('File list in the archived file: ')
select destination, fileNameList 
insert into IgnoreStream;

@info(name='fileUnarchiveQuery')
from UnArchiveFileStream#file:unarchive(source, destination, false)
select source, destination
insert into UnarchivedStream;

from UnarchivedStream#log('File unarchived successfully: ')
select *
insert into SearchFilesInDirectory;

@info(name='listFilesInDirectoryQuery')
from SearchFilesInDirectory#file:search(str:concat(destination, '/archive'))
select *
insert into FileNameListStream;

from FileNameListStream#log('File list in the unarchived directory: ')
select destination, fileNameList 
insert into IgnoreStream;
```