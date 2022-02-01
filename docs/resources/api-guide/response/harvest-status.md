## Harvest Status


|Element/property|Description|
|--- |--- |
|RqUID|A Unique Request Identifier.|
|Status|The status of the request.|
|HarvestSts|The status of the Harvest operation. This aggregate will be present only of the request was carried out successfully.|
|UpdateErrorCode|This element (representing 3 digit error code) will be present if there is a harvesting error that prevents the account from being added|
|FILoginParamList|This aggregate will be present if FI throws challenge questions while trying to login to FI site.|

#### Possible Status Code

|StatusCode|Severity|StatusDesc|Condition|Action API Partner should take to resolve the error|
|--- |--- |--- |--- |--- |
|4070|Error|Validation failure|One or more input fields were not valid.|Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD.|
|4310|Error|Harvesting Error|Error code generated if there is a harvesting error during 'account add' or 'add more accounts' operations.|Partner should refer the returned 'UpdateErrorCode' in the Harvester Error Codes section of this document for corresponding action.|
|7701|Error|This financial institution requires a multi-factor authentication process that is currently not enabled for this home|The financial institution requires an image based multi-factor authentication that is currently not enabled for this home. Please reach out to CashEdge team for enabling the same.||
