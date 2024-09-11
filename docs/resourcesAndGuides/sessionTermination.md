## Session Termination

To end a session before the token expires, initiate a token revocation
request:

### Method and Endpoint

| POST | /connect/revocation |
|------|---------------------|

### Request

| Parameter         | Req | Param Type | Data Type       | Description                                                 |
|-----------|-----|-------|--------|------------------------------------------|
| client\_id        | Req | body       | string          | Fiserv-provided ID; will be assigned during implementation. |
| token             | Req | body       | string          | Token to revoke.                                            |
| token\_type\_hint | Req | body       | "access\_token" |                                                             |

### Response

HTTP response 200 - OK returned for successful requests.

### Sample API Usage

#### Request Form Data

client\_id:yourSpecificClientId  
token:ee5612220a264169bbb4d42bd88413fa92327a894734031f7f619e8e12dba7d0  
token\_type\_hint:access\_token

#### Response

| 200 OK |
|--------|