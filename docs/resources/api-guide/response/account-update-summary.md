## Account Update Summary

| Element / property | Description |
| --- | --- |
| Status | The status of the request. |
| RqUID | The Unique Request Identifier sent by the Server Site. |
| AccountUpdateSummaryRs | The aggregate containing recent account update details for the requested account.This aggregate will be present only of the request was carried out successfully. |
| FILoginAcctId | FI Login identifier. |
| FIId | FI Identifier. |
| LastUpdateAttempt | Time of last account details update. |
| AcctId | Account Identifier. |
| AcctNickName | Account nickname for AcctID. |
| LastUpdateSuccess | Last successful update time. |
| LastUpdateStatusCode | Last update status code. |

#### Possible Status Code

| StatusCode | Severity | StatusDesc | Condition | Action API Partner should take to resolve the error |
| --- | --- | --- | --- | --- |
| 4070 | Error | Validation failure. | One or more input fields were not valid. | Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD. |
