# Health

This API allows the calling application to perform a health check. This
can be used to test connectivity and ensure availability of the
application.

## Get Health Check Status

The health check pings an endpoint. If successful, a 200 OK response is
returned with the text “Application Up”.

### Method and Endpoint

| GET | /api/Health |
|------|------------|

### Response

HTTP response 200 – OK returned for successful requests.

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/Health |
|----------------------------------------------------------|

#### Response

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p>200 OK</p>
<p>Application Up</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

