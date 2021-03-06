## Create Accounts

| Element / property | Description |
| --- | --- |
| RqUID | Unique Request Identifier. The Client Site sends this element with the request. |
| Status | The status of the request. |
| HarvestID | The identification assigned to the Harvest request. This element will be present only of the request that was carried out successfully |
| FILoginAcctId | FI Login identifier. This element will be present only of the request that was carried out successfully. |
| FIUserLoginAcctInfo | In response to an add account, record is inserted. This aggregate contains that information. When the HarvestAddRq is sent with the “AddNewAccts” aggregate, the “ClassifiedStatus” of this aggregate will be “Pending”. When the HarvestAddRq is sent with the “AddMoreAccts” aggregate, the “ClassifiedStatus” of this aggregate will reflect the value stored in the data store. This element will be present only of the request that was carried out successfully. |

#### Possible Status Code

| StatusCode | Severity | StatusDesc | Condition | Action API Partner should take to resolve the error |
| --- | --- | --- | --- | --- |
| 0 | Info | Success | The service provider successfully processed the request. |  |
| 100 | Error | General Error | There was an error that prevented the service provider from processing the transaction. No additional information is provided. | If this error continues to occur, please reach out to us the timestamp and CEUserId. |
| 4070 | Error | Validation failure | One or more input fields were not valid. | Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD. |
| 4370 | Error | Invalid account classification attributes. | The account attributes combinations required for account classification is invalid. • Instrument • AccountOwnership • RetirementStatus | Partner has to reach out to us with CEUserID and timestamp. |
