# AllData® Web Services API Specifications

## Overview

This document describes the web services API usage of the Fiserv AllData application. A RESTful API web service handles communication between the partner application and Fiserv, using HTTP protocol over SSL/TLS as the communication medium.

To extract data from Fiserv, the partner application sends a request with certain partner-identifying parameters and user authentication tokens. Upon receipt, the Fiserv server performs the requested task and responds with the results.

The AllData application supports two data formats: XML and JSON. Please contact your AllData sales representative for a link to the updated JSON schema/sandbox testing environment for the web services.

The AllData system provides web service access to:

- add financial institution (&quot;FI&quot;) seed data
- add, delete, and manage users
- add, delete, and manage user accounts with the FIs
- harvest user account information daily and on demand
- retrieve various kinds of transactions to provide accurate and up-to-date information about user assets and holdings

## AllData Service Integration

This section describes the different partner integration options with the AllData system.

### Full API WS Integration

In this model, RESTful web services APIs handle the complete flow from user registrations to data pull and account management. Partners develop custom user interfaces for account setup and management using the web services. Partners can invoke the web services using XML or JSON requests. Partners can also sign up for optional nightly batch files that allow AllData to push data.

### UI Widgets and API Integration

Please see the [Next-Gen Widgets Integration Guide](?path=/docs/alldata-next-gen.md) available on Developer Studio. 

## Full API Web Service Integration

In this model, partners do not use the AllData UI for account setup and management. Instead, partners develop custom user interfaces that communicate with AllData using web service APIs. The following is a list of the different categories of APIs available in the AllData system, each with a brief description.

- **User management:** APIs that support activities to manage users
- **External FI seed data:** APIs for partners to fetch information about FI data sources
- **Account management:** APIs to support user account management activities like user creation
- **Account harvesting:** APIs to allow periodic and on-demand retrieval of user account details
- **Account data management:** APIs to allow clients to retrieve user account data from AllData
- **Client management:** APIs to manage activities related to client-advisor relationships
- **Transaction categorization:** APIs to support transaction categorization

### User Management

AllData user management APIs support various activities to manage users. Partners use the Add User API to add one or more users to the AllData platform. The API expects typical user profile information such as name and address. Once AllData adds a user to its database, it returns a unique CEUserID that must persist in the partner database. The ID accompanies any user actions with AllData.

The following diagram details the typical add user process.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-02.png" alt="Figure 2"/>

The User Management module uses the following web services:

- createUser
- deleteUser
- getUserProfile
- updateUserProfile
- updateUserPassword

### Initial Financial Institution List Setup

As part of the initial integration with AllData, partners must fetch a list of institutions Fiserv supports and store it in their systems using the web services. After this initial load, subsequent synchronizations fetch only newly added and updated institution data.

**Note:** The AllData FI list has thousands of entries. Given the size of the list, we recommend that partners batch getFIDetails calls, with a limit of 500 FIs returned per call. This process should be executed server to server, taking into consideration that the fetch times may be longer and should not abort with a short timeout.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-03.png" alt="Figure 3"/>

### Periodic Maintenance Refresh

Partners should check for new and updated institution data daily by passing the current time and previous refresh time to obtain the delta. The following diagram represents this periodic data maintenance process.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-04.png" alt="Figure 4"/>

### External FI Seed Data

The following APIs are involved in external FI seed data management:

- getFinancialInstInfo
- searchFinancialInst

### External FI Messages (Special Instructions)

After fetching the supported institutions, partners must retrieve and store &quot;FI Messages&quot; (special instructions) associated with some institutions. Special instructions include actions users must perform before adding accounts. For example, some institution websites require users to provide consent to allow third-party aggregators to harvest account information; special instructions from such an institution would contain the user action to provide consent on its website.

After the initial load, partners perform periodic synchronizations that fetch only new and updated FI Message data.

The API used to fetch FI Messages is getFIMessageInfo.

### Add External Account (Held Away Accounts)

AllData account management APIs support activities required to aggregate a user&#39;s externally held (&quot;held away&quot;) accounts. Using the FI seed data and user login credentials, Fiserv fetches the user accounts at a given FI. The user chooses the specific accounts to aggregate and monitor, then Fiserv persists and updates the account information on a nightly basis. Partners can also refresh these accounts on demand.

#### Add Account workflow (without multi-factor authentication)

