## FI Message Info


|Element / property|Description|
|--- |--- |
|RqUID|A Unique Request Identifier.|
|Status|This Aggregate returns the status of the API request.|
|SignonRs|The Admin credentials are received as part of this request.|
|Status|The Status of SignonRs. For StatusCode please refer the signon service|
|FIMessagesRs|This aggregate provides the status of the FIMessageRq status.|
|Status|Status of the FIMessage response.|
|Message|FI Messages are special instructions provided for users to follow to aggregate information from that FI.|
|UpdateErrorCode|Harvesting error code in which instance, this message to be shared with User. Possible values: ERRORRESOLUTION(ErrorBasedAlerts) ADDACCOUNTS(AddAccountMessages) INFORMATIVE(InformativeAlerts)|
|MessageType|Contains message type information.|
|StartDate|Starting date to share this message with User.|
|EndDate|End date to stop sharing this message with User.|

#### Possible Status Code

|StatusCode|Severity|StatusDesc|Condition|Action API Partner should take to resolve the error|
|--- |--- |--- |--- |--- |
|0|Info|Success|The service provider successfully processed the request.||
|100|Error|General Error|There was an error that prevented the service provider from processing the transaction. No additional information is provided.|If this error continues to occur, please reach out to us the timestamp and CEUserId.|
|1740|Error|Authentication Failed|The customer could not be authenticated due to an incorrect HomeID, login ID or password.|Verify the user Login ID and Password|
|4130|Error|Invalid role|A signon has been attempted by a user in a role other than the authorized role.||
|4012|Error|Invalid HomeID|The HomeID specified in the request is invalid.||
|4010|Error|Invalid partnerID|The partnerID specified in the request is invalid.||
