# Monitoring Stream Processor

The Status Dashboard of WSO2 SP allows you to monitor the metrics of a
stand-alone WSO2 SP instance or a WSO2 SP cluster. This involves
monitoring whether all processes of the WSO2 SP setup are working in a
healthy manner, monitoring the current status of a SP node, and viewing
metrics relating to the history of a node or the cluster.  Both JVM
level metrics or Siddhi application level metrics can be viewed from the
monitoring dashboard.

The following sections cover how to configure the Status Dashboard and
analyze statistics relating to your WSO2 SP deployment in it.

-   [Configuring the Status
    Dashboard](_Configuring_the_Status_Dashboard_)
-   [Viewing Statistics](_Viewing_Statistics_)

## App Overview

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

  

  ## Viewing Statistics
  
  To view the status dashboard, follow the procedure below:
  
  1.  Start the dashboard for your worker node by issuing one of the
      following commands:  
      For Windows: `          dashboard.bat         `  
      For Linux : `          ./dashboard.sh         `
  2.  Access the Status Dashboard via the following URL format.  
  
      `           https://localhost:<SP_DASHBOARD_PORT>/sp-status-dashboard          `
  
      e.g., <https://0.0.0.0:9643/sp-status-dashboard>  
        
      After login this opens  the Status Dashboard with the nodes that you
      have already added as shown in the example below.  
      ![](attachments/112391026/112391027.png) [  
      I](https://0.0.0.0:9643/sp-status-dashboard) f no nodes are
      displayed, add the nodes for which you wnt to view statistics by
      following the steps in [Worker Overview - Adding a worker to the
      dashboard](Node-Overview_112391028.html#NodeOverview-AddWorker) .
  
  For a detailed descripion of each page in this dashboard, see the
  following topics:
  
  -   [Node Overview](_Node_Overview_)
  -   [Viewing Node-specific Pages](_Viewing_Node-specific_Pages_)
  -   [Viewing Worker History](_Viewing_Worker_History_)
  -   [Viewing Statistics for Siddhi
      Applications](_Viewing_Statistics_for_Siddhi_Applications_)
  -   [Viewing Statistics for Parent Siddhi
      Applications](_Viewing_Statistics_for_Parent_Siddhi_Applications_)
  -   [App Overview](_App_Overview_)

