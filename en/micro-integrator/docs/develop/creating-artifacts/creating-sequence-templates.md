# Creating Sequence Templates

Follow the instructions given below to create a new Sequence Template in WSO2 Integration Studio.

## Instructions

Follow these steps to create a new scheduled task.

1.  Right click the project and go to **New → Template** to open the
    **New Template Artifact** dialog.
2.  Type a unique name for the template and specify **Sequence Template** as the type of template
    you are creating.
3.  Click **Finish** . The template is created in the
    `          src/main/synapse-config/templates         ` folder under
    the ESB Config project you specified. When prompted, you can open
    the file in the editor, or you can right-click the template in the
    project explorer and click **Open With \> ESB Editor**.

## Properties

The parameters available to configure the Sequence Template are as follows.

| Parameter Name      | Description                                                       |
|---------------------|-------------------------------------------------------------------|
| Name                | The name of the Sequence Template                                 |
| onError             | Select the error sequence that needs to be invoked.               |
| Trace Enabled       | Whether or not trace is to be enabled for the sequence.           |
| Statistics Enabled  | Whether or not statistics is to be enabled for the sequence.      |
| Template Parameters | The input parameter that are supported by this Sequence Template. |

## Examples
..

## Guides
