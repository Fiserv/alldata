# AllData® Next-Gen Widgets Integration Guide
November 2021



## Executive Summary

### Overview

Fiserv provides three integration options to leverage our industry leading financial product and services suite:

- Web services APIs that expose available data and functionality directly within the partner&#39;s UI
- Brandable UI that uses the Fiserv user interface for ease of deployment and integration
- Configurable widgets providing specific functionality that can be embedded within the partner&#39;s UI

Fiserv widgets provide compelling value propositions to financial institutions (FIs) wanting to leverage AllData® product and services, including:

- Tight integration of data and functionality within a partner&#39;s UI
- Reduced implementation, integration, and maintenance costs associated with developing a front end for aggregation-specific functionality

### Objectives

The primary objectives of this document include the following.

- Explain the different approaches to integrate the new AllData Add Account widget within a partner application.
- Describe the functionality available using the new AllData Add Account widget, Alert Resolution widget, and Account Management widget.
- Identify the configurable parameters and setup needed to access the widget.

### Scope and Assumptions

The scope of this document includes following AllData widgets:

- Add Accounts
- Alert Resolution
- Account Management



## Integration Approaches

Widgets are web resources, composed of a few web pages and accessed like normal HTTP URLs. They can be embedded in any UI or displayed in separate dialogs/windows like any other internet-based resource. The user must sign on to Fiserv and establish a session using SAML or single sign-on (SSO) using web services before accessing any widgets.

### SAML 2.0 SSO

The user will log in to the partner application. When the user accesses a widget by clicking a URL or button, the partner will initiate the SAML 2.0 SSO to sign the user in to Fiserv. The SAML request will include the SAML artifact (sent using POST parameter **SAMLResponse** ) and the widget URL. After the Fiserv SSO server validates the assertion, the user will be redirected to the widget URL.

The SAML 2.0 artifact must contain the following attributes to sign on the user and establish a session.

- **user\_id:** Username of a user, for example, _jsmith_ – It must match the username given during user registration (advisor or client).
- **ce\_pwd:** Password provided during user registration – The password must comply with Fiserv password policies.
- **user\_type:** User role – Valid values are &quot;advisor&quot; and &quot;client.&quot;
- **partner\_id:** An ID constant assigned to a partner at the time of initial setup
- **home\_id:** An ID assigned to a home (broker/dealer) of a partner at the time of initial home setup

The other optional attributes partner can send are:

