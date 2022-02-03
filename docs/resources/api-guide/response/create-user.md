## Create User


|Element / property|Description|
|--- |--- |
|RqUID|A Unique Request Identifier.|
|Status|This Aggregate returns the status of the API request.|
|SignonRs|The Admin credentials are received as part of this request.|
|Status|The Status of SignonRs. For StatusCode please refer the signon service|
|UserAddRs|The user info/profile.|
|CEUserID|The CashEdge internal identifier for the added user. This element will be present only if the user registration is successful.|
|Status|The status of the userAdd Request.|

#### Possible Status Code

|StatusCode|Severity|StatusDesc|Condition|Action API Partner should take to resolve the error|
|--- |--- |--- |--- |--- |
|0|Info|Success|The service provider successfully processed the request.||
|100|Error|General Error|There was an error that prevented the service provider from processing the transaction. No additional information is provided.|If this error continues to occur, please reach out to us the timestamp and CEUserId.|
|4012|Error|Invalid HomeID|The HomeID specified in the request is invalid.||
|4030|Error|User already registered|The user identified by partnerID:HomeID:UserID is already registered with the Server.||
|4031|Error|Partner client identifier already registered|test||
|4050|Error|Invalid encryption scheme|The encryption scheme specified in the request is not valid.||
|4070|Error|Validation failure|One or more input fields were not valid.|Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD.|
|4623|Error|Day Phone is required|DayPhone is madatory if the partner home is configured.||
|4626|Error|Total phone numbers do not match allowed occurrence|Telephone numbers provided should not exceed the configured occurance value.||
|4628|Error|Invalid Email Address|E-Mail address provided in the request is invalid||
|4629|Error|Only one day phone value is allowed|DayPhone is provided more than once.||
|4630|Error|Only one evening phone value is allowed|EveningPhone is provided more than once.||
|7621|Error|Entitlement parameters are invalid|||
