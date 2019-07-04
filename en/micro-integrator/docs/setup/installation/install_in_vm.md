# Installing in a VM

Follow the steps given below to install and run the Micro Integrator on
a VM.

## Installing the WSO2 Micro Integrator

Follow the steps below:

1.  Go to the WSO2 Micro Integrator [product page](https://wso2.com/integration/micro-integrator/) and download.

    > Note that there are several options for installing the product in
        various environments. Use the available links for more information
        on each option.
    

2.  If you used the installer, double-click to open the installation
    wizard, which will guide you through the installation. When you
    finish, the product will be installed and ready for use.

### Accessing the HOME directory

Let's call the installation location of your product the
**MI_HOME** directory.

If you used the **installer** to install the product, this is located in
a place specific to your OS as shown below:

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
         <td><code>              /Library/WSO2/MicroIntegrator/1.0.0             </code></td>
      </tr>
      <tr class="even">
         <td>Windows</td>
         <td><code>              C:\Program Files\WSO2\MicroIntegrator\1.0.0             </code></td>
      </tr>
      <tr class="odd">
         <td>Ubuntu</td>
         <td><code>              /usr/lib/wso2/MicroIntegrator/1.0.0             </code></td>
      </tr>
      <tr class="even">
         <td>CentOS</td>
         <td><code>              /usr/lib64/MicroIntegrator/1.0.0             </code></td>
      </tr>
   </tbody>
</table>
### Uninstalling the product

If you used the **installer** to install the product, you can uninstall
by following the steps given below:

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
<td><div class="content-wrapper">
<p>Open a terminal and run the following command as the root user:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>sudo bash /Library/WSO2/MicroIntegrator/<span class="fl">1.0.</span><span class="dv">0</span>/uninstall.<span class="fu">sh</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="even">
<td>Windows</td>
<td>Go to the <strong>Start Menu -&gt; Programs -&gt; WSO2 -&gt; Uninstall Micro Integrator 1.0.0</strong> or search <strong>Uninstall Micro Integrator 1.0.0</strong> and click the shortcut icon. This will uninstall the product from your computer.</td>
</tr>
<tr class="odd">
<td>Ubuntu</td>
<td><div class="content-wrapper">
<p>Open a terminal and run the following command:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a>sudo apt-get purge wso2mi-<span class="fl">1.0.</span><span class="dv">0</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="even">
<td>CentOS</td>
<td><div class="content-wrapper">
<p>Open a terminal and run the following command:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb3" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb3-1"><a href="#cb3-1"></a>sudo yum remove wso2ei-<span class="fl">1.0.</span><span class="dv">0</span>-x86_<span class="dv">64</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

## Running the Micro Integrator

Start the WSO2 Micro Integrator by following the instructions given
below.

### Using the installer

* On **MacOS/Linux/CentOS**, open a terminal and execute the command given below.
  ``` java
  sudo wso2mi-1.0.0
  ```
  The operation log keeps running until the profile starts, which usually
       takes several seconds. Wait until the profile fully boots up and
       displays a message similar to " *WSO2 Carbon started in n seconds.* "
* On **Windows**, go to **Start Menu -\> Programs -\> WSO2 -\> Micro Integrator.** This
will open a terminal and start the relevant profile.

### Using the binary distribution

1.  Before you execute the product startup script, be sure to set the
    JAVA HOME in your machine. Use a [JDK that is compatible with WSO2
    Micro
    Integrator](https://docs.wso2.com/display/compatibility/Tested+Operating+Systems+and+JDKs)
    .
2.  Open a terminal and navigate to the
    `          <MI_HOME>/bin/         ` directory, where
    `          <MI_HOME>         ` is the home directory of your product
    distribution.
3.  Execute the relevant command.

    * On **MacOS/Linux/CentOS**
      ``` java
      sh micro-integrator.sh
      ```
          If you have **installed the product using the installer**
          , and you want to manually run the product startup script from the
          /bin directory, you need to use the 'sudo
          launcher\_micro-integrator' command. This script automatically
          assigns the JAVA HOME of your VM to the root user of your Micro
          Integrator instance.
    * On **Windows**
      ``` java
      micro-integrator.bat
      ```

By default, the HTTP listener port is 8290 and the default HTTPS
listener port is 8253.

## Stopping the Micro In tegrator

To stop the Micro Integrator runtime, press Ctrl+C in the command
window.