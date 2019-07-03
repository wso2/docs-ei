# App Overview

When you open the WSO2 SP Status Dashboard, the [Node
Overview](_Node_Overview_) page is displayed by default. If you want to
view all the Siddhi applications deployed in your WSO2 SP setup, click
on the **App View** tab (marked in the image below). The **App
Overview** tab opens and all the Siddhi applications that are currently
deployed are displayed as shown in the image below.

![](attachments/112391082/112391084.png)

The status is displayed in green for active Siddhi applications, and in
red for inactive Siddhi applications.

If no Siddhi applications are deployed in your WSO2 SP setup, the
following message is displayed.

![](attachments/112391082/112391083.png)

  

The Siddhi applications are listed under the deployment mode in which
they are deployed (i.e., **Single Node Deployment** , **HA Deployment**
, and **Distributed Deployment** ).  

!!! info

If your WSO2 SP setup is a distributed deployment, only the parent
Siddhi applications are displayed in this tab.


The following information is displayed for each Siddhi aplication.

-   **Siddhi Application** : The name of the Siddhi application.
-   **Status** : This indicates whether the Siddhi application is
    currently active or inactive.
-   **Deployed Time** : The time duration that has elapsed since the
    Siddhi application was deployed in the SP setup.
-   **Deployed Node** : The host and the port of the SP node in which
    the Siddhi application is displayed.

The purpose of this tab is to check the status of all the Siddhi
applications that are currently deploed in the SP setup.

If you click on a Siddhi Application under **Single Node Deployment** or
**HA Deployment** , information specific to that Siddhi application is
displayed as explained in [Viewing Statistics for Siddhi
Applications](_Viewing_Statistics_for_Siddhi_Applications_) .

If you click on the parent Siddhi application under **Distributed
Deployment** , information specific to that parent Siddhi application is
displayed as explained in [Viewing Statistics for Parent Siddhi
Applications](_Viewing_Statistics_for_Parent_Siddhi_Applications_) .

If you click on a deployed node, information specific to that node is
displayed as explained in [Viewing Node-specific
Pages](_Viewing_Node-specific_Pages_) .

  

  
