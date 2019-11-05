# Exposing a Google Spreadsheet

This example demonstrates how data in a Google Spreadsheet can be exposed as a data service.

## Prerequisites

A google spreadsheet can be a private spreadsheet or a publicly exposed (published to the web) spreadsheet. This example uses a public spreadsheet. 

Note the following before you begin:

-   The spreadsheet is required to be published to the
    web. To publish a spreadsheet, select **File -> Publish** **to the
    web** from the spreadsheet's user interface. Use the spreadsheet's
    URL for the **Google Spreadsheet URL** field.
-   You cannot define SQL-like queries for a public spreadsheet. This also means that you cannot
    add or modify the data in the spreadsheet. That is, you can only get
    data from a public spreadsheet.

## Synapse configuration
Given below is the data service configuration you need to build. See the instructions on how to [build and run](#build-and-run) this example.

```xml
<data name="GSpread" transports="http https">
   <config enableOData="false" id="GoogleSpreadsheet">
      <property name="gspread_datasource">https://docs.google.com/spreadsheets/d/1o1pCmMFcbWZ_eJ54ymcb486GctTdsR6b4kFERmKrR1w/edit?hl=en&amp;hl=en#gid=0 </property>
      <property name="gspread_visibility">public</property>
   </config>
   <query id="Q1" useConfig="GoogleSpreadsheet">
      <gspread>
         <worksheetnumber>1</worksheetnumber>
         <startingrow>2</startingrow>
         <maxrowcount>5</maxrowcount>
         <hasheader>true</hasheader>
         <headerrow>1</headerrow>
      </gspread>
      <result element="Customers" rowName="Customer">
         <element column="customerNumber&#x9;" name="customerNumber&#x9;" xsdType="string"/>
         <attribute column="customerName" name="customerName" xsdType="string"/>
         <attribute column="city" name="city" xsdType="string"/>
      </result>
   </query>
   <operation name="CustomerDetails">
      <call-query href="Q1"/>
   </operation>
</data>
```

!!! Info
    The above sample uses a public spreadsheet. Note the following if you are using a private spreadsheet:
    
    -   The **Redirect URI** should contain the same hostname as the **Authorized Redirect** URl, as well as the host on which the server runs.
    -   If the server is running on your machine, you can simply
        use `                 localhost                ` as the
        hostname (or the direct IP address, which is 127.0.0.1).
    -   If the server is running on a local network, you must
        always use a hostname instead of the direct IP address.
        This is because publicly shared IPs cannot be used. You
        also need to ensure that the hostname you use is known
        to the browser by registering it in your
        `                 /etc/hosts                ` file.
    -   For the purpose of this example, be sure to use
            `localhost` as the IP in the above URL.

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create a Data Service project](../../../../develop/creating-projects/#data-services-project).
4. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.
5. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator. 