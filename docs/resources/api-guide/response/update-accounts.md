## Update Accounts


|Element/property|Description|
|--- |--- |
|Status|The status of the request.|
|RqUID|The Unique Request Identifier sent by the Server Site.|
|HarvestID|The identification assigned to the Harvest request.This element will be present only if the request was carried out successfully.|

#### Possible Status Code

|StatusCode|Severity|StatusDesc|Condition|Action API Partner should take to resolve the error|
|--- |--- |--- |--- |--- |
|4350|Error|Update in progress.|An update is already in progress. A new update (update or add) request cannot be submitted unless the existing update is completed.|Partner should not invoke update or add until the existing update completes.|
|4351|Error|Update in progress.|An add or update is already in progress. A new update (update or add) request cannot be submitted unless the existing update is completed.|Partner should not invoke update or add until the existing update completes.|
|4070|Error|Validation failure|One or more input fields were not valid.|Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD.|
|4140|Error|Invalid FILoginAcctId|The FILoginAcctId specified in the request is invalid.||
|4480|Error|Invalid AcctId|The AcctId in the request does not match with any accounts for this user or is null.||
|4430|Error|No accounts found or accounts not eligible for update|The query criteria did not match with any accounts in the database eligible for update.|This error occurs when updateAccounts API is invoked for a User with no eligible accounts to update. That is the user is having all the accounts with invalid credentials error or accounts exist under the suspended FI's. Partner has to confirm that the User has eligible accounts for update.|
|4060|Info|No data available|No data is available to satisfy the request. This could mean that the user has not registered any financial institutions; there are no transactions in the specified date range and so on.|If this error occurs on invoking getAccountDetails API, it means user has not added any financial institutions. Please make sure user has added accounts successfully under any financial institutions. If this error occurs on invoking transactions APIs, make sure the 'LastSuccessfulUpdate' date is returned in getAccountDetails API for that specific account and the specified date range lies within the 'LastSuccessfulUpdate' date.|
|100|Error|General Error|There was an error that prevented the service provider from processing the transaction. No additional information is provided.|If this error continues to occur, please reach out to us the timestamp and CEUserId.|
|4470|Error|The FI Login Account is suspended and cannot be updated. Please update the login information to resume aggregation activity.|When harvesting account login credentials are rejected, the Account Harvest Status (AcctHarvestStatus) is disabled. The FI Login Account Info List Inquiry Request (FILoginAcctInfoListInqRq) will return this error message in the response (FILoginAcctInfoListInqRs) to convey that the account is locked in the CashEdge system and the institution login credentials should be corrected.||
