# Installing the Streaming Integrator in a Virtual Machine

Follow the steps given below to install and run WSO2 Streaming Integrator on a VM.

## Installing the Streaming Integrator

Follow the steps below:

1.  Go to the Streaming Integrator [product page](https://wso2.com/integration/streaming-integrator/) and click **Download** to get the **product installer**. The installer that is compatible with you operating system is downloaded.

    !!! Info
        Alternatively, go to **Other Installation Options** and click **Binary** to download the product distribution as a ZIP file.

2.  If you used the installer, double-click to open the installation
    wizard, that guides you through the installation. When you
    finish, the product is installed and ready for use.

### Accessing the HOME directory

Let's call the installation location of your product the
**`<SI_HOME>`** directory.

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
         <td><code>              /Library/WSO2/StreamingIntegrator/1.0.0             </code></td>
      </tr>
      <tr class="even">
         <td>Windows</td>
         <td><code>              C:\Program Files\WSO2\StreamingIntegrator\1.0.0             </code></td>
      </tr>
      <tr class="odd">
         <td>Ubuntu</td>
         <td><code>              /usr/lib/wso2/StreamingIntegrator/1.0.0             </code></td>
      </tr>
      <tr class="even">
         <td>CentOS</td>
         <td><code>              /usr/lib64/StreamingIntegrator/1.0.0             </code></td>
      </tr>
   </tbody>
</table>

### Uninstalling the product

If you used the **installer** to install the product, you can uninstall by following the steps given below:

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
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>sudo bash /Library/WSO2/StreamingIntegrator/<span class="fl">1.0.</span><span class="dv">0</span>/uninstall.<span class="fu">sh</span></span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="even">
<td>Windows</td>
<td>Go to the <strong>Start Menu -&gt; Programs -&gt; WSO2 -&gt; Uninstall Streaming Integrator 1.0.0</strong> or search <strong>Uninstall Micro Integrator 1.0.0</strong> and click the shortcut icon. This uninstalls the product from your computer.</td>
</tr>
<tr class="odd">
<td>Ubuntu</td>
<td><div class="content-wrapper">
<p>Open a terminal and run the following command:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a>sudo apt-get purge wso2si-<span class="fl">1.0.</span><span class="dv">0</span></span></code></pre></div>
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

## Running the Streaming Integrator

Start the WSO2 Streaming Integrator by following the instructions given below.

### Using the installer

* On **MacOS/Linux/CentOS**, open a terminal and execute the command given below.
  ```bash
  sudo wso2si-1.0.0
  ```
  The operation log keeps running until the profile starts, which usually
       takes several seconds. Wait until the profile fully boots up and
       displays a message similar to " *WSO2 Carbon started in n seconds.* "
* On **Windows**, go to **Start Menu -\> Programs -\> WSO2 -\> Streaming Integrator.** This
opens a terminal and start the relevant profile.

### Using the binary distribution

1.  Before you execute the product startup script, be sure to set the
    JAVA HOME in your machine. Use a [JDK that is compatible with WSO2
    Streaming
    Integrator](https://docs.wso2.com/display/compatibility/Tested+Operating+Systems+and+JDKs).

2.  Open a terminal and navigate to the
    `<SI_HOME>/bin/         ` directory, where
    `<SI_HOME>` is the home directory of your product
    distribution.
3.  Execute the relevant command.

    * On **MacOS/Linux/CentOS**
      ```bash
      sh streaming-integrator.sh
      ```
      If you have **installed the product using the installer**, and you want to manually run the product startup script from the
      `/bin` directory, you need to issue the `sudo
      launcher_streaming-integrator` command. This script automatically
      assigns the JAVA HOME of your VM to the root user of your Streaming
      Integrator instance.

    * On **Windows**
      ```bash
      streaming-integrator.bat
      ```

By default, the HTTP listener port is 8290 and the default HTTPS
listener port is 8253.

## Stopping the Streaming Integrator

To stop the Streaming Integrator runtime, press Ctrl+C in the command
window.

