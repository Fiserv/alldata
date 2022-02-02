## Credit Card Transactions

| Element / property | Description |
| --- | --- |
| Status | The status of the request. |
| RqUID | The Unique Request Identifier sent by the Server Site. |
| CCAcctTrns | Credit card transactions. |

#### Possible Status Code

| StatusCode | Severity | StatusDesc | Condition | Action API Partner should take to resolve the error |
| --- | --- | --- | --- | --- |
| 4070 | Error | Validation failure | One or more input fields were not valid. | Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD. |
| 4550 | Error | Invalid Date Range | This error occurs when Start Date specified in the date range greater than current Date. | |
| 4551 | Error | Invalid date range. Modified date range returned no results. | This error occurs when Start Date specified in the date range greater than current Date. | Default maximum range is 90 days; this is configurable for the Partner requesting extended transaction history. |
