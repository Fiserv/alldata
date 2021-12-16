# AllData Account Aggregation

Account aggregation lets users view all their online financial accounts such as checking, savings, investment, retirement, insurance, and credit cards from a range of FIs on a single web page.

## Data Harvesting Techniques

Aggregation uses a combination of techniques to gather the various account data:

- **Web service connectivity:** A direct connection to the FI using a web service
- **File download:** AllData downloads files from FI websites for account data.
- **User credential-based harvesting:** AllData harvests FI websites for client account balances and transactions.

### Web service connectivity

Data collection uses a direct connection to the FI through web services. Due to the overhead in providing a dedicated service for aggregators, many small to mid-size institutions do not provide this service.

### File download

FIs make their user data available in downloadable QIF, OFX, CSV, and TXT files upon user authentication at login. AllData uses this information to fetch the user account and transaction data.

### User credential-based harvesting

This technique involves collecting customer account data from FI web pages with a sophisticated web crawling engine that uses customer login credentials. This approach has high success rates and can handle multi-factor authentication (MFA) scenarios using the AllData API. A dedicated connectivity engineering team supports the underlying infrastructure to ensure data availability and validity.

## High-Level Architecture

This diagram shows the high-level architecture and data flow between a partner, AllData, and institutions.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-01.png" alt="Figure 1"/>
</p>

# AllData Service Integration

This section describes the different partner integration options with the AllData system.

## Full API WS Integration

In this model, RESTful web services APIs handle the complete flow from user registrations to data pull and account management. Partners develop custom user interfaces for account setup and management using the web services. Partners can invoke the web services using XML or JSON requests. Partners can also sign up for optional nightly batch files that allow AllData to push data.

## UI Widgets and API Integration

In this model, the AllData service is integrated through individual widgets embedded or overlaid in the partner web/native/mobile application. A widget is a web resource composed of web pages and accessed as any HTTP URL. Users authenticate with AllData to establish valid SSO sessions before accessing widgets. Partners can configure the widgets&#39; functionality and look and feel using CSS.

## UI Portal Integration (PFM)

In this model, the AllData PFM UI is used for account administration, and partners use data pull daily to retrieve the latest account information available from the Fiserv platform for all users.

The following table lists the different integration types and the API sets they typically use.

| API set | AllData API integration | UI portal integration | UI widget / API integration |
| --- | --- | --- | --- |
| User management | ✔ | ✔ | ✔|   
| External FI seed data | ✔ |  |  |
| Host account management | ✔ | ✔ | ✔ |
| Account management | ✔ | |  |  |  
| Account harvesting | ✔ | |  |  |
| Account data pull API | ✔ |  | ✔ ||  
| Advisor management | ✔ | |||
| Client management | ✔ | |||
| Home setup | ✔ | |||
| Transaction categorization | ✔ | |||

# Full API Web Service Integration

In this model, partners do not use the AllData UI for account setupandmanagement. Instead, partners develop custom user interfaces that communicate with AllData using web service APIs. The following is a list of the different categories of APIs available in the AllData system, each with a brief description.

- **User management:** APIs that support activities to manage users
- **External FI seed data:** APIs for partners to fetch information about FI data sources
- **Account management:** APIs to support user account management activities like user creation
- **Account harvesting:** APIs to allow periodic and on-demand retrieval of user account details
- **Account data management:** APIs to allow clients to retrieve user account data from AllData
- **Client management:** APIs to manage activities related to client-advisor relationships
- **Transaction categorization:** APIs to support transaction categorization
- **Advisor management:** APIs to allow working with wealth management advisors (applies only to AllData Advisor platform)
- **Miscellaneous:** Other APIs, such as for PCI compliance

## User Management

AllData user management APIs support various activities to manage users. Partners use the Add User API to add one or more users to the AllData platform. The API expects typical user profile information such as name and address. Once AllData adds a user to its database, it returns a unique CEUserID that must persist in the partner database. The ID accompanies any user actions with AllData.

The following diagram details the typical add user process.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-02.png" alt="Figure 2"/>
</p>

The User Management module uses the following web services:

- createUser
- deleteUser
- getUserProfile
- updateUserProfile
- updateUserPassword

