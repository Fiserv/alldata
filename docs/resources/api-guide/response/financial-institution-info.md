## Financial Institution Info


<table>
    <thead>
        <th>Element / property</th>
        <th>Description</th>
    </thead>
    <tbody>
        <tr>
            <td>Status</td>
            <td>The status of the request.</td>
        </tr>
        <tr>
            <td>RqUID</td>
            <td> Unique Request Identifier. The Client Site sends this element with the request.</td>
        </tr>
        <tr>
            <td>FIInfoDataList</td>
            <td> List of FI along with their requisite information.<br>This aggregate will be present, only if the
                request was carried out successfully and this option was desired in the
                request. </td>
        </tr>
        <tr>
            <td>FIIdList</td>
            <td> List of FI id’s.<br>This aggregate will be present, only if the request was carried out successfully
                and this option was desired in the request.</td>
        </tr>
        <tr>
            <td>Status</td>
            <td>The status of the FIInfoRs <br><br>For Sevirity: Info<br> StatusCode - StatusDesc: <br><br> 0 -
                Success<br><br>For Sevirity: Error<br> StatusCode - StatusDesc: <br><br>100 - General Error<br>4060 - No
                data available<br>4070 - Invalid data in request<br>4260 - Invalid Date<br>9001 - Support for this ABA
                routing number coming soon.<br>9002 - ABA number is not supported.<br></td>
        </tr>
    </tbody>
</table>