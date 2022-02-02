## Update Offline Account

| Element / property | Description |
| --- | --- |
| Status | The status of the request. |
| RqUID | Unique Request Identifier. The Client Site sends this element with the request. |

#### Possible Status Code

| StatusCode | Severity | StatusDesc |
| --- | --- | --- |
|4480| Error |Invalid AcctId.|
|4490| Error |Invalid Advisor Access Tag.|
|5013| Error |Invalid AcctName.|
|5011| Error |AdvAccess value specified in the request is invalid.|
|4510| Error |Invalid Account Type Id.|
|4270| Error |Invalid Amount.|
|2740| Error |Invalid currency code.|
|4530| Error |Invalid Balance Type.|
|4260| Error |Invalid Date.|
|4370| Error |Invalid combinations for Account attributes, which are Instrument, Ownership and Retirement Status.|
|100| Error |General error.|
|0 | Info |Success.|
