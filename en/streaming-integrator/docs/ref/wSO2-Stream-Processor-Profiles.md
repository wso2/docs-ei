# WSO2 Stream Processor Profiles

WSO2 Stream Processor has four profiles in order to run different
functions. When you start and run any of these profiles, you are running
only the subset of Stream Processor features that are specific to that
profile. Some use cases supported require you to run two or more of
these profiles in parallel.

The four profiles are as follows.

-   [Editor profile](#WSO2StreamProcessorProfiles-Editorprofile)
-   [Dashboard profile](#WSO2StreamProcessorProfiles-Dashboardprofile)
-   [Worker profile](#WSO2StreamProcessorProfiles-Workerprofile)
-   [Manager profile](#WSO2StreamProcessorProfiles-Managerprofile)

## Editor profile

<table>
<tbody>
<tr class="odd">
<td>Purpose</td>
<td><div class="content-wrapper">
<p>This runs the developer environment where the following can be carried out:</p>
<ul>
<li>Creating Siddhi applications/Siddhi application templates.</li>
<li>Testing and debugging Siddhi applications to determine whether they are ready to be used in a production environment.</li>
</ul>
!!! info
<ul>
<li>For more information about creating and testing Siddhi applications, see <a href="https://docs.wso2.com/display/SP440/Understanding+the+Development+Environment">Understanding the Development Environment</a> .</li>
<li>For more information about creating business templates, see <a href="https://docs.wso2.com/display/SP440/Creating+a+Business+Rule+Template">Creating a Business Rule Template</a> .</li>
</ul>

</div></td>
</tr>
<tr class="even">
<td>Starting and running the profile</td>
<td><p>Navigate to the <code>              &lt;SP_HOME&gt;/bin             </code> directory and issue one of the following commands:</p>
<ul>
<li>For Windows: <code>               editor.bat              </code></li>
<li>For Linux: <code>               ./editor.sh              </code></li>
</ul></td>
</tr>
<tr class="odd">
<td>Deployment</td>
<td><div class="content-wrapper">
<p>To deploy a Siddhi application in this profile, place the relevant <code>               &lt;SIDDHI_APPLICATION_NAME&gt;.siddhi              </code> file in the <code>               &lt;SP_HOME&gt;/wso2/editor/deployment/workspace              </code> directory.</p>
!!! info
<p>The Siddhi files created and saved via the <a href="https://docs.wso2.com/display/SP440/Stream+Processor+Studio+Overview">Stream Processor Studio</a> are stored in the <code>               &lt;SP_HOME&gt;/wso2/editor/deployment/workspace              </code> directory by default.</p>

</div></td>
</tr>
<tr class="even">
<td>Tools shipped</td>
<td><p>When you start WSO2 SP in the editor profile, the URLs to access the following tools appear in the start-up logs.</p>
<ul>
<li>Stream Processor Studio</li>
<li>Template Editor</li>
</ul></td>
</tr>
</tbody>
</table>

  

## Dashboard profile

<table>
<tbody>
<tr class="odd">
<td>Purpose</td>
<td><p>This profile is available for the following purposes:</p>
<ul>
<li>Visualizing processed data via dashboards. For more information, see <a href="https://docs.wso2.com/display/SP440/Visualizing+Data">Visualizing Data</a> .</li>
<li>Deploying and managing business templates and business rules. For more information, see <a href="https://docs.wso2.com/display/SP440/Creating+Business+Rules">Creating Business Rules</a> .</li>
<li>Running the Status Dashboard to monitor the health of your WSO2 SP deployment. For more information, see <a href="https://docs.wso2.com/display/SP440/Monitoring+Stream+Processor">Monitoring Stream Processor</a> .</li>
</ul></td>
</tr>
<tr class="even">
<td>Starting and running the profile</td>
<td><p>Navigate to the <code>              &lt;SP_HOME&gt;/bin             </code> directory and issue one of the following commands:</p>
<ul>
<li>For Windows: <code>               dashboard.bat              </code></li>
<li>For Linux: <code>               ./dashboard.sh              </code></li>
</ul></td>
</tr>
<tr class="odd">
<td>Deployment</td>
<td><p>The following deployments are possible:</p>
<ul>
<li>Custom widgets can be deployed by placing the compiled react code in the <code>               &lt;SP_HOME&gt;/wso2/dashboard/deployment/web-ui-apps/portal/extensions/widgets              </code> directory. For more information, see <a href="https://docs.wso2.com/display/SP440/Creating+Custom+Widgets">Creating Custom Widgets</a> .</li>
<li>Dashboards that need to be imported can be deployed by placing the JSON file with the dashboard configuration in the <code>               &lt;SP_HOME&gt;/wso2/dashboard/resources/dashboards              </code> directory. For more information, see <a href="https://docs.wso2.com/display/SP440/Importing+and+Exporting+Dashboards">Importing and Exporting Dashboards</a> .</li>
<li>Business rules that need to deployed can be added in the <code>               &lt;SP_HOME&gt;/wso2/dashboard/resources/business-rules              </code> directory. For more information, see <a href="https://docs.wso2.com/display/SP440/Managing+Business+Rules#ManagingBusinessRules-ManagingBusinessRules-Deployingbusinessrules">Managing Business Rules - Deploying Business Rules</a> .</li>
</ul></td>
</tr>
<tr class="even">
<td>Tools</td>
<td><p>When you start WSO2 SP in the dashboard profile, the URLs to access the following tools appear in the start-up logs.</p>
<ul>
<li>Dashboard Portal</li>
<li>Business Rules Manager</li>
<li>Status Dashboard</li>
</ul></td>
</tr>
</tbody>
</table>

## Worker profile

<table>
<tbody>
<tr class="odd">
<td>Purpose</td>
<td>This profile runs the Siddhi applications in a production environment when WSO2 SP is deployed in a single node or as a <a href="https://docs.wso2.com/display/SP440/Minimum+High+Availability+Deployment">minimum HA cluster.</a> For more information, see <a href="_Deploying_Streaming_Applications_">Deploying Streaming Applications</a> .</td>
</tr>
<tr class="even">
<td>Starting and running the profile</td>
<td><p>Navigate to the <code>              &lt;SP_HOME&gt;/bin             </code> directory and issue one of the following commands:</p>
<ul>
<li>For Windows: <code>               worker.bat              </code></li>
<li>For Linux: <code>               ./worker.sh              </code></li>
</ul></td>
</tr>
<tr class="odd">
<td>Deployment</td>
<td>To deploy a Siddhi application in this profile, place the relevant <code>             &lt;SIDDHI_APPLICATION_NAME&gt;.siddhi            </code> in the <code>             &lt;SP_HOME&gt;/wso2/worker/deployment/siddhi-files            </code> directory.</td>
</tr>
</tbody>
</table>

## Manager profile

<table>
<tbody>
<tr class="odd">
<td>Purpose</td>
<td>This profile runs the distributed Siddhi applications in a production environment when WSO2 SP is set up as a fully distributed deployment. For more information, see <a href="https://docs.wso2.com/display/SP440/Fully+Distributed+Deployment">Fully Distributed Deployment</a> .</td>
</tr>
<tr class="even">
<td>Starting and running the profile</td>
<td><p>Navigate to the <code>              &lt;SP_HOME&gt;/bin             </code> directory and issue one of the following commands:</p>
<ul>
<li>For Windows: <code>               manager.bat              </code></li>
<li>For Linux: <code>               ./manager.sh              </code></li>
</ul></td>
</tr>
<tr class="odd">
<td>Deployment</td>
<td>To deploy a Siddhi application in this profile, place the relevant <code>             &lt;SIDDHI_APPLICATION_NAME&gt;.siddhi            </code> file in the <code>             &lt;SP_HOME&gt;/wso2/manager/deployment/siddhi-files            </code> directory.</td>
</tr>
</tbody>
</table>
