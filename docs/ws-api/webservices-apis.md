<!-- TODO : Fix links for each APIs-->

# User Management Web Services

This chapter lists each User Management API in a table with a resource URL, descriptive information, and a link to API Explorer documentation.

## createUser
![createUser](https://raw.githubusercontent.com/Fiserv/alldata/md_fix/assets/images/ws-api/01.png)

[createUser API docs](../api/?type=post&path=/WealthManagementWeb/ws/UserMgmt/createUser)

## deleteUser
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>deleteUser</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/UserMgmt/deleteUser</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to unregister a previously registered user.
                <li>User data including accounts, transactions, etc., are deleted.</li>
                <li>Admin users cannot be deleted using this service.</li>
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/UserMgmt/deleteUser">deleteUser API docs</a>
            </td>
        </tr>
    </tbody>
</table>

## getUserProfile
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getUserProfile</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/UserMgmt/getUserProfile</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to retrieve user profile information such as name, address, and contact details.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/UserMgmt/getUserProfile">getUserProfile API docs</a>
            </td>
        </tr>
    </tbody>
</table>

## updateUserProfile
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>updateUserProfile</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/UserMgmt/updateUserProfile</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to update user profile information such as name, address, and contact details.<br>
                <b>Note:</b> Fiserv does not validate address or email.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/UserMgmt/updateUserProfile">updateUserProfile API docs</a>
            </td>
        </tr>
    </tbody>
</table>

## updateUserPassword
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>updateUserPassword</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/UserMgmt/updateUserPassword</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to change the password of the current user identified by the credentials in this request&#39;s SignonRq section.
                <li>Admin user&#39;s password cannot be modified using this request.</li>
                <br />
                <b>Note:</b> This API only updates the user password for a partner&#39;s customer AllData credentials. To update FI credentials, use the updateUserCredentials API.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/UserMgmt/updateUserPassword">updateUserPassword API docs</a>
            </td>
        </tr>
    </tbody>
</table>

## signon
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>signon</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/UserMgmt/SignonRq</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to sign on to the AllData service. It authenticates the user and generates a session token. The session token can be used for subsequent calls and this token will expire after 18 minutes of ideal time.
                This session token is also required to launch the Add Account widget flow.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/UserMgmt/signon">Signon API docs</a>
            </td>
        </tr>
    </tbody>
</table>


# External FI Seed Data Web services

This chapter lists each External FI Seed Data API in a table with a resource URL, descriptive information, and a link to API Explorer documentation.

## getFinancialInstInfo
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getFinancialInstInfo</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/SeedDataInq/getFinancialInstInfo</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API can be used in three different ways:
                <li>Retrieve FI IDs of all FIs Fiserv supports.</li>
                <li>Retrieve FI details given a set of FI IDs.</li>
                <li>Incremental refresh of FI seed data for a specific timeframe (typically every day).</li><br>
                This API also includes information about the extended services supported in an FI. When clients opt-in to such services, information on FI capability to support the service is included in this API response.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/SeedDataInq/getFinancialInstInfo">getFinancialInstInfo API docs</a>
            </td>
        </tr>
    </tbody>
</table>


## searchFinancialInst
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>searchFinancialInstitution</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/SeedDataInq/searchFinancialInst</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                Provide at least three characters to search for FIs. Valid values for &#39;FIInfoRequired&#39; are:
                <ul>
                    <li>FIIDInfo - retrieves only IDs</li>
                    <li>FIIDInfoList - retrieves list of FIs with detail information</li>
                    <li>FINameInfoList – retrieves list of FIs with just ID and name</li>
                </ul>
                <li>By default, this API returns FI IDs.</li>
                <li>Use the &#39;FISearchSize&#39; parameter to limit the number of result records.</li>
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/SeedDataInq/searchFinancialInst">searchFinancialInstitution API docs</a>
            </td>
        </tr>
    </tbody>
</table>


# External FI Messages Web service

This chapter presents the External FI Messages API in a table with a resource URL, descriptive information, and a link to its API Explorer documentation.

## getFIMessageInfo
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getFIMessageInfo</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/FIMessagesInq/getFIMessageInfo</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used fetch the FI Messages (special instructions) connected to the institutions.It returns a collection of all FIs that have custom messages. Partners use this API on a periodic basis to collect FI specific messages and display them to their customers during the login flow.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/FIMessaegesInq/getFIMessageInfo">getFIMessageInfo API docs</a>
            </td>
        </tr>
    </tbody>
</table>


# Account Management Web Services

This chapter lists each Account Management API in a table with a resource URL, descriptive information, and a link to API Explorer documentation.

## initiateAddAccounts
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>initiateAddAccounts</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountMgmt/initiateAddAccounts</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is also referred to as InitiateAddAccounts or HarvestAddRequest and is used to add new accounts or add more account to existing parent connection.
                <li>Requires FI ID, trust mode, and login parameters as an input.</li><br>
                This request is also used to send in MFA answers using the previously started HarvestID and additional login parameters requested.
                <li>If FI login parameters such as UserID or password are incorrect, error condition will occur with specific error code.</li>
                <li>Returns HarvestID (or run ID): Represents the running session at FI and is required while checking status.</li>
                <li>Returns FILoginAccountID: This identifier uniquely identifies the online login account of the user with an FI.</li>
                <li>If user tries to add the same login name again, &quot;Account already exists&quot; error (4300) occurs.</li><br>
                Refer to the [Add Account workflow (OAuth)](#_Add_Account_workflow_1) when invoking this API for OAuth-enabled FIs.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/initiateAddAccounts">initiateAddAccounts API Docs</a>
            </td>
        </tr>
    </tbody>
</table>


## getAddAccountStatus
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>initiateAddAccounts</td>
        </tr>
        <table>
            <tbody>
                <tr>
                    <td><b>Web Service Name</b></td>
                    <td>getAddAccountStatus</td>
                </tr>
                <tr>
                    <td><b>Resource URL</b></td>
                    <td>&lt;FiservWSUrl&gt;/AccountMgmt/getAddAccountStatus</td>
                </tr>
                <tr>
                    <td><b>Description</b></td>
                    <td>
                        This API is used to poll the status of InitiateAddAccounts.
                        <li>Requires HarvestID and RunID from &#39;initiateAddAccounts&#39; response</li>
                        <li>Response includes status of &quot;completed,&quot; &quot;harvest error,&quot; or &quot;in progress&quot;</li><br>
                        Check status periodically until service successfully harvests accounts, errors out, or times out.<br><br>
                        Handle errors such as login failure (301), multi-factor authentication (303), and wrong MFA answer (304) using the HarvestAddRq-AddMoreAccts element.
                        <br><br>
                        <b>Note:</b> Additional information is needed in case of MFA.
                        <br><br>
                        Refer to the [Add Account workflow (OAuth)](#_Add_Account_workflow_1) when invoking this API for OAuth-enabled FIs.
                    </td>
                </tr>
                <tr>
                    <td><b>API Explorer</b></td>
                    <td>
                        <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getAddAccountStatus">getAddAccountStatus API Docs</a>
                    </td>
                </tr>
            </tbody>
        </table>
    </tbody>
</table>


## getNewAccounts
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getNewAccounts</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountMgmt/getNewAccounts</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is invoked after the initiateAddAccounts request has completed. It returns the list of financial accounts found on the FI website. At this point these accounts are not created in Fiserv database. The basic
                account information available after login to FI site is harvested, but information such as balance, transactions, and positions may not be available.<br/><br/>
                This API also includes account ownership information (full account
                number, account owner name, routing number, and contact information – address, email, and phone number) when clients opt-in for that extended service.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getNewAccounts">getNewAccounts API Docs</a>
            </td>
        </tr>
    </tbody>
</table>



## createAccounts
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>createAccounts</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountMgmt/createAccounts</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                User can choose accounts to add within AllData for harvesting.
                <br><br>
                Fiserv classifies the harvested accounts based on certain account classification rules. The user can either override this classification or provide the classification if not already set.
                <br><br>
                After successfully adding accounts, call update account APIs to run on demand harvesting to retrieve account data.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/createAccounts">createAccounts API docs</a>
            </td>
        </tr>
    </tbody>
</table>

## deleteAccounts
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>deleteAccounts</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountMgmt/deleteAccounts</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to delete either a parent CFI (FILoginAcctId) or a given financial account.
                <li>If a parent CFI ID is given, all associated financial accounts are also deleted.</li>
                <li>All account data including transactions and positions are deleted.</li>
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/deleteAccounts">deleteAccounts API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## updateAccountCredentials
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>updateAccountCredentials</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountMgmt/updateAccountCredentials</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API can be used to update the user credentials information stored within AllData.After updating credentials in AllData, partner should validate the credentials by invoking on-demand account harvesting flow.
                <br><br>
                <b>Note:</b> This API is not applicable for OAuth-enabled FIs.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/updateAccountCredentials">updateAccountCredentials API docs</a>
            </td>
        </tr>
    </tbody>
</table>

## maintainAccount
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>maintainAccount</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountMgmt/maintainAccount</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to modify nickname at Fiserv, account type, and properties such as retirement status.<br>
                Provide extended account type while modifying account type.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/maintainAccount">maintainAccount API Docs</a>
            </td>
        </tr>
    </tbody>
</table>


## addOfflineAccount
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>addOfflineAccount</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/ClientMgmt/addOfflineAccount</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to add offline accounts to client. Offline accounts are tangible assets not held in typical financial institutions. Examples of offline accounts are jewelry, automobiles, boats, RVs, rentals, paintings,
                and heirlooms.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/ClientMgmt/addOfflineAccount">addOfflineAccount API Docs</a>
            </td>
        </tr>
    </tbody>
</table>


## updateOfflineAccount
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>updateOfflineAccount</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/ClientMgmt/updateOfflineAccount</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to modify client offline accounts.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/ClientMgmt/updateOfflineAccount">updateOfflineAccount API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## addUpdateHostAccounts
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>addUpdateHostAccounts</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountMgmt/addUpdateHostAccounts</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API used to add and update client banking/credit card accounts from host FIs.
                <br><br>
                <b>Add:</b> Account number, account name, and account type are mandatory elements to add client host accounts. Balance and transaction
                information are optional elements.
                <br><br>
                <b>Update:</b> The host accounts are identified using either combination of (FIId + AcctNumber) or AcctId in the API request.
                <li>If the combination of (FIId + AcctNumber) or AcctId matches between the AllData and the API request, the API updates the account attributes (balance, account name, transactions) in AllData.</li>
                <li>If the combination of (FIId + AcctNumber) or AcctId does not match, the API adds a new host account in the AllData system.</li>
                <li>The response API will notify the partner about the status of each host account processed as part of the request API.</li>
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/addUpdateHostAccounts">addUpdateHostAccounts API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## HostAccountsStatus
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>HostAccountsStatus</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountMgmt/hostAccountsStatus</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API returns the status of host accounts transaction categorization. Possible return values are:
                <li>InProgress</li>
                <li>Completed</li>
                <li>Failure</li>
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/hostAccountsStatus">hostAccountsStatus API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

# Account Harvesting APIs

This chapter lists each Account Harvesting API in a table with a resource URL, descriptive information, and a link to API Explorer documentation.

## updateAccounts
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>updateAccounts</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/HarvestAccountData/updateAccounts</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API can be used to trigger on
                demand account harvesting. Typically, all added accounts get harvested in nightly batch mode. If a user (or CSR) wants to see latest account data, partners can call this asynchronous API, poll the harvesting status and,
                after it completes, refresh the account data for the user.
                <li>Updates all accounts of a user from list of FIs, all accounts under list of parent CFI IDs, or list of individual accounts</li>
                <li>Update accounts under a single parent CFI ID after the initial add.</li>
                <li>RunID and HarvestID returned in the response are required when checking call status.</li>
                <br>
                This API also provides for requesting extended transactions history of the accounts on demand (any time) for the clients who opt in for
                that service. The request should include the ExtendedTrnsDays element with the number of days expected.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/HarvestAccountData/updateAccounts">updateAccounts API Docs</a>
            </td>
        </tr>
    </tbody>
</table>


## getHarvestStatus
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getHarvestStatus</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/HarvestAccountData/getHarvestStatus</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                The API returns the status of the update done in the updateAccounts API. Requires RunID and HarvestID from the updateAccounts response. This API has the IncludeDetail attribute that returns status based on the values
                passed. The typical values of this tag are &quot;FIStatus&quot; and &quot;AcctStatus.&quot;
                <li>FIStatus: Returns the status of FIs specified in the updateAccounts API</li>
                <li>AcctStatus: Returns the status of accounts of FI. If the above values are not specified, API returns the status of all the accounts specified in the updateAccounts API.</li>
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/HarvestAccountData/getHarvestStatus">getHarvestStatus API Docs</a>
            </td>
        </tr>
    </tbody>
</table>


## getAccountUpdateSummary
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getAccountUpdateSummary</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/Account Data Inquiry Service/getAccountUpdateSummary</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is primarily for clients using widget implementation. After users add accounts using the Add Account widget and the return URL is received from AllData, invoke this API to get the status of accounts update, before invoking the data pull APIs.
                <br><br>
                This API returns status of all accounts updated under that CEUserID within the last hour.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getAccountUpdateSummary">getAccountUpdateSummary API Docs</a>
            </td>
        </tr>
    </tbody>
</table>


# Account Data Pull APIs

This chapter lists each Account Data Pull API in a table with a resource URL, descriptive information, and a link to API Explorer documentation.

## getAccountsSummary
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getAccountsSummary</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountDataInq/getAccountsSummary</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to fetch the account summary information for a specified FI account.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getAccountsSummary">getAccountsSummary API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## getAccountDetails
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getAccountDetails</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountDataInq/getAccountDetails</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API can be used to fetch detail information about one or more FI login accounts of a user.
                <li>It can be used to retrieve information about one or more login accounts.</li>
                <li>If no ID is specified, it returns information about all of user&#39;s FI login accounts.</li>
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getAccountDetails">getAccountDetails API Docs</a>
            </td>
        </tr>
    </tbody>
</table>


## getInvestmentTrans
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getInvestmentTrans</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountDataInq/getInvestmentTrans</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to fetch the transactions for an investment account. It returns the transactions for the specified date range.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getInvestmentTrans">getInvestmentTrans API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## getCreditCardTrans
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getCreditCardTrans</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountDataInq/getCreditCardTrans</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to fetch the transactions for a credit card account. It returns the transactions for the specified date range.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getCreditCardTrans">getCreditCardTrans API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## getBankingTrans
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getBankingTrans</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountDataInq/getBankingTrans</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to fetch the transactions for a banking account. It returns the transactions for the specified date range.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getBankingTrans">getBankingTrans API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## getOtherAccountTrans
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getOtherAccountTrans</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountDataInq/getOtherAccountTrans</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to fetch the transactions for other asset/liability/biller account. It returns the transactions for the specified date range.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getOtherAccountTrans">getOtherAccountTrans API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## getTransactions
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getTransactions</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountDataInq/getTransactions</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used for searching for transactions in specified FI login accounts according to specified search criterion.
                <br><br>
                TrnType can be used to either retrieve debit transactions, credit transactions or both.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getTransactions">getTransactions API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## getDeletedTrans
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getDeletedTrans</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountDataInq/getDeletedTrans</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to get the deleted transactions of all accounts in the home or by account group with a specified date range and row index of the batch.
                <br><br>
                The client/partner accumulating the transaction data in their system
                uses this API to remove the deleted transactions to avoid duplication of transactions in their system.
                <br><br>
                This process is referred to as &quot;transaction deduping&quot; and is performed to keep transaction data in sync between AllData and the FI. It updates the AllData system with any changes found in old transactions posted in the FI source.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getDeletedTrans">getDeletedTrans API Docs</a>
            </td>
        </tr>
    </tbody>
</table>


## getInvestmentPos
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getInvestmentPos</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountDataInq/getInvestmentPos</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to retrieve investment positions for an investment account.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getInvestmentPos">getInvestmentPos API Docs</a>
            </td>
        </tr>
    </tbody>
</table>


## getEmployerStockOptions
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getEmployerStockOptions</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountDataInq/getEmployerStockOptions</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to retrieve the stock options details of an ESOP account.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getEmployerStockOptions">getEmployerStockOptions API Docs</a>
            </td>
        </tr>
    </tbody>
</table>


# Transaction Categorization APIs

Transaction categorization is an optional feature that can be turned on for partners that require AllData to categorize transactions. AllData can only perform this for banking and credit card account types.

This chapter lists each transaction categorization API in a table with a resource URL, descriptive information, and a link to API Explorer documentation. See the AllData Transaction Categorization – API Specification document for more details.

## getTxnCategoriesInfo
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getTxnCategoriesInfo</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/SeedDataInq/getTxnCategoriesInfo</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to get the transactions categories from the AllData system for a specific home.
                <br><br>
                Use Admin User role to get all the standard transaction categories and subcategories AllData provides. Clients must pull this complete standard set initially and later make this request to get any new category added or will be intimated to make this request if any new category added.
                <br><br>
                Use AllData User role to get custom subcategories created by the specific user.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/SeedDataInq/getTxnCategoriesInfo">getTxnCategoriesInfo API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## categorizeTransaction
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>categorizeTransaction</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/TxnMgmt/categorizeTransaction</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to assign/update the subcategory of transactions and apply this user choice to similar transactions in future.
                <br><br>
                This API can also be used to create a user specific custom subcategory and assign it to transactions, and to apply the subcategory to similar future transactions.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/TxnMgmt/categorizeTransaction">CategorizeTransaction API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## deleteSubCategory
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>deleteSubCategory</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/TxnMgmt/deleteSubCategory</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to delete the custom subcategory created by specific user.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/TxnMgmt/categorizeTransaction/deleteSubcategory">DeleteSubCategory API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

# Budgets

This chapter lists each Budget-related API in a table with a resource URL, descriptive information, and a link to API Explorer documentation. Budgets are used in PFM use cases.

## createBudget
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>createBudget</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/BudgetMgmt/createBudget</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to create budgets.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/BudgetMgmt/createBudget">createBudget API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## deleteBudget
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>deleteBudget</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/BudgetMgmt/deleteBudget</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to delete the budgets created by users.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/BudgetMgmt/deleteBudget">deleteBudget API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## editBudget
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>editBudget</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/BudgetMgmt/editBudget</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to edit the existing budgets.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/BudgetMgmt/editBudget">editBudget API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## findBudget
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>findBudget</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/BudgetMgmt/findBudget</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to find the status of user-created budgets and to track their progress.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/BudgetMgmt/findBudget">findBudget API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

# Goals

This chapter lists each Goal-related API in a table with a resource URL, descriptive information, and a link to API Explorer documentation. Goals are used in PFM use cases.

## createGoal
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>createGoal</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/GoalMgmt/createGoal</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to create a goal.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/GoalMgmt/createGoal">createGoal API Docs</a>
            </td>
        </tr>
    </tbody>
</table>


## editGoal
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>editGoal</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/GoalMgmt/editGoal</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to edit the goal, changing its target amount, target date, or the contribution percentage of the account.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/GoalMgmt/editGoal">editGoal API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## deleteGoal
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>deleteGoal</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/GoalMgmt/deleteGoal</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to delete a goal.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/GoalMgmt/deleteGoal">deleteGoal API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

## findGoal
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>findGoal</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/GoalMgmt/findGoal</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to find a goal and to check the status of the goal and the account(s) associated with it.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/GoalMgmt/findGoal">findGoal API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

# Bank Statement Download Services

This chapter lists each Bank Statement Download Services-related API in a table with a resource URL, descriptive information, and a link to API Explorer documentation. AllData can download bank statements for a select list of FIs. By default, AllData can download up to three historical statements once customer consent is received. See the PDF Statement Download Service document for more details.

## bankStatementConsent
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>bankStatementConsent</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountMgmt/bankStatementConsent</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to provide user consent and FI consent to download bank account statements from the financial institution website.
                <li>User consent and FI consent should be provided separately using this API.</li>
                <ul>
                    <li>User consent is the primary consent, which is to be provided first.</li>
                    <li>On providing user consent, AllData will set FI consent to true for all eligible FIs under the user profile.</li>
                </ul>
                <li>FI consent can be revoked in one or more FIs when the user does not wish to obtain statements from any specific FIs.</li>
                <li>FI consent can be provided again for consent-revoked FIs.</li>
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/bankStatementConsent">bankStatementConsent API Docs</a>
            </td>
        </tr>
    </tbody>
</table>


## getBankStmtDetails
<table>
    <tbody>
        <tr>
            <td><b>Web Service Name</b></td>
            <td>getBankStmtDetails</td>
        </tr>
        <tr>
            <td><b>Resource URL</b></td>
            <td>&lt;FiservWSUrl&gt;/AccountDataInq/getBankStmtDetails</td>
        </tr>
        <tr>
            <td><b>Description</b></td>
            <td>
                This API is used to get user and FI consent details and the details of the FIs and bank accounts under the user profile that are eligible for downloading the bank account statements.
                <br><br>
                It also includes the details of account statements available for download under each of those accounts.
                <br><br>
                <b>Note:</b> The availability of statements is dependent on connectivity with the source site. Refer to institution(s) or account(s) errors and resolve any associated issues to get the statements.
            </td>
        </tr>
        <tr>
            <td><b>API Explorer</b></td>
            <td>
                <a href="../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getBankStmtDetails">getBankStmtDetails API Docs</a>
            </td>
        </tr>
    </tbody>
</table>

<br>
<br>
<hr>

© 2019-2021 Fiserv, Inc. or its affiliates. All rights reserved. This work is confidential, and its use is strictly limited. Use is permitted only in accordance with the terms of the agreement under which it was furnished. Any other use, duplication, or dissemination without the prior written consent of Fiserv, Inc. or its affiliates is strictly prohibited. The information contained herein is subject to change without notice. Except as specified by the agreement under which the materials are furnished, Fiserv, Inc. and its affiliates do not accept any liabilities with respect to the information contained herein and are not responsible for any direct, indirect, special, consequential or exemplary damages resulting from the use of this information. No warranties, either express or implied, are granted or extended by this document.

[http://www.fiserv.com](http://www.fiserv.com/)

Fiserv is a registered trademark of Fiserv, Inc.

Other brands and their products are trademarks or registered trademarks of their respective holders and should be noted as such.