## Initial Financial Institution List Setup

As part of the initial integration with AllData, partners must fetch a list of institutions Fiserv supports and store it in their systems using the web services. After this initial load, subsequent synchronizations fetch only newly added and updated institution data.

**Note:** The AllData FI list has over 12,500 entries. Given the size of the list, we recommend that partners batch getFIDetails calls, with a limit of 500 FIs returned per call. This process should be executed server to server, taking into consideration that the fetch times may be longer and should not abort with a short timeout.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-03.png" alt="Figure 3"/>
</p>

## Periodic Maintenance Refresh

Partners should check for new and updated institution data daily by passing the current time and previous refresh time to obtain the delta. The following diagram represents this periodic data maintenance process.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-04.png" alt="Figure 4"/>
</p>

## External FI Seed Data

The following APIs are involved in external FI seed data management:

- getFinancialInstInfo
- searchFinancialInst

## External FI Messages (Special Instructions)

After fetching the supported institutions, partners must retrieve and store &quot;FI Messages&quot; (special instructions) associated with some institutions. Special instructions include actions users must perform before adding accounts. For example, some institution websites require users to provide consent to allow third-party aggregators to harvest account information; special instructions from such an institution would contain the user action to provide consent on its website.

After the initial load, partners perform periodic synchronizations that fetch only new and updated FI Message data.

The API used to fetch FI Messages is getFIMessageInfo.

## Add External Account (Held Away Accounts)

AllData account management APIs support activities required to aggregate a user&#39;s externally held (&quot;held away&quot;) accounts. Using the FI seed data and user login credentials, Fiserv fetches the user accounts at a given FI. The user chooses the specific accounts to aggregate and monitor, then Fiserv persists and updates the account information on a nightly basis. Partners can also refresh these accounts on demand.

### Add Account workflow (without multi-factor authentication)

