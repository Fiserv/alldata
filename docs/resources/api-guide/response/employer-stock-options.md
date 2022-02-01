## Employer Stock Options

| Element / property | Description |
| --- | --- |
| Status | The status of the request. |
| RqUID | The Unique Request Identifier sent by the Server Site. |
| EmployerStockOption | The aggregate containing Stock options details for the requested account.This aggregate will be present only of the request was carried out successfully. |
| FIAcctId | The account identification of the investment FI Account for which the positions are being returned. |
| StockOption | This aggregate containing Stock Options details for the requested account. |
| Symbol | Ticker Symbol of the Company |
| Desc | Name of the Company. |
| Cusip | CUSIP / Asset ID of the Security |
| Price | Price of single stock of the Company |
| MarketValue | Total market value of the Vested Options |
| GrantTypeList | Types of Grant Possible Values: NQO,NQO,NQO,PSU,RSA,RSU |
| Grants | This aggregate containing Grants for the requested account |
| Grant | This aggregate containing the details of a Grant. |
| GrantID | Unique sequence ID for a Grant provided by CashEdge. |
| GrantNo | Unique identifier of the Grant |
| GrantDt | Issue date of Grant. |
| GrantType | Type of Grant. |
| ExcerisePrice | Excerise Price or Strike Price of Grant. |
| FirstVestDt | First Vesting Date of Grant. |
| ExpirationDt | Expiration Date of Grant. |
| SharesGranted | Number of shares in the Grant. |
| VestPeriods | Total number of vesting periods. |
| VestPeriodsPerYr | Number of vesting periods per year. |
| SharesVested | Number of shares vested or excercisable. |

#### Possible Status Code

| StatusCode | Severity | StatusDesc | Condition | Action API Partner should take to resolve the error |
| --- | --- | --- | --- | --- |
| 4070 | Error | Validation failure. | One or more input fields were not valid. | Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD. |
