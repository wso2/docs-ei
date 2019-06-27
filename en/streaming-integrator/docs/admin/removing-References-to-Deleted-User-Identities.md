# Removing References to Deleted User Identities

This section covers how to remove references to deleted user identities
in WSO2 SP by running the [Forget-me tool](_Forget-me_Tool_Overview_) .

!!! tip

Before you begin

-   Note that this tool is designed to run in offline mode (i.e., the
    server should be shut down or run on another machine) in order to
    prevent unnecessary load to the server. If this tool runs in online
    mode (i.e., when the server is running), DB lock situations on the
    H2 databases may occur.  
-   If you have configured any JDBC database other than the H2 database
    provided by default, copy the relevant JDBC driver to the
    `          <SP_HOME>/wso2/tools/identity-anonymization-tool/lib         `
    directory.


1.  Open a new terminal window and navigate to the
    `          <SP_HOME>/bin         ` directory.
2.  Execute one of the following commands depending on your operating
    system:

    -   On Linux/Mac OS:
        `            ./forgetme.sh -U <username>           `
    -   On Windows: `            forgetme.bat -U <username>           `

        !!! info
    
        Note
    
        The commands specified above use only the
        `           -U <username>          ` option, which is the only
        required option to run the tool. There are several other optional
        command line options that you can specify based on your requirement.
        The supported options are described in detail below.
    
        <table>
        <thead>
        <tr class="header">
        <th>Command Line Option</th>
        <th>Description</th>
        <th>Required</th>
        <th>Sample Value</th>
        </tr>
        </thead>
        <tbody>
        <tr class="odd">
        <td>U</td>
        <td>The name of the user whose identity references you want to remove.</td>
        <td>Yes</td>
        <td><code>               -U john.doe              </code></td>
        </tr>
        <tr class="even">
        <td>d</td>
        <td>The configuration directory to use when the tool is run.<br />
        If you do not specify a value for this option, the <code>               &lt;SP_HOME&gt;/wso2/tools/identity-anonymization-tool-x.x.x/conf              </code> directory (which is the default configuration directory of the tool) is used.</td>
        <td>No</td>
        <td><code>               -d &lt;TOOL_HOME&gt;/conf              </code></td>
        </tr>
        <tr class="odd">
        <td>T</td>
        <td><div class="content-wrapper">
        <p>The tenant domain of the user whose identity references you want to remove.</p>
        !!! info
        <p>If you specify a tenant domain via this option, use the <code>                 TID                </code> option to specify the ID of which the references must be removed.</p>

    </div></td>
    <td>No</td>
    <td><p><code>                -T acme-company               </code></p>
    <p>The default value is <code>                carbon.super               </code></p></td>
    </tr>
    <tr class="even">
    <td>TID</td>
    <td><div class="content-wrapper">
    <p>The tenant ID of the user whose identity references you want to remove.</p>
        !!! info
        <p>It is required to specify a tenant ID if you have specified a tenant domain via the <code>                 TID                </code> option.</p>

    </div></td>
    <td>No</td>
    <td><code>               -TID 2346              </code></td>
    </tr>
    <tr class="odd">
    <td>D</td>
    <td>The user store domain name of the user whose identity references you want to remove.</td>
    <td>No</td>
    <td><p><code>                -D Finance-Domain               </code></p>
    <p>The default value is <code>                PRIMARY               </code> .</p></td>
    </tr>
    <tr class="even">
    <td>pu</td>
    <td>The pseudonym with which the user name of the user whose identity references you want to remove should be replaced. If you do not specify a pseudonym when you run the tool, a random UUID value is generated as the pseudonym by default.</td>
    <td>No</td>
    <td><p><code>                -pu “123-343-435-545-dfd-4”               </code></p></td>
    </tr>
    <tr class="odd">
    <td>carbon</td>
    <td><p>The CARBON HOME. This should be replaced with the variable <code>                $CARBON_HOME               </code> in directories configured in the main configuration file.</p></td>
    <td>No</td>
    <td><code>               -carbon “/usr/bin/wso2sp/wso2sp4.1.0              </code></td>
    </tr>
    </tbody>
    </table>