- **fi\_id:** Required for add more accounts – This ID represents the FI the user has account with and is available when pulling the FI data using the _getFinancialInstInfo_ API. This parameter is required to add a new institution in a deep linking implementation. (See [Deep Linking](?path=/docs/alldata-next-gen/implementation-approaches.md#deep-linking))
- **login\_acct\_id:** Required for error resolution and add more accounts – This ID represents the user&#39;s login account the institution. This information is available using the _getAccountDetails_ API.
- **acct\_id:** Required for error resolution – This ID represents the user&#39;s account with the institution.
- **return\_url:** Partner preferred URL to return control back after completion of action in the widget – Refer to individual widget documentation to find out what parameters are sent back on this URL invocation.
- **keepalive\_url:** Partner preferred URL to keep the partner session alive
- **error\_url:** Partner preferred URL to which user will be redirected to if an error occurs during widget invocation
- **offline\_account\_url:** Partner preferred URL to direct the user to add offline accounts within the partner&#39;s screens and functionality
- **css\_url:** Partner preferred URL to retrieve cascading style sheet – This URL will override **css\_url** set up at the home level. This URL is appended to the end of CE CSS URLs so that the partner can define only the styles they want to override.
- **invocation\_mode:** Partners preferring to embed the widget within their application or to launch the widget as a popup can use this parameter. Possible values are &quot;embedded&quot; and &quot;popup.&quot;
- **partner\_app\_id:** Required for FIs using OAuth model – This ID is shared by partners during implementation. It is unique to each application the partner integrates with the AllData widget. It is used to access user account information from OAuth-implemented institutions.

The widget URL itself must be sent as a POST parameter named **RelayState**. After the SSO server establishes a session, the user can access any widget without needing to sign on again.

### SSO Using Session Token

If the partner does not support SAML 2.0, they can still redirect users to Fiserv AllData widgets using an alternative form of SSO. In this approach the partner establishes a session using a web service API and passes that session token to the widget URL to access the functionality.

Partners who do not support SAML 2.0 can redirect users to AllData widgets using an alternative form of SSO by establishing a session with a web service API and passing that session token to the widget URL.

The steps to access the widgets are:

1. Ensure that the user is registered with Fiserv AllData. If the user is not registered, call AllData RESTful web service API _UserMgmt/createUser_.
2. Call Fiserv AllData RESTful web service (or the XML) API _ClientMgmt/signon_ to authenticate the user. This service will respond with the session token after signing on successfully.
3. Use the Fiserv provided URL to access the appropriate widget. You must pass the session token as a POST parameter named **sessionToken** when invoking the URL. (GET is not supported.)
4. Use the appropriate web service or XML API to pull data after the user has completed working with the widget.

The _ClientMgmt/signon_ web service expects the following data elements in the request:

- Username ( **UserID** )
- Password ( **UserPassword** )
- Home ID ( **HomeID** )
- Partner ID ( **partnerID** )

Please note:

- The other optional parameters defined in the [SAML 2.0 SSO section](#saml-20-sso) can also be sent as POST parameters when invoking widget URL.
- A single session token can be used to invoke multiple widgets so long as the session is still valid.
- A session becomes invalid after timing out from 15 minutes of inactivity or after using the _ClientMgmt/signoff_ API.
- Every widget invocation needs a valid session token passed through as a POST parameter with the invocation URL.

Optional parameters to pass to the widget&#39;s URL through POST parameters are:

- **fi\_id:** Required to add more accounts – This ID represents the FI where the user has an account and is available when pulling the institution data with the _getFinancialInstInfo_ API. This parameter is required to add a new institution in a deep linking implementation. (See [Deep Linking](?path=/docs/alldata-next-gen/implementation-approaches.md#deep-linking) section.)
- **login\_acct\_id:** Required for error resolution and to add more accounts – This ID represents the login account the user has with the FI. This information is available using the _getAccountDetails_ API.
- **acct\_id:** Required for error resolution – This ID represents the account the user has with the FI.
- **return\_url:** Partner preferred URL to return control back – Refer to individual widget documentation for the parameters sent back on this URL invocation.
- **keepalive\_url:** Partner preferred URL to keep the partner session alive
- **error\_url:** Partner preferred URL to which user will be redirected if an error occurs during widget invocation
- **offline\_account\_url:** Partner preferred URL to direct the user to add offline accounts within the partner&#39;s screens and functionality
- **css\_url:** Partner preferred URL to retrieve cascading style sheet – This URL will override the **css\_url** set up at the home level. This URL is appended at the end of CE CSS URLs so that the partner can define only the styles they want to override.
- **invocation\_mode:** Partners preferring to embed the widget within their application using iframe or to launch the widget as a popup can use this parameter. Possible values are &quot;embedded&quot; and &quot;popup.&quot;
- **partner\_app\_id:** Required for FIs using OAuth model – This ID is shared by partners during implementation. It is unique to each application the partner integrates with the AllData widget. It is used to access user account information from OAuth-implemented institutions.

**Signon Request/Response**

| Web service name | Update status check |
|---|---|
| Resource URL | &lt;FiservWSUrl&gt;/UserMgmt/signon  |
| Description | This API will authenticate the user and generate a session token. |
| API Explorer Link | [signon](../api/?type=post&path=/WealthManagementWeb/ws/UserMgmt/signon) |

##### Example URL to invoke Add Accounts widget:

&lt;Domain URL&gt;PFM\_UI/widgets/base/addaccounts/addAccountsWidget.iface

…with the mandatory POST parameter &quot;sessionToken=&lt;sessionToken&gt;&quot; and any optional parameters required for the scenario

### Reverse Keep Alive URL

Partners may occasionally need to extend AllData sessions even after the AllData widget is closed. To extend an AllData session, open a &quot;keep-alive&quot; URL from the partner application modeled after the following.

&lt;Domain URL&gt;keepAlive/keepAlive.iface

…with the mandatory POST parameter &quot;sessionToken=&lt;sessionToken&gt;&quot;

### Logout URL

The AllData session times out after 15 minutes of user inactivity. A partner wanting to log out gracefully before 15 minutes can use a logout URL modeled after the following.

&lt;Domain URL&gt;logout

…with the mandatory POST parameter &quot;sessionToken=\&lt;sessionToken\&gt;&quot;

Widget invocation may return response codes in certain situations. The following table lists the possible codes and their causes.


| Response code | Reason |
|---|---|
| 3000 | Session expired |
| 3002 | Invalid login account ID **login_acct_id** |
| 3003 | When the add flow is successful without returning any account, the user can re-initiate the add flow for the same FI or a different FI. If this response code persists, report the issue to Fiserv. |
| 3005 | Invalid **partner_application_id** required for OAuth FIs |


---

© 2020-2021 Fiserv, Inc. or its affiliates. All rights reserved. This work is confidential, and its use is strictly limited. Use is permitted only in accordance with the terms of the agreement under which it was furnished. Any other use, duplication, or dissemination without the prior written consent of Fiserv, Inc. or its affiliates is strictly prohibited. The information contained herein is subject to change without notice. Except as specified by the agreement under which the materials are furnished, Fiserv, Inc. and its affiliates do not accept any liabilities with respect to the information contained herein and are not responsible for any direct, indirect, special, consequential or exemplary damages resulting from the use of this information. No warranties, either express or implied, are granted or extended by this document.

[http://www.fiserv.com](http://www.fiserv.com/)

Fiserv is a registered trademark of Fiserv, Inc.

Other brands and their products are trademarks or registered trademarks of their respective holders and should be noted as such.




This document has been created by Fiserv and is classified **Fiserv Confidential**. This document is restricted to the received party and not to be forwarded or transferred without the approval of Fiserv.


