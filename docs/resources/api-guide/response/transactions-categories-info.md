## Transactions Categories Info


|Element / property|Description|
|--- |--- |
|RqUID|A Unique Request Identifier.|
|Status|This Aggregate returns the status of the API request.|
|SignonRs|The Admin credentials are received as part of this request.|
|Status|The status of the Transcation Categories Info Request.|
|CategoryRec|This Aggregate will return the Category Name and ID for transactions.|
|Name|This Aggregate will return the Category Name for transaction|
|Id|This Aggregate will return the Category Id for transaction  This element will be present only if the user registration is successful.|
|Status|The status of the TransCategoriesInfoRq|

#### Possible Status Code

|StatusCode|Severity|StatusDesc|Condition|Action API Partner should take to resolve the error|
|--- |--- |--- |--- |--- |
|0|Info|Success|The service provider successfully processed the request.||
|100|Error|General Error|There was an error that prevented the service provider from processing the transaction. No additional information is provided.|If this error continues to occur, please reach out to us the timestamp and CEUserId.|
|1740|Error|Authentication Failed|The customer could not be authenticated due to an incorrect HomeID, login ID or password.|Verify the user Login ID and Password|
|4060|Info|No data available|No data is available to satisfy the request. This could mean that the user has not registered any financial institutions; there are no transactions in the specified date range and so on.|If this error occurs on invoking getAccountDetails API, it means user has not added any financial institutions. Please make sure user has added accounts successfully under any financial institutions. If this error occurs on invoking transactions APIs, make sure the 'LastSuccessfulUpdate' date is returned in getAccountDetails API for that specific account and the specified date range lies within the 'LastSuccessfulUpdate' date.|
