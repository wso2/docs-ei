# Quick Tour - WSO2 Integration Studio

WSO2 Integration Studio is a development environment used to design your
integration scenarios and develop them. This can be used to develop
services, features and artifacts as well as manage their links and
dependencies through a simplified graphical editor.

## Workbench

When you start using WSO2 Integration Studio, one of the first decisions
you have to make is where to store your projects and files. This
location is called the workspace. The Workbench is essentially different
windows and editors that can be used for various purposes related to
your workspace. This includes, the Project Explorer, the Editor,
Template Guide, Outline, etc. Multiple Workbench windows can be opened
simultaneously for an enhanced and fine-tuned developer experience.

![workbench](../assets/img/workbench/workbench-integration-studio.png)  

## Editor

You can associate different editors with different types of files. For
example, when you open a file for editing by double-clicking it in one
of the navigation views, the associated editor opens in the Workbench.
If there is no associated editor for a resource, the Workbench attempts
to launch an external editor outside the Workbench. (On Windows, the
Workbench will first attempt to launch the editor in place as an OLE
document. This type of editor is referred to as an embedded editor. For
example, if you have a .doc file in the Workbench and Microsoft Word is
registered as the editor for .doc files in your operating system, then
opening the file will launch Word as an OLE document within the
Workbench editor area. The Workbench menu bar and toolbar will be
updated with options for Microsoft Word.)

Any number of editors can be open at once, but only one can be active at
a time. The main menu bar and toolbar for the Workbench window contain
operations that are applicable to the active editor.

Tabs in the editor area indicate the names of resources that are
currently open for editing. An asterisk (\*) indicates that an editor
has unsaved changes.

By default, editors are stacked in the editor area, but you can choose
to tile them in order to view source files simultaneously.

Here is an example of a text editor in the Workbench. This is the source
view of a pom.xml file.

![editor](../assets/img/workbench/workbench-editor.png)

## Project explorer

The Project Explorer provides a view of all the files and folder
structure within a project.

![project explorer](../assets/img/workbench/workbench-project-explorer.png)

## Outline

The Outline view displays an outline of a structured file that is
currently open in the editor area, and lists structural elements. It
enables you to hide certain fields, methods, and types, and also allows
you to sort and filter to find what you want. The contents of the
Outline view are editor specific. For example, in a Java source file,
the structural elements are classes, fields, and methods. The contents
of the toolbar are also editor specific.

![outline](../assets/img/workbench/workbench-outline.png)

## Properties

The properties view displays property names and values for a selected
item such as a resource. Toolbar buttons allow you to toggle to display
properties by category or to filter advanced properties. Another toolbar
button allows you to restore the selected property to its default
value.  

![properties](../assets/img/workbench/workbench-properties.png)

## Console

The Console View displays a variety of console types depending on the
type of development and the current set of user settings.

The three consoles that are provided by default with WSO2 Integration
Studio are:

-   **Process Console** - Shows standard output, error, and input
-   **Stacktrace Console** - Well-formatted Java stacktrace with
    hyperlinks to specific source code locations
-   **CVS Console** - Displays output from CVS operations

## Template guide

The template guide includes a list of sample projects that represent
integration scenarios. You can use these to explore WSO2 Micro Integrator and see a
sample of how it addresses common integration problems.

## What's Next?

-   See [Installing WSO2 Integration Studio](../develop/installing-WSO2-Integration-Studio.md) for installation instructions.
-   See [Working with WSO2 Integration Studio](../develop/working-with-wso2-integration-studio.md) for more information on how to setup and use tooling.
-   See [Troubleshooting WSO2 Integration Studio](../develop/troubleshooting-WSO2-Integration-Studio.md) for information on troubleshooting errors you may run into while using EI Tooling.
