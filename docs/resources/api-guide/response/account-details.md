## Account Details

| Element / property | Description |
| --- | --- |
| Status | The status of the request. |
| RqUID | The Unique Request Identifier sent by the Server Site. |
| FILoginAcctInfoList | List of FILoginAcctInfo aggregates, one for each FI Login Account specified in the request.This aggregate list will be present only if the request was carried out successfully. |
| HTMLInfo | This element is present if HTMLSniplet or XML_AND_HTMLSniplet have been provided in the request. This aggregate will be present only if the request was carried out successfully. |

#### Possible Status Code

| StatusCode | Severity | StatusDesc | Condition | Action API Partner should take to resolve the error |
| --- | --- | --- | --- | --- |
| 4070 | Error | Validation failure. | One or more input fields were not valid. | Partner should make sure the mandatory parameters are sent in the request and in the defined format as in the corresponding XSD. |
