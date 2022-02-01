## Categorize Transaction


|Element / property|Description|
|--- |--- |
|RqUID|A Unique Request Identifier.|
|Status|This Aggregate returns the status of the API request.|
|SignonRs|The Admin credentials are received as part of this request.|
|Status|The status of the Transscation Categories Info Request.|
|TransID|Unique Transaction Identifier.|
|Status|The status of the UpdateTxnCategoryRq|

#### Possible Status Code

|StatusCode|Severity|StatusDesc|Condition|Action API Partner should take to resolve the error|
|--- |--- |--- |--- |--- |
|0|Info|Success|The service provider successfully processed the request.||
|100|Error|General Error|There was an error that prevented the service provider from processing the transaction. No additional information is provided.|If this error continues to occur, please reach out to us the timestamp and CEUserId.|
|1740|Error|Authentication Failed|The customer could not be authenticated due to an incorrect HomeID, login ID or password.|Verify the user Login ID and Password|
|4060|Info|No data available|No data is available to satisfy the request. This could mean that the user has not registered any financial institutions; there are no transactions in the specified date range and so on.|If this error occurs on invoking getAccountDetails API, it means user has not added any financial institutions. Please make sure user has added accounts successfully under any financial institutions. If this error occurs on invoking transactions APIs, make sure the 'LastSuccessfulUpdate' date is returned in getAccountDetails API for that specific account and the specified date range lies within the 'LastSuccessfulUpdate' date.|
|4070|Error|Validation failure|One or more input fields were not valid.|Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD.|
|4100|Error|Invalid FIAcctId|The FIAcctId specified in the request is invalid.||
|4110|Error|AcctType mismatch|The account specified in the request is not of the type specified in the request.|Partner can verify AcctType of the accounts from getAccountDetails API response.|
|4120|Error|Not owner|The FILogin Account or FI Account for which a user is requesting details does not belong to the user.|This error occurs when FILoginAcctId or AcctId sent by Partner doesnot exist in our system or the Id's belong to a different CEUserID in the system.|
|4150|Error|Invalid AcctType|The AcctType or ExtAcctType specified in the request are invalid.|Partner can verify AcctType or ExtAcctType of the accounts from getAccountDetails API response.|
|7601|Error|Validation failure|Transaction Not Found.||
|7602|Error|Validation failure|Category Not Found.||
|7603|Error|Validation failure|SubCategory Not Found.||
|7604|Error|Validation failure|Subcategory already exists.||
|7610|Error|Validation failure|Invalid account Type.||
|7611|Error|Request originated from an invalid IP address|Requested orinigated from unknown (not whitelisted) ip address.|Partner has to reach out to us to whitelist any new IP's or make the request from whitelisted Ips.|
