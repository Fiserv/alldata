## Verify Account


|Element / property|Description|
|--- |--- |
|RqUID|Unique Request Identifier. The Client Site sends this element with the request.|
|Status|The status of the request.|
|SignonRs|Data containing signon information to login to Fiserv System, it is used to authenticate user to access service.|
|RunId|Harvest request identification.|
|OAuthInfo|This will give the OAuth related info.|
|Status|The status of the request|

#### Possible Status Code

|StatusCode|Severity|StatusDesc|Condition|Action API Partner should take to resolve the error|
|--- |--- |--- |--- |--- |
|0|Info|Success|The service provider successfully processed the request.||
|100|Error|General Error|There was an error that prevented the service provider from processing the transaction. No additional information is provided.|If this error continues to occur, please reach out to us the timestamp and CEUserId.|
|4070|Error|Validation failure|One or more input fields were not valid.|Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD.|
|4322|Error|Avoid gathering and sending User Credentials in OAuth FI|||
|4322|Error|Avoid gathering and sending User Credentials in OAuth FI|||
|4460|Error|FI Login Credential exceed max length|Login credentials entered by user exceed the maximum length allowed at financial institution web site||
|4320|Error|FI Login Credentials Required.|Error code generated when an “add more account” operation is attempted for a low trust account without providing the login credentials.||
|7619|Error|FI Not Available.|||



