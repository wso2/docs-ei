# Working with Mediators via Tooling

If you need to create a custom mediator that performs some logic on a
message, you can either create a new mediator project , or import an
existing mediator project using WSO2 Integration Studio.

!!! tip

You need to have WSO2 Integration Studio installed to create a new
message store or to import an existing message store. For instructions,
see [Installing WSO2 Integration
Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio)
.


Once a mediator project is finalised, you can export it as a deployable
artifact by right-clicking on the project and selecting **Export Project
as Deployable Archive** . This creates a JAR file that you can deploy to
the EI. Alternatively, you can group the mediator project as a Composite
Application Project, create a Composite Application Archive (CAR), and
deploy it to the EI.

!!! info

A URL classloader is used to load classes in the mediator (class
mediators are not deployed as OSGi bundles). Therefore, it is only
possible to refer to the class mediator from artifacts packed in the
same CAR file in which the class mediator is packed. Accessing the class
mediator from an artifact packed in another CAR file is not possible.
However, it is possible to refer to the class mediator from a sequence
packed in the same CAR file and call that sequence from any other
artifact packed in other CAR files.


#### Creating a mediator project

Follow these steps to create a new mediator.

!!! tip

Alternatively, you can import a mediator project. To do this, o pen WSO2
Integration Studio, click **File** , and then click **Import** . Next,
select **Existing WSO2 Projects into workspace** under the **WSO2**
category, click **Next** and upload the pre-packaged project.


1.  Open **WSO2 Integration Studio** , click ****Miscellaneous → Create
    New Mediator Project **** in the ****Getting Started**** tab as
    shown below.
2.  Leave the first option selected and click **Next** . The New
    Mediator Creation Wizard appears.  
    ![](attachments/119131468/119131473.png){width="550"}  
    Do the following:  
    1.  Type a unique name for the project.
    2.  Specify the package and class names you are creating.
    3.  Optionally specify the location where you want to save the
        project (or leave the default location specified).
    4.  Optionally specify the working set, if any, that you want to
        include in this project.
3.  A Maven POM file will be generated automatically for this project.
    If you want to include parent POM information in the file from
    another project in this workspace, click **Next** , click the
    **Specify Parent from Workspace** check box, and then select the
    parent project.
4.  Click **Finish** .

The mediator project is created in the workspace location you specified
with a new mediator class that extends
`         org.apache.synapse.mediators.AbstractMediator        ` .

#### Importing a Java Mediator Project

Follow the steps below to import a Java mediator project (that includes
a Java class, which extends the
`         org.apache.synapse.mediators.AbstractMediator        `
class) to WSO2 Integration Studio.

1.  Open **WSO2 Integration Studio** , click ****Miscellaneous → Create
    New Mediator Project **** in the ****Getting Started**** tab as
    shown below.
2.  Select **Import From Workspace** and click **Next** .
3.  Specify the mediator project in this workspace that you want to
    import. Only projects with source files that extend
    `          org.apache.synapse.mediators.AbstractMediator         `
    are listed. Optionally, you can change the location where the
    mediator project will be created and add it to working sets.
4.  Click **Finish** .

The mediator project you selected is created in the location you
specified.

!!! info

The mediator projects you create using WOS2 Integration Studio are of
the
`         org.wso2.developerstudio.eclipse.artifact.mediator.project.nature        `
nature by default. Follow the steps below to view this nature added to
the `         <PROJECT_NAME>/target/.project        ` file of the Java
mediator project you imported.

-   Click the **View Menu** icon, and click **Filters and
    Customization** .  
    ![](attachments/119131468/119131471.png){width="613" height="294"}
-   Deselect **.\*resources** , and click **OK** .

