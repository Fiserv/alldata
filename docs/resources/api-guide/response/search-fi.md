## Search FI


|Element / property|Description|
|--- |--- |
|RqUID|Unique Request Identifier. The Client Site sends this element with the request.|
|Status|The status of the request.|
|SignonRs|Data containing signon information to login to Fiserv System, it is used to authenticate user to access service.|
|FISearchRs|Response container for the FI information.|
|RunId|Harvest request identification.|
|FIVerifyInfoList|An array list of fiverifyInfo, such as FIID, FIURL, loginParams, oauthFI.|
|LoginParams|contains the FI login parameters details.|
|Status|The status of the request|

#### Possible Status Code

|StatusCode|Severity|StatusDesc|Condition|Action API Partner should take to resolve the error|
|--- |--- |--- |--- |--- |
|0|Info|Success|The service provider successfully processed the request.||
|100|Error|General Error|There was an error that prevented the service provider from processing the transaction. No additional information is provided.|If this error continues to occur, please reach out to us the timestamp and CEUserId.|
|4070|Error|Validation failure|One or more input fields were not valid.|Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD.|
|4360|Error|Incomplete Data For Request Completion.|The data provided for Request completion is not sufficient. Typically insufficient data provided for “add new accounts”, “add more accounts” or “account maintenance” operations and so on.||
|4060|Info|No data available|No data is available to satisfy the request. This could mean that the user has not registered any financial institutions; there are no transactions in the specified date range and so on.|If this error occurs on invoking getAccountDetails API, it means user has not added any financial institutions. Please make sure user has added accounts successfully under any financial institutions. If this error occurs on invoking transactions APIs, make sure the 'LastSuccessfulUpdate' date is returned in getAccountDetails API for that specific account and the specified date range lies within the 'LastSuccessfulUpdate' date.|



