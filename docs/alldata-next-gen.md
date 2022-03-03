# AllData® Next-Gen Widget Integration Guide

## Executive Summary

### Overview

Fiserv provides three integration options to leverage our industry leading financial product and services suite:

- Web services APIs that expose available data and functionality directly within the partner&#39;s UI
- Brandable UI that uses the Fiserv user interface for ease of deployment and integration
- Configurable widgets providing specific functionality that can be embedded within the partner&#39;s UI

Fiserv widgets provide compelling value propositions to financial institutions (FIs) wanting to leverage AllData product and services, including:

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


## Integration Approach

Widgets are web resources, composed of a few web pages and accessed like normal HTTP URLs. They can be embedded in any UI or displayed in separate dialogs/windows like any other internet-based resource. The user must sign on to Fiserv and establish a session using SAML or single sign-on (SSO) using web services before accessing any widgets.


### SSO Using Session Token

Partners can redirect users to Fiserv AllData widgets using SSO by establishing a session using a web service API and passing the session token to the widget URL.

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

- Optional parameters can also be sent as POST parameters at widget invocation.
- A single session token can be used to invoke multiple widgets so long as the session is still valid.
- A session becomes invalid after timing out from 15 minutes of inactivity or after using the _ClientMgmt/signoff_ API.
- Every widget invocation needs a valid session token passed through as a POST parameter with the invocation URL.

Optional parameters to pass to the widget&#39;s URL through POST parameters are:

