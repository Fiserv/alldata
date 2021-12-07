## Bank Statement Details

| Element / property | Description |
| --- | --- |
| RqUID | Unique Request Identifier. The Client Site sends this element with the request. |
| Status | The status of the request. |
| UserBankStmtConsent | Contains the user opt-in / opt-out status for Bank statement. |
| FIAcctsStmtList | List of FILoginAcctIds and their corresponding consent status is provided. |
| AcctBankStmtDetail | Provides AcctID and corresponding bank statement document lists. |

#### Possible Status Code

| StatusCode | Severity | StatusDesc | Condition | Action API Partner should take to resolve the error |
| --- | --- | --- | --- | --- |
| 0 | Info | Success | The service provider successfully processed the request. | | 
| 100 | Error | General Error | There was an error that prevented the service provider from processing the transaction. No additional information is provided. | If this error continues to occur, please reach out to us the timestamp and CEUserId. |
| 1087 | Error | Bank Statement download not enabled at home. | This service is disabled for the requested home. Contact FISERV for enabling the property. | |
| 1740 | Error | Authentication Failed | The customer could not be authenticated due to an incorrect HomeID, login ID or password. | Verify the user Login ID and Password. |
| 4070 | Error | Validation failure | One or more input fields were not valid. | Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD. |
| 4130 | Error | Invalid role | A signon has been attempted by a user in a role other than the authorized role. | |
