## Refresh an Access Token

To renew the access token the standard OAuth refresh token process
should be used.

### Method and Endpoint

| POST | /connect/token |
|------|----------------|

### Request

| Parameter      | Req | Param Type | Data Type        | Description                                                 |
|-----------|-----|-------|--------|------------------------------------------|
| client\_id     | Req | body       | string           | Fiserv-provided ID; will be assigned during implementation. |
| refresh\_token | Req | body       | string           | Token received from initial grant or previous refresh.      |
| grant\_type    | Req | body       | "refresh\_token" |                                                             |

### Response

| Parameter      | Req | Data Type | Description                                                      |
|------------|----|----------|-----------------------------------------------|
| access\_token  | Req | string    | The access token string as issued by the authorization server.   |
| expires\_in    | Req | integer   | The duration of time the access token is granted for.            |
| token\_type    | Req | string    | The type of token.                                               |
| refresh\_token | Req | string    | A refresh token that can be used to obtain another access token. |

### Sample API Usage

#### Request Form Data

client\_id: yourSpecificClientId  
refresh\_token:55dc7e5…  
grant\_type:refresh\_token

#### Response

```json
{
"access_token": "5402665…",
"expires_in": 600,
"token_type": "Bearer",
"refresh_token": "26346ca9d926a792…"
}
```