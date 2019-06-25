# Installing in a VM

See the topics given below on how to install WSO2 Enterprise Integrator:

## Download and install the product

If the installation [prerequisites](_Installation_Prerequisites_) are
satisfied, follow the steps below:

1.  Go to the WSO2 Integration [product page](https://wso2.com/integration) and download the product.
    
    > Note that there are several options for installing the product in various environments. Use the available links for more information on each option.
    

2.  If you downloaded the **installer** (.pkg file), double-click to
    open the installation wizard, which will guide you through
    the installation. When you finish, the product will be installed and
    ready for use.

    > **Want to install WSO2 EI as a service?**
    
    > See the following topics in the WSO2 administration guide for instructions:
        - [Installing WSO2 Products as a Windows Service](https://docs.wso2.com/display/ADMIN44x/Installing+as+a+Windows+Service)
        - [Installing WSO2 Products as a Linux Service](https://docs.wso2.com/display/ADMIN44x/Installing+as+a+Linux+Service)

## Access the HOME directory

Let's call the installation location of your product the
**\<EI\_HOME\>** directory.

If you installed the product using the **installer**, this is located in a place specific to your OS as shown below:

<table style="width:100%;">
   <colgroup>
      <col style="width: 9%" />
      <col style="width: 90%" />
   </colgroup>
   <thead>
      <tr class="header">
         <th>OS</th>
         <th>Home directory</th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>Mac OS</td>
         <td><code>/Library/WSO2/EnterpriseIntegrator/6.5.0</code></td>
      </tr>
      <tr class="even">
         <td>Windows</td>
         <td><code>C:\Program Files\WSO2\EnterpriseIntegrator\6.5.0\</code></td>
      </tr>
      <tr class="odd">
         <td>Ubuntu</td>
         <td><code>/usr/lib/wso2/EnterpriseIntegrator/6.5.0</code></td>
      </tr>
      <tr class="even">
         <td>CentOS</td>
         <td><code>/usr/lib64/EnterpriseIntegrator/6.5.0</code></td>
      </tr>
   </tbody>
</table>

## Uninstall the product

If you installed the product using the **installer**, you can uninstall it by following the instructions:

<table>
   <thead>
      <tr class="header">
         <th>OS</th>
         <th>Instructions</th>
      </tr>
   </thead>
   <tbody>
      <tr class="odd">
         <td>Mac OS</td>
         <td>
            <div class="content-wrapper">
               <p>Open a terminal and run the following command as the root user:</p>
               <div class="code panel pdl" style="border-width: 1px;">
                  <div class="codeContent panelContent pdl">
                     <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence">
                        <pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>sudo bash /Library/WSO2/EnterpriseIntegrator/<span class="fl">6.5.</span><span class="dv">0</span>/uninstall.<span class="fu">sh</span></span></code></pre>
                     </div>
                  </div>
               </div>
            </div>
         </td>
      </tr>
      <tr class="even">
         <td>Windows</td>
         <td>Go to the <strong>Start Menu -&gt; Programs -&gt; WSO2 -&gt; Uninstall Enterprise Integrator 6.5.0</strong> or search <strong>Uninstall Enterprise Integrator 6.5.0</strong> and click the shortcut icon. This will uninstall the product from your computer.</td>
      </tr>
      <tr class="odd">
         <td>Ubuntu</td>
         <td>
            <div class="content-wrapper">
               <p>Open a terminal and run the following command:</p>
               <div class="code panel pdl" style="border-width: 1px;">
                  <div class="codeContent panelContent pdl">
                     <div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence">
                        <pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a>sudo apt-get purge wso2ei-<span class="fl">6.5.</span><span class="dv">0</span></span></code></pre>
                     </div>
                  </div>
               </div>
            </div>
         </td>
      </tr>
      <tr class="even">
         <td>CentOS</td>
         <td>
            <div class="content-wrapper">
               <p>Open a terminal and run the following command:</p>
               <div class="code panel pdl" style="border-width: 1px;">
                  <div class="codeContent panelContent pdl">
                     <div class="sourceCode" id="cb3" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence">
                        <pre class="sourceCode java"><code class="sourceCode java"><span id="cb3-1"><a href="#cb3-1"></a>sudo yum remove wso2ei-<span class="fl">6.5.</span><span class="dv">0</span>-x86_<span class="dv">64</span></span></code></pre>
                     </div>
                  </div>
               </div>
            </div>
         </td>
      </tr>
   </tbody>
</table>

## What's next?

-   See the instructions for [running the
    product](_Running_the_Product_) .