# Exposing a Web Resource

This example demonstrates how a web resource can be exposed as a data service.

## Synapse configuration
Given below is the data service configuration you need to build. See the instructions on how to [build and run](#build-and-run) this example.

```xml
<data name="WebResource" transports="http https local">
   <config enableOData="false" id="WebHarvestDataSource">
      <property name="web_harvest_config">&lt;config&gt;&#xd; &lt;var-def name='AppInfo'&gt;&#xd;  &lt;xslt&gt;&#xd;   &lt;xml&gt;&#xd;    &lt;html-to-xml&gt;&#xd;     &lt;http method='get' url='https://play.google.com/store/apps'/&gt;&#xd;    &lt;/html-to-xml&gt;&#xd;   &lt;/xml&gt;&#xd;   &lt;stylesheet&gt;&#xd;   &lt;![CDATA[ &lt;xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"&gt;&#xd;        &lt;xsl:output method="xml" omit-xml-declaration="yes" indent="yes"/&gt;&#xd;        &lt;xsl:template match="/"&gt;&#xd;                 &lt;AppInfo&gt;&#xd;                    &lt;xsl:for-each select="//*[@class='details']"&gt;&#xd;                    &lt;App&gt;&#xd;                     &lt;Title&gt;&lt;xsl:value-of select="a[@class='title']"/&gt;&lt;/Title&gt;&#xd;                     &lt;Description&gt;&lt;xsl:value-of select="div[@class='description']"/&gt;&lt;/Description&gt;&#xd;                    &lt;/App&gt;&#xd;                  &lt;/xsl:for-each&gt;&#xd;                 &lt;/AppInfo&gt;&#xd;                &lt;/xsl:template&gt;&#xd;         &lt;/xsl:stylesheet&gt;&#xd;   ]]&gt;&#xd;   &lt;/stylesheet&gt;&#xd;  &lt;/xslt&gt;&#xd; &lt;/var-def&gt;&#xd;&lt;/config&gt;</property>
   </config>
   <query id="webquery" useConfig="WebHarvestDataSource">
      <scraperVariable>AppInfo</scraperVariable>
      <result element="AppInfo" rowName="App">
         <element column="Title" name="Title" xsdType="string"/>
         <element column="Description" name="Description" xsdType="string"/>
      </result>
   </query>
   <operation name="GetResourceOp">
      <call-query href="webquery"/>
   </operation>
   <resource method="GET" path="AppInfo">
      <call-query href="webquery"/>
   </resource>
</data>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create a Data Service project](../../../../develop/creating-projects/#data-services-project).
3. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator. 

You can send an HTTP GET request to invoke the data service using cURL
as shown below.

```bash
curl -X GET http://localhost:8290/services/WebResource.HTTPEndpoint/AppInfo
```

This will return the response in XML.