# Adding a Priority Executor

!!! info

Note

Please note that this feature is deprecated.


**Priority executors** can be used to execute sequences with a given
priority. This allows the user to control the resources allocated to
executing sequences and prevent high priority messages from getting
delayed and dropped.

Follow the instructions below to add a new priority executor to the WSO2
EI.

1\. Sign in. Enter your user name and password to log on to the EI
Management Console.

2\. Click on "Main" in the left menu to access the "Manage" menu.

![](attachments/119131408/119131409.png)

3\. In the "Manage" menu, click on "Priority Executors" under "Service
Bus."

![](attachments/119131408/119131422.png)

4\. In the "Priority Executors" window, click on the "Add Executor" link.

![](attachments/119131408/119131410.png)

5\. Specify the options of a new priority executor in the "Priority
Executor Design" widow.

-   **Executor Name** - Name of the Executor.
-   **Fixed Size Queues** - Whether fixed size queues are used or not.
-   **Queues** - Individual Queue Configurations. See the detailed
    information below.
-   **Next Queue Algorithm**
-   **Max** - Maximum Number of Threads in the Executor.
-   **Core** - Core Number of Threads in the Executor.
-   **Keep-Alive** - Keep Alive time for Threads.

![](attachments/119131408/119131421.png)

!!! info

Tip

An executor should have at least two or more queues. If only one queue
is used, there is no point in using a priority executor.


6\. Each and every priority has a queue associated with it. A message is
put in to one of the queues corresponding to its priority. To add a
queue to an executor, click on the "Add Queue" link.

![](attachments/119131408/119131413.png)

7\. Specify the "Queues" options:

-   **Priority** - Priority of the Queue.
-   **Size** - Size of the Queue. This option is visible only if fixed
    size queues are selected.

![](attachments/119131408/119131414.png)

!!! info

Tip

You can remove a queue from an executor clicking on the "Delete" link in
the actions column.

![](attachments/119131408/119131420.png)


8\. Click on the "Save" button to add an executor to the list.

![](attachments/119131408/119131418.png)

9\. A new priority executor appears in the list.

![](attachments/119131408/119131417.png)