Partners use the following steps to add new accounts without multi-factor authentication (MFA). (See the [next section](#add-account-workflow-with-multi-factor-authentication) for the same process with MFA.) Partners should cache the FI seed data daily before calling these web services.

1. **initiateAddAccounts**

This step initiates the process of adding external accounts. This request triggers a harvesting request using HarvestAddRq with FI login parameters (username/password) or FILoginAcctId. AllData connects to the FI website and harvests account information. Refer to [Account Management Web Services](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/initiateAddAccounts) for the details of the API.

2. **getAddAccountStatus**

Invoke this API after the initiateAddAccounts request has started. It returns the status of InitiateAddAccounts. Invoke this API at a periodic interval to check the status. Refer to [Account Management Web Services](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getAddAccountStatus) for the details of the API.

3. **getNewAccounts**

Invoke this API after the initiateAddAccounts request has completed. It returns the list of financial accounts found on the FI website. Currently these accounts are not created in the AllData system. Refer to [Account Management Web Services](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getNewAccounts) for the details of the API.

4. **createAccounts**

Invoke this API to create the accounts in AllData system. Fiserv classifies the harvested accounts based on certain account classification rules. The user can either override this classification or provide the classification if not already set. After successfully adding accounts the partner must call update account APIs to run on-demand harvesting to retrieve account data. Refer to [Account Management Web Services](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/createAccounts) for API details. The following figure depicts the typical non-MFA Add Account workflow.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-05.png" alt="Figure 5"/>

#### Add Account workflow (with multi-factor authentication)

AllData system supports multi-factor authentication (MFA) as part of the Add Account workflow which some FIs may require. AllData supports these with special handling of such accounts. When the first-time login credentials fail with MFA error (303 error code), AllData APIs fetch the MFA details (question or image) and present it to the partner. The partner provides the answer to the question on behalf of the user and the response is saved in the AllData system to access the user accounts in the future.

Below are the possible MFA scenarios.

- One text question, one text answer
- One text question, multiple text answers
- One image question, one text answer
- One image question, multiple image answers
- One text question, multiple image answers

The following are the changes the partner will implement for image-based MFA.

1. AllData will send the image ID in the FIMFAQuestions aggregate as part of the HarvestAddStsInqRs or HarvestStsInqRs.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-06.png" alt="Figure 6"/>

2. The partner calls a URL with the image ID generated in the previous step. The base64-encoded image is transmitted to the client browser using the HTTPS connection. See [Appendix C: MFA Image-Retrieving URL](?path=/docs/ws-api/appendices.md#appendix-c-mfa-image-retrieving-url) for the URL details. After decoding the payload, handle downloaded images in the Portable Network Graphics (.png) format.

**Note:** AllData provides an additional option to the partner to get the base64-encoded image in the HarvestAddStsInqRs or HarvestStsInqRs in lieu of image ID. Partners must contact the AllData team to turn on the property to send the actual image as part of the API request.

AllData sends the base64-encoded image in the FIMFAQuestions aggregate as part of the HarvestAddStsInqRs or HarvestStsInqRs.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-07.png" alt="Figure 7"/>

When an attempt to add or update account is already initiated, and while checking the status of the request, the multi-factor authentication flow described in the following diagram occurs.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-08.png" alt="Figure 8"/>

#### Add Account workflow (OAuth)

FIs are implementing OAuth (Open Authorization) protocol to secure their users&#39; sensitive information and to prevent their users&#39; credentials from falling into third-party hands. AllData supports OAuth authorization in the Add Account workflow.

Unlike the other Add Account workflows, partners perform the following steps for OAuth-enabled FIs instead of gathering users&#39; FI credentials.

1. **FIInfoRqRs**

The FIInfo is enhanced with a new element isOAuthFI to help partners determine whether the FI is OAuth-enabled. A value of &quot;True&quot; in this isOAuthFI element indicates the FI is OAuth-enabled.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-09.png" alt="Figure 9"/>

2. **initiateAddAccounts**

As in the standard approach to add new accounts, partners must invoke the initiateAddAccounts API to add accounts under OAuth-enabled institutions. The new parameter PartnerAppID is included in this API request and is a mandatory element when invoking the API to add accounts under OAuth-enabled institutions. Partners must send the PartnerAppID of the application from which the attempt is initiated and that PartnerAppID must be a registered one with the FI. Fiserv performs primary validation to confirm that the PartnerAppID is registered and returns an &quot;Invalid Partner Application ID&quot; response code if not registered (error code 4333). Partners must contact Fiserv to register any new application with the FI.

On invoking the initiateAddAccounts API request with the PartnerAppID, Fiserv sends back the response with the new aggregate OAuthInfo for the OAuth-enabled institutions.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-10.png" alt="Figure 10"/>

3. **getAddAccountStatus**

The partner shares the OAuthUrl to the user, who visits the institution-hosted site to provide login credentials and consent for the accounts to aggregate. Then the partner invokes the getAddAccountStatus API with the OAuthRqID that Fiserv sent in the initiateAddAccounts API response (instead of the RunID used in other Add Account workflows.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-11.png" alt="Figure 11"/>

After the user action at the institution site is complete and the institution sends the token to Fiserv to access user account information, Fiserv sends back the response to getAddAccountStatus with the HarvestID. Then the partner uses the HarvestID as the RunID to invoke getAddAccountStatus and continues polling the status.

After getAddAccountStatus returns a &quot;completed&quot; response, the partner continues with the regular Add Account workflow, invoking the getNewAccounts and createAccounts APIs to complete the add process. The update process for accounts owned by users who have completed the OAuth procedure for OAuth-enabled FIs is the same as the account update process for FIs that do not use OAuth.

If the token shared by the FI expires or becomes invalid, Fiserv marks the institution with error code 301 (Login failure – invalid login credentials) and expects the user to re-authorize at the institution-hosted site to get the new token. To clear the error, the partner must invoke the initiateAddAccounts API with the &#39;AddMoreAccounts&#39; option and include PartnerAppID. Fiserv sends a response with OAuthInfo and the partner follows the same steps above until getAddAccountStatus returns a &quot;completed&quot; response. Then the partner invokes the updateAccounts API to update the accounts and refresh their data.

The following diagram depicts the OAuth Add Account workflow.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-12.png" alt="Figure 12"/>

#### Delete Account workflow

To delete an account from AllData, partners invoke the Delete Account (FIDeleteRq) API on the AllData web services system when the account is deleted from the partner system.

This API is used to delete a connection to an FI established by the partner for a consumer (represented by FILoginAcctId) or a given financial account under that connection (represented by AcctId).

### Account Management

Account Management uses the following web services APIs:

- **initiateAddAccounts:** Partners call this API with client FI login parameters to establish a connection. AllData returns a HarvestID and RunID which the partner uses asynchronously to check the operation status.
- **getAddAccountStatus:** Partners use this API with RunID and HarvestID to poll the status of the initiateAddAccounts API.
- **getNewAccounts:** This API returns the list of accounts within an FI connection once successfully authenticated.
- **createAccounts:** Partners call this API with a list of accounts to be added to AllData for harvesting.
- **deleteAccounts:** Partners use this API to remove accounts from AllData for an FI connection, and the connection itself. Deleting the accounts removes all the harvested data from AllData. Deleting the FI connection (FILoginAcctId) removes all the accounts harvested within the FI, along with user credentials.
- **updateAccountCredentials:** If the user credentials for a stored connection change outside of AllData, partners call this API to update the stored credentials within AllData for an FI connection.
- **maintainAccount:** Partners use this API call to change account classifications, account nicknames, and so on.

### Harvest Account Data

Account Harvesting is a process that gathers the latest account information from the FI. There are two methods of update:

- Auto Update
- On-Demand Update

#### Auto Update

Auto Update is a process by which AllData updates all the accounts classified in automatic fashion on a nightly basis. AllData does not perform nightly updates for inactive users. Partners can elect to receive batch files on a customized schedule.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-15.png" alt="Figure 15"/>

#### On-Demand Update

Users trigger On-Demand updates manually to get the latest account information.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-16.png" alt="Figure 16"/>

The data refresh uses the following web services APIs:

- **updateAccounts:** This asynchronous API returns a RunID and a HarvestID. Partners use these values to poll for update status.
- **getHarvestStatus:** Partners use this API to poll the updateAccounts API call using RunID and HarvestID.

### Retrieve Account Data

Once the accounts are added to AllData system, the AllData engine automatically refreshes the accounts data nightly. The information gathered as part of harvesting is made available through web service APIs.

The transaction data pull APIs can be used to retrieve transactions for each account type and time period. The different transactions APIs are given below.

#### Banking transactions

The Banking transactions can be extracted from the AllData system for a certain time period using the getBankingTrans WS API. See [Account Data Pull APIs](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getBankingTrans) for the details of the web service.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-17.png" alt="Figure 17"/>

#### Credit Card transactions

The Credit Card transactions can be extracted from the AllData system for a certain time period using the getCreditCardTrans WS API. See [Account Data Pull APIs](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getCreditCardTrans) for the details of the web service.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-18.png" alt="Figure 18"/>

#### Investment transactions

The Investment transactions can be extracted from the AllData system for a certain time period using the getInvestmentTrans WS API. See [Account Data Pull APIs](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getInvestmentTrans) for the details of the web service.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-19.png" alt="Figure 19"/>

#### Other Account transactions

The asset/liability/biller account transactions can be extracted from the AllData system for a certain time period using the getOtherAccountTrans WS API. See [Account Data Pull APIs](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getOtherAccountTrans) for the details of the web service.

<img style="display:block;margin:0 auto;" src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-20.png" alt="Figure 20"/>

The account details and positions data pull APIs can be used to retrieve account summary and investment positions for each account type and time period.

Data pulls involve the following web services APIs:

- **getAccountsSummary:** This API returns metadata details for an individual account.
- **getAccountDetails:** This API returns a list of metadata for all harvested accounts within an FI connection.
- **getInvestmentTrans:** This API returns transaction details for an investment account.
- **getCreditCardTrans:** This API returns transaction details for a credit card account.
- **getBankingTrans:** This API returns transaction details for a banking account.
- **getOtherAccountTrans:** This API returns transaction details for liability accounts like billers, mortgages, loans, insurance, and other liability account types (except credit cards).
- **getInvestmentPos:** This API returns the investment positions within an investment account.


## Web Services Setup and Usage

### Web Services Setup

The following are the prerequisites for web services API integration with Fiserv.

1. Environment setup in AllData system
2. Fiserv provides partner with admin user credentials
3. Server URL to submit web services API requests
4. Session handover URL (for single sign-on (SSO))

### URLs

Please note that the server URLs differ between UAT and production environments. The URLs are provided as part of the setup.

## Appendix A: API Error Codes

The following table lists all valid response status codes for the status aggregate defined earlier, their severities, their default text provided in the StatusDesc element, conditions that may trigger them, and, in some cases, information to help API partners resolve issue.

|Code|Severity|StatusDesc default text|Condition|Partner resolution action|
|---|---|---|---|---|
|0|Info|Success|The service provider successfully processed the request.||
|50|Warning|Partial success|Partial success||
|100|Error|General Error|An error prevented the service provider from processing the transaction. No additional information is available.|If this error persists, please contact us with the CEUserID and timestamp.|
|600|Error|Unsupported message|The request received from the partner is not currently supported.||
|1020|Error|Required element not included|Required element is missing in the request received from the partner.|Ensure API request provides all mandatory elements. Refer to Swagger documentation.|
|1740|Error|Authentication Failed|The customer could not be authenticated due to an incorrect HomeID, login ID, or password.|Verify the user login ID and password.|
|2740|Error|Invalid currency code|The currency code specified in the request is invalid.||
|4000|Error|Invalid UserID|The UserID specified in the request is invalid.||
|4010|Error|Invalid partnerID|The partnerID specified in the request is invalid.||
|4012|Error|Invalid HomeID|The HomeID specified in the request is invalid.||
|4020|Error|Invalid password|The password specified in the request is invalid.||
|4030|Error|User already registered|The user identified by partnerID:HomeID:UserID is already registered with the server.||
|4040|Error|User not registered|The user identified by partnerID:HomeID:UserID is not registered with the server.||
|4050|Error|Invalid encryption scheme|The encryption scheme specified in the request is invalid.||
|4060|Info|No data available|No data is available to satisfy the request. This could mean that the user has not registered any financial institutions, there are no transactions in the specified date range, and so on.|If this error occurs on invoking getAccountDetails API, it means user has not added any financial institutions. Confirm the user successfully added accounts under any financial institutions.If this error occurs on invoking transactions APIs, confirm the LastSuccessfulUpdate date is returned in getAccountDetails API for that specific account and the specified date range lies within the LastSuccessfulUpdate date.|
|4070|Error|Validation failure|One or more input fields were invalid.|Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding Swagger documentation.|
|4080|Error|SessInfo creation failed|The Fiserv server was unable to create a SessInfo security token.|Reinvoke the signon API to get the new SessInfo token. If the error persists, please contact us with the CEUserID and timestamp.|
|4090|Error|Invalid SessInfo|The SessInfo token specified in the request is invalid. The session token is invalid or expired.|Generate a new SessInfo token and reinvoke the process flow with the new SessInfo token. If the error persists, please contact us with the CEUserID and timestamp.|
|4100|Error|Invalid FIAcctId|The FIAcctId specified in the request is invalid.||
|4110|Error|AcctType mismatch|The account specified in the request is not of the type specified in the request.|Verify AcctType of the accounts from getAccountDetails API response.|
|4120|Error|Not owner|The FILogin Account or FI Account for which a user is requesting details does not belong to the user.|The FILoginAcctId or AcctId sent by partner does not exist in the Fiserv system or the IDs belong to a different CEUserID in the system.|
|4130|Error|Invalid role|A sign-on has been attempted by a user in a role other than the authorized role.||
|4140|Error|Invalid FILoginAcctId|The FILoginAcctId specified in the request is invalid.||
|4150|Error|Invalid AcctType|The AcctType or ExtAcctType specified in the request is invalid.|Verify AcctType or ExtAcctType of the accounts from getAccountDetails API response.|
|4210|Error|Invalid XML Document|The request received from the partner site is not a valid XML document.||
|4220|Error|UserID unavailable|The UserID specified in the request is not available for registration.||
|4240|Error|UserDataPush failed|An attempt to push a user&#39;s data failed.||
|4250|Error|Invalid transaction type|The transaction type specified in the request is invalid.|Expected transaction type is Debit or Credit. Any other value returns this error.|
|4260|Error|Invalid date|The date specified in the request is invalid.||
|4270|Error|Invalid amount|The amount specified in the request is invalid.||
|4280|Error|Invalid search criterion|A search criterion specified in the request is invalid.||
|4290|Error|Invalid BroadcastMsgID.|The BroadcastMsgID specified in the request is invalid.||
|4300|Error|Account Already Exists|There was an attempt to add an account that already exists.|Do not send an AddNewAccounts request for a user with the same set of credentials in an FI. In case of an error with initial attempt with that set of credentials, refer to the [Harvester Error Codes](#appendix-b-harvester-error-codes) section of this document for information and resolution suggestions.|
|4310|Error|Harvesting Error|There was a harvesting error during the &quot;account add&quot; or &quot;add more accounts&quot; operation.|Research the returned UpdateErrorCode in the [Harvester Error Codes](#appendix-b-harvester-error-codes) section of this document for information and resolution suggestions.|
|4320|Error|FI Login Credentials Required|An &quot;add more account&quot; operation was attempted for a low-trust account without providing the login credentials.||
|4322|Error|Avoid gathering and sending User Credentials in OAuth FI|User credentials sent in the request for adding accounts in an OAuth-enabled FI.|Partner should refer to [Add Account workflow (OAuth)](?path=/docs/ws-api/integrations.md#add-account-workflow-oauth) and follow the instructions when initiating add under an OAuth-enabled FI.|
|4330|Error|Invalid HarvestID|The harvest ID / run ID provided in the request is invalid.||
|4332|Error|Invalid OAuth Request ID|The OAuth request ID sent in the request is invalid.||
|4333|Error|Invalid Partner Application ID|The partner application ID is invalid.||
|4335|Error|Partner Application ID is required|Partner application ID is missing in the request.|Partner should ensure mandatory parameters are sent in the request.|
|4336|Error|OAuth configuration is required|There is an error in OAuth configuration.|If this error persists, please contact us with the CEUserID and timestamp.|
|4350|Error|Update in progress.|An update is already in progress. A new update (update or add) request cannot be submitted until the existing update is completed.|Allow the running update to complete before invoking update or add.|
|4360|Error|Incomplete Data for Request Completion.|The data provided for Request completion is not enough.Typically, this means insufficient data was provided for &quot;add new accounts,&quot; &quot;add more accounts,&quot; or &quot;account maintenance&quot; operations, and so on.||
|4370|Error|Invalid account classification attributes.|The account attributes combination required for account classification is invalid : Instrument, AccountOwnership, RetirementStatus|Please contact us with CEUserID and timestamp.|
|4390|Error|Invalid combination of FILoginAcctId and HarvestAddFetch(s)|Data contained in HarvestAddFetchAcctList does not match the FILoginAcctId. Especially created for the HarvestCreateAcctsRq.|Confirm the FILoginAcctId sent in the request is correct and consistent through the harvest add flow.|
|4410|Error|Invalid FIId|The FIId provided is not supported or does not exist in the database or the number provided is null or non-numerical.||
|4420|Error|Count of Accounts is high for this LoginAcctId|The number of accounts eligible for data pull submitted in AdvFILoginAcctInqRq is greater than the configured value.||
|4430|Error|No accounts found or accounts not eligible for update|The query criteria did not match with any accounts in the database eligible for update.|The updateAccounts API is invoked for a user with no eligible accounts to update. That is, all of the user&#39;s accounts have invalid credentials or they exist under suspended FIs.Confirm that the user has eligible accounts for update.|
|4440|Error|Duration is more than 3 days|The maximum duration allowed for deleted transactions API is three days.||
|4450|Error|No New Accounts|This is a warning message generated, when the harvest action during an &quot;account add&quot; or &quot;add more accounts&quot; operation was successful, but no new accounts were fetched from FI site.|This could be a scripting error or there could simply be no new accounts to add.|
|4460|Error|FI Login Credential exceed max length|Login credentials entered by user exceed the maximum length allowed at financial institution website.||
|4470|Error|The FI Login Account is suspended and cannot be updated. Please update the login information to resume aggregation activity.|When harvesting account login credentials are rejected, the Account Harvest Status (AcctHarvestStatus) is disabled. The FI Login Account Info List Inquiry Request (FILoginAcctInfoListInqRq) returns this error message in the response (FILoginAcctInfoListInqRs) to convey that the account is locked in the Fiserv system.|Correct the institution login credentials.|
|4471|Error|This account is at a financial institution that is not supported within this service. The account data will not be able to be refreshed/updated because support is now unavailable for this financial institution.|Received in the status aggregate when attempting to update an account or add more accounts to an FI that is suspended in Fiserv.|Inform customers that the FI is temporarily unavailable.|
|4480|Error|Invalid AcctId|The AcctId in the request does not match with any accounts for this user or is null.||
|4510|Error|Invalid Account Type Id|The account type id provided is invalid and does not exist in the database or is null.||
|4530|Error|Invalid Balance Type|The balance type provided is invalid.||
|4540|Error|Account Group specified in the request is invalid|The account group provided is invalid. Possible values are: Bill, Cash, Credit, Insurance, Investment, and Other||
|4550|Error|Invalid Date Range|The Start Date specified in the date range greater than current date.||
|4551|Error|Invalid date range. Modified date range returned no results.|Date range is a maximum of 90 days for all the transaction pull APIs except Deleted transactions API. No transactions are available in Fiserv for the modified date range.|The default maximum range is 90 days; this is configurable for partners requesting extended transaction history.|
|4560|Error|FI URL cannot be NULL|FI URL provided in the request is empty. FI URL is a mandatory field.||
|4570|Error|FI Name cannot be NULL|FI Name provided in the request is empty. FI Name is a mandatory field.||
|4580|Error|FI has already been requested and is under development|The Fiserv Data Operations team is already working on developing scripts for the requested FI.||
|4590|Error|FI is already supported|The requested FI is already supported.|Search for this FI or refresh the FI seed data.|
|4591|Error|No FIs found matching the search criteria provided|The FI search criteria provided did not match with any FIs in the database.||
|4600|Error|Request ID is required|Request ID (RqUID) is a mandatory field. Each request should have a unique request ID so partners can reconcile the responses they receive.||
|4603|Error|Person Name is required|PersonName aggregate is mandatory in PersonInfo aggregate.||
|4604|Error|First Name is required|FirstName is mandatory in PersonName aggregate.||
|4605|Error|Last Name is required|LastName is mandatory in PersonName aggregate.||
|4615|Error|E-Mail address passed in the request is invalid|The email address provided in the request is invalid.||
|4620|Error|UserProfile is required|UserProfile aggregate is mandatory if the partner home is configured.||
|4621|Error|PersonInfo is required|PersonInfo aggregate is mandatory if the partner home is configured.||
|4622|Error|Contact Info is required|Contact Info aggregate is mandatory if the partner home is configured.||
|4623|Error|Day Phone is required|DayPhone is mandatory if the partner home is configured.||
|4624|Error|Evening Phone is required|EveningPhone is mandatory if the partner home is configured.||
|4625|Error|Phone number has non-numeric data|DayPhone and/or EveningPhone provided are non-numerical.||
|4626|Error|Total phone numbers do not match allowed occurrence|Telephone numbers provided should not exceed the configured occurrence value.||
|4627|Error|Email Address is required|Email address is mandatory if the partner home is configured.||
|4628|Error|Invalid Email Address|The email address provided in the request is invalid.||
|4629|Error|Only one day phone value is allowed|DayPhone is provided more than once.||
|4630|Error|Only one evening phone value is allowed|EveningPhone is provided more than once.||
|4710|Error|User is locked|The requested user is locked in the Fiserv system.|Please contact us with the CEUserID.|
|5012|Error|Trust mode cannot be changed, as only high trust mode is allowed.|Trust mode cannot be changed if the partner home is configured for high-trust mode only.|Occurs when partners send other trust modes (low or medium trust) when editing the FI credentials.|
|5013|Error|Invalid AcctName|When creating or updating an offline account, an empty or null value was sent for account name element.||
|7601|Error|Transaction Not Found|There is no transaction with given criteria.|Verify the transaction ID sent in the request.|
|7602|Error|Category Not Found|The category provided does not exist in the Categorization Engine seed data.||
|7603|Error|Subcategory Not Found|The subcategory provided does not exist in the Categorization Engine seed data.||
|7604|Error|Subcategory label already exist|The user tried to add a category that already exists.||
|7605|Error|Not Processed|This error is related to category engine processing indicating that the transaction category update was not processed.||
|7611|Error|Request originated from an invalid IP address|Request originated from unknown (not whitelisted) IP address.|Contact Fiserv to whitelist any new IPs or make the request from whitelisted IPs.|
|7613|Error|Unsupported End Date, is lesser than 5 days from today|EndDt provided in AutoHarvestStsRq is less than 5 days from today.||
|7701|Error|This financial institution requires a multi-factor authentication process that is currently not enabled for this home|The FI requires an image-based multi-factor authentication that is currently not enabled for this home. Please contact Fiserv to enable it.||


## Appendix B: Harvester Error Codes

The following table lists and describes error codes returned as partof harvesting.

|Error code|Error|Message|Description|Partner resolution action|
|---|---|---|---|---|
|100|Internal software error|Internal software error. Please try updating again in a few minutes.|Fiserv tried harvesting information from an institution but could not harvest the information successfully despite getting response from the institution.|Temporary issue: Partner should send HarvestAddRq/HarvestRq again. If it still fails, then it may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries. These errors could be due to many reasons, such as FI server is busy or FI site is down. Retries usually resolve the problem.|
|103|Nonexistent URL or network failure.|Network failure. Please try updating again in a few minutes.|This is typically caused when there is a change in the hostname of the URL that is being accessed, but sometimes occurs due to temporary issues.|Temporary issue: Partner should send HarvestAddRq/HarvestRq again. If it still fails, then it may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries.|
|104|Time out error|We cannot establish a connection to your financial institution website at this time. Please try updating again in a few minutes.|Fiserv tried to connect to the institution where your account exists and was unable to reach the host institution due to network traffic. This is NOT a session timeout. When we fail to make a connection with the FI site after a few tries, it is classified as 104 error.|Temporary issue: Partner should send HarvestAddRq/HarvestRq again. If it still fails, then it may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries.|
|105|Target server error or server down|We are receiving a message that the institution&#39;s website is temporarily unavailable. We will update the account when the website becomes available|Fiserv tried to connect to the institution where your account exists, and host institution is returning server error.|Temporary Issue - The partner needs to send HarvestAddRq/HarvesRq again. If it still fails, then it may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries.|
|106|Connection failed|We are not able to update your account because of network traffic on the internet. Please try updating again in a few minutes.|Fiserv tried to connect to the institution where your account exists and was unable to reach the host institution due to connection failure.|Temporary issue: Partner should send HarvestAddRq/HarvestRq again. If it still fails, then it may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries.|
|107|SSL error|We cannot establish a connection to your financial institution website at this time. Please try updating again in a few minutes.|This is typically caused by a failure to establish SSL connection with the FI website. Usually it is a failure that AllData can fix.|Temporary issue: Partner should send HarvestAddRq/HarvestRq again. If it still fails, then it may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries.|
|108|Server error (FI website maintenance)|Your financial institution website is not available.Please try updating again later.|This happens when the FI website returns a positive response but displays a message that the account information is unavailable. This is established based on a positive key text assertion.|Temporary issue: Partner should send HarvestAddRq/HarvestRq again. If it still fails, then it may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries.|
|109|Client error|We cannot establish a connection to your financial institution website at this time. Please try updating again in a few minutes.|This is an extremely rare error. It occurs due to a change in the site or some temporary issues.|Temporary issue: Partner should send HarvestAddRq/HarvestRq again. If it still fails, then it may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries.|
|110|Account information unavailable|We did not find any relevant account values displayed in the financial institution website for this account which could be aggregated. Please check your account status in financial institution website.|Account-specific issues such as account value unavailable to harvest fall in this category. In this case, there will not be any required values displayed in the FI website for an account to harvest and it&#39;s not related to temporary FI website maintenance issues.For example, no balance account could be on the lines of a closed account or inactive due to specific reasons. User should check the account status on FI website and then decide to drop or retain the account.|Partner should send HarvestAddRq/HarvestRq again when all the information is available in FI website is available. If it still fails, then it may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries.|
|121|Account not updated during nightly refresh|There is a delay in updating this account due to a system issue. Please check after some time or retry updating the account.|Due to technical reasons, there could be a delay in account update during nightly refresh. Fiserv will investigate this error to resolve it.|Temporary issue: Partner should send HarvestAddRq/HarvestRq again. If it still fails, then it may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries.|
|200|Site change|We are in the process of upgrading our product to rectify this problem. Please try again later.|This along with error code 100 is a blanket category for a change in the website layout. This is also a generic error code that will be presented before getting assigned with an appropriate error code.|Temporary issue: Partner should send HarvestAddRq/HarvestRq again. If it still fails, then it may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries.|
|201|Site change - account maintenance needed|We can no longer find this account in your online access at this financial institution website.|This is caused by a change in display of account identifier by the FI website leading to a mismatch in account identification process and inability of the AllData harvesting process to identify an account. This could also be caused by a user closing an account with the FI and therefore account is not available on the site.|Partner should send FIDeleteRq to delete the failing accounts and use HarvestAddRq-AddMoreAccts for adding new accts.|
|202|Multiple matches found|We are not able to update this account at this time as we are currently upgrading our data collection process for this financial institution.|Failure at the time of identifying accounts during account update process because multiple matches were found for the account.|Partner should report the issue to Fiserv.|
|203|No account found|It appears that this account can no longer be located within the institution&#39;s website. Please confirm this by logging into the Institution&#39;s website and delete this account from Fiserv if it does not exist.|Account not found in FI website. Account&#39;s presence on FI website will be monitored for a few days and then marked as a missing account (204) if it is still not found.|Partner should send FIDeleteRq to delete the failing accounts and use HarvestAddRq-AddMoreAccts for adding new accts.|
|204|No account found|This account no longer exists in the institution&#39;s website. Please delete this account as it does not exist.|This error code is meant to clearly indicate to the user that the account is missing on the FI website and to seek user&#39;s intervention to delete the account from Fiserv application.These accounts are not harvested nightly.|Partner should send FIDeleteRq to delete the failing accounts and use HarvestAddRq-AddMoreAccts for adding new accts.|
|205|Empty fetch list|We are not able to update this account at this time as we are currently upgrading our data collection process for this financial institution.|Failure at the time of identifying accounts during account update process because no accounts were found.|Partner should report the issue to Fiserv.|
|208|Unable to determine type of account|We are unable to determine what type of an account you have at this financial institution.|This error code is meant to indicate that our account classification engine is unable to determine the type of this account using information our harvesting engine finds at the financial institution.|Partner should classify this account based on additional information they have about this account or may ask the user to classify it as the user will know the type of account.|
|209|Incorrect account type classification|Incorrect account type classification. Please reclassify the account type correctly to aggregate values for this account.|Account type incorrectly classified, hence driving the harvest failure during account update.For example, an investment account is classified as a checking account type.|Partner should send UserAcctModRq with correct account attributes and then send HarvestRq|
|300|Login failure|Please verify that the financial institution username/password that you entered is correct. If the login credentials are correct, please try again later.|This is a result of inability to establish a session due to login failure. This login failure is different from 301 (below) in that it excludes cases where the FI website displays an explicit message about any incorrect login credentials.|UserOFILoginCredentialsModRq followed by HarvestAddRq-AddMoreAcct|
|301|Login failure - invalid login credentials|We cannot login with username/password combination you provided.Please make sure the login information is correct.|The FI website displays a clear message that the login credentials are incorrect.|UserOFILoginCredentialsModRq followed by HarvestAddRq-AddMoreAcct|
|302|Agreement page notification|Agreement page notification. Please log into your online account at this financial institution and follow the instructions presented by the financial institution.|This is typically caused when the FI site displays an intermediate page that cannot be skipped or there is some action that the user needs to take to proceed with online banking. It requires user intervention on the FI website.|User must clear the intermediate page by logging into the FI website. Then partner should send HarvestAddRq/HarvestRq.|
|303|Additional information required|Multi-factor authentication failure. Your financial institution website requires additional information to proceed.|The error is displayed when harvester detects a challenge question as part of MFA (multi-factor authentication).|HarvestAddRq-AddMoreAcct to answer the MFA question and pass the original RunID and session ID|
|304|Incorrect answer provided to the site key challenge question|Multi-factor authentication failure. Please make sure that you provide the right answer to the challenge question.|Displays after providing answer to the challenge question as part of MFA (multi-factor authentication) the FI site returns an identifiable message indicating that the answer was wrong.|HarvestAddRq-AddMoreAcct with FILoginAcctId only|
|305|Incorrect third parameter provided to the site|Invalid client or account identifier. Please make sure that you provide the correct client or account identifier.|This is applicable only for advisor access FIs and not for advisor aggregation FIs.|Treat similarly to 301.|
|306|Incorrect FI selection|Incorrect financial institution selection. Please select the correct financial institution to add your account.|Applies only to account setup process, where the user selects an incorrect FI for account addition. For example, advisors choosing Client Access FI to add. However, if a FI moves some of its accounts to another FI then it could occur during update.|Delete the account (FIDeleteRq) that caused this error.HarvestAddRq-AddNewAcct|
|307|Account locked|Your account is locked at this financial institution. Please contact your financial institution for more information.|Account is locked on FI website and the harvest process fails. User should contact financial institution to unlock the account.|User must call FI to unlock the logon. Then partner should send HarvestAddRq/HarvestRq.|
|311|Harvest failure|We are in the process of upgrading our product to rectify this problem. Please try again later.|Script failure during login process resulting in an add account or update failure. Fiserv will investigate and resolve this script failure. This is also a generic error code that is returned for all failures during login before getting assigned to an appropriate error code.|Temporary issue: Partner should send HarvestAddRq/HarvestRq again. If it still fails, then it may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries.|
|400|Database update failure|Database update failure. Please try updating again in a few minutes.|This is an internal error for AllData database failures. This is typically caused by a database configuration issue and AllData can usually fix it. This error is typically not specific to an FI script and is rare.|Temporary issue: Partner should send HarvestAddRq/HarvestRq again. If it still fails, there may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries.|
|600|Institution on watch list|We cannot currently add your accounts because we are in the process of upgrading our support for this institution. This upgrade may take several more days. We will place this institution in your pending accounts list. Please try adding accounts again in a few days.|This error is encountered when a user tries to add an FI which is under development in AllData system. After the scripts are developed, the error code will be reset to enable user access.|Data Operations is in the process of developing the scripts for the FI. This error can be reported to Fiserv to escalate the completion of FI script development.|
|999|Internal Software Error – Unknown Error|Internal Software Error. Please try updating again in a few minutes.|This represents the residual category and is very minimal.|Temporary Issue - The partner needs to send HarvestAddRq/HarvesRq again. If it still fails, then it may be a harvesting error. Partner should report the issue to Fiserv after several unsuccessful retries.|

## Appendix C: MFA Image-Retrieving URL

The partner must call an image-retrieving URL in the &lt;img src&gt; HTML tag to get the image from the AllData system. The base64-encoded image is transmitted to the client browser via HTTPS connection to be rendered in the partner UI application. The partner can also request that Fiserv enable transmitting a base64-encoded version of the image directly in the web service response.

Details of Fiserv image-retrieving URL:

**Prod URL:**

&lt;img src=&quot;https://&lt;domain-name&gt;/downloadMFAimage?imageId=&lt;ImageID&gt;&quot;/&gt;

**UAT URL:**

&lt;img src=&quot;`https://aggqa.alldata.Fiserv.com/downloadMFAimage?imageId=`&lt;ImageID&gt;&quot;/&gt;

Method Type: Get

Input Param: imageId

Response:

- On Success: The base64-encoded image
- On Failure: Null stream is passed to the partner.

**Note:** The image will be deleted from the AllData system for that image ID once invoked using the image-retrieving URL.
