# Uploading a Created Data Service

After [creating](_Creating_a_Data_Service_from_Scratch_) a data service
( `         .dbs        ` file) file from scratch, deploy it to the ESB
profile of WSO2 Enterprise Integrator (WSO2 EI) using one of the methods
listed below:  
  

-   [Uploading via the management
    console](#UploadingaCreatedDataService-Uploadingviathemanagementconsole)
-   [Uploading as a hot
    deployment](#UploadingaCreatedDataService-Uploadingasahotdeployment)

### Uploading via the management console

Follow the steps given below to upload the data service through the
management console:

1.  Download the product installer from
    [here](http://wso2.com/integration/) , and run the installer.  
    Let's call the installation location of your product the
    **\<EI\_HOME\>** directory.

    If you installed the product using the **installer** , this is
    located in a place specific to your OS as shown below:

    <table style="width:100%;">
    <colgroup>
    <col style="width: 9%" />
    <col style="width: 90%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>OS</th>
    <th>Home directory</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Mac OS</td>
    <td><code>                /Library/WSO2/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    <tr class="even">
    <td>Windows</td>
    <td><code>                C:\Program Files\WSO2\EnterpriseIntegrator\6.5.0\               </code></td>
    </tr>
    <tr class="odd">
    <td>Ubuntu</td>
    <td><code>                /usr/lib/wso2/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    <tr class="even">
    <td>CentOS</td>
    <td><code>                /usr/lib64/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    </tbody>
    </table>

2.  Start the ESB profile:

    -   [**On MacOS/Linux/CentOS**](#c2b5334b859e44ddb7572ebe1950cfe3)
    -   [**On Windows**](#f1921eeb0bbd46d3bae4d2ebd1c2d5fb)

    Open a terminal and execute the following command:

    ``` java
        wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

3.  Go to the management console:
    `          https://localhost:9443/carbon         ` .
4.  Sign in using `          admin         ` as the username and
    password.
5.  Click **Data Service \> Upload** .
6.  Select the database backup (.dbs) file and click **Upload** .

        !!! tip
    
        Don't' have a database backup file? Upload a sample data service
        that is stored in the
        `           <EI_HOME>/samples/data-services/dbs          ` folder.
    

7.  If the file is deployed successfully, the **Deployed Services**
    window appears with the new data service listed. From here, you can
    manage your service. See [Working with Data
    Services](_Working_with_Data_Services_) .

### Uploading as a hot deployment

Alternatively, copy the database backup (.dbs) file to
`         <EI_HOME>/repository/deployment/server/dataservices        `
folder.  
It will be deployed instantly as hot deployment, which is enabled in
Data Services Server by default.

!!! note

When adding a data service file to the ESB profile for the first time,
you need to create a folder named `         dataservices        ` in the
`         <EI_HOME>/repository/deployment/server        ` directory.

