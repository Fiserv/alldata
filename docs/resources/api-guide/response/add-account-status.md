## Add Account Status

| Element / property | Description |
| --- | --- |
| RqUID | Unique Request Identifier. The Client Site sends this element with the request. |
| Status | The status of the request. |
| HarvestID | The identification assigned to the Harvest request.<br>This element will be present only of the request that was carried out successfully |
| FILoginAcctId | FI Login identifier.<br>This element will be present only of the request that was carried out successfully. |
| FIUserLoginAcctInfo | In response to an add account, record is inserted. This aggregate contains that information.<br>When the HarvestAddRq is sent with the “AddNewAccts” aggregate, the “ClassifiedStatus” of this aggregate will be “Pending”.<br>When the HarvestAddRq is sent with the “AddMoreAccts” aggregate, the “ClassifiedStatus” of this aggregate will reflect the value stored in the data store.<br>This element will be present only of the request that was carried out successfully. |

#### Possible Status Code

| StatusCode | Severity | StatusDesc | Condition | Action API Partner should take to resolve the error |
| --- | --- | --- | --- | --- |
| 0 | Info | Success | The service provider successfully processed the request. |  |
| 100 | Error | General Error | There was an error that prevented the service provider from processing the transaction. No additional information is provided. | If this error continues to occur, please reach out to us the timestamp and CEUserId. |
| 4070 | Error | Validation failure | One or more input fields were not valid. | Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD. |
| 7701 | Error | This financial institution requires a multi-factor authentication process that is currently not enabled for this home | The financial institution requires an image based multi-factor authentication that is currently not enabled for this home. Please reach out to CashEdge team for enabling the same. |  |
| 4310 | Error | Harvesting Error | Error code generated if there is a harvesting error during 'account add' or 'add more accounts' operations. | Partner should refer the returned 'UpdateErrorCode' in the Harvester Error Codes section of this document for corresponding action. |
