## Sign On


|Element / property|Description|
|--- |--- |
|RqUID|A Unique Request Identifier.|
|Status|This Aggregate returns the status of the API.|
|SignonRs|This Aggregate will return the following. CEUserID : This is the CashEdge internal identifier for a user.  SessInfo: A security token that can be used to identify a signed-on user.|
|Status|The status of the SigonRq|

#### Possible Status Code

|StatusCode|Severity|StatusDesc|Condition|Action API Partner should take to resolve the error|
|--- |--- |--- |--- |--- |
|0|Info|Success|The service provider successfully processed the request.||
|100|Error|General Error|There was an error that prevented the service provider from processing the transaction. No additional information is provided.|If this error continues to occur, please reach out to us the timestamp and CEUserId.|
|1740|Error|Authentication Failed|The customer could not be authenticated due to an incorrect HomeID, login ID or password.|Verify the user Login ID and Password|
|4010|Error|Invalid partnerID|The partnerID specified in the request is invalid.||
|4012|Error|Invalid HomeID|The HomeID specified in the request is invalid.||
|4040|Error|User not registered|The user identified by partnerID:HomeID:UserID is not registered with the &lt;Server&gt;.||
|4050|Error|Invalid encryption scheme|The encryption scheme specified in the request is not valid.||
|4070|Error|Validation failure|One or more input fields were not valid.|Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD.|
|4080|Error|SessInfo creation failed|The CashEdge Server was unable to create a SessInfo security token.|Partner should reinvoke the SingOn API to get the new SessInfo token. If the error continues to occur, please reach out to us with the timestamp and CEUserId.|
|4090|Error|Invalid SessInfo|The SessInfo token specified in the request is invalid.|This error occurs when the Session token is invalid or expired; Partner should generate a new SessInfo token and reinvoke the process flow with the new SessInfo token. If the error continues to occur, please reach out to us with the timestamp and CEUserId.|
|4130|Error|Invalid role|A signon has been attempted by a user in a role other than the authorized role.||
|4710|Error|User is locked|The requested user is locked in CashEdge system.|Partner has to reach out to us with CEUserID.|
|7611|Error|Request originated from an invalid IP address|Requested orinigated from unknown (not whitelisted) ip address.|Partner has to reach out to us to whitelist any new IP's or make the request from whitelisted Ips.|
