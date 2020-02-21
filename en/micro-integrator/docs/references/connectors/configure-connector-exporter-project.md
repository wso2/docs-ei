## Configure the connector in WSO2 Integration Studio

1. Open WSO2 Integration Studio and create an ESB Solution Project.
<p><img src="/assets/img/connectors/solution-project.png" title="Creating a new ESB Solution Project" width="800" alt="Creating a new ESB Solution Project" /></p>

2. Right click on the project that you created and click on 'Add or Remove Connector' -> Add Connector.
<p><img src="/assets/img/connectors/add-connector.png" title="Add or Remove Connector" width="800" alt="Add or Remove Connector" /></p>

3. It will be navigated to the WSO2 Connector Store. 
4. Search for File Connector and download it to the workspace. 
<p><img src="/assets/img/connectors/search-connector.png" title="Search Connector in the Connector Store" width="800" alt="Search Connector in the Connector Store" /></p>

5. Now it needs to add a Connector Exporter Project. 
6. Go to File -> New -> Other -> WSO2 -> Extensions -> Project Types -> Connector Exporter Project
<p><img src="/assets/img/connectors/connector-exporter-project-1.png" title="Add Connector Exporter Project" width="800" alt="Add Connector Exporter Project" /></p>

<p><img src="/assets/img/connectors/connector-exporter-project-2.png" title="Add Connector Exporter Project" width="800" alt="Add Connector Exporter Project" /></p>

7. Provide a name for Connector Exporter Project and in the next screen tick on 'Specify the parent from workspace' and select the File Connector project. 
<p><img src="/assets/img/connectors/connector-exporter-project-naming.png" title="Naming Connector Exporter Project" width="800" alt="Naming Connector Exporter Project" /></p>

8. 10. Now it needs to add the Connector to Connector Exporter project that we just created. Right Click on the Connector Exporter project -> New -> Add Remove Connectors -> Add Connector -> Add from Workspace -> File Connector
<p><img src="/assets/img/connectors/adding-connector-to-exporter-project-1.png" title="Adding Connector to Exporter Project" width="800" alt="Adding Connector to Exporter Project" /></p>

<p><img src="/assets/img/connectors/adding-connector-to-exporter-project-2.png" title="Adding Connector to Exporter Project" width="800" alt="Adding Connector to Exporter Project" /></p>

<p><img src="/assets/img/connectors/adding-connector-to-exporter-project-3.png" title="Adding Connector to Exporter Project" width="800" alt="Adding Connector to Exporter Project" /></p>