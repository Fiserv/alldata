## Financial Institution Info

| Element / property | Description |
| --- | --- |
| Status | The status of the request. |
| RqUID | Unique Request Identifier. The Client Site sends this element with the request. |
| FIInfoDataList | List of FI along with their requisite information.<br>This aggregate will be present, only if the request was carried out successfully and this option was desired in the request. |
| FIIdList | List of FI id’s.<br>This aggregate will be present, only if the request was carried out successfully and this option was desired in the request. |

#### Possible Status Code

| StatusCode | Severity | StatusDesc |
| --- | --- | --- |
| 0 | Info | Success | 
| 100 | Error | General Error | 
| 4060 | Error | No data available | 
| 4070 | Error | Invalid data in request | 
| 4260 | Error | Invalid Date | 
| 9001 | Error | Support for this ABA routing number coming soon. | 
| 9002 | Error | ABA number is not supported. |