## Search Financial Institution


<table>
    <thead>
        <th>Element / property</th>
        <th>Description</th>
    </thead>
    <tbody>
        <tr>
            <td>RqUID</td>
            <td>Unique Request Identifier. The ClientSite sends this element with the request.</td>
        </tr>
        <tr>
            <td>Status</td>
            <td>The status of the request.</td>
        </tr>
        <tr>
            <td>FIInfoDataList</td>
            <td>Listof FI along with their requisite information.<br>This aggregate willbe present, only if the request was carried out successfully and thisoption was desired in the request.</td>
        </tr>
        <tr>
            <td>FIIdList</td>
            <td>Listof FI id’s.<br>This aggregate will be present, only if the request was carried out successfully and this option was desired in the request.</td>
        </tr>
        <tr>
            <td>Status</td>
            <td>The status of the FISearchRs <br><br>For Sevirity: Info<br> StatusCode - StatusDesc: <br><br> 0 -
                Success<br><br>For Sevirity: Error<br> StatusCode - StatusDesc: <br><br>100 - General Error<br>4060 - No
                data available<br>4070 - Invalid data in request<br>4360 - Data insufficient to carry out
                request<br>4591 - No FIs found matching the search criteria provided<br></td>
        </tr>
    </tbody>
</table>