- **fi\_id:** Required to add more accounts – This ID represents the FI where the user has an account and is available when pulling the institution data with the _getFinancialInstInfo_ API. This parameter is required to add a new institution in a deep linking implementation. (See [Deep Linking](#deep-linking) section.)
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

## Add Accounts Widget

This section describes the workflow and functionality of the Add Accounts widget. The Add Accounts workflow starts with the user selecting the financial institution holding the account(s) and sharing login credentials to allow the system to add the account(s) and aggregate data from the institution. AllData provides two approaches to launching the Add Accounts widget in a partner application: The default method is the user searching for an institution or selecting one from a list of popular FIs. The alternate approach is called &quot;[deep linking](#deep-linking).&quot;

### Add Accounts Workflow

By default, the Add Accounts widget includes the necessary screens to walk the user through adding a single financial institution at a time. After submitting credentials for an FI, the harvesting process starts, and the UI shows a progress indicator until the next screen displays. Any harvesting errors must be resolved before the user continues. After the account balances are successfully retrieved, a confirmation screen appears in the Add Accounts widget.

**Add Accounts workflow:**

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/next-gen-widgets-integration-guide/next-gen-widgets-integration-guide-00.png"/>

When the Add Accounts widget is launched, the FI search screen appears first. The FI search screen presents the user with four different options to continue: (1) enter a search string for the desired FI, (2) select from the list of popular FIs, (3) navigate to the offline account addition screens, and (4) add real estate values powered by Zillow.

Partners have the option to configure the popular financial institutions of their choice to display when launching the Add Account widget. The popular financial institution logos are displayed in the FI search screen, and the user can choose a logo to add one or more accounts under the selected institution. Partners have a choice of ten or twelve institutions to show under the popular list, based on whether they choose to display the Add Offline Accounts and Zillow logos.

The FI search algorithm uses &quot;word searching&quot; instead of &quot;letter searching.&quot; The search string returns matches starting at the beginning of each of the words in the FI name, instead of any part of the FI name.

When the user enters three letters, an auto-complete dropdown appears below the search field. The number of matches to the user-entered text appears below the search field. The auto-complete functionality initially loads the first ten matches, with the institutions&#39; respective logos. A **Show More** button appears at the bottom of the widget to optionally display more matching institutions from the result set.

Clicking **Show More** includes another ten institution from the result set in the drop-down list. Repeated clicks on the button reveal additional sets of ten institutions from the results until there are no further matches, when the **Show More** button disappears.

From the search results screen the user can enter another search string, return to the initial Add Accounts screen, or select an FI from the results list to add accounts. Selecting a result opens the credentials submission screen for that institution.

When FI request their users to do additional setup to allow aggregating from their website, the information to do additional setup will be provided as special instructions on choosing FI to add accounts. Users need to follow the special instructions before adding accounts from that FI.

Same instructions will be provided on the top of credentials submission screen of that FI. Users can choose **Show More** to view the complete instructions.

After the user provides credentials, an &quot;in-progress&quot; screen displays while the harvesting process attempts to access the FI and retrieve the accounts. The user must wait for the harvesting process to return a result before continuing the process.

After successfully harvesting data, the widget shows the Account Classification screen if it is configured. Otherwise it shows the Account Confirmation screen if it is configured. If both screens are turned off, the widget calls the **return\_url** to return control back to the partner application. This invocation of **return\_url** contains two parameters ( **FILoginAcctId** and **AcctId** ) which signify the newly added accounts. The **FILoginAcctId** contains one ID referring to the parent login ID record that the user submitted the credentials for. The **AcctId** contains a comma-separated list of accounts that are accessible with these login credentials and are referred to as children accounts.

If the harvest attempt fails due to a harvest alert, the corresponding alert resolution screen appears.

If the harvesting process encounters any errors, they will display within the Add Accounts widget. The alert content and workflows are included in [Appendix A – Harvesting Alert Resolution Screens](#harvesting-alert-resolution-screens).

After the user resolves the harvesting error through the alert resolution screen, another harvesting attempt initiates. The Add Accounts widget displays the in-progress screen until the harvest completes.

If the harvest attempt is successful, and if the configuration parameter is set to True, the Account Classification screen appears. If the harvest attempt fails due to a harvest alert, the next screen will display either the corresponding alert resolution screen or the account confirmation screen, depending on the partner configuration.

After the accounts are retrieved, if the Account Classification screen is enabled, an inventory of the child accounts appears with the account names, types, and totals.

On the Account Classification screen, the user can do the following.

- Select accounts to add or exclude from the service
- Edit account names and account types
- Resolve account type classification errors (208 and 209), if applicable

After successfully adding accounts to the user&#39;s profile, a confirmation screen appears. It lists each added account by name along with the last four digits of its account number, account type, and current balance.

From the confirmation screen, the user can close the Add Accounts screens or start the Add Accounts process over again by clicking **Add More Accounts** and returning to the FI search screen.

After the Add Accounts widget process is complete, the widget calls the return URL to return control back to the partner application.

### Add Accounts Workflow (OAuth)

OAuth (Open Authorization) is an open-standard authorization framework for token-based authorization over the internet. It allows users to grant third-party websites or applications access to their protected resources without revealing their login credentials or identity. OAuth also helps financial institutions to identify trusted intermediaries (the third parties) and allow them to gather user financial data with APIs.

AllData supports FIs that have implemented OAuth. Partner host applications are registered with Fiserv for identification purposes. Fiserv notifies partners when institutions implement OAuth and activates the protocol for partners that are ready to support them.

### Text Configurations via Resource Bundles

AllData widgets use resource bundles to persist most of the text visible in the widget screens. This allows partners to customize the language and content to meet their business requirements. The following tables give label names and default values of configurable text in the Add Accounts widget’s primary screens.


**Initial screen**

| # | Label | Default value |
|---|---|---|
| 1 | Widget title | Add Accounts |
| 2 | Search for your FI | Search for your financial institution |
| 3 | FI search suggestion | Enter Bank or Login URL |
| 4 | Popular FI Select | Or Choose from Popular Financial Institutions where you have an account. |


**Login Credentials screen**

| # | Label | Default value |
|---|---|---|
| 5 | Widget title | Add Accounts |
| 6 | Enter your credentials | Enter your credentials for this institution |
| 7 | Primary message | The login credentials for this institution’s website may be incorrect. |
| 8 | Instructions | Login to &lt;FI name&gt; website and check for the following: |
| 9 | Step 1 | If you are able to login, please re-enter the same credentials below: |
| 10 | Step 2 | If you unable to login, contact customer support directly at &lt;Institution Name&gt;. |

**Retrieval screen**

| # | Label | Default value |
|---|---|---|
| 11 | Widget title | Add Accounts |
| 12 | Connecting and retrieval assurance message | Connecting the institution and securely accessing your account… |


**Account Confirmation screen:**

| # | Label | Default value |
|---|---|---|
| 13 | Widget title | Add Accounts |
| 14 | Added Accounts | List of all account that have been added for this institution. |
| 15 | Continue adding more accounts | To continue adding more accounts, click Add more Accounts. |

### Button Label Configuration

The label of every button available in the Add Account widget is configurable. The following tables list the button label names and default values.

**Initial screen – search results**

| # | Description | Default label |
|---|---|---|
| 16 | Show More button | Show More |


**Login Credentials screen**

| # | Description | Default label |
|---|---|---|
| 17 | **Select Another FI** button | Select Another Institution |
| 18 | **Next** button | Next |
| 19 | **Submit** button | Submit |


**Account Confirmation screen**

| # | Description | Default label |
|---|---|---|
| 20 | Add More Accounts button | Add More Accounts |
| 21 | Close button | Close |

### Offline Account Link

On the initial screen of the Add Accounts widget, there is an option in the popular institutions called **+Add Offline Accounts**. The URL to which this button points is configurable. The property may be passed to the Fiserv application via a pre-configured property or via a parameter in SSO (refer to the [Integration Approaches section](#integration-approach) for more information).

If the property is not set, the **+Add Offline Accounts link** will direct to the native offline account functionality. If the property is set to a partner-specified URL, the link will open the URL within the widget frame.

The native Add Offline Account screen lets users select an account type from a predefined selection list, provide a name for the account, and enter a value amount. 

### Zillow (Zestimate®)

Zillow is the leading real estate marketplace dedicated to empowering consumers with data, inspiration, and knowledge around the place they call home. The &quot;Zestimate&quot; home valuation model is Zillow&#39;s estimate of a home&#39;s market value. The Zestimate incorporates public and user-submitted data, and considers home facts, location, and market conditions.

Fiserv has built the capability to aggregate Zestimate property values from Zillow with property information shared by users. To obtain the property value, Fiserv and Zillow have agreed to brand Zillow as the information provider wherever the Zillow FI and Zestimate information appear. Partners who wish to enable Zillow as an option in the Add Accounts widget should do the Zillow branding in their UI when displaying the Zillow FI information.

Include the following in the partner UI when displaying the Zillow (Zestimate) information.

- Zillow logo: [http://www.zillow.com/widgets/GetVersionedResource.htm?path=/static/logos/Zillowlogo\_150x40.gif](http://www.zillow.com/widgets/GetVersionedResource.htm?path=/static/logos/Zillowlogo_150x40.gif)
- Zillow &quot;Terms of Use&quot; link: [http://www.zillow.com/corp/Terms.htm](http://www.zillow.com/corp/Terms.htm)
- &quot;What&#39;s a Zestimate&quot; link: [http://www.zillow.com/zestimate/](http://www.zillow.com/zestimate/)

### FI List

Partners can define the financial institutions that are available for users to add in the Add Accounts widget. If an FI is blocked, it will not appear in any search results and cannot be added to the partner&#39;s home. Partners should work with the Fiserv Professional Services team to get a full list of FIs that are supported and determine which ones to allow/block to meet their business requirements.

### Popular Financial Institutions List

Partners can configure the FIs that appear as popular institutions represented by their logos in the initial Add Accounts screen. Additionally, partners may choose whether to display twelve popular FI logos or ten logos plus an **Add Offline Accounts** button and a **Zillow** button on the initial screen of the widget. Clicking an institution logo in the popular institutions list takes the user to the corresponding credential submission screen.

The popular institutions list is configurable but will display a default list if not configured by the partner.

### Add Account Screens CSS Definition

##### Primary CSS Classes

The following table provides an inventory of the primary CSS classes that control the look and formatting of the Add Accounts widget screens:

|#|CSS class|Description|CSS parameter and default value|
|---|---|---|---|
|1|Background color|This parameter controls the background color that obscures the main screen when the Add Accounts screen overlays are activated.|.wrapper { background: #fff; margin: 15px 0; }|
|2|Widget screen header|This parameter controls the font style of the main header of the Add Accounts screen overlays.|h1.main { font-size: 20px; font-weight: 600; line-height: 28px; align-items: center; display: flex; color: #333; margin-bottom: 5px; }|
|3|Gray color button|This parameter controls the color of the button on the Add Accounts screen.|.btn-secondary { color: #fff; background-color: #6c757d; border-color: #6c757d; }|
|4|Blue color button|This parameter controls the color of the button on the Add Accounts screen.|.btn-primary { color: #fff; background-color: #007bff; border-color: #007bff; }|
|5|Error header|This parameter controls the font style of the error header on the Add Accounts screen.|.alert_redtext { background:url(../images/icons/alerts.png) no-repeat 5px 0px; font-size:1.15em; color:#b83320; padding:5px 0 10px 40px; margin:8px 0px; font-weight:bold; }|
|6|Content background|This parameter controls the content background.|.accSelect { background: #f1f1f1; font-size: 14px; padding: 15px 20px 5px 20px; border-radius: 5px; -webkit-border-radius: 5px; /* Safari 3-4, iOS 1-3.2, Android 1.6- \*/ -moz-border-radius: px; /* Firefox 1-3.6 \*/ }|
|7|Links|This parameter controls the font style of the links (for example, offline account, FI URL, and popular FI links).|a { color: #007bff; text-decoration: none; background-color: transparent; }|
|8|Search results message|This parameter controls the panel that displays below the search field.|.resultCount { margin: 5px 0 0 0; color: #666; }|
|9|Validation error message|This parameter controls the panel that displays when there are field level validation errors.|.iceMsgsError, .warningtip { list-style: none; color: #000000; background-color: #fcc5c5; padding: 0.75rem 1.25rem; margin-bottom: 1rem; border: 1px solid #e02020; border-radius: 0.25rem; display: block; }|


### Images

All images and material icons in the Add Accounts widget are configurable. The URLs where each of the images are retrieved from are configured in the Widget CSS file.


|#|Default image|Description|CSS parameter and default values|Default size|
|---|---|---|---|---|
|1|<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/next-gen-widgets-integration-guide/next-gen-widgets-integration-guide-36.png"/>|Large alert icon|.alert_redtext {background:url(../images/icons/alerts.png) no-repeat 5px 0px; font-size:1.15em; color:#b83320;  padding:0px 0px 10px 40px; margin:8px 0px; font-weight:normal; }|23x23|
|2|<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/next-gen-widgets-integration-guide/next-gen-widgets-integration-guide-37.png"/>|Eye icon|.eyeIcon { background: #fff; border: 1px solid #bebebe; cursor: pointer; border-left: 0px; }||
|3|<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/next-gen-widgets-integration-guide/next-gen-widgets-integration-guide-38.png"/>|Lock icon|.urlLink { display: flex; line-height: 24px; margin-bottom: 15px; }||
|4|<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/next-gen-widgets-integration-guide/next-gen-widgets-integration-guide-39.png"/>|Close window icon|.cboxClose{ float:right; background:url(../images/celightbox/button_close.png) top right no-repeat; width:8px; height:8px; display:block; margin:-10px -10px; margin:5px; cursor: pointer; }|10x10|


### Widget Configuration Parameters

The following table provides the possible functionalities configuration which partners can enable or disable in their implementation.

|Parameter name|Description|Accepted values|Default|
|---|---|---|---|
|CSSURL|Partner preferred URL for widget specific CSS. This URL is appended at the end of all CE CSS files so that partners can define only the styles they want to override.|||
|AddOfflineAccountLink|Displays the “Add Offline Account” logo link under popular financial institution list|True, False|True|
|EnableZillow|Displays the “Zillow” logo link under popular financial institution list|True, False|False|
|EnableAccountClassification|Displays the account classification page|True, False|True|
|EnableAccountConfirmation|Displays page for the user to confirm adding all accounts|True, False|True|
|IncludeClassifiedAccounts|Includes the all the added accounts (True). Displays only the accounts with classification errors (False) on the Account Classification page|True, False|True|
|addAccount.flow.display. process.step.graphic|Includes the Add flow process steps in top right corner of the widget|True, False|True|
|EnableFIRequest|Displays “Request a new institutions support” button in case the search results do not return any FI|True, False|False|
|invocation_mode|Enables or disables the widget’s “close” (**×**) button: (1) Pop-up mode: Enables close button, (2) Embedded mode: Disables close button, (3) Native app integration: Button is disabled by default|embedded, popup||


### Widget Invocation

The following process diagram explains the web services APIs the partner needs to call before and after the invocation of the Add Accounts widget.

**Default Add Accounts widget invocation process:**

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/next-gen-widgets-integration-guide/next-gen-widgets-integration-guide-40.png"/>

By default, the Add Accounts widget displays an Account Classification and Confirmation page. If your implementation disables this page and is configured in such a way that Add Accounts returns control back to you after accounts are pulled from the FI, use the following flow where you need to confirm whether the harvesting was successful and whether detailed harvesting for retrieving account details is complete.

**Add Accounts Widget Invocation Process when Account Add Confirmation is not configured:**

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/next-gen-widgets-integration-guide/next-gen-widgets-integration-guide-41.png"/>

### Widget URL

Access the Add Account widget in the partner integration environment with the following URL.

&lt;Domain URL&gt;PFM_UI/widgets/base/addaccounts/addAccountsWidget.iface

…with the mandatory POST parameter &quot;sessionToken=&lt;sessionToken&gt;&quot; and any optional parameters required for the scenario

The partner integration team provides the production URL.

- **Parameters required for invocation:** The required parameters for Add Accounts widget are **sessionToken** and **return\_url**. It is recommended that you send the **keepalive\_url** and **error\_url**.
- **Parameters on return:** Add Accounts widget will invoke **return\_url** , with **FILoginAcctId** and **AcctId** parameters.
- **Error conditions**: Add Accounts widget handles any harvesting errors in interactive manner unless the user closes the widget prematurely.

### Data Pull APIs

By default, when the Add Accounts widget has completed its flow successfully, the harvesting engine would have completed pulling relevant account information for all the added accounts. This includes account summary information, transactions, and any investment positions. This harvested data is made available through data pull APIs such as _AccountDataInq/getAccountDetails_. Refer to the AllData Web Services API Specifications Guide provided for more details.

If the Account Classification and Confirmation screens are disabled, the widget returns control back when it finds some accounts at the FI. Then a back-end process starts harvesting the newly added accounts for more details. It is recommended that you keep polling the harvest status using _getAccountUpdateSummary_ / AccountUpdateSummaryRq API until the harvesting is complete, and then invoke the data pull APIs. The _getAccountUpdateSummary_ / AccountUpdateSummaryRq API uses this request ID to pull information about ongoing harvest run. You can use Fiserv provided user ID as input parameter for the _getAccountUpdateSummary_ / AccountUpdateSummaryRq API.


| Web service name | Update status check |
|---|---|
| Resource URL | &lt;FiservWSUrl&gt;/AccountDataInq/getAccountUpdateSummary |
| Description | This API is used to get the account refresh status after the accounts are added using add accounts widget |
| API Explorer Link | [getAccountUpdateSummary](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getAccountUpdateSummary) |

### Frequently Asked Questions

The following are some frequently asked questions on how the Add Accounts widget works.

1. What information is sent back on **return_url**?

   - Add Accounts widget invokes **return\_url** after its processing is complete and sends the newly added login account ID at the FI ( **FILoginAcctId** ) as well as account IDs for all the accounts added in this widget session. These accounts are listed in comma-separated values with param **AcctId**.

2. What happens if the user selects an FI that is already registered and gives the same credentials?

   - The Add Accounts widget recognizes that this set of credentials for the chosen FI is already stored and initiates the Add More Accounts process. A harvesting attempt is made to find more accounts at the FI that can be added for aggregation. Any newfound accounts will be added using a similarly configured process as the original request, such as taking into account specific screens enabled using home-level configuration parameters. If the **return\_url** parameter exists, it will be invoked with the corresponding **FILoginAcctId** and a comma-separated list of all accounts.

3. What happens if the user chooses an FI that is already registered, provides same user credentials, and there are no new accounts found?

   - If the **return\_url** parameter exists, it is invoked with existing **FILoginAcctId** and existing **AcctId**s.
   - If **return\_url** is not set, the Add Accounts widget shows a message that no new accounts were found.

4. What error conditions are possible?

   - The Add Accounts widget handles any harvesting errors interactively unless the user closes the widget prematurely. In such a case, there may be harvesting errors that will be made available through the data pull APIs. If the user chooses same FI and provides same user ID, the Add Accounts widget resumes previous attempt and presents the existing error to the user.

## Alert Resolution Widget

### Alert Resolution Widget Overview

The Alert Resolution widget allows partners to take advantage of a library of screens that walk users through errors they may encounter during the account harvesting process. If an account is in error, the Alert Resolution widget will identify the correct alert resolution screens to display for the corresponding account. The widget screens will inform the user of the issue preventing aggregation from functioning correctly and guide the user through any steps required to resolve the error. Refer to [Appendix A – Harvesting Alert Resolution Screens](#appendix) for error content and workflows.

### Alert Resolution Widget Integration Options

Partners have multiple options for exposing the Alert Resolution widget screens to their users:

**Integrate Alert Resolution widget** – The Alert Resolution widget can be embedded and launched directly from the appropriate locations on partner application screens at the account level. Partners wishing to use their own account listing screens may choose to identify outstanding alerts in their own screens.

When a user attempts to resolve the alert, the partner application will call the Alert Resolution widget URL and provide the necessary identifiers for the respective account (**FILoginAcctId** and/or **AcctId**). The Alert Resolution widget will identify the associated error and display the necessary screens to resolve it.

### Alert Resolution Screens CSS Definition

##### Primary CSS Classes

The following table provides an inventory of the primary CSS classes that control the look and formatting of the Alert Resolution widget screens:

|#|CSS class|Description|CSS parameter and default value|
|---|---|---|---|
|1|Background color|This parameter controls the background color that obscures the main screen when the Add Accounts screen overlays are activated.|.wrapper { background: #fff; margin: 15px 0; }|
|2|Widget screen header|This parameter controls the font style of the main header of the Add Accounts screen overlays.|h1.main { font-size: 20px; font-weight: 600; line-height: 28px; align-items: center; display: flex; color: #333; margin-bottom: 5px; }|
|3|Gray color button|This parameter controls the color of the button on the Add Accounts screen.|.btn-secondary { color: #fff; background-color: #6c757d; border-color: #6c757d; }|
|4|Blue color button|This parameter controls the color of the button on the Add Accounts screen.|.btn-primary { color: #fff; background-color: #007bff; border-color: #007bff; }|
|5|Error header|This parameter controls the font style of the error header on the Add Accounts screen.|.alert_redtext { background:url(../images/icons/alerts.png) no-repeat 5px 0px; font-size:1.15em; color:#b83320; padding:5px 0 10px 40px;  margin:8px 0px; font-weight:bold; }|

### Images

All images and icons in the Alert Resolution widget are configurable. The URLs where each of the images are retrieved from are configured in the Widget CSS file.

|#|Default image|Description|CSS parameter and default values|Default size|
|---|---|---|---|---|
|1|<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/next-gen-widgets-integration-guide/next-gen-widgets-integration-guide-36.png"/>|Large alert icon|.alert_redtext { background:url(../images/icons/alerts.png) no-repeat 5px 0px; font-size:1.15em; color:#b83320;  padding:0px 0px 10px 40px; margin:8px 0px; font-weight:normal; }|23x23|
|2|<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/next-gen-widgets-integration-guide/next-gen-widgets-integration-guide-37.png"/>|Eye icon|.eyeIcon { background: #fff; border: 1px solid #bebebe; cursor: pointer; border-left: 0px; }||
|3|<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/next-gen-widgets-integration-guide/next-gen-widgets-integration-guide-38.png"/>|Lock icon|.urlLink { display: flex; line-height: 24px; margin-bottom: 15px; }||
|4|<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/next-gen-widgets-integration-guide/next-gen-widgets-integration-guide-39.png"/>|Close window icon|.cboxClose { float:right; background:url(../images/celightbox/button_close.png) top right no-repeat; width:8px; height:8px; display:block; margin:-10px -10px; margin:5px; cursor: pointer; }|10x10|

### Text Configuration via Resource Bundles

AllData widgets use resource bundles to persist most of the text that is displayed to users in the widget screens. This allows partners to customize the language and content to meet their business requirements. The following section describes the configurable text in the screens used by the Error Resolution widget.

Alert Resolution widget screens have two basic formats: Either no user action is required, or the user can choose an action. 

1. **No user action required** - Partners can customize the following elements:
   - Error message text based on error code
   - Body content

2. **The user can choose an action** - Partners can customize the following elements:
   - Error message text based on error code
   - Subheading text
   - Text for options
   - Text guiding the user to customer support and customer support link
   - Enable/disable **Remove Account** button
   - Label and style of **Cancel** button

### Widget Invocation

The following process diagram explains the web services APIs the partner calls before and after the invocation of the Alert Resolution widget.

**Alert Resolution widget invocation process diagram:**

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/next-gen-widgets-integration-guide/next-gen-widgets-integration-guide-53.png"/>

### Widget URL

The Alert Resolution widget can be accessed using following URL for the partner integration environment. The production URL will be provided later by the partner integration team.

Alert Resolution widget URL for partner integration environment is:

&lt;Domain URL&gt;PFM\_UI/widgets/base/addaccounts/alerts/resolveAlertWidget.iface

…with the mandatory POST parameter &quot;sessionToken=&lt;sessionToken&gt;&quot; and any optional parameters for the scenario

- **Parameters required for invocation:** For Error Resolution widget apart from **sessionToken** , the other required parameters are **login\_acct\_id** and/or **acct\_id**. When both these parameters are sent, the widget first looks at account level errors and if there are none, it uses the **login\_acct\_id** to check for errors present at that level. Invoking this widget also requires the **return\_url** parameter. It is recommended that you send the **keepalive\_url** and **error\_url**.
- **Parameters on return:** The Error Resolution widget will invoke **return\_url** with action parameter that will indicate whether the user merely cancelled out of the widget or did take some action to mitigate the error. The possible values are &quot;Cancel,&quot; &quot;Deleted,&quot; and &quot;Submit.&quot; If the error resolution results in adding accounts to the **login\_acct\_id** , the newly added **acct\_id** s are returned as list of comma-separated values along with the **login\_acct\_id**.
- **Error conditions:** In case there are processing errors, the Error Resolution widget invokes **error\_url** sent on the request. If **error\_url** is absent, the Error Resolution widget displays a message for the user. The following are the possible error conditions.

  - **Missing Required Parameters:** The invocation request does not have the required parameters present. (Error code 500)
  - **Invalid Parameter Passed:** When passing invalid **login\_acct\_id** or **acct\_id** during widget invocation. (Error code 510)
  - **System Error:** There was a processing error or invalid parameters were sent. (Error code 520)

### Data Pull APIs

There are multiple outcomes of an Error Resolution widget invocation:

1. The user chose to cancel out of widget or there was no user action possible for the error condition. In this case, the **action** parameter will have value &quot;Cancel&quot; and no further action is required from the partner. If the error persists beyond stated resolution time, you may have to contact Fiserv customer service.

2. The user chose to act on the error by deleting the account. In this case, the **action** parameter will have value &quot;Deleted.&quot; The partner is expected to use data pull APIs to sync up the accounts for that user.

3. The user chose to provide additional information to resolve the error. In this case the **action** parameter will have value &quot;Submit.&quot; The partner is expected to poll for completion of harvesting by checking the harvest status using _getAccountHarvestStatus_ / AccountHarvestStsInqRq API and then using data pull APIs to find out if the error resolution was successful.

### Frequently Asked Questions

The following are some of the frequently asked questions about how the Add Accounts widget works.

1. What information is sent back on **return\_url**?

   - The Alert Resolution widget invokes **return\_url** after its processing is complete with the **action** parameter. If the alert resolution results in adding more accounts, the newly added account IDs are sent as a list of comma-separated values with **AcctId** parameter along with the **FILoginAcctId**.

2. When is harvesting triggered in the Alert Resolution flow?

   - The following error codes trigger a harvesting update when resolved: 300, 301, 302, 303, 304, 307, and 201 (if the user chooses to match up the accounts retrieved from FI). This harvested data is made available through data pull APIs. Please refer to the AllData XML/Web Services Specifications Guide provided for more details.

   - On the similar lines to that of Add Accounts flow, it is recommended that you keep polling the harvest status using _getAccountHarvestStatus_ / AccountHarvestStsInqRq API until the harvesting is complete before invoking the data pull APIs. The **FILoginAcctId** of the newly added login account is sent back as a parameter to the **return\_url**. The _getAccountHarvestStatus_ / AccountHarvestStsInqRq API uses this **FILoginAcctId** to pull information about the ongoing harvest run.

3. When does the Alert Resolution widget invoke the Add More Accounts flow?

   - If the user had stopped the Add Accounts flow with FI login-level harvesting errors, there will be no child accounts present for that parent FI login account. If the user tries to resolve such a harvesting error, the harvesting update gets triggered as if the user is trying to add more accounts from that FI. The newly added accounts information will be sent back on the **return\_url** as discussed in question 2.

4. How do we know if the error was resolved successfully?

   - When the user submits the information such as new credentials, to resolve the error, the Alert Resolution widget notes that and triggers backend harvesting. The success of that attempt cannot be known until harvesting completes. We recommend that you keep polling the status for harvest completion and check the information using data pull APIs.

5. Could there be multiple errors with the same account?

   - It is possible that there are multiple errors or even errors at multiple levels: FI level, FI login account level, and account level. Resolving errors at high levels in the hierarchy (such as by our scripting team) may uncover other errors. After an alert resolution action is taken, you should use data pull APIs to check for further errors.

## Return Scenarios

### Return URL scenarios

The following table provides details on different use case scenarios in which AllData returns control back to the partner application by calling the **return\_url** that the partner shares. It includes whether the scenario has an account confirmation page and details of the parameters and parameter values sent along with **return\_url**.


| Widget/ implementation | Scenario | Acct conf pg? | Parameter name(s) | Parameter value(s) | Other param(s) |
|---|---|---|---|---|---|
| Add Account | When accounts are added | No | AcctId, FILoginAcctId | &lt;account IDs&gt; | |
| Add Account | After adding accounts, widget close action in confirmation page | Yes | Action | Close | |
| Add Account | No accounts found in FI to add | No | Action | NoNewAccountsFound | |
| Add Account | On Add More Accounts, all the accounts are already added and there are no new accounts in FI to add. | No | Action | NoNewAccountsAdded | |
| Add Account | On Add More Accounts, one or more of the user’s existing accounts was not found at the FI. The user must resolve this error prior to adding more accounts. | No | Action | NoAccountsAdded | |
| Add Account | On passing invalid PartnerAppID (unregistered partner app ID) – primarily required when adding OAuth FIs | N/A | Action | Close | errorCode= 3005 |
| Add Account – deep linking | When accounts are added | No | AcctId, FILoginAcctId | &lt;account IDs&gt; | |
| Add Account – deep linking | After adding accounts, closing the widget from the confirmation page | Yes | Action | Close | |
| Add Account – deep linking | No accounts found in FI to add | No | Action | NoNewAccountsFound | |
| Add Account – deep linking | On Add More Accounts, all the accounts are already added and there are no new accounts in FI to add. | No | Action | NoNewAccountsAdded | |
| Add Account – deep linking | On Add More Accounts, one or more of the user’s existing accounts was not found at the FI. The user must resolve this error prior to adding more accounts. | No | Action | NoAccountsAdded | |
| Add Account – deep linking | On passing invalid FI ID in widget invocation | N/A | Action | Close | errorCode= 3001 |
| Add Account – deep linking | On passing invalid FI account ID in widget invocation | N/A | Action | Close | errorCode= 3002 |
| Add Account – deep linking | On passing an empty account ID and an invalid or empty FI ID in widget invocation | N/A | Action | Close | errorCode= 3001, 3002 |
| Add Account – deep linking | When the Add process is successful and no accounts are returned – on clicking Start Over in the widget, return_url is invoked with this Action and errorCode | Yes | Action | Cancel | errorCode= 3003 |
| Add Account – deep linking | On passing invalid PartnerAppID (unregistered partner app ID) – primarily required when adding OAuth FIs | N/A | Action | Close | errorCode= 3005 |
| Alert Resolution | All user intervention scenarios in Resolve Alerts after taking action to resolve error and submit, such as after updating login credentials, editing MFA answers, and changing account types (Child Accounts available) | N/A | Action | Submit | |
| Alert Resolution | All user intervention scenarios in Resolve Alerts after taking action to resolve error and submit, such as after updating login credentials and editing MFA answers (No Child Accounts available – Add process will be initiated). After adding accounts, widget close action in confirmation page | Yes | Action | Close | |
| Alert Resolution | All user intervention scenarios in Resolve Alerts after taking action to resolve error and submit, such as after updating login credentials and editing MFA answers (No Child Accounts available – Add process will be initiated. When accounts are added | No | AcctId, FILoginAcctId | &lt;account IDs&gt; | |
| Alert Resolution | User clicks Select New Institution in error 306 alert. | N/A | Action | SelectNewInstitution | |
| Alert Resolution | On passing invalid PartnerAppID (unregistered partner app ID) – primarily required when adding OAuth FIs | N/A | Action | Close | errorCode= 3005 |
| All widgets | On clicking Cancel or Close in any widget | N/A | Action | Cancel | |

### Error URL scenarios

This table provides details on error scenarios during widget invocation in which AllData redirects users to the **error\_url** the partner shares, and error code details specific to each scenario in parameter values sent with the **error\_url**. None of the following scenarios require additional parameters.

| Widget/ implementation | Scenarios | Parameter name | Parameter value(s) | Type |
|---|---|---|---|---|
| All widgets | Session timeout | errorCode | 3000 | Error URL |
| All widgets / invocation | When partner passes unregistered (not whitelisted with Fiserv) domain name in css_url | errorCode | 3010 | Error URL |
| All widgets / invocation | When partner passes unregistered (not whitelisted with Fiserv) domain name in return_url | errorCode | 3011 | Error URL |
| All widgets / invocation | When partner passes unregistered (not whitelisted with Fiserv) domain name in error_url | errorCode | 3012 | Error URL |
| All widgets / invocation | When partner passes unregistered (not whitelisted with Fiserv) domain name in keepalive_url | errorCode | 3013 | Error URL |
| All widgets / invocation | When partner passes unregistered (not whitelisted with Fiserv) domain name in logout_url | errorCode | 3014 | Error URL |
| All widgets / invocation | When partner passes unregistered (not whitelisted with Fiserv) domain name in offline_url | errorCode | 3015 | Error URL |
| All widgets / invocation | When partner does not pass mandatory parameter in SSO request | errorMsg | Missing Mandatory Params: Values | Error URL |
| Alert Resolution widget | When the expected login_acct_id or acct_id is not passed during widget invocation | errorCode | 500 | Error URL |
| Alert Resolution widget | When passing invalid login_acct_id or acct_id during widget invocation | errorCode | 510 | Error URL |
| Alert Resolution widget | System error – When an error internal to Fiserv occurs during widget invocation | errorCode | 520 | Error URL |

## Implementation Approaches

Partners have alternate approaches to integrate AllData widgets into their applications:

- **Deep linking:** Start the Add Accounts flow directly from the FI credentials submission screen.
- **Native app integration:** Deploy the responsively designed widgets within mobile web and native app implementations.

The following sections describe both approaches in detail.

### Deep Linking

This approach allows partners to directly launch the Add Account widget with the login credentials submission screen for a specific FI that the user chooses, bypassing the default step of searching for the institution. This approach gives partners the flexibility to manage the FI search on their side, with more control over the institutions they wish to support for their users. Partners can pull data from supported financial institutions using the _getFinancialInstInfo_ API. Please refer to the AllData Web Services Specifications Guide provided for more details. Other than bypassing the initial FI search process, the Add Accounts flow for deep linking remains the same as the default flow.

The deep linking approach requires either one of the following parameters in both SAML 2 and session token SSO options for launching the Add Accounts widget.

- **fi\_id:** Required when initiating a fresh add attempt under an institution – This ID represents the FI the user has account with and is available when pulling the FI data using _getFinancialInstInfo_ API.
- **login\_acct\_id:** Required for error resolution and add more accounts available from an existing institution under user profile – This ID represents the login account the user has with the financial institution. This information is available using _getAccountDetails_ API.

In deep linking the following response codes are applicable to different scenarios, such as when the mandatory parameter is sent incorrectly, or missed when launching the widget.

| Response code | Reason |
|---|---|
| 3000 | Session expired |
| 3001 | Invalid financial institution ID **fi_id** |
| 3002 | Invalid login account ID **login_acct_id** |
| 3003 | When the add flow is successful without returning any account, the user can re-initiate the add flow for the same FI or a different FI. If this response code persists, report the issue to Fiserv. |
| 3005 | Invalid **partner_application_id** required for OAuth FIs |

### Native App Integration

AllData widgets have a responsive design for optimal display in web browsers on desktop, tablet, and mobile devices. The widgets can also be integrated into a partner&#39;s native app. With the responsive design the widgets adjust seamlessly to mobile device screens.

When integrating widgets in a native app, the parameter **host\_app\_type** is required to identify the type of native app implementation, passing a value of either &quot;ios&quot; or &quot;android.&quot;

When identified as a native app integration, the **Cancel** and **Close** buttons in the widget will be removed and partners must manage the navigation back to their source application from the AllData widget when the user wants to close or cancel the widget.

### Widget Integration Using WebView and Native App

This section provides an architectural overview of the mobile integration and invoking of AllData widgets in WebView. It covers the primary components and interfaces comprising the target state solution.

#### Current SSO Process Description and Elements

See the [Integration Approaches](#integration-approach) section for a description of the SSO process and a list of all the applicable elements.

### System Environment

#### SSO for Mobile

This leverages the aggregation SSO built for online AllData. The parent app must get the one-use SSO key from the existing middleware. The SSO key is then passed to the URL to invoke the AllData widget in WebView. URLs are confirmed during implementation. Partners who continue to invoke the existing Aggregation URL are internally redirected to the Aggregation UI.

#### Sample Pseudocode for Invoking WebView

##### Android

```
WebView loadView =(WebView)
eSignDetailView.findViewById(R.id.<WEBVIEWID>);
    loadView.getSettings().setJavaScriptEnabled(true);
    loadView.setWebChromeClient(new WebChromeClient());
    loadView.loadUrl(<<PFMUrl>>);
```

##### iOS

```
- (void)viewDidLoad {
    [super viewDidLoad];

    NSURL *url = [NSURL URLWithString:@"<<PFMUrl>>"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url];
    [self. responsiveUIWebView  loadRequest:urlRequest];

    self. responsiveUIWebView .delegate = self;
}
```

#### Session Management

##### Keeping banking session alive

Both iOS and Android platform broadcast touch events for a WebView, so that the parent app can listen and keep the session alive.

##### Handling session timeouts in Aggregation

Aggregation will call a function on the parent app when the session times out. 

#### Sample Pseudocode for Handling Session Management

##### Android

```
i. Create interface and expose "Aggregation" TimeOut method and annotate this method with @JavascriptInterface

public class WebAppInterface {
    Context mContext;
    /** Instantiate the interface and set the context */
    WebAppInterface(Context c) {
        mContext = c;
    }

    /** Show a toast from the web page */
    @JavascriptInterface
    public void AllDataUITimeOut() {
        //Parent app can perform action based on time out
    }
}

ii. Call following method on WebView

loadView.addJavascriptInterface(new WebAppInterface(getActivity()), "AllDataAndroidInterface");
```

##### iOS

Controller will extend UIWebViewDelegate and have the following delegate method implemented.

```
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType
{
    if ([[[request URL] absoluteString] hasPrefix:@"AllDataUIiOSInterface"]) 
    {
        // Call the given selector
        [self performSelector:@selector(AllDataUITimeOut)];
        // Cancel the location change
        return NO;
    }
    return YES;
}
- (void) AllDataUITimeOut
{
  //Parent app can perform action based on time out
    NSLog(@&"Inside AllDataUITimeOut ");
}
```

### iOS: Cross-Site Tracking and Blocking All Cookies

On March 24, 2020, Apple released updates for its operating systems for desktop and mobile devices.

- Desktop/laptop: OSx 10.15.4 Mojave
- iPhone/iPad: iOS 13.4

With these updates, a change was made to the default setting for the way the Safari browser handles cross-site tracking (third-party cookies). As a result, users who have installed these OS versions or higher cannot access AllData Connect widgets from Mac computers or iOS mobile devices, when the partner has integrated AllData Connect widgets with an iframe (embedded) implementation.

##### User experience

Clients who do not have a custom domain URL setup to access AllData widgets and did iframe implementation, their users attempting to access AllData widgets from a Mac computer or iPad/iPhone using the above listed operating systems versions and higher, receive the error.

##### User action required

In the event a user encounters this problem with Safari, user has to change privacy settings in the Safari browser to allow cookies and cross-site tracking.

##### Desktop / laptop instructions

To change the settings within the Safari browser on a desktop or laptop computer, follow these steps:

1. Go to the **Safari** menu and select **Preferences**.

2. Go to the **Privacy** tab and uncheck the &quot;Prevent cross-site tracking&quot; and &quot;Block all cookies&quot; options.

##### iPad and iPhone instructions

To change the settings within the Safari browser on an iPad, follow these steps:

1. **Go to Settings for Safari:** Open the “Settings” app on your device and select Safari.

2. **Change Privacy & Security Settings:** In the “Privacy & Security” section of the Settings screen, ensure the options for “Prevent Cross-Site Tracking” and “Block All Cookies” are turned off.

## Appendix

### Harvesting Alert Resolution Screens

Harvesting alerts are listed below by error code. The alert titles and descriptive text appear in boxes, followed by any further information, such as user intervention steps.

### Error 100

<!-- theme: danger -->
> **Currently unable to update accounts at this institution**
> 
> We are actively working on this issue. When this issue has been resolved, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** Fiserv will resolve the issue. After it is resolved, refreshing the account will clear the error.

### Error 103

<!-- theme: danger -->
> **Currently unable to access accounts at this institution**
> 
> There were problems with this institution&#39;s website when we last attempted to update your account. When this issue is resolved at _institution name_, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** Issue with FI website

### Error 104

<!-- theme: danger -->
> **Currently unable to access accounts at this institution**
> 
> There were problems with this institution&#39;s website when we last attempted to update your account. When this issue is resolved by **institution name** , your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** Issue with FI website

### Error 105

<!-- theme: danger -->
> **Currently unable to access accounts at this institution**
> 
> There were problems with this institution&#39;s website when we last attempted to update your account. When this issue is resolved by _institution name_, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** Issue with FI website

### Error 106

<!-- theme: danger -->
> **Currently unable to access accounts at this institution**
> 
> There were problems with this institution&#39;s website when we last attempted to update your account. When this issue is resolved by _institution name_, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** Issue with FI website

### Error 107

<!-- theme: danger -->
> **Currently unable to access accounts at this institution**
> 
> There were problems with this institution&#39;s website when we last attempted to update your account. When this issue is resolved by _institution name_, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** Issue with FI website

### Error 108

<!-- theme: danger -->
> **Currently unable to access accounts at this institution**
> 
> There were problems with this institution&#39;s website when we last attempted to update your account. When this issue is resolved by _institution name_, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** Issue with FI website

### Error 109

<!-- theme: danger -->
> **Currently unable to update accounts at this institution**
> 
> We are actively working on this issue. When this issue has been resolved, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** This type of alert remains on the user interface until it is resolved by Fiserv and the account is refreshed.

### Error 110

<!-- theme: danger -->
> This may be a closed or inactive account that has not been deleted from the institution&#39;s website.
> 
> 1. If this account has not been closed or inactivated, contact customer support.
> 2. If this account has been closed or inactivated, remove it from your profile.
> 3. If you do not know whether or not this account has been closed or inactivated, login to the _institution name_ website and check the account status.

**User intervention required:** Clicking **Remove Account** resolves the alert by initiating the process to delete the account from the user&#39;s profile. If this is the only account associated with the credentials at the FI for the user, then the entire FI is deleted from the user&#39;s profile.

### Error 121

<!-- theme: danger -->
> **Currently unable to update accounts at this institution**
> 
> We are actively working on this issue. When this issue has been resolved, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** This type of alert will remain on the user interface until Fiserv resolves the issue and the account is refreshed.

### Error 200

<!-- theme: danger -->
> **Currently unable to update accounts at this institution**
> 
> We are actively working on this issue. When this issue has been resolved, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** Fiserv will resolve the issue. After it is resolved, refreshing the account will clear the error.

### Error 201

<!-- theme: danger -->
> The name or number for this account may have changed or this account may no longer be available.
> 
> Login to the _institution name_ website and check for the following:
> 1. If the name or number for this account has changed, click the &quot;Match Accounts&quot; button to select the correct account from a list of available accounts.
> 2. If this is a closed or inactive account, remove it from your profile.
> 3. If the name or number for this account has not changed and this is not a closed or inactive account, contact customer support.

**User intervention required** – There are two options to resolve the 201 alert:

- Clicking **Remove Account** resolves the alert by initiating the process to delete the account from the user&#39;s profile. If this account is the only account associated with the credentials at the FI for the user, then the entire FI is deleted from the user&#39;s profile.
- Clicking **Match Accounts** resolves the alert by initiating the process to retrieve additional accounts using the credentials for the FI. A progress indicator appears on the overlay screen while the process retrieves accounts.

The retrieved accounts appear for the user to choose from:

Clicking **Match Accounts** updates the existing account with the selected account&#39;s identifier (NickNameAtFI, NickNameAtCE, account number, and Misc fields).

If the match process does not find any new accounts, the following message appears to notify the user and the Resolve Alert box includes the option to delete the account from the user&#39;s profile.

### Error 202

<!-- theme: danger -->
> **Currently unable to update accounts at this institution**
> 
> We are actively working on this issue. When this issue has been resolved, your accounts will be automatically updated. We apologize for the inconvenience.

**User intervention required:** This type of alert will remain on the user interface until Fiserv resolves the issue and the account is refreshed.

### Error 203

<!-- theme: danger -->
> **Successful login but this account may no longer be available**
> 
> Login to the _institution name_ website and check for the following:
> 1. If this account is no longer available, remove it from your profile.
> 2. If this account is available, contact customer support.

**User intervention required:** Clicking **Remove Account** resolves the alert by initiating the process to delete the institution from the user&#39;s profile. If this account is the only account associated with the credentials at the FI for the user, then the entire FI is deleted from the user profile.

### Error 204

<!-- theme: danger -->
> **Successful login but this account may no longer be available**
> 
> Login to the _institution name_ website and check for the following:
> 1. If this account is no longer available, remove it from your profile.
> 2. If this account is available, contact customer support.

**User intervention required:** Clicking **Remove Account** resolves the alert by initiating the process to delete the account from the user&#39;s profile. If this account is the only account associated with the credentials at the FI for the user, then the entire FI is deleted from the user profile.

### Error 205

<!-- theme: danger -->
> **Currently unable to update accounts at this institution**
> 
> We are actively working on this issue. When this issue has been resolved, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** Fiserv will resolve the issue. After it is resolved, refreshing the account will clear the error.

### Error 208

<!-- theme: danger -->
> **This account could not be automatically assigned to an account type.**
> 
> 1. Select the account type that best describes this account.
> 2. If you do not know which account type to select, contact customer support.

**User intervention required:** Clicking **Submit** saves the selected account type for the account, closes the overlay screen, and initiates an account update.

### Error 209

<!-- theme: danger -->
> **This account may be incorrectly classified. (For example, if an investment account is incorrectly classified as a checking account, it will fail to update.)**
> 
> 1. Select the account type that best describes this account.
> 2. If you do not know which account type to select, contact customer support.

**User intervention required:** Clicking **Submit** saves the selected account type, closes the overlay screen, and initiates an account update.

### Error 300

<!-- theme: danger -->
> **The login credentials for this institution&#39;s website may be incorrect.**
> 
> Login to the _institution name_ website and check for the following:
> 1. If you are able to login, re-enter the same credentials below.
> 2. If you are unable to login, contact customer support directly at _institution name_.

**User intervention required:** Clicking **Submit** validates and saves the credentials for the FI, then closes the overlay screen and initiates an account update.

### Error 301

<!-- theme: danger -->
> **The login credentials for this institution&#39;s website may be incorrect.**
> 
> Login to the _institution name_ website and check for the following:
> 1. If you are able to login, re-enter the same credentials below.
> 2. If you are unable to login, contact customer support directly at _institution name_.

**User intervention required:** Clicking **Submit** saves the credentials for the FI, closes the overlay screen, and initiates an account update.

### Error 302

<!-- theme: danger -->
> **This institution requires additional action on an intermediate page (such as an updated agreement or additional 30questions) in order to complete the login.**
> 
> 1. Login to the _institution name_ website and follow any additional instructions.
> 2. Click on the &quot;Update Accounts&quot; button to update all accounts at this institution.
> 3. If your accounts still do not update, contact customer support.

**User intervention required:** Clicking **Update Accounts** closes the overlay screen and initiates an account update.

### Error 303

<!-- theme: danger -->
> **Additional security question(s) are required by this institution&#39;s website in order to complete the login. You may be prompted to answer multiple questions or to answer the same question more than once.**
> 
> &lt;additional security question with input field&gt;

**User intervention required:** Clicking **Submit** saves the responses to the MFA challenge question(s) and initiates an account update.

### Error 304

<!-- theme: danger -->
> **One or more of your responses to this institution&#39;s security questions may be incorrect.**
> 
> Click the button below to try answering the security questions again.

**User intervention required:** Clicking **Try Again** allows the user to give new answers to the MFA challenge questions. If valid, the new responses are saved, the overlay screen closes, and the account is updated.

### Error 306

<!-- theme: danger -->
> **This may be the wrong institution website for this account.**
> 
> Login to the _institution name_ website and check for the following:
> 1. If you are unable to login, this may be an incorrect website. Select the correct website by clicking the button below. This will also delete any partial accounts that may have been created for this incorrect website.
> 2. If you think this is the correct website, contact customer support.

**User intervention required:** Clicking **Select New Institution** deletes the existing account and sends the user to the Add Accounts screen, where they can start the Add Accounts process from the beginning.

### Error 307

<!-- theme: danger -->
> **This institution has locked your account.**
> 
> 1. Login to the _institution name_ website and follow the instructions to unlock the account.
> 2. If you are able to unlock the account, re-enter the updated login credentials below.
> 3. If you are unable to unlock the account, contact customer support directly at _institution name_.
> 4. For all other issues, contact customer support.

**User intervention required:** Clicking **Next** validates and saves the credentials for the FI, then closes the overlay screen and initiates an account update.

### Error 311

<!-- theme: danger -->
> **Currently unable to update accounts at this institution**
> 
> We are actively working on this issue. When this issue has been resolved, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** Fiserv will resolve the issue. After it is resolved, refreshing the account will clear the error.

### Error 312

<!-- theme: danger -->
> Due to an updated connection process with &lt;FI name&gt;, this institution requires all existing users to re-confirm their consent to share data with this application…

**User intervention required:** The institution has implemented OAuth and the user&#39;s profile is suspended until it is migrated to the OA model. Clicking **Authenticate** opens the institution website where the user must authenticate their login credentials, provide updated consent for account sharing, and select the profile(s) to migrate.

### Error 400

<!-- theme: danger -->
> **Currently unable to update accounts at this institution**
> 
> We are actively working on this issue. When this issue has been resolved, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** Fiserv will resolve the issue. After it is resolved, refreshing the account will clear the error.

### Error 600

<!-- theme: danger -->
> **Currently unable to update accounts at this institution**
> 
> We are actively working on this issue. When this issue has been resolved, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** Fiserv will resolve the issue. After it is resolved, refreshing the account will clear the error.

### Error 999

<!-- theme: danger -->
> **Currently unable to update accounts at this institution**
> 
> We are actively working on this issue. When this issue has been resolved, your accounts will be automatically updated. We apologize for the inconvenience.

**Information alert:** Fiserv will resolve the issue. After it is resolved, refreshing the account will clear the error.

## Appendix B

### Add Account Label Configuration Options

The following table lists the possible label configurations in the widgets. Partners wishing to customize widget labels can change the default values as desired.

| # | Page | Object type | Name | Default value |
|---|---|---|---|---|
| 1 | Initial screen | Label | Widget title | Add Accounts |
| 2 | Initial screen | Label | Search for your FI | Search for your financial institution: |
| 3 | Initial screen | Label | Select Popular Institutions | Or choose from popular financial institutions where you have an account |
| 4 | Initial screen | Label | Note label for suspended FI | Note: Institutions with (!) sign are currently not available |
| 5 | Initial screen | Label | If FI not listed | Please check the name and try again |
| 6 | Initial screen | Label | Offline account message | If you have an asset or liability that does not have online access (real estate, auto, jewelry, etc.), click here to add an offline account |
| 7 | Initial screen | Button label | Next button | Next |
| 8 | Login Credentials screen | Label | Widget title | Add Accounts |
| 9 | Initial screen | Button label | Next button | Next |
| 10 | Login Credentials screen | Label | Widget title | Add Accounts |
| 11 | Login Credentials screen | Label | Enter your credentials | Enter your credentials for this institution |
| 12 | Login Credentials screen | Label | Login credentials error message | All fields are required. Please check your entries and try again. |
| 13 | Login Credentials screen | Label | Error message 301 | The login credentials for this institution’s website may be incorrect. |
| 14 | Login Credentials screen | Label | Connecting to FI site | Connecting the institution and securely accessing your account… |
| 15 | Account Classification screen | Label | Account Selection | Select the accounts from this institution that you want to connect. |
| 16 | Account Classification | Button label | Next button | Next |
| 17 | Account Confirmation screen | Label | Widget title | Add Accounts |
| 18 | Account Confirmation screen | Label | Added so far | List of all accounts added for this institution. |
| 19 | Account Confirmation screen | Label | Continue adding more accounts | To continue adding more accounts, click Add More Accounts. |
| 20 | Account Confirmation screen | Button label | Add More Accounts button | Add More Accounts |
