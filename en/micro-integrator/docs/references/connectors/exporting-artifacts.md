In this section we focus on creating the Connector Exporter Project and Exporting the CApp. CApp is the deployable artefact on the Enterprise Integrator runtime.

## Creating Connector Exporter Project
1. Go to **File** -> **New** -> **Other** -> **WSO2** -> **Extensions** -> **Project Types** -> **Connector Exporter Project**
  <img src="/assets/img/connectors/connector-exporter-project-1.png" title="Add Connector Exporter Project" width="800" alt="Add Connector Exporter Project"/>

2. Enter a name for the Connector Exporter Project. 

3. In the next screen select, **Specify the parent from workspace** and select the specific ESB Solution Project you created from the dropdown. 
  <img src="/assets/img/connectors/connector-exporter-project-naming.png" title="Naming Connector Exporter Project" width="800" alt="Naming Connector Exporter Project" />

4. Now you need to add the Connector to Connector Exporter Project that you just created. Right click on the Connector Exporter Project and select, **New** -> **Add Remove Connectors** -> **Add Connector** -> **Add from Workspace** -> **Connector**
  <img src="/assets/img/connectors/adding-connector-to-exporter-project-1.png" title="Adding Connector to Exporter Project" width="800" alt="Adding Connector to Exporter Project" />

5. Once you are directed to the workspace, it displays all the connectors that exist in the workspace. You can select the relevant connector and click **Ok**. 
  <img src="/assets/img/connectors/adding-connector-to-exporter-project-3.png" title="Selecting Connector from Workspace" width="800" alt="Selecting Connector from Workspace" />


## Exporting the Composite Application Project
1. Right click on Composite Application Project and click on **Export Composite Application Project**
  <img src="/assets/img/connectors/capp-project1.png" title="Export as a Carbon Application" width="800" alt="Export as a Carbon Application" />

2. Select an **Export Destination** where you want to save the .car file. 

3. In the next **Create a deployable CAR file** screen, select the both created ESB Solution Project and the Connector Exporter Project to save as below and click **Finish**.
  <img src="/assets/img/connectors/saving-projects.png" title="Create a deployable CAR file" width="800" alt="Create a deployable CAR file" />
