## Tax Statement Consent

| Element / property | Description |
| --- | --- |
| RqUID | Unique Request Identifier. The Client Site sends this element with the request. |
| Status | This aggregate provides the status of the API request |
| SignonRs | This aggregate provides the status of the user authentication status. |

#### Possible Status Code

| StatusCode | Severity | StatusDesc | Condition | Action API Partner should take to resolve the error |
| --- | --- | --- | --- | --- |
| 0 | Info | Success | The service provider successfully processed the request. |  |
| 100 | Error | General Error | There was an error that prevented the service provider from processing the transaction. No additional information is provided. | If this error continues to occur, please reach out to us the timestamp and CEUserId. |
| 1089 | Error | Tax Statement download not enabled at home. |  |  |
| 4130 | Error | Invalid role | A signon has been attempted by a user in a role other than the authorized role. |  |
