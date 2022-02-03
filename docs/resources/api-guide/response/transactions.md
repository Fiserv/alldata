## Transactions

| Element / property | Description |
| --- | --- |
| Status | The status of the request. |
| RqUID | The Unique Request Identifier sent by the Server Site. |
| FIAcctTrnsList | Transactions corresponding to the search criterion specified in the request. |
| SearchCriterion | The search criterion specified in the request. |
| FILoginAcctTrns | Transactions corresponding to the search criterion in all accounts across all specified FI Login Accounts. |
| FILoginAcctId | Identifies the FI Login Account. |
| FIInfo | FI Information for the FI Login Account. |
| FIAcctTrns | Transactions corresponding to the search criterion specified in the request for all FI Account in this FI Login Account. |

#### Possible Status Code

| StatusCode | Severity | StatusDesc | Condition | Action API Partner should take to resolve the error |
| --- | --- | --- | --- | --- |
| 4070 | Error | Validation failure | One or more input fields were not valid. | Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD. |
| 4012 | Error | Invalid HomeID | The HomeID specified in the request is invalid. | |
| 4280 | Error | Invalid search criterion | Search criterion specified in the request is invalid. | |
| 4250 | Error | Invalid transaction type | Transaction type specified in the request is invalid. | Expected transaction type is Debit or Credit. Anything mentioned other than Debit or Credit will return this error. |
| 4260 | Error | Invalid date | Date specified in the request is invalid. | |
| 4270 | Error | Invalid amount | Amount specified in the request is invalid. | |