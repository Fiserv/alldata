## Other Account Transactions

| Element / property | Description |
| --- | --- |
| Status | The status of the request. |
| RqUID | The Unique Request Identifier sent by the Server Site. |
| OtherAcctTrns | Other account transactions. |
| FIAcctId | The account identification of the Other account for which transactions are being returned. |
| SelectionCriterion | The selection range as per which transactions were requested. |
| BankAcctTrnRec | Other account type transaction record. |
| TrnID | Unique transaction ID assigned by CashEdge. |
| FITrnID | Unique transaction ID assigned by the Financial Institution from where this transaction was fetched, if available from the Financial Institution. |
| Status | The status of the OtherAcctTrnInqRq |

#### Possible Status Code

| StatusCode | Severity | StatusDesc | Condition | Action API Partner should take to resolve the error |
| --- | --- | --- | --- | --- |
| 0 | Info | Success | The service provider successfully processed the request.|
| 100 | Error | General Error | There was an error that prevented the service provider from processing the transaction. No additional information is provided. If this error continues to occur, please reach out to us the timestamp and CEUserId.
| 1740 | Error | Authentication Failed | The customer could not be authenticated due to an incorrect HomeID, login ID or password. Verify the user Login ID and Password |
| 4070 | Error | Validation failure | One or more input fields were not valid. | Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD. |
| 4130 | Error | Invalid role | A signon has been attempted by a user in a role other than the authorized role.|
| 4550 | Error | Invalid Date Range | This error occurs when Start Date specified in the date range greater than current Date. | |
| 4551 | Error | Invalid date range. Modified date range returned no results. | This error occurs when Start Date specified in the date range greater than current Date. | Default maximum range is 90 days; this is configurable for the Partner requesting extended transaction history. |
