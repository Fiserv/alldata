## Update Account Credentials

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
| 4360 | Error | Incomplete Data For Request Completion. | The data provided for Request completion is not sufficient. Typically insufficient data provided for “add new accounts”, “add more accounts” or “account maintenance” operations and so on. |  |
| 4100 | Error | Invalid FIAcctId | The FIAcctId specified in the request is invalid. |  |
| 4321 | Error | Invalid FI Login parameter id provided | Error code generated when an “add more account” operation is attempted for a low trust account without providing the login credentials. |  |
| 4460 | Error | FI Login Credential exceed max length | Login credentials entered by user exceed the maximum length allowed at financial institution web site |  |
| 4320 | Error | FI Login Credentials Required. | Error code generated when an “add more account” operation is attempted for a low trust account without providing the login credentials. |  |
| 7619 | Error | FI Not Available. |  |  |
| 4300 | Error | Account Already Exists | Attempt to add an account that already exists. | When user attempts to add the same set of FI credentials which already exists in the system. |
| 5012 | Error | Trust mode cannot be changed, as only high trust mode is allowed. | Trust mode cannot be changes if the partner home is configured for high trust mode only. | This error occurs when Partner sends other trust modes (Low or Medium trust) when editing the FI credentials |
