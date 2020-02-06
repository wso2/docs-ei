# Salesforce Connector Configuration

[[Prerequisites]](#Prerequisites) [[Salesforce Operations]](#salesforce-operations) [[Logging out of Salesforce]](#logging-out-of-salesforce)

## Prerequisites

### Importing the Salesforce Certificate

To use the Salesforce connector, add the <salesforce.init>  element to your configuration before carrying out any other Salesforce operations.

Before you start configuring the connector, import the Salesforce certificate to your WSO2 Enterprise Integrator (EI) client keystore.

* Follow the steps below to import the Salesforce certificate into the EI client keystore:

    1. To view the certificate, log in to your Salesforce account in your browser (e.g., https://login.salesforce.com), and click the lock on the address bar.
    2. Export the certificate to the file system.
    3. Import the certificate to the EI client keystore using either the following command or the EI Management Console:
    ```
    keytool -importcert -file <certificate file> -keystore <EI>/repository/resources/security/client-truststore.jks -alias "Salesforce"
    ```
    4. Restart the server and deploy the following Salesforce configuration:

###### init
```xml
<salesforce.init>
    <username>MyUsername</username>
    <password>MyPassword</password>
    <loginUrl>https://login.salesforce.com/services/Soap/u/42.0</loginUrl>
    <blocking>false</blocking>
</salesforce.init>
```
###### Properties
* username:  The username to access the Salesforce account.
* password:  The password provided here is a concatenation of the user password and the security token provided by Salesforce.
* loginUrl:  The login URL to access the Salesforce account.
* blocking:  Indicates whether the connector needs to perform blocking invocations to Salesforce. (Supported in WSO2 ESB 4.9.0 and later.) 

```text
*  Users can obtain a security token by changing the password or resetting the security token using the Salesforce user interface. The new security token is sent to the email address recorded in the user's Salesforce record.
*  The response of this operation is attached to the message body and is used for subsequent Salesforce operations.
*  The session ID is saved in the property salesforce.sessionId and the server URL is saved in salesforce.serviceUrl. If the given login details are invalid, the specified fault sequence is triggered.
```
---
__Note :__

Secure Vault is supported for encrypting passwords. See, [Working with Passwords](https://docs.wso2.com/display/ADMIN44x/Encrypting+Passwords+with+Cipher+Tool) on integrating and using Secure Vault.

---
**Re-using Salesforce configurations**

You can save the Salesforce connection configuration as a [local entry](https://docs.wso2.com/display/EI620/Working+with+Local+Registry+Entries) and then easily reference it with the configKey attribute in your operations. For example, if you saved the above <salesforce.init> entry as a local entry named MySFConfig, you could reference it from an operation like getUserInfo as follows:

```xml
<salesforce.getUserInformation configKey="MySFConfig"/>
```
The Salesforce connector operation examples use this convention to show how to specify the connection configuration for that operation. In all cases, the configKey attribute is optional if the connection to Salesforce has already been established and is required only if you need to specify a different connection from the current connection.

## Salesforce Operations

Now that you have connected to Salesforce, use the information in the following topics to perform various operations with the connector.

### Working with sObjects in Salesforce

The following operations allow you to work with sObjects. Click an operation name to see details on how to use it.

| Operation        | Description |
| ------------- |-------------|
| [describeGlobal](#retrieving-a-list-of-available-objects)    | Retrieves the objects that are available in the system. |
| [describeSObject](#retrieving-metadata-for-a-specific-object-type)      | Retrieves metadata (such as name, label, and fields, including the field properties) for a specific object type. |
| [describeSObjects](#retrieving-metadata-for-multiple-object-types)    | Retrieves metadata (such as name, label, and fields, including the field properties) for multiple object types. |


An sObject refers to any object that can be stored in the [Force.com](https://www.salesforce.com) platform database and includes all the standard and custom objects in your organization. With the Salesforce connector operations, you can define an sObject structure in the message payload, which you can send directly from the caller or generate using the [PayloadFactory mediator](https://docs.wso2.com/display/EI620/PayloadFactory+Mediator) in ESB as follows:

```xml
<payloadFactory>
    <format>                                           
        <sfdc:sObjects xmlns:sfdc="sfdc" type="Account">
          <sfdc:sObject>
             <sfdc:Name>value01</sfdc:Name>             
          </sfdc:sObject>
          <sfdc:sObject>                                
             <sfdc:Name>value02</sfdc:Name>
          </sfdc:sObject>
       </sfdc:sObjects>
    </format>
    <args/>
</payloadFactory>
```
##### Operation details

This section provides more information on each of the operations.

###### Retrieving a list of available objects**

To retrieve a list of objects that are available in the system, use salesforce.describeGlobal. 

**describeGlobal**
```xml
<salesforce.describeGlobal configKey="MySFConfig"/>
```

**Sample response**

Given below is a sample response for the describeGlobal operation.

```xml
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:partner.soap.sforce.com" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <soapenv:Header>
        <LimitInfoHeader>
            <limitInfo>
                <current>29</current>
                <limit>15000</limit>
                <type>API REQUESTS</type>
            </limitInfo>
        </LimitInfoHeader>
    </soapenv:Header>
    <soapenv:Body>
        <describeGlobalResponse>
            <result>
                <encoding>UTF-8</encoding>
                <maxBatchSize>200</maxBatchSize>
                <sobjects>
                    <activateable>false</activateable>
                    <createable>false</createable>
                    <custom>false</custom>
                    <customSetting>false</customSetting>
                    <deletable>false</deletable>
                    <deprecatedAndHidden>false</deprecatedAndHidden>
                    <feedEnabled>false</feedEnabled>
                    <keyPrefix xsi:nil="true"/>
                    <label>Accepted Event Relation</label>
                    <labelPlural>Accepted Event Relations</labelPlural>
                    <layoutable>false</layoutable>
                    <mergeable>false</mergeable>
                    <name>AcceptedEventRelation</name>
                    <queryable>true</queryable>
                    <replicateable>false</replicateable>
                    <retrieveable>true</retrieveable>
                    <searchable>false</searchable>
                    <triggerable>false</triggerable>
                    <undeletable>false</undeletable>
                    <updateable>false</updateable>
                </sobjects>
                .
                .
            </result>
        </describeGlobalResponse>
    </soapenv:Body>
</soapenv:Envelope>
```
**Related Salesforce documentation**

[https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_describeglobal.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_describeglobal.htm)

###### Retrieving metadata for a specific object type

To retrieve metadata (such as name, label, and fields, including the field properties) for a specific object type, use salesforce.describeSobject and specify the following properties. 

**describeSobject**
```xml
<salesforce.describeSObject configKey="MySFConfig">
    <sobject>Account</sobject>
</salesforce.describeSObject>
```

**Properties**
* sobject: The object type of where you want to retrieve the metadata.

**Sample response**

Given below is a sample response for the describeSObject operation.

```xml
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:partner.soap.sforce.com" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <soapenv:Header>
        <LimitInfoHeader>
            <limitInfo>
                <current>31</current>
                <limit>15000</limit>
                <type>API REQUESTS</type>
            </limitInfo>
        </LimitInfoHeader>
    </soapenv:Header>
    <soapenv:Body>
        <describeSObjectResponse>
            <result>
                <activateable>false</activateable>
                <childRelationships>
                    <cascadeDelete>false</cascadeDelete>
                    <childSObject>Account</childSObject>
                    <deprecatedAndHidden>false</deprecatedAndHidden>
                    <field>ParentId</field>
                    <relationshipName>ChildAccounts</relationshipName>
                </childRelationships>
                .
                .
            </result>
        </describeSObjectResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

**Related Salesforce documentation**

[https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_describesobject.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_describesobject.htm)

###### Retrieving metadata for multiple object types

To retrieve metadata (such as name, label, and fields, including the field properties) for multiple object types returned as an array, use salesforce.describeSobjects and specify the following properties. 

**describeSobjects**
```xml
<salesforce.describeSobjects configKey="MySFConfig">
    <sobjects xmlns:sfdc="sfdc">{//sfdc:sObjects}</sobjects>
</salesforce.describeSobjects>
```

**Properties**
* sobjects: An XML representation of the object types of where you want to retrieve the metadata.

**Sample request**

Given below is a sample request that can be handled by the describeSobjects operation.

```xml
<payloadFactory>
    <format>
        <sfdc:sObjects xmlns:sfdc="sfdc">
            <sfdc:sObjectType>Account</sfdc:sObjectType>
            <sfdc:sObjectType>Contact</sfdc:sObjectType>
         </sfdc:sObjects>
     </format>
     <args/>
 </payloadFactory>
 
<salesforce.describeSobjects configKey="MySFConfig">
    <sobjects xmlns:sfdc="sfdc">{//sfdc:sObjects}</sobjects>
</salesforce.describeSobjects>
```

**Sample response**

Given below is a sample response for the describeSobjects operation.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:partner.soap.sforce.com" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <soapenv:Header>
        <LimitInfoHeader>
            <limitInfo>
                <current>51</current>
                <limit>15000</limit>
                <type>API REQUESTS</type>
            </limitInfo>
        </LimitInfoHeader>
    </soapenv:Header>
    <soapenv:Body>
        <describeSObjectsResponse>
            <result>
                <activateable>false</activateable>
                <childRelationships>
                    <cascadeDelete>false</cascadeDelete>
                    <childSObject>Account</childSObject>
                    <deprecatedAndHidden>false</deprecatedAndHidden>
                    <field>ParentId</field>
                    <relationshipName>ChildAccounts</relationshipName>
                </childRelationships>
                .
                .
            </result>
            <result>
                <activateable>false</activateable>
                <childRelationships>
                    <cascadeDelete>false</cascadeDelete>
                    <childSObject>AcceptedEventRelation</childSObject>
                    <deprecatedAndHidden>false</deprecatedAndHidden>
                    <field>RelationId</field>
                    <relationshipName>AcceptedEventRelations</relationshipName>
                </childRelationships>
                .
                .
            </result>
        </describeSObjectsResponse>
    </soapenv:Body>
</soapenv:Envelope>
```
**Related Salesforce documentation**

[https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_describesobjects.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_describesobjects.htm)

**Sample configuration**

Following example illustrates how to connect to Salesforce with the init operation and describeGlobal operation.

1. Create a sample proxy as below :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse"
       name="salesforce_describeGlobal"
       startOnLoad="true"
       statistics="disable"
       trace="disable"
       transports="http,https">
   <target>
      <inSequence>
         <property xmlns:ns="wso2.connector.salesforce"
                   expression="//ns:loginUrl/text()"
                   name="loginUrl"/>
         <property xmlns:ns="wso2.connector.salesforce"
                   expression="//ns:username/text()"
                   name="username"/>
         <property xmlns:ns="wso2.connector.salesforce"
                   expression="//ns:password/text()"
                   name="password"/>
         <property xmlns:ns="wso2.connector.salesforce"
                   expression="//ns:blocking/text()"
                   name="blocking"/>
         <salesforce.init>
            <loginUrl>{$ctx:loginUrl}</loginUrl>
            <username>{$ctx:username}</username>
            <password>{$ctx:password}</password>
            <blocking>{$ctx:blocking}</blocking>
         </salesforce.init>
         <salesforce.describeGlobal/>
         <respond/>
      </inSequence>
      <outSequence>
         <send/>
      </outSequence>
   </target>
   <description/>
</proxy>                              
```
2. Create an XML file named describeGlobal.xml and copy the XML configurations given below:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                  xmlns:urn="wso2.connector.salesforce">
    <soapenv:Header/>
    <soapenv:Body>
        <urn:loginUrl>https://login.salesforce.com/services/Soap/u/30.0</urn:loginUrl>
        <urn:username>john@gmail.com</urn:username>
        <urn:password>john@123CtGoiPE3mCdjgUHlto8HJ3</urn:password>
        <urn:blocking>false</urn:blocking>
    </soapenv:Body>
</soapenv:Envelope>                              
```
3. Replace the credentials with your values.

4. Execute the following curl command:

```bash
curl http://localhost:8280/services/salesforce_describeGlobal -H "Content-Type: text/xml" -d @describeGlobal.xml
```
5. Salesforce returns an XML response similar to the response given below:
 
```xml
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:partner.soap.sforce.com" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <soapenv:Header>
        <LimitInfoHeader>
            <limitInfo>
                <current>29</current>
                <limit>15000</limit>
                <type>API REQUESTS</type>
            </limitInfo>
        </LimitInfoHeader>
    </soapenv:Header>
    <soapenv:Body>
        <describeGlobalResponse>
            <result>
                <encoding>UTF-8</encoding>
                <maxBatchSize>200</maxBatchSize>
                <sobjects>
                    <activateable>false</activateable>
                    <createable>false</createable>
                    <custom>false</custom>
                    <customSetting>false</customSetting>
                    <deletable>false</deletable>
                    <deprecatedAndHidden>false</deprecatedAndHidden>
                    <feedEnabled>false</feedEnabled>
                    <keyPrefix xsi:nil="true"/>
                    <label>Accepted Event Relation</label>
                    <labelPlural>Accepted Event Relations</labelPlural>
                    <layoutable>false</layoutable>
                    <mergeable>false</mergeable>
                    <name>AcceptedEventRelation</name>
                    <queryable>true</queryable>
                    <replicateable>false</replicateable>
                    <retrieveable>true</retrieveable>
                    <searchable>false</searchable>
                    <triggerable>false</triggerable>
                    <undeletable>false</undeletable>
                    <updateable>false</updateable>
                </sobjects>
                .
                .
            </result>
        </describeGlobalResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

### Working with Records in Salesforce

The following operations allow you to work with Records. A Record is an instance of a particular sObject. Click an operation name to see details on how to use it.

| Operation        | Description |
| ------------- |-------------|
| [create](#creating-records)    | Creates records in Salesforce. |
| [update](#updating-records)      | Updates a record in Salesforce. |
| [upsert](#updating-and-inserting-records)    | Updates existing records and inserts a new record in a single operation. |
| [search](#searching-records)    | Search for records in Salesforce. |
| [query](#querying-records)      |  	Retrieves data from a record in Salesforce. |
| [retrieve](#retrieving-specific-records)    | Retrieves data from a record in Salesforce when the record ID is provided. |
| [delete](#deleting-records)    | Deletes a record in Salesforce. |
| [undelete](#restoring-records)      | Restores a record that was previously deleted in Salesforce. |

##### Operation details

This section provides further details on the operations related to records.


###### Creating records

To create one or more record, use salesforce.create and specify the following properties. 

**create**

```xml
<salesforce.create configKey="MySFConfig">
    <allOrNone>0</allOrNone>
    <allowFieldTruncate>0</allowFieldTruncate>
    <sobjects xmlns:sfdc="sfdc">{//sfdc:sObjects}</sobjects>
</salesforce.create>
```

**Properties**
* allOrNone: Whether to rollback changes if an object fails (see Common Parameters).
* allowFieldTruncate: Whether to truncate strings that exceed the field length (see Common Parameters).
* sobjects: XML representation of the records to add.

**Sample request**

Given below is a sample request that can be handled by the create operation.

```xml
<payloadFactory>
    <format>
       <sfdc:sObjects xmlns:sfdc="sfdc" type="Account">
          <sfdc:sObject>
              <sfdc:Name>wso2123</sfdc:Name>
           </sfdc:sObject>
           <sfdc:sObject>
             <sfdc:Name>abc123</sfdc:Name>
           </sfdc:sObject>
        </sfdc:sObjects>
    </format>
    <args/>
</payloadFactory>
 
<salesforce.create>
    <allOrNone>0</allOrNone>
    <allowFieldTruncate>0</allowFieldTruncate>
    <sobjects xmlns:sfdc="sfdc">{//sfdc:sObjects}</sobjects>
</salesforce.create>
```

**Sample response**

Given below is a sample response for the create operation.

```xml
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:partner.soap.sforce.com">
    <soapenv:Header>
        <LimitInfoHeader>
            <limitInfo>
                <current>9</current>
                <limit>15000</limit>
                <type>API REQUESTS</type>
            </limitInfo>
        </LimitInfoHeader>
    </soapenv:Header>
    <soapenv:Body>
        <createResponse>
            <result>
                <id>0036F00002mdwl2QAA</id>
                <success>true</success>
            </result>
        </createResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

**Related Salesforce documentation**

[https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_create.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_create.htm)

###### Updating records

To update one or more existing records, use salesforce.update and specify the following properties. 

**update**

```xml
<salesforce.update configKey="MySFConfig">
    <allOrNone>0</allOrNone>
    <allowFieldTruncate>0</allowFieldTruncate>
    <sobjects xmlns:sfdc="sfdc">{//sfdc:sObjects}</sobjects>
</salesforce.update>
```

**Properties**
* allOrNone: Whether to rollback changes if an object fails (see Common Parameters).
* allowFieldTruncate: Whether to truncate strings that exceed the field length (see Common Parameters).
* sobjects: XML representation of the records to add.

**Sample request**

Given below is a sample request that can be handled by the update operation.

```xml
<payloadFactory>
    <format>
        <sfdc:sObjects xmlns:sfdc="sfdc" type="Account">
          <sfdc:sObject>
             <sfdc:Id>0019000000aaMkZ</sfdc:Id>
             <sfdc:Name>newname01</sfdc:Name>
          </sfdc:sObject>
          <sfdc:sObject>
             <sfdc:Id>0019000000aaMkP</sfdc:Id>
             <sfdc:Name>newname02</sfdc:Name>
          </sfdc:sObject>
       </sfdc:sObjects>
    </format>
    <args/>
</payloadFactory>
 
<salesforce.update>
    <allOrNone>0</allOrNone>
    <allowFieldTruncate>0</allowFieldTruncate>
    <sobjects xmlns:sfdc="sfdc">{//sfdc:sObjects}</sobjects>
</salesforce.update>
```
**Sample response**

Given below is a sample response for the update operation.

```xml
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:partner.soap.sforce.com">
    <soapenv:Header>
        <LimitInfoHeader>
            <limitInfo>
                <current>53</current>
                <limit>15000</limit>
                <type>API REQUESTS</type>
            </limitInfo>
        </LimitInfoHeader>
    </soapenv:Header>
    <soapenv:Body>
        <updateResponse>
            <result>
                <id>0016F00002S4Wj0QAF</id>
                <success>true</success>
            </result>
        </updateResponse>
    </soapenv:Body>
</soapenv:Envelope>
```
**Related Salesforce documentation**

[https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_update.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_update.htm)

###### Updating and inserting records

To update existing records and insert new records in a single operation, use salesforce.upsert and specify the following properties. 

**upsert**

```xml
<salesforce.upsert configKey="MySFConfig">
    <allOrNone>0</allOrNone>
    <allowFieldTruncate>0</allowFieldTruncate>
    <externalId>Id</externalId>
    <sobjects xmlns:sfdc="sfdc">{//sfdc:sObjects}</sobjects>
</salesforce.upsert>
```

**Properties**
* allOrNone: Whether to rollback changes if an object fails (see Common Parameters).
* allowFieldTruncate: Whether to truncate strings that exceed the field length (see Common Parameters).
* externalId: The field containing the record ID, that is used by Salesforce to determine whether to update an existing record or create a new one. This is done by matching the ID to the record IDs in Salesforce. By default, the field is assumed to be named "Id".
* sObjects: XML representation of the records to update and insert. When inserting a new record, you do not specify sfdc:Id.

---
**Set the externalId field :**
If you need to give any existing externalId field of sObject to externalId then the payload should be with that externalId field and value as follows in sample
---

**Sample to set ExternalId field and value**

```xml
<payloadFactory>
    <format>
        <sfdc:sObjects xmlns:sfdc="sfdc" type="Account">
          <sfdc:sObject>
             <sfdc:sample__c>{any value}</sfdc:sample__c>
             <sfdc:Name>newname001</sfdc:Name>
          </sfdc:sObject>
       </sfdc:sObjects>
    </format>
    <args/>
</payloadFactory>
 
<salesforce.upsert>
    <allOrNone>0</allOrNone>
    <allowFieldTruncate>0</allowFieldTruncate>
    <externalId>sample__c</externalId>
    <sobjects xmlns:sfdc="sfdc">{//sfdc:sObjects}</sobjects>
</salesforce.upsert>
```

**Sample request**

Given below is a sample request that can be handled by the upsert operation.

```xml
<payloadFactory>
    <format>
        <sfdc:sObjects xmlns:sfdc="sfdc" type="Account">
          <sfdc:sObject>
             <sfdc:Id>0019000000aaMkZ</sfdc:Id>
             <sfdc:Name>newname001</sfdc:Name>
          </sfdc:sObject>
          <sfdc:sObject>
             <sfdc:Name>newname002</sfdc:Name>
          </sfdc:sObject>
       </sfdc:sObjects>
    </format>
    <args/>
</payloadFactory>
 
<salesforce.upsert>
    <allOrNone>0</allOrNone>
    <allowFieldTruncate>0</allowFieldTruncate>
    <externalId>Id</externalId>
    <sobjects xmlns:sfdc="sfdc">{//sfdc:sObjects}</sobjects>
</salesforce.upsert>
```

**Sample response**

Given below is a sample response for the upsert operation.

```xml
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:partner.soap.sforce.com">
    <soapenv:Header>
        <LimitInfoHeader>
            <limitInfo>
                <current>54</current>
                <limit>15000</limit>
                <type>API REQUESTS</type>
            </limitInfo>
        </LimitInfoHeader>
    </soapenv:Header>
    <soapenv:Body>
        <upsertResponse>
            <result>
                <created>false</created>
                <id>0016F00002S4Wj0QAF</id>
                <success>true</success>
            </result>
            <result>
                <created>true</created>
                <id>0016F00002pUVTMQA4</id>
                <success>true</success>
            </result>
        </upsertResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

**Related Salesforce documentation**

[https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_upsert.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_upsert.htm)

###### Searching records

To search for records, use salesforce.search and specify the search string. If you already know the record IDs, use retrieve instead. 

**search**

```xml
<salesforce.search configKey="MySFConfig">
    <searchString>FIND {map*} IN ALL FIELDS RETURNING Account (Id, Name), Contact, Opportunity, Lead</searchString>
</salesforce.search>
```

**Properties**
* searchString: The SQL query to use to search for records.

**Sample response**

Given below is a sample response for the search operation.

```xml
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:partner.soap.sforce.com" xmlns:sf="urn:sobject.partner.soap.sforce.com" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <soapenv:Header>
        <LimitInfoHeader>
            <limitInfo>
                <current>56</current>
                <limit>15000</limit>
                <type>API REQUESTS</type>
            </limitInfo>
        </LimitInfoHeader>
    </soapenv:Header>
    <soapenv:Body>
        <searchResponse>
            <result>
                <searchRecords>
                    <record xsi:type="sf:sObject">
                        <sf:type>Account</sf:type>
                        <sf:Id>0016F00002SN7qiQAD</sf:Id>
                        <sf:Id>0016F00002SN7qiQAD</sf:Id>
                        <sf:Name>GenePoint</sf:Name>
                    </record>
                </searchRecords>
            </result>
        </searchResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

**Related Salesforce documentation**

[https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_search.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_search.htm)

###### Querying records

To retrieve data from an object, use salesforce.query and specify the following properties. If you already know the record IDs, you can use retrieve instead. 

---
**Note**: If you want your search results to include deleted records that are available in the Recycle Bin, use salesforce.queryAll in place of salesforce.query.
---

**query**

```xml
<salesforce.query configKey="MySFConfig">
    <batchSize>200</batchSize>
    <queryString>select id,name from Account</queryString>
</salesforce.query>
```

**Properties**
* batchSize: The number of records to return. If more records are available than the batch size, you can use the queryMore operation to get additional results.
* queryString: The SQL query to use to search for records.

**Sample request**

Following is a sample configuration to query records. It also illustrates the use of queryMore operation to get additional results:

```xml
<salesforce.query>
    <batchSize>200</batchSize>
    <queryString>select id,name from Account</queryString>
</salesforce.query>
<!-- Execute the following to get the other batches -->
<iterate xmlns:sfdc="http://wso2.org/salesforce/adaptor" continueParent="true" expression="//sfdc:iterator">
    <target>
        <sequence>
            <salesforce.queryMore>
                <batchSize>200</batchSize>
            </salesforce.queryMore>
        </sequence>
    </target>
</iterate>
```

**Sample response**

Given below is a sample response for the query operation.

```xml
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:partner.soap.sforce.com" xmlns:sf="urn:sobject.partner.soap.sforce.com" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <soapenv:Header>
        <LimitInfoHeader>
            <limitInfo>
                <current>58</current>
                <limit>15000</limit>
                <type>API REQUESTS</type>
            </limitInfo>
        </LimitInfoHeader>
    </soapenv:Header>
    <soapenv:Body>
        <queryResponse>
            <result xsi:type="QueryResult">
                <done>true</done>
                <queryLocator xsi:nil="true"/>
                <records xsi:type="sf:sObject">
                    <sf:type>Account</sf:type>
                    <sf:Id>0016F00002SasNYQAZ</sf:Id>
                    <sf:Id>0016F00002SasNYQAZ</sf:Id>
                    <sf:Name>wso2New</sf:Name>
                </records>
                .
                .
                <size>129</size>
            </result>
        </queryResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

**Related Salesforce documentation**

[https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_query.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_query.htm)

###### Retrieving specific records

If you know the IDs of the records you want to retrieve, use salesforce.retrieve and specify the following properties. If you do not know the record IDs, use query instead.

**retrieve**

```xml
<salesforce.retrieve configKey="MySFConfig">
    <fieldList>id,name</fieldList>
    <objectType>Account</objectType>
    <objectIDS xmlns:sfdc="sfdc">{//sfdc:sObjects}</objectIDS>
</salesforce.retrieve>
```

**Properties**
* fieldList: A comma-separated list of the fields you want to retrieve from the records.
* objectType: The object type of the records.
* sobjects: XML representation of the records to retrieve.

**Sample request**

Given below is a sample request that can be handled by the retrieve operation.

```xml
<payloadFactory>
   <format>
      <sfdc:sObjects xmlns:sfdc="sfdc">
         <sfdc:Ids>0019000000aaMkK</sfdc:Ids>
         <sfdc:Ids>0019000000aaMjl</sfdc:Ids>
      </sfdc:sObjects>
   </format>
   <args/>
</payloadFactory>
 
<salesforce.retrieve configKey="MySFConfig">
    <fieldList>id,name</fieldList>
    <objectType>Account</objectType>
    <objectIDS xmlns:sfdc="sfdc">{//sfdc:sObjects}</objectIDS>
</salesforce.retrieve>
```

**Sample response**

Given below is a sample response for the retrieve operation.

```xml
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:partner.soap.sforce.com" xmlns:sf="urn:sobject.partner.soap.sforce.com" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <soapenv:Header>
        <LimitInfoHeader>
            <limitInfo>
                <current>60</current>
                <limit>15000</limit>
                <type>API REQUESTS</type>
            </limitInfo>
        </LimitInfoHeader>
    </soapenv:Header>
    <soapenv:Body>
        <retrieveResponse>
            <result xsi:type="sf:sObject">
                <sf:type>Account</sf:type>
                <sf:Id>0016F00002S4Wj0QAF</sf:Id>
                <sf:Id>0016F00002S4Wj0QAF</sf:Id>
                <sf:Name>newname01</sf:Name>
            </result>
        </retrieveResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

**Related Salesforce documentation**

[https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_retrieve.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_retrieve.htm)

###### Deleting records

To delete one or more records, use salesforce.delete and specify the following properties. 

**delete**

```xml
<salesforce.delete configKey="MySFConfig">
   <allOrNone>0</allOrNone>
   <sobjects xmlns:sfdc="sfdc">{//sfdc:sObjects}</sobjects>
</salesforce.delete>
```

**Properties**
* allOrNone: Whether to rollback changes if an object fails (see Common Parameters).
* sobjects: XML representation of the records to delete, as shown in the following example.

**Sample request**

Given below is a sample request that can be handled by the delete operation.

```xml
<payloadFactory>
   <format>
     <sfdc:sObjects xmlns:sfdc="sfdc">
         <sfdc:Ids>0019000000aaMkZ</sfdc:Ids>
         <sfdc:Ids>0019000000aaMkP</sfdc:Ids>
      </sfdc:sObjects>
   </format>
   <args/>
</payloadFactory>
 
<salesforce.delete>
   <allOrNone>0</allOrNone>
   <sobjects xmlns:sfdc="sfdc">{//sfdc:sObjects}</sobjects>
</salesforce.delete>
```

**Sample response**

Given below is a sample response for the delete operation.

```xml
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:partner.soap.sforce.com">
    <soapenv:Header>
        <LimitInfoHeader>
            <limitInfo>
                <current>63</current>
                <limit>15000</limit>
                <type>API REQUESTS</type>
            </limitInfo>
        </LimitInfoHeader>
    </soapenv:Header>
    <soapenv:Body>
        <deleteResponse>
            <result>
                <id>0016F00002S4Wj0QAF</id>
                <success>true</success>
            </result>
        </deleteResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

**Related Salesforce documentation**

[https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_delete.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_delete.htm)

###### Restoring records

To restore records that were previously deleted, use salesforce.undelete and specify the following properties. 

**undelete**

```xml
<salesforce.undelete configKey="MySFConfig">
    <allOrNone>0</allOrNone>
    <sobjects xmlns:sfdc="sfdc">{//sfdc:sObjects}</sobjects>
</salesforce.undelete>
```

**Properties**
* allOrNone: Whether to rollback changes if an object fails (see Common Parameters).
* sobjects: XML representation of the records to restore, as shown in the following example.

**Sample request**

Given below is a sample request that can be handled by the undelete operation.

```xml
<payloadFactory>
   <format>
      <sfdc:sObjects xmlns:sfdc="sfdc">
         <sfdc:Ids>0019000000aaMkZ</sfdc:Ids>
         <sfdc:Ids>0019000000aaMkP</sfdc:Ids>
      </sfdc:sObjects>
    </format>
    <args/>
</payloadFactory>
 
<salesforce.undelete>
    <allOrNone>0</allOrNone>
    <sobjects xmlns:sfdc="sfdc">{//sfdc:sObjects}</sobjects>
</salesforce.undelete>
```

**Sample response**

Given below is a sample response for the undelete operation.

```xml
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:partner.soap.sforce.com">
    <soapenv:Header>
        <LimitInfoHeader>
            <limitInfo>
                <current>64</current>
                <limit>15000</limit>
                <type>API REQUESTS</type>
            </limitInfo>
        </LimitInfoHeader>
    </soapenv:Header>
    <soapenv:Body>
        <undeleteResponse>
            <result>
                <id>0016F00002S4Wj0QAF</id>
                <success>true</success>
            </result>
        </undeleteResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

**Related Salesforce documentation**

[https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_undelete.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_calls_undelete.htm)

**Sample configuration**

Following example illustrates how to connect to Salesforce with the init operation and query operation.

1. Create a sample proxy as below :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse"
       name="salesforce_query"
       startOnLoad="true"
       statistics="disable"
       trace="disable"
       transports="http,https">
   <target>
      <inSequence>
         <property xmlns:ns="wso2.connector.salesforce"
                   expression="//ns:loginUrl/text()"
                   name="loginUrl"/>
         <property xmlns:ns="wso2.connector.salesforce"
                   expression="//ns:username/text()"
                   name="username"/>
         <property xmlns:ns="wso2.connector.salesforce"
                   expression="//ns:password/text()"
                   name="password"/>
         <property xmlns:ns="wso2.connector.salesforce"
                   expression="//ns:blocking/text()"
                   name="blocking"/>
         <property xmlns:ns="wso2.connector.salesforce"
                   expression="//ns:batchSize/text()"
                   name="batchSize"/>
         <property xmlns:ns="wso2.connector.salesforce"
                   expression="//ns:queryString/text()"
                   name="queryString"/>
         <salesforce.init>
            <loginUrl>{$ctx:loginUrl}</loginUrl>
            <username>{$ctx:username}</username>
            <password>{$ctx:password}</password>
            <blocking>{$ctx:blocking}</blocking>
         </salesforce.init>
         <salesforce.query>
            <batchSize>{$ctx:batchSize}</batchSize>
            <queryString>{$ctx:queryString}</queryString>
         </salesforce.query>
         <respond/>
      </inSequence>
      <outSequence>
         <send/>
      </outSequence>
   </target>
   <description/>
</proxy>
                                                             
```

2. Create an XML file named query.xml and copy the XML configurations given below:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                  xmlns:urn="wso2.connector.salesforce">
    <soapenv:Header/>
    <soapenv:Body>
        <urn:loginUrl>https://login.salesforce.com/services/Soap/u/30.0</urn:loginUrl>
        <urn:username>john@gmail.com</urn:username>
        <urn:password>john@123CtGoiPE3mCdjgUHlto8HJ3</urn:password>
        <urn:blocking>false</urn:blocking>
        <urn:queryString>select id,name from Account</urn:queryString>
        <urn:batchSize>2000</urn:batchSize>
    </soapenv:Body>
</soapenv:Envelope>                           
```

3. Replace the credentials with your values.

4. Execute the following curl command:

```bash
curl http://localhost:8280/services/salesforce_query -H "Content-Type: text/xml" -d @query.xml
```

5. Salesforce returns an XML response similar to the response given below:
 
```xml
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:partner.soap.sforce.com" xmlns:sf="urn:sobject.partner.soap.sforce.com" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <soapenv:Header>
        <LimitInfoHeader>
            <limitInfo>
                <current>58</current>
                <limit>15000</limit>
                <type>API REQUESTS</type>
            </limitInfo>
        </LimitInfoHeader>
    </soapenv:Header>
    <soapenv:Body>
        <queryResponse>
            <result xsi:type="QueryResult">
                <done>true</done>
                <queryLocator xsi:nil="true"/>
                <records xsi:type="sf:sObject">
                    <sf:type>Account</sf:type>
                    <sf:Id>0016F00002SasNYQAZ</sf:Id>
                    <sf:Id>0016F00002SasNYQAZ</sf:Id>
                    <sf:Name>wso2New</sf:Name>
                </records>
                .
                .
                <size>129</size>
            </result>
        </queryResponse>
    </soapenv:Body>
</soapenv:Envelope>
```


[Working with the Recycle bin](recyclebin.md)

[Working with logged in User](user.md)

[Working with Emails](emails.md)


### Common parameters

Listed below are parameters that are common when working with records and working with the recycle bin in Salesforce.


| Name | Description | Default value |
| ------------- | ------------- | ------------- |
| allowFieldTruncate | Set to 1 to truncate string values if they exceed the defined field length. | 0 |
| allOrNone | Set to 1 to roll back changes if any object fails when multiple objects are sent. If set to 0 (false), some records can be processed successfully while others are marked as failed in the call results. | 0 |


## Logging out of Salesforce
To log out of Salesforce and close the current connection, use salesforce.logout.

**logout**
```xml
<salesforce.logout/>
```
