# Export a Project

With the Integration Studio, you can export created projects and import them back again. Let's export a **Maven Multi Module Project** as an example.

## Prerequisites

1. It is recommended to [get the latest updates](../../develop/installing-WSO2-Integration-Studio#get-the-latest-updates) for your [WSO2 Integration Studio](../../develop/installing-WSO2-Integration-Studio) before trying these instructions.
2. Create a **Maven Multi Module Project** and an **Integration Project** (Optional) following the instructions in [here](create-multi-maven-project.md).

## Exporting a Maven Multi Module Project
Now, you can see the **Maven Multi Module Project** and the **Integration Project** (optionally) in the project explorer as following.

    <img src="../../assets/img/create_project/proj_explorer_Integration_proj_in_multi_module.png" width="400">

Follow the steps given below to export the project.   

1.  Right click on the **Maven Multi Module Project** and click **Export**.

    <img src="../../assets/img/create_project/export_dialog_1.png" width="1000">

2. In the **Export** dialog that opens, navigate to **WSO2** folder, select **Projects Export as Archive File** in it and click **Next**.
    
    !!! Info
            You also select the **Projects Export as File System** to export as folders.
            
    <img src="../../assets/img/create_project/export_dialog_2.png" width="1000">
    
3.  Click on **Browse** and give a path and a name for the archive file. You can also select other projects that should be included in the archive file.

4.  Click **Finish** to generate the archive file.

## Import the exported project (Optional)
In order to verify that the project was successfully exported, let's import the exported project back to the workspace doing the following steps:

1.  First, the existing projects must me removed as the **Integration Studio** does not allow to import projects with same names as the existing projects. To remove the projects, do the following steps:
    
    1. Right click on the **Maven Multi Module Project** and click on **Delete**.
    
    2. In the **Delete Resources** dialog that opens, click **OK**. Make sure to tick **Delete project contents on disk** option as well.
    
    3. Click **Continue** on the confirmation dialog that opens.

2.  Click on the **File** and click on **Import**.

3.  In the **Export** dialog that opens, navigate to **WSO2** folder, select **Existing WSO2 Projects into workspace** in it and click **Next**.
    
    <img src="../../assets/img/create_project/import_project_1.png" width="500">
 
4.  In the next dialog, select **Select archive file** option and select the archive file through the **Browse** button.
    
    <img src="../../assets/img/create_project/import_project_2.png" width="500">
    
5.  Click **Finish** to import the project.

Now see the projects have been imported as the following.

    <img src="../../assets/img/create_project/proj_explorer_Integration_proj_in_multi_module.png" width="400">
