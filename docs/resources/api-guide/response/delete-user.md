## Delete User


|Element/property|Description|
|--- |--- |
|Status|The status of the request.|
|RqUID|A Unique Request Identifier.|
|Status|The status of the Delete user|

#### Possible Status Code

|StatusCode|Severity|StatusDesc|Condition|Action API Partner should take to resolve the error|
|--- |--- |--- |--- |--- |
|0|Info|Success|The service provider successfully processed the request.||
|100|Error|General Error|There was an error that prevented the service provider from processing the transaction. No additional information is provided.|If this error continues to occur, please reach out to us the timestamp and CEUserId.|
|4012|Error|Invalid HomeID|The HomeID specified in the request is invalid.||
|4040|Error|User not registered|The user identified by partnerID:HomeID:UserID is not registered with the &lt;Server&gt;.||
|4070|Error|Validation failure|One or more input fields were not valid.|Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD.|
|4350|Error|Update in progress.|An update is already in progress. A new update (update or add) request cannot be submitted unless the existing update is completed.|Partner should not invoke update or add until the existing update completes.|
