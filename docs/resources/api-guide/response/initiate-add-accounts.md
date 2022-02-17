## Initiate Add Accounts

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
| 4471 | Error | This account is at a financial institution that is not supported within this service. The account data will not be able to be refreshed/updated because support is now unavailable for this financial institution. | The partner should expect to receive status code 4471 in the status aggregate when attempting to update an account or add more accounts to an FI that is suspended in CashEdge. Partners should handle this error code and inform the customers that the FI is temporarily unavailable. |  |
| 4320 | Error | FI Login Credentials Required. | Error code generated when an “add more account” operation is attempted for a low trust account without providing the login credentials. |  |
| 4321 | Error | Invalid FI Login parameter id provided | Error code generated when an “add more account” operation is attempted for a low trust account without providing the login credentials. |  |
| 4350 | Error | Update in progress. | An update is already in progress. A new update (update or add) request cannot be submitted unless the existing update is completed. | Partner should not invoke update or add until the existing update completes. |
| 4460 | Error | FI Login Credential exceed max length | Login credentials entered by user exceed the maximum length allowed at financial institution web site |  |
| 7619 | Error | FI Not Available. |  |  |
| 5011 | Error | AdvAccess value specified in the request is invalid |  |  |
| 4360 | Error | Incomplete Data For Request Completion. | The data provided for Request completion is not sufficient. Typically insufficient data provided for “add new accounts”, “add more accounts” or “account maintenance” operations and so on. |  |
| 4470 | Error | The FI Login Account is suspended and cannot be updated. Please update the login information to resume aggregation activity. | When harvesting account login credentials are rejected, the Account Harvest Status (AcctHarvestStatus) is disabled. The FI Login Account Info List Inquiry Request (FILoginAcctInfoListInqRq) will return this error message in the response (FILoginAcctInfoListInqRs) to convey that the account is locked in the CashEdge system and the institution login credentials should be corrected. |  |
