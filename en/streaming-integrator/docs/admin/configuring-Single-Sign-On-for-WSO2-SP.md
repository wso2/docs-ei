# Configuring Single Sign-On for WSO2 SP

!!! note

The functionality described in this section is not yet released.


SSO (Single Sign-On) allows you to be authenticated to access one
application, and gain access to multiple other applications without
having to repeatedly provide your credentials for authentication
purposes. This section explains how you can configure single sign-on for
the WSO2 Dashboard Portal, Status Dashboard and the Business Rules
Manager.

!!! tip

Before you begin:

Configure the external identity provider (IdP) that you are using for
SSO. By default, WSO2 SP uses WSO2 IS (versions 5.4.0 and later) as the
Identity Provider. For detailed instructions to configure WSO2 IS for
this scenario, see [OAuth2 Token Validation and
Introspection](http://docs.wso2.com/identity-server/OAuth2%20Token%20Validation%20and%20Introspection)
.

If you want to use any other identity provider, make sure that it
supports OAuth 2 Dynamic Client Registration, and do the required
configurations (which differ based on the IdP).


  

-   [Enabling SSO](#ConfiguringSingleSign-OnforWSO2SP-EnablingSSO)
-   [Testing the SSO
    configuration](#ConfiguringSingleSign-OnforWSO2SP-TestingtheSSOconfiguration)

  

## Enabling SSO

To configure SSO for the WSO2 SP, open the
`         <SP_HOME>/conf/dashboard/deployment.yaml        ` file and
update it as follows:

1.  In the `           auth.configs          ` section, start creating a
    new entry with a new client type. You need an external IdP client
    for SSO. Therefore, enter `           external          ` as the
    type.

    `             auth.configs:            `  
    **`              type: external             `**

2.  To enable SSO, set the `           ssoEnabled          ` property as
    shown below.

    `             auth.configs:            `  
    `             type: external                                        ssoEnabled: true                         `

3.  In order to allow SSO to function in your SP setup, you need to set
    the following properties under the `           ssoEnabled          `
    property.

    `             auth.configs:            `  
    `             type: external            `  
    `             ssoEnabled: true            `  
    **`              properties:             `**  
    **`              kmDcrUrl:                             https://localhost:9443/identity/connect/register                           `**  
    **`              kmTokenUrl:                             https://localhost:9443/oauth2                           `**  
    **`              kmUsername: admin             `**  
    **`              kmPassword: admin             `**  
    **`              idpBaseUrl:                             https://localhost:9443/scim2                           `**  
    **`              idpUsername: admin             `**  
    **`              idpPassword: admin             `**  
    **`              portalAppContext: portal             `**  
    **`              statusDashboardAppContext: monitoring             `**  
    **`              businessRulesAppContext : business-rules             `**  
    **`              databaseName: WSO2_OAUTH_APP_DB             `**  
    **`              cacheTimeout: 900             `**  
    **`              baseUrl:                             https://localhost:9643                           `**  
    **`              grantType: authorization_code             `**

    The purposes of these properties are explained in the table below.

    <table>
    <thead>
    <tr class="header">
    <th>Property</th>
    <th>Default Value</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><strong><code>                kmDcrUrl               </code></strong></td>
    <td><code>               https://localhost:9443/identity/connect/register              </code></td>
    <td>The Dynamic Client Registration (DCR) endpoint of the key manager in the IdP.</td>
    </tr>
    <tr class="even">
    <td><strong><code>                kmTokenUrl               </code></strong></td>
    <td><code>               https://localhost:9443/oauth2              </code></td>
    <td>The token endpoint of the key manager in the IdP.</td>
    </tr>
    <tr class="odd">
    <td><strong><code>                kmUsername               </code></strong></td>
    <td><code>               admin              </code></td>
    <td>The username for the key manager in the IdP.</td>
    </tr>
    <tr class="even">
    <td><strong><code>                kmPassword               </code></strong></td>
    <td><code>               admin              </code></td>
    <td>The password for the key manager in the IdP.</td>
    </tr>
    <tr class="odd">
    <td><strong><code>                idpBaseUrl               </code></strong></td>
    <td><code>               https://localhost:9443/scim2              </code></td>
    <td>The SCIM2 endpoint of the IdP.</td>
    </tr>
    <tr class="even">
    <td><strong><code>                idpUsername               </code></strong></td>
    <td><code>               admin              </code></td>
    <td>The username for the IdP.</td>
    </tr>
    <tr class="odd">
    <td><strong><code>                idpPassword               </code></strong></td>
    <td><code>               admin              </code></td>
    <td>The password for the IdP.</td>
    </tr>
    <tr class="even">
    <td><strong><code>                portalAppContext               </code></strong></td>
    <td><code>               portal              </code></td>
    <td>The application context of the Dashboard Portal application in WSO2 SP.</td>
    </tr>
    <tr class="odd">
    <td><strong><code>                statusDashboardAppContext               </code></strong></td>
    <td><code>               monitoring              </code></td>
    <td>The application context of the Status Dashboard application in WSO2 SP.</td>
    </tr>
    <tr class="even">
    <td><strong><code>                businessRulesAppContext               </code></strong></td>
    <td><code>               business-rules              </code></td>
    <td>The application context of the Business Rules application in WSO2 SP.</td>
    </tr>
    <tr class="odd">
    <td><strong><code>                databaseName               </code></strong></td>
    <td><code>               WSO2_OAUTH_APP_DB              </code></td>
    <td>The application context of the Business Rules application in WSO2 SP.</td>
    </tr>
    <tr class="even">
    <td><strong><code>                cacheTimeout               </code></strong></td>
    <td><code>               900              </code></td>
    <td>The cache timeout for the validity period of the token in seconds.</td>
    </tr>
    <tr class="odd">
    <td><strong><code>                baseUrl               </code></strong></td>
    <td><code>               https://localhost:9643              </code></td>
    <td><p>The base URL to which the token should be redirected after the code returned</p>
    <p>from the <strong>Authorization Code</strong> grant type is used to get the token.</p></td>
    </tr>
    <tr class="even">
    <td><strong><code>                grantType               </code></strong></td>
    <td><code>               authorization_code              </code></td>
    <td>The grant type used in the OAuth application token request.</td>
    </tr>
    <tr class="odd">
    <td><strong><code>                externalLogoutURL               </code></strong></td>
    <td><code>                               https://localhost:9443/samlsso                             </code></td>
    <td>The URL via which you can llog out from the external IDP provider side in the SSO.</td>
    </tr>
    </tbody>
    </table>

4.  Save your changes.

## Testing the SSO configuration

Once the above changes are made, you can start the dashboard server of
WSO2 SP and access all the UIs in it with a single sign-on. To try this
out, follow the steps below:

1.  Start the dashbaord server by issuing one of the following commands:
    -   **On Windows** : `            dashboard.bat --run           `
    -   **On Linux/Mac OS** : `             sh dashboard.sh            `

2.  Access the Dashboard Portal via the following URL.  

    `           https://localhost:9643/portal                     `

3.  In the dialog box that appears to sign in, enter
    `          admin         ` as both the user name and the password,
    and then click **LOG IN** .
4.  Now access the Business Rules Manager via the following URL.  
    `          https://localhost:9643/b                     usiness-rules                              `  
    No dialog box appears for the Business Rules Manager. This because
    you provided your credentials to access the Dashboard Portal, and
    the activation of SSO makes that sign-in valid for all the UIs
    accessible via the dashboard profile.
