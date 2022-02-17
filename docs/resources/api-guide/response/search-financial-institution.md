## Search Financial Institution

| Element / property | Description |
| --- | --- |
| Status | The status of the request. |
| RqUID | Unique Request Identifier. The Client Site sends this element with the request. |
| FIInfoDataList | List of FI along with their requisite information. This aggregate will be present, only if the request was carried out successfully and this option was desired in the request. |
| FIIdList | List of FI id’s. This aggregate will be present, only if the request was carried out successfully and this option was desired in the request. |

#### Possible Status Code

| StatusCode | Severity | StatusDesc |
| --- | --- | --- |
| 0 | Info | Success | 
| 100 | Error | General Error | 
| 4060 | Error | No data available | 
| 4070 | Error | Invalid data in request | 
| 4360 | Error | Data insufficient to carry out request | 
| 4591 | Error | No FIs found matching the search criteria provided | 