Partners use the following steps to add new accounts without multi-factor authentication (MFA). (See the [next section](#_Add_Account_Workflow) for the same process with MFA.) Partners should cache the FI seed data daily before calling these web services.

1. **initiateAddAccounts**

This step initiates the process of adding external accounts. This request triggers a harvesting request using HarvestAddRq with FI login parameters (username/password) or FILoginAcctId. AllData connects to the FI website and harvests account information. Refer to [Account Management Web Services](#_bookmark42) for the details of the API.

1. **getAddAccountStatus**

Invoke this API after the initiateAddAccounts request has started. It returns the status of InitiateAddAccounts. Invoke this API at a periodic interval to check the status. Refer to [Account Management Web Services](#_getAddAccountStatus) for the details of the API.

1. **getNewAccounts**

Invoke this API after the initiateAddAccounts request has completed. It returns the list of financial accounts found on the FI website. Currently these accounts are not created in the AllData system. Refer to [Account Management Web Services](#_getNewAccounts) for the details of the API.

1. **createAccounts**

Invoke this API to create the accounts in AllData system. Fiserv classifies the harvested accounts based on certain account classification rules. The user can either override this classification or provide the classification if not already set. After successfully adding accounts the partner must call update account APIs to run on-demand harvesting to retrieve account data. Refer to [Account Management Web Services](#_bookmark45) for API details. The following figure depicts the typical non-MFA Add Account workflow.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-05.png" alt="Figure 5"/>
</p>

### Add Account workflow (with multi-factor authentication)

AllData system supports multi-factor authentication (MFA) as part of the Add Account workflow which some FIs may require. AllData supports these with special handling of such accounts. When the first-time login credentials fail with MFA error (303 error code), AllData APIs fetch the MFA details (question or image) and present it to the partner. The partner provides the answer to the question on behalf of the user and the response is saved in the AllData system to access the user accounts in the future.

Below are the possible MFA scenarios. See the AllData MFA Sample Responses document for more detailed information.

- One text question, one text answer
- One text question, multiple text answers
- One image question, one text answer
- One image question, multiple image answers
- One text question, multiple image answers

The following are the changes the partner will implement for image-based MFA.

1. AllData will send the image ID in the FIMFAQuestions aggregate as part of the HarvestAddStsInqRs or HarvestStsInqRs.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-06.png" alt="Figure 6"/>
</p>


2. The partner calls a URL with the image ID generated in the previous step. The base64-encoded image is transmitted to the client browser using the HTTPS connection. See [Appendix C: MFA Image-Retrieving URL](#_Appendix_C:_MFA) for the URL details. After decoding the payload, handle downloaded images in the Portable Network Graphics (.png) format.

**Note:** AllData provides an additional option to the partner to get the base64-encoded image in the HarvestAddStsInqRs or HarvestStsInqRs in lieu of image ID. Partners must contact the AllData team to turn on the property to send the actual image as part of the API request.

AllData sends the base64-encoded image in the FIMFAQuestions aggregate as part of the HarvestAddStsInqRs or HarvestStsInqRs.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-07.png" alt="Figure 7"/>
</p>

When an attempt to add or update account is already initiated, and while checking the status of the request, the multi-factor authentication flow described in the following diagram occurs.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-08.png" alt="Figure 8"/>
</p>

### Add Account workflow (OAuth)

FIs are implementing OAuth (Open Authorization) protocol to secure their users&#39; sensitive information and to prevent their users&#39; credentials from falling into third-party hands. AllData supports OAuth authorization in the Add Account workflow.

Unlike the other Add Account workflows, partners perform the following steps for OAuth-enabled FIs instead of gathering users&#39; FI credentials.

1. **FIInfoRqRs**

The FIInfo is enhanced with a new element isOAuthFI to help partners determine whether the FI is OAuth-enabled. A value of &quot;True&quot; in this isOAuthFI element indicates the FI is OAuth-enabled.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-09.png" alt="Figure 9"/>
</p>

1. **initiateAddAccounts**

As in the standard approach to add new accounts, partners must invoke the initiateAddAccounts API to add accounts under OAuth-enabled institutions. The new parameter PartnerAppID is included in this API request and is a mandatory element when invoking the API to add accounts under OAuth-enabled institutions. Partners must send the PartnerAppID of the application from which the attempt is initiated and that PartnerAppID must be a registered one with the FI. Fiserv performs primary validation to confirm that the PartnerAppID is registered and returns an &quot;Invalid Partner Application ID&quot; response code if not registered (error code 4333). Partners must contact Fiserv to register any new application with the FI.

On invoking the initiateAddAccounts API request with the PartnerAppID, Fiserv sends back the response with the new aggregate OAuthInfo for the OAuth-enabled institutions.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-10.png" alt="Figure 10"/>
</p>

1. **getAddAccountStatus**

The partner shares the OAuthUrl to the user, who visits the institution-hosted site to provide login credentials and consent for the accounts to aggregate. Then the partner invokes the getAddAccountStatus API with the OAuthRqID that Fiserv sent in the initiateAddAccounts API response (instead of the RunID used in other Add Account workflows.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-11.png" alt="Figure 11"/>
</p>

After the user action at the institution site is complete and the institution sends the token to Fiserv to access user account information, Fiserv sends back the response to getAddAccountStatus with the HarvestID. Then the partner uses the HarvestID as the RunID to invoke getAddAccountStatus and continues polling the status.

After getAddAccountStatus returns a &quot;completed&quot; response, the partner continues with the regular Add Account workflow, invoking the getNewAccounts and createAccounts APIs to complete the add process. The update process for accounts owned by users who have completed the OAuth procedure for OAuth-enabled FIs is the same as the account update process for FIs that do not use OAuth.

If the token shared by the FI expires or becomes invalid, Fiserv marks the institution with error code 301 (Login failure – invalid login credentials) and expects the user to re-authorize at the institution-hosted site to get the new token. To clear the error, the partner must invoke the initiateAddAccounts API with the &#39;AddMoreAccounts&#39; option and include PartnerAppID. Fiserv sends a response with OAuthInfo and the partner follows the same steps above until getAddAccountStatus returns a &quot;completed&quot; response. Then the partner invokes the updateAccounts API to update the accounts and refresh their data.

The following diagram depicts the OAuth Add Account workflow.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-12.png" alt="Figure 12"/>
</p>

### Add Account workflow (PCI compliant FI)

If the FI is PCI compliant, partners must implement the following in addition to one of the Add Account workflows above.

1. **FIInfoRqRs**

The FIInfo is enhanced with a new element (HasCardData) to help the partner determine whether the FI is PCI compliant. A value of &quot;Y&quot; in this HasCardData element indicates PCI compliance.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-13.png" alt="Figure 13"/>
</p>

1. **GetWMAccessKey**

Partners must call a **GetWMAccessKey** web service to generate the WMAccessKey. Refer to the [Miscellaneous chapter](#_GetWMAccessKey) for the details of this web service.

  1. Partners should not send card information to AllData web services system.
  2. Partners must send card info to Fiserv PCI system and get token to use in place of card number.
  3. The Fiserv PCI system requires the WMAccessKey for access.
  4. The WMAccessKey is valid for 15 minutes.

1. **CardToken**

Partners call a URL with the WMAccessKey generated in the previous step along with the &quot;CardInfo&quot; to generate the CardToken. See the AllData PCI Integration document for details.

1. **HarvestAddRq:**

Call HarvestAddRq with FI login parameters providing username/password of the FI along with the CardToken generated in step 3 in the \&lt;CryptVal\&gt; node.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-14.png" alt="Figure 14"/>
</p>

1. **HarvestAddFetchRq:**

Call HarvestAddFetchRq if the HarvestAddStsInqRq returned a &quot;completed&quot; status passing in a harvest ID.

1. **HarvestAddCreateRq:**

Call HarvestAddCreateRq and provide the accounts to create on the AllData side.

### Delete Account workflow

To delete an account from AllData, partners invoke the Delete Account (FIDeleteRq) API on the AllData web services system when the account is deleted from the partner system.

This API is used to delete a connection to an FI established by the partner for a consumer (represented by FILoginAcctId) or a given financial account under that connection (represented by AcctId).

## Account Management

Account Management uses the following web services APIs:

- **initiateAddAccounts:** Partners call this API with client FI login parameters to establish a connection. AllData returns a HarvestID and RunID which the partner uses asynchronously to check the operation status.
- **getAddAccountStatus:** Partners use this API with RunID and HarvestID to poll the status of the initiateAddAccounts API.
- **getNewAccounts:** This API returns the list of accounts within an FI connection once successfully authenticated.
- **createAccounts:** Partners call this API with a list of accounts to be added to AllData for harvesting.
- **deleteAccounts:** Partners use this API to remove accounts from AllData for an FI connection, and the connection itself. Deleting the accounts removes all the harvested data from AllData. Deleting the FI connection (FILoginAcctId) removes all the accounts harvested within the FI, along with user credentials.
- **updateAccountCredentials:** If the user credentials for a stored connection change outside of AllData, partners call this API to update the stored credentials within AllData for an FI connection.
- **maintainAccount:** Partners use this API call to change account classifications, account nicknames, etc.

## Harvest Account Data

Account Harvesting is a process that gathers the latest account information from the FI. There are two methods of update:

- Auto Update
- On-Demand Update

### Auto Update

Auto Update is a process by which AllData updates all the accounts classified in automatic fashion on a nightly basis. AllData does not perform nightly updates for inactive users. Partners can elect to receive batch files on a customized schedule.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-15.png" alt="Figure 15"/>
</p>

### On-Demand Update

Users trigger On-Demand updates manually to get the latest account information.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-16.png" alt="Figure 16"/>
</p>

The data refresh uses the following web services APIs:

- **updateAccounts:** This asynchronous API returns a RunID and a HarvestID. Partners use these values to poll for update status.
- **getHarvestStatus:** Partners use this API to poll the updateAccounts API call using RunID and HarvestID.

## Retrieve Account Data

Once the accounts are added to AllData system, the AllData engine automatically refreshes the accounts data nightly. The information gathered as part of harvesting is made available through web service APIs.

The transaction data pull APIs can be used to retrieve transactions for each account type and time period. The different transactions APIs are given below.

### Banking transactions

The Banking transactions can be extracted from the AllData system for a certain time period using the getBankingTrans WS API. See [Account Data Pull APIs](#_getBankingTrans) for the details of the web service.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-17.png" alt="Figure 17"/>
</p>

### Credit Card transactions

The Credit Card transactions can be extracted from the AllData system for a certain time period using the getCreditCardTrans WS API. See [Account Data Pull APIs](#_bookmark58) for the details of the web service.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-18.png" alt="Figure 18"/>
</p>

### Investment transactions

The Investment transactions can be extracted from the AllData system for a certain time period using the getInvestmentTrans WS API. See [Account Data Pull APIs](#_getInvestmentTrans) for the details of the web service.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-19.png" alt="Figure 19"/>
</p>

### Other Account transactions

The asset/liability/biller account transactions can be extracted from the AllData system for a certain time period using the getOtherAccountTrans WS API. See [Account Data Pull APIs](#_bookmark59) for the details of the web service.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-20.png" alt="Figure 20"/>
</p>

The account details and positions data pull APIs can be used to retrieve account summary and investment positions for each account type and time period.

Data pulls involve the following web services APIs:

- **getAccountsSummary:** This API returns metadata details for an individual account.
- **getAccountDetails:** This API returns a list of metadata for all harvested accounts within an FI connection.
- **getInvestmentTrans:** This API returns transaction details for an investment account.
- **getCreditCardTrans:** This API returns transaction details for a credit card account.
- **getBankingTrans:** This API returns transaction details for a banking account.
- **getOtherAccountTrans:** This API returns transaction details for liability accounts like billers, mortgages, loans, insurance, and other liability account types (except credit cards).
- **getInvestmentPos:** This API returns the investment positions within an investment account.

# UI Widgets and API Integration

In this model, the AllData Aggregation service is integrated as individual widgets embedded or overlaid in the partner web application or mobile application. See the AllData Widget Integration Guide for complete partner widget documentation.

An AllData widget is a web resource composed of web pages and accessed as any HTTP URL. Partners must use the user credentials registered with AllData to invoke the sign-on API and get a valid session token before accessing any widgets.

Using the session token this way allows users to seamlessly access AllData widgets from partner applications without entering additional identifiers, passwords, or other credentials.

Most API partners choose to integrate the **AllData Add Accounts widget** into their applications. It enables adding financial institutions using their FI-issued credentials.

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-21.png" alt="Figure 21"/>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-22.png" alt="Figure 22"/>
</p>

## User Registration and Sign-On

User registration and sign-on involves the following APIs.

- **createUser:** Partners use this to create customer profiles within an AllData home.
- **signon:** This API allows partners to use AllData customer credentials to get an SSO token. This token is required to launch the Add Accounts Widget flow.

See [User Management Web Services](#_User_Management_Web) for more details on user registration and sign-on APIs.

# UI Portal (AllData PFM) Integration

In this model, a partner uses a hosted AllData PFM UI for account administration and daily data updates.

The createUser WS API request includes performing AllData PFM user registration, and the AllData PFM UI is used for account administration to add, edit, and delete accounts in the user&#39;s login.

The user passes seamlessly from the partner&#39;s secure web application to the AllData PFM secure web portal without entering an additional identifier, password, or other credentials.

_AllData PFM:_

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-23.png" alt="Figure 23"/>
</p>

_AllData PFM – Account Administration:_

<p align="center">
  <img src="https://raw.githubusercontent.com/Fiserv/alldata/develop/assets/images/alldata-ws-api-specs-4.1/alldata-ws-api-specs-4.1-24.png" alt="Figure 24"/>
</p>

© 2019-2021 Fiserv, Inc. or its affiliates. All rights reserved. This work is confidential, and its use is strictly limited. Use is permitted only in accordance with the terms of the agreement under which it was furnished. Any other use, duplication, or dissemination without the prior written consent of Fiserv, Inc. or its affiliates is strictly prohibited. The information contained herein is subject to change without notice. Except as specified by the agreement under which the materials are furnished, Fiserv, Inc. and its affiliates do not accept any liabilities with respect to the information contained herein and are not responsible for any direct, indirect, special, consequential or exemplary damages resulting from the use of this information. No warranties, either express or implied, are granted or extended by this document.

[http://www.fiserv.com](http://www.fiserv.com/)

Fiserv is a registered trademark of Fiserv, Inc.

Other brands and their products are trademarks or registered trademarks of their respective holders and should be noted as such.