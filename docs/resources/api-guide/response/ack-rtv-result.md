## AckRTV Result


|Element / property|Description|
|--- |--- |
|RqUID|Unique Request Identifier. The Client Site sends this element with the request.|
|Status|The status of the request.|
|SignonRs|Data containing signon information to login to Fiserv System, it is used to authenticate user to access service.|
|AckRTVResultRs|Response container for the RTV Result.|
|Status|The status of the request|

#### Possible Status Code

|StatusCode|Severity|StatusDesc|Condition|Action API Partner should take to resolve the error|
|--- |--- |--- |--- |--- |
|0|Info|Success|The service provider successfully processed the request.||
|100|Error|General Error|There was an error that prevented the service provider from processing the transaction. No additional information is provided.|If this error continues to occur, please reach out to us the timestamp and CEUserId.|
|4070|Error|Validation failure|One or more input fields were not valid.|Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD.|


