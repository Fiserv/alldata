## Investment Positions


| Element / property | Description |
| --- | --- |
| Status | The status of the request. |
| RqUID | The Unique Request Identifier sent by the Server Site. |
| InvAcctPos | The aggregate containing investment positions for the requested account.This aggregate will be present only of the request was carried out successfully. |
| FIAcctId | The account identification of the investment FI Account for which the positions are being returned. |
| DtAsOf | Date at which the securities position was retrieved. |
| InvPos | The aggregate containing investment positions for the requested account. |
| MFPos | The aggregate containing positions in mutual funds for the requested account. |
| PosID | Identifier for the security. CashEdge will attempt to provide a unique identifier for each security, provided the underlying FI provides a mechanism for uniquely identifying a security. |
| SecInfo | Information about the security. |
| SecPos | Position and value of the security. |
| ACHIndicator | Indicates whether this holding is ACH enabled. Possible values: Enabled Disabled |
| Ticker | Ticker symbol of the Security. |
| SecDesc | Full name of the Security. |
| Marginable | Indicates whether this holding is Marginable Possible values: Y ,N |
| SecurityType | Type of Security |
| Property | Miscellaneous properties associated with this position. Possible values: AssetID AssetIDType |
| UnitPrice> | Price of single security. |
| Units | Number of securities. |
| MarketValue | Total market value of this security. |
| CostBasis | |
| Status | The status of the InvAcctPosInqRq |

#### Possible Status Code

| StatusCode | Severity | StatusDesc | Condition | Action API Partner should take to resolve the error |
| --- | --- | --- | --- | --- |
| 0 | Info | Success | The service provider successfully processed the request. | | 
| 100 | Error | General Error | There was an error that prevented the service provider from processing the transaction. No additional information is provided. | If this error continues to occur, please reach out to us the timestamp and CEUserId. |
| 1740 | Error | Authentication Failed | The customer could not be authenticated due to an incorrect HomeID, login ID or password. | Verify the user Login ID and Password. |
| 4070 | Error | Validation failure | One or more input fields were not valid. | Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD. |
| 4130 | Error | Invalid role | A signon has been attempted by a user in a role other than the authorized role. | |
