# Installing via the Installer

Follow the steps given below to install the WSO2 Micro Integrator runtime and its monitoring Dashboard.

## Download and install

Go to the WSO2 Micro Integrator [product page](https://wso2.com/integration/#), click **Download**, and then click the **Installer**.

The **product installer** that is compatible with your operating system will be downloaded.

Double-click to open the installation wizard, which will guide you through the installation. When you finish, all the runtimes of WSO2 Enterprise Integrator will be installed and ready for use.

## Starting the MI server

If you installed the Micro Integrator using the installer, use the following instructions.

-  On **MacOS/Linux/CentOS**

      Open a terminal and execute the command given below.

      ```bash
      sudo wso2mi
      ```

      If you have **installed the product using the installer** and you want to manually run the product startup script from the `MI_HOME/bin` directory, you need to use the following command:

      ```bash
      sudo sh launcher_micro-integrator.sh
      ```

      This script automatically assigns the JAVA HOME of your VM to the root user of your Micro Integrator instance.

-  On **Windows**

      Go to **Start Menu -> Programs -> WSO2 -> Enterprise Integrator**. This will open a terminal and start the relevant profile.

## Stopping the MI server

To stop the Micro Integrator runtime, press Ctrl+C in the command window.

## Accessing the MI_HOME directory

**MI_HOME** is the installation location of the Micro Integrator runtime. When you use the **installer**, the `MI_HOME` is located in a place specific to your OS as shown below:

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
         <td><code>/Library/WSO2/EnterpriseIntegrator/7.1.0/micro-integrator</code></td>
      </tr>
      <tr class="even">
         <td>Windows</td>
         <td><code>C:\Program Files\WSO2\Enterprise Integrator\7.1.0\micro-integrator</code></td>
      </tr>
      <tr class="odd">
         <td>Ubuntu</td>
         <td><code>/usr/lib/wso2/wso2ei/7.1.0/micro-integrator</code></td>
      </tr>
      <tr class="even">
         <td>CentOS</td>
         <td><code>/usr/lib64/wso2/wso2ei/7.1.0/micro-integrator</code></td>
      </tr>
   </tbody>
</table>

## Uninstalling the product

If you used the **installer** to install WSO2 Enterprise Integrator, you can uninstall by following the steps given below:

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
  <code>sudo bash /Library/WSO2/EnterpriseIntegrator/7.1.0/uninstall.sh</code>
</div>
</div>
</div></td>
</tr>
<tr class="even">
<td>Windows</td>
<td>Go to <strong>Start Menu -&gt; Programs -&gt; WSO2 -&gt; Uninstall Enterprise Integrator 7.1.0</strong> or search <strong>Uninstall Enterprise Integrator 7.1.0</strong> and click the shortcut icon. This will uninstall the product from your computer.</td>
</tr>
<tr class="odd">
<td>Ubuntu</td>
<td><div class="content-wrapper">
<p>Open a terminal and run the following command:</p>
<code>sudo apt purge wso2ei-7.1.0</code>
</div>
</div>
</div></td>
</tr>
<tr class="even">
<td>CentOS</td>
<td><div class="content-wrapper">
<p>Open a terminal and run the following command:</p>
<code>sudo yum remove wso2ei-7.1.0</code>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>
