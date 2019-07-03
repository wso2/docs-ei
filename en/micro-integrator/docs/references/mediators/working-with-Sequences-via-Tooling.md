# Working with Sequences via Tooling

You can create a sequence in your ESB Config project or in the registry
and then add it right to that project's mediation workflow, or you can
refer to it from a sequence mediator in the same ESB Config project or
another project in this Eclipse workspace.

This section describes how to [create a new
sequence](#WorkingwithSequencesviaTooling-create) or [import an existing
sequence](#WorkingwithSequencesviaTooling-import) from an XML file (such
as a Synapse Configuration file), and how to [use the
sequence](#WorkingwithSequencesviaTooling-use) in your mediation flow.

#### About dynamic sequences

WSO2 Integration Studio allows you to create a Registry Resource
project, which can be used to store Resources and Collections you want
to deploy to the registry of a Carbon Server through a Composite
Application (C-App) project. When you create a sequence, you can save it
as a dynamic sequence in the Registry Resource project and refer to that
sequence from the mediation flow. At runtime, when you deploy the CAR
file with both the Registry Resource project and mediation flow, the ESB
profile looks up and uses the sequence from the registry.

#### Creating a new sequence

Follow these steps to create a new, reusable sequence that you can add
to your mediation workflow or refer to from a sequence mediator, or to
create a sequence mediator and its underlying sequence all at once.

**To create a reusable sequence:**

1.  Create an ESB Config project: Open **WSO2 Integration Studio**
    , click ****Miscellaneous → Create New Config Project **** in the
    ****Getting Started**** tab, and follow the instructions on the
    wizard.  
    ![](attachments/119131387/119133603.png){width="800" height="397"}
2.  Right-click the ESB Config project on the project explorer, click
    **Sequence** .
3.  Select **Create New Sequence** and click **Next** .
4.  Specify a unique name for the sequence.

        !!! info
    
        Creating a Main Sequence
    
        If you want to create the default main sequence that just sends
        messages without mediation, be sure to name it
        `           main          ` , which automatically populates the
        sequence with the default in and out sequences.
    

5.  Do one of the following:  
    -   To save the sequence in an existing ESB Config project in your
        workspace, click **Browse** and select that project.
    -   To save the sequence in a new ESB Config project, click **Create
        new Project** and create the new project.
    -   To save the sequence as a [dynamic
        sequence](#WorkingwithSequencesviaTooling-DynamicSeq) in a
        Registry Resource project, click **Make this as Dynamic
        Sequence** , specify the registry space (Governance or
        Configuration), click the **Browse** button at the top of the
        dialog box next to **Save Sequence in** and select the registry
        resource project, and then type the sequence name in the
        Registry Path box.
6.  Optionally, in the **Advanced Configuration** section, specify
    another sequence to run when there is an error and the endpoint
    where messages should be sent.
7.  Click **Finish** . The sequence is created in the sequences folder
    under the ESB Config or Registry Resource project you specified, and
    the sequence is open in the editor.
8.  Add the endpoints and other sequences you want in this sequence and
    then click **File \> Save** .

The sequence is now available in the **Defined Sequences** section of
the tool palette and ready for use.

**To create a sequence when creating a sequence mediator:**

1.  With your proxy service open in the editor, click **Sequence
    Mediator** in the tool palette and then click the location in the
    mediation workflow where you want to add this sequence.  
    The sequence mediator is added to the workflow with a default name,
    which is highlighted and ready for you to change.
2.  Type the name you want for this sequence mediator and press
    **Enter** .
3.  Double-click the sequence mediator you just added. A sequence is
    created and opened in the editor using the same name you entered for
    the sequence mediator.
4.  Add the endpoints and other sequences you want in this sequence, and
    then click **Save** .

The mediation workflow is updated with the endpoints you added to the
sequence. The sequence is also now available in the **Defined
Sequences** section of the tool palette and ready for use in other
meditation workflows.

#### Importing a sequence

Follow these steps to import an existing sequence from an XML file (such
as a Synapse configuration file) into an ESB Config project.

1.  Create an ESB Config project: Open **WSO2 Integration Studio**
    , click ****Miscellaneous → Create New Config Project **** in the
    ****Getting Started**** tab, and follow the instructions on the
    wizard.
2.  Right-click the ESB Config project on the project explorer, click
    **Sequence** .
3.  Select **Import Sequence** and click **Next** .
4.  Specify the sequence file by typing its full path name or clicking
    **Browse** and navigating to the file.
5.  In the **Save Sequence In** field, specify an existing ESB Config
    project in your workspace where you want to save the sequence, or
    click **Create new Project** to create a new ESB Config project and
    save the sequence there.
6.  In the **Advanced Configuration** section, select the sequences you
    want to import.
7.  Click **Finish** . The sequences you selected are created in the
    sequences folder under the ESB Config project you specified, and the
    first sequence is open in the editor.

#### Using a sequence

After you create a sequence, it appears in the **Defined Sequences**
section of the tool palette. To use this sequence in a mediation flow,
click the sequence in the tool palette and then click the spot on the
canvas where you want the sequence to appear in the flow. The editor
automatically adds any endpoints you used in your sequence.

![](attachments/119131387/119131388.png){width="900"}

If you want to use a sequence from a different project or from the
registry, you need to create a sequence mediator and then refer to the
sequence as follows:

1.  Click **Sequence Mediator** on the tool palette, and then click the
    spot on the canvas where you want the sequence to appear in the
    mediation workflow.
2.  Press **Enter** to accept the default name for now.
3.  In the **Properties** pane at the bottom of the window, click
    **Static Reference Key** , and then click the browse **\[...\]**
    button on the right.  
    ![](attachments/119131387/119131389.png){width="900"}
4.  In the Resource Key Editor, click **Registry** if the sequence is
    stored in the registry or **Workspace** if it's in another ESB
    Config project in this Eclipse workspace.
5.  If you are trying to select a sequence from the registry and no
    entries appear in the dialog box, click the add registry connection
    button (the first button in the upper right corner) and connect to
    the registry where the sequence resides.
6.  Navigate to the sequence you want, select it and click **OK** , and
    then click **OK** again.

The sequence mediator name and static reference key are updated to point
to the sequence you selected.
