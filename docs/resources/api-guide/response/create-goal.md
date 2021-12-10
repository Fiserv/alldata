## Create Goal


|Element / property|Description|
|--- |--- |
|RqUID|A Unique Request Identifier.|
|Status|This Aggregate returns the status of the API.|
|SignonRs|This aggregate provides the status of the user authentication status.|
|GoalId|The identification assigned to the Goal request. This element will be present only of the request that was carried out successfully.|
|StatusCode|The status code for the response.A code of “0” implies complete success. Other status codes could mean either success or failure depending upon the value of the severity of the code.|
|Severity|Conveys the status of the request, whether the request was processed successfully, or whether some error was encountered while processing the request.|
|StatusDesc|A meaningful message that conveys the nature of the error, if any.|
|CEUserID|The CashEdge internal identifier for the added user.|
|Status|The status of the GoalAddRq|


#### Possible Status Code

|StatusCode|Severity|StatusDesc|Condition|Action API Partner should take to resolve the error|
|--- |--- |--- |--- |--- |
|0|Info|Success|The service provider successfully processed the request.||
|100|Error|General Error|There was an error that prevented the service provider from processing the transaction. No additional information is provided.|If this error continues to occur, please reach out to us the timestamp and CEUserId.|
|1008|Error|Validation failed|Goal Amount should be positive.||
|1740|Error|Authentication Failed|The customer could not be authenticated due to an incorrect HomeID, login ID or password.|Verify the user Login ID and Password|
|4070|Error|Validation failure|One or more input fields were not valid.|Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD.|
|4130|Error|Invalid role|A signon has been attempted by a user in a role other than the authorized role.||
|5160|Error|Validation failure|Goal Name must be greater than 5 characters.||
|5140|Error|Validation failure|Duplicate goal name is found.||
|5130|Error|Validation failure|You may not allocate more than 100% of a linked account across all goals.||
|5110|Error|Validation failure|Invalid AccountId||
|5120|Error|Validation failure|Target End Date must be a future date